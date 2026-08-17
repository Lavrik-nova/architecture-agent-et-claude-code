# 09 · Ce que la mesure a trouvé

Deux relecteurs externes ont lu ce dépôt avant vous. Les deux ont posé la même
question : où sont les chiffres. Les voici, avec ce qu'ils ne prouvent pas.

---

## Le premier chiffre du projet

**121 questions réelles, gelées.** Tirées du trafic véritable, figées une fois
pour toutes, rejouées contre la version en production. 242 requêtes sous un
plafond de 300. Zéro erreur technique. Chaque requête étiquetée et conservée dans
le journal des dialogues, donc rejouable.

**Quatre juges indépendants**, chacun avec un contexte neuf et la grille
d'évaluation. Aucun des quatre n'a vu la course qui produisait les réponses. Un
agent qui se note lui-même donne toujours une bonne note ; c'est le même
processus qui répond et qui juge.

```
substantiel dès le premier tour ........ 76 / 121  =  62,8 %

des 45 manques :
   33  « consultez le site » pour ce que le site n'affiche pas
    9  un fait que la base possède et qui n'est pas remonté
    3  faux

prix et codes promo .................... 2 / 23   =  8,7 %
disponibilité et points de vente ....... 10 / 19
spécifications produit ................. 26 / 34
garantie ............................... 11 / 13
commande et livraison .................. 20 / 21
```

**Deuxième axe — l'exactitude**, notée seulement là où une source confirmée
tranche : 37 justes, 9 fautes, 50 affirmées avec assurance sans source, 25
indéterminées. Là où c'est tranchable : **80 % de justesse.**

Les neuf fautes ont toutes la même forme. **La base possède la réponse et la
réplique la contredit.** Ce n'est pas un trou de connaissance, c'est un défaut de
remontée — donc réparable, et réparable au bon endroit.

---

## Le chiffre le plus utile est le pire

**Prix et codes promo : 8,7 %.** Deux réponses satisfaisantes sur vingt-trois.

C'est la catégorie la plus faible de loin, et c'est celle qui coûte des ventes.
Aucune moyenne ne l'aurait montrée : à 62,8 % global, un tableau de bord serein
aurait affiché « ça progresse » pendant que la question qui précède l'achat
échouait neuf fois sur dix.

C'est la raison d'être d'une mesure par catégorie. Une note globale mesure le
confort de celui qui la publie.

---

## Les juges se sont trompés, et l'erreur est inscrite

Cinq verdicts déclaraient une adresse de courriel affichée en clavardage comme
une faute factuelle, en citant deux fichiers de consignes. Une décision du
propriétaire, plus récente et inscrite dans le code, dit exactement le contraire.

Les cinq verdicts ont été **reclassés et le compte est passé de 15 fautes à 9**,
avec la trace de la correction plutôt qu'une modification silencieuse.

Ce qui compte n'est pas la correction. C'est ce qu'elle a révélé : **le corpus de
consignes interdit encore ce que la décision du propriétaire autorise, et le
modèle lit les deux.** Les juges ont trébuché dessus. Le modèle trébuche pareil,
en silence, du côté du client.

Deux sources vivantes, aucune règle de préséance. Voilà le vrai défaut, et il n'a
été trouvé que parce que quelqu'un a refusé d'effacer une erreur gênante.

---

## Ce que ce chiffre ne prouve pas

Écrit ici pour qu'on ne me le cite pas plus fort que je ne l'ai dit.

- **Une seule course**, à une température fixée. Une autre course donnera un
  autre nombre.
- **Un juge modèle**, sans échantillon revu à la main. Les juges se sont déjà
  trompés une fois — c'est documenté plus haut.
- **Aucune ligne de base.** Personne n'a mesuré ce que ces 121 questions donnaient
  sans l'agent. Sans cela, 62,8 % n'est comparable qu'à une autre course du même
  jeu sous la même grille.

**C'est une référence, pas un verdict.** La différence est tout le sujet de ce
document.

---

## Le retour en arrière, avec l'heure et le numéro de ligne

Un drapeau de configuration devait **étendre** le contexte du clavardage. Il le
**remplaçait**. Deux lignes dans le fichier d'entrée.

Ce qui disparaissait silencieusement de l'invite en production :

| Perdu | Volume |
|---|---|
| Règles métier actives | 4 |
| Leçons tirées de l'exploitation | 14, dont 6 des 31 derniers jours |
| Couche mémoire unifiée | 390 lignes |
| Consignes de langue et de mémoire persistante | 2 fichiers |
| Note « dernier contact » | présente pour 171 clients sur 171 |
| Guide de ton, avec un correctif approuvé la veille | 1 |
| Historique de conversation | 6 tours au lieu de 10 |

**Rien ne plantait.** Le clavardage répondait, poliment, avec un contexte amputé.

**Vérifié contre l'image déployée, avec les numéros de ligne — pas contre la
branche.** La branche dit ce qu'on a voulu écrire. L'image dit ce qui tourne
devant le client. Sur ce projet, les deux ont déjà divergé.

Trois catégories de client changeaient de comportement : un client anglophone, un
client de retour, et toute règle ajoutée dans le dernier mois. **Aucune des trois
n'était dans les six cas du test de fumée.** Six cas qui passaient au vert au
moment même où trois catégories réelles se dégradaient.

**Drapeau désactivé immédiatement**, mise en service suivante, santé confirmée.
Le second drapeau est resté actif, avec sa raison écrite — dans cette
configuration le bloc s'ajoute au contexte complet et rien n'est perdu — et avec
l'aveu que **cette configuration précise n'a jamais fait partie du test de
fumée** non plus.

### Les trois règles que cet incident a produites

**Vérifier contre ce qui est déployé.** Pas contre la branche, pas contre
l'intention.

**Un test de fumée qui ne couvre pas les catégories de clients ne mesure rien.**
Six cas au vert ont accompagné une perte réelle jusqu'en production.

**Une panne silencieuse est pire qu'une panne bruyante.** Rien n'est tombé.
C'est précisément pour cela que ça a duré.

---

## Le critique a arrêté une couche dont tous les tests étaient verts

Le plus inconfortable des trois, et le plus instructif.

Une nouvelle couche de raisonnement passait tout : **44 assertions sur 44**, **44
sur 44 qui rougissent sous mutation** — donc les tests mordent vraiment — et
sortie **identique octet pour octet sur 12 entrées sur 12** avec le drapeau
désactivé. Par tous les critères que j'avais posés, c'était prêt.

Un sous-agent adverse l'a bloquée avant la validation, avec six défauts.

> Les tests étaient verts et mesuraient la mauvaise chose : le catalogue de
> tests que j'avais assemblé protégeait une implémentation que le vrai
> catalogue casse.

Trois des reproches ont été **vérifiés en lisant le code plutôt que crus sur
parole.** Les trois tiennent :

- Une règle de famille de produits ne se déclenchait jamais, parce que les vrais
  noms du catalogue ne contiennent pas le mot sur lequel elle s'appuie.
- Une fonction « est-ce une question sur un produit ? » existait déjà et n'était
  pas utilisée. **Mesuré sur 837 messages réels : 796 auraient reçu une consigne
  de doute, dont 645 hors sujet.**
- Une docstring affirmait que le bloc de prix était retenu dans un cas précis. Il
  ne l'était pas. Le contexte aurait porté « les seuls prix utilisables » et « ne
  donne aucun prix » en même temps.

Deux défauts de plus que le critique a vus et que je n'avais pas : un
rapprochement par préfixe transformait des mots courants en noms de produits, et
le système déduisait une famille à partir d'une couleur contenue dans un nom —
exactement l'inférence que le propriétaire avait interdite.

**Mon propre test censé interdire cette inférence ne pouvait pas la voir : il
lisait des chaînes de caractères, pas un comportement.**

### Ce que cet incident a réellement changé

La conclusion n'était pas « corriger la couche ». C'était que **l'ordre des
étapes est faux** : une famille de produits ne peut pas se lire dans la façon
dont une ligne de catalogue est nommée, parce que les noms viennent de deux
sources aux orthographes différentes et changent en une semaine. Il faut d'abord
le champ typé.

Et cette conclusion-là **n'est pas la mienne à prendre.** Les deux options
honnêtes ont été écrites, avec une recommandation et sa raison, et la décision
revient au propriétaire du système.

---

## Le mur comptait au lieu de bloquer

Dernier, et le plus court à dire.

Une réponse a expédié à un client une adresse annulée par une décision écrite.
La règle correspondante figurait bien parmi les règles bloquantes. Elle est
partie quand même.

Le garde-fou déterministe tournait **en mode observation** en production. Il
notait la faute au lieu de l'arrêter — il comptait la queue qu'il avait été
construit pour tenir.

Le corriger change ce que le client voit. C'est donc une décision du propriétaire
et une étape à part entière, pas un effet de bord.

**La leçon transférable :** un contrôle en mode observation n'est pas un contrôle,
et le tableau de bord ne fait pas la différence. Il affiche du vert dans les deux
cas.

---

## Pourquoi ce document existe

Deux relecteurs ont écrit, indépendamment, que le dépôt disait « production »
sans jamais montrer un chiffre ni un incident daté. Ils avaient raison, et la
matière existait — dans l'historique du projet, pas dans la vitrine.

C'est un défaut classique et il vaut la peine d'être nommé : **on documente ce
qu'on a compris, et on oublie de documenter ce qu'on a payé pour comprendre.** Le
second a plus de valeur.

---

**Voir aussi :** [03 · La pile de raisonnement](03-pile-de-raisonnement.md) ·
[07 · Attraper une mauvaise réponse](07-attraper-une-mauvaise-reponse.md) ·
[08 · Ce qui est délibérément absent](08-deliberement-absent.md)
