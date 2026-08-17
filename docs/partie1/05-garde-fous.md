# 05 · Les garde-fous

> Dire à un modèle de ne pas faire quelque chose, c'est une demande. Ce n'est pas
> un contrôle. Toute la conception décrite ici découle de cette seule phrase.

## Les deux boîtes

Chaque action disponible pour l'assistant va dans une des deux boîtes. Il n'y en a
pas de troisième, et les cas ambigus vont dans la seconde.

### Boîte A — réversible

Lire et analyser des fichiers. Produire du texte, du code, des plans, des
brouillons. Modifier des fichiers à l'intérieur de l'arbre de travail du projet.
Les opérations de gestion de version qui restent locales.

**Cela avance sans confirmation.** Demander confirmation sur du travail réversible
est le moyen le plus rapide de s'entraîner à approuver sans lire — et c'est ainsi
que les confirmations sur *l'autre* boîte cessent de fonctionner.

### Boîte B — irréversible ou tournée vers l'extérieur

- Publier sur un dépôt distant ; publication forcée ; tout ce qui porte
  `--force` ou `--no-verify`
- Suppression récursive ; écrasement en masse
- Modification de l'intégration ou du déploiement continus
- Envoyer quoi que ce soit vers l'extérieur : courriel, message, demande de
  fusion, appel à un service tiers
- Écrire hors du répertoire du projet
- Tout ce qui touche aux données ou aux identifiants de production

**Cela s'arrête et attend.** Pas « demander gentiment » — s'arrêter.

## Le protocole d'arrêt

Quand une action tombe dans la boîte B, quatre étapes, dans l'ordre :

1. **Expliquer en langage clair : quoi, pourquoi, et quel est le risque.** Écrit
   pour une personne qui décide, pas pour une personne qui code. Si l'explication
   exige que le lecteur comprenne déjà le système, ce n'est pas une explication.
2. **Nommer les actifs concernés, précisément.** Pas « il aura accès aux
   fichiers » — nommer le fichier d'identifiants, la base de production, les
   dossiers clients. C'est la formulation abstraite qui permet à une mauvaise
   décision de paraître acceptable.
3. **Recommander une relecture extérieure** pour tout ce qui est conséquent. Un
   second avis d'un système indépendant, sur une capture d'écran, coûte quelques
   minutes.
4. **Attendre une phrase d'autorisation explicite et non ambiguë.**

Le dernier point n'est pas une cérémonie. « Oui », « ok », « d'accord » et
« vas-y » sont ce qu'on tape en pensant à autre chose. Choisissez une phrase qu'on
ne peut pas produire par réflexe, et n'honorez que celle-là.

## Où vivent les vrais contrôles

Un contrôle qui n'existe que sous forme de texte d'instruction est une suggestion.
Les vrais contrôles vivent dans le harnais.

| Contrôle | Où | Pour |
|---|---|---|
| Vérification déterministe avant exécution | Accroche avant appel d'outil | Motifs de commande dangereux, suppression récursive, écritures hors projet, sortie de données, chemins d'identifiants, déploiement |
| Vérification après changement | Accroche après appel d'outil | Tests ciblés, validation de schéma, analyse statique |
| Vérification de fin | Accroche d'arrêt | Si un répertoire sensible a changé sans fiche de décision ni cas d'évaluation, l'exiger avant de terminer |
| Isolation | Bac à sable ou compte séparé | Tout ce qui exécute du code tiers |

Trois règles sur les accroches, apprises au prix fort :

**Garder les appels de modèle hors du chemin critique.** Un classificateur sur
chaque demande ajoute de la latence, du coût et un point de panne à chaque tour.
Les accroches servent aux vérifications déterministes. Si une vérification exige
du jugement, elle appartient à la conversation, pas au pipeline.

**Les accroches globales protègent ; les accroches de projet vérifient.** Une
accroche à l'échelle de la machine ne devrait bloquer que les opérations
véritablement dangereuses. Tout ce qui doit connaître le fonctionnement de *ce*
projet appartient au projet.

**Ne pas traiter un mécanisme expérimental comme une barrière de production.** Si
la documentation qualifie une fonction d'expérimentale, elle peut conseiller mais
ne doit pas être la seule chose entre vous et une action irréversible.

## Code tiers : ce que les contrôles couvrent réellement

Cette section mérite d'exister séparément, parce que c'est là que le raisonnement
se ramollit le plus souvent.

Quand vous installez une compétence ou une extension tierce, **elle s'exécute avec
les privilèges de votre compte.** Les règles de permission dans la configuration
de l'assistant encadrent *les appels d'outils de l'assistant*. Elles ne
contraignent pas un processus enfant. Une fois qu'un script tourne, il peut lire
tout ce que votre compte peut lire.

L'inventaire honnête avant d'installer quoi que ce soit est donc : *jusqu'où mon
compte peut-il atteindre ?* Sur une machine de travail typique, cela comprend des
identifiants de production, des jetons de déploiement, des dossiers clients et
tous les dépôts que vous avez clonés. C'est le rayon d'action, et aucun fichier de
configuration ne le réduit.

**Ce qui ne fonctionne pas :**
- Les listes d'interdiction dans la configuration de l'assistant — mauvaise couche.
- Le nombre d'étoiles et la licence. Preuves de popularité, pas de sûreté.
- « C'est ouvert » — vrai, et sans portée tant que personne ne l'a lu.

**Ce qui fonctionne :**
- Lire les scripts avant la première exécution. Quelques centaines de lignes en
  général. Pas une garantie, mais une vraie vérification plutôt qu'un substitut.
- Une isolation réelle : compte séparé, conteneur, machine virtuelle. Efficace, et
  cela coûte la commodité qui donnait envie de l'outil.
- Quand un service doit être automatisé sous un compte, utiliser un **compte dédié
  créé pour cela**, pour que le pire résultat soit la perte de quelque chose de
  vide.

**Une discipline de formulation que je m'applique.** Si je n'ai pas lu le code,
j'écris « non audité », jamais « aucune télémétrie trouvée ». Licence et adoption
sont des preuves de popularité, pas d'un audit, et les décrire comme un audit est
la façon dont une équipe en vient à croire qu'elle a fait une diligence qu'elle
n'a jamais faite. La formulation est le contrôle.

## L'ordonnancement est un contrôle de sécurité

Le garde-fou le moins évident, et le plus souvent sauté.

**N'installez pas de nouvel outillage dans la même fenêtre qu'un déploiement en
production.** Non parce que l'installation est dangereuse, mais parce que deux
changements simultanés détruisent l'attribution. Quand quelque chose casse le
lendemain matin, vous ne pouvez pas dire lequel des deux en est la cause, et vous
passerez plus de temps sur cette question que l'outil ne vous en aurait jamais
fait gagner.

« Un changement à la fois » s'énonce d'habitude au niveau du code. Cela vaut avec
la même force au niveau de la journée de travail.

---

**Voir aussi :** [01 · Ce qui est chargé](01-ce-qui-est-charge.md) ·
[06 · Comment je sais que ça marche](06-comment-je-le-sais.md)
