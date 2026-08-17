# 02 · L'économie du contexte

> La connaissance n'est pas gratuite à porter. Elle est gratuite à *ranger* et
> coûteuse à *transporter*. Presque toutes les erreurs de configuration que je
> vois viennent de la confusion entre les deux.

## Les trois paliers

Le contexte de travail d'un assistant contient exactement trois sortes de
matière, distinguées par le moment où elles se chargent.

### Palier 1 — toujours chargé, toujours payé

Le fichier d'instructions du projet est lu au début de chaque session et porté à
chaque tour. Une ligne ici est facturée sur la demande où elle est indispensable
et sur les quatre cents demandes où elle ne l'est pas.

**Test d'admission.** Une règle appartient au palier 1 seulement si elle satisfait
les deux conditions :

1. **Elle est vérifiable.** Quelqu'un qui lit le résultat peut dire si elle a été
   suivie. « Demander avant d'ajouter une dépendance » est vérifiable. « Écrire du
   code propre » ne l'est pas — c'est une humeur.
2. **L'enfreindre coûte cher ou est irréversible.** Une convention qui se corrige
   en une minute après coup n'a pas besoin d'être portée à chaque demande.

Tout ce qui échoue à l'un des deux tests descend au palier 2 ou 3. Cela coupe
généralement un fichier existant de moitié au premier passage.

### Palier 2 — toujours chargé, mais une à trois lignes

La `description` d'une compétence est chargée en permanence ; **son corps ne l'est
pas.** Cette asymétrie est le mécanisme sur lequel tout le système repose.

La description n'est pas un résumé. C'est une **spécification de déclencheurs** :
les mots, tournures et situations qui signifient « le corps de ceci devient
pertinent ». Écrivez-la dans la langue que l'utilisateur tape réellement. Une
description en anglais ne s'activera pas de façon fiable sur une demande écrite en
français, et l'échec est silencieux — la compétence ne se déclenche simplement
pas, et tout le monde conclut qu'elle ne fonctionne pas.

Une bonne description nomme les deux côtés :

```yaml
description: >
  Connaissance pour les choix d'architecture coûteux dans les systèmes
  d'agents — mémoire, récupération, évaluation, orchestration, fiabilité.
  Retrouver un patron éprouvé avant d'en inventer un.
  Pas pour : correctifs de bogues, remaniements, tests, documentation,
  mises à jour de dépendances, retouches de ton.
```

La clause « pas pour » fait un vrai travail. Sans elle, une compétence formulée
trop largement s'active sur tout, et l'on a discrètement reconstruit le palier 1 à
un prix plus élevé.

### Palier 3 — chargé à la demande, gratuit au repos

Corps des compétences, pages de wiki, fiches de décision, matière de référence. Ce
palier peut être arbitrairement grand. Dix mille mots de contraintes durement
acquises ne coûtent rien sur les demandes qui n'en ont pas besoin.

Presque tout appartient ici. L'instinct de promouvoir une page au palier 1 parce
qu'elle est *importante* est l'instinct à combattre. L'importance n'est pas le
critère — **la fréquence du besoin l'est.**

## La règle de placement

> Si vous devez connaître une règle pour vous rendre compte qu'il est temps de la
> lire, elle va dans la description.
> Si vous n'en avez besoin qu'une fois commencé, elle va dans le corps.

Exemple travaillé. Une politique de traitement de matériel vidéo :

| Règle | Palier | Pourquoi |
|---|---|---|
| « Un lien vidéo veut dire : lancer la compétence vidéo » | 2 — description | Sans cela, vous n'ouvrez jamais le corps |
| « Par défaut, mode par scènes, plafond de 100 images » | 3 — corps | Pertinent seulement une fois lancé |
| « La transcription va sur le disque ; seul un résumé entre en contexte » | 3 — corps | Idem |
| « Un enregistrement contenant des données client ne quitte jamais la machine » | 1 — invariant | Vérifiable, et l'enfreindre est irréversible |

Quatre règles sur un seul outil, réparties sur trois paliers. Mettre les quatre au
palier 1 est l'erreur courante et coûte environ huit fois plus cher sans rien
apporter.

## Ce que cela remplace

**Ne mettez pas un classificateur à modèle sur chaque demande.** Il est
techniquement possible de faire tourner un modèle rapide sur chaque invite
entrante pour décider quel contexte injecter. C'est un mauvais échange : de la
latence à chaque tour, du coût à chaque tour, et un nouveau composant qui peut
tomber à chaque tour — pour accomplir ce qu'une description bien écrite fait à
l'intérieur de la requête existante.

Réservez les accroches programmatiques au travail **déterministe** : bloquer une
commande dangereuse, lancer les tests après une modification, vérifier qu'une
fiche de décision accompagne un changement dans un répertoire sensible. Une
vérification déterministe sur le chemin critique est peu coûteuse et honnête. Un
appel de modèle sur ce même chemin n'est ni l'un ni l'autre.

## Réviser le budget

Le palier 1 grossit par accrétion. Personne n'ajoute quarante lignes d'un coup ;
tout le monde en ajoute deux, onze fois. Mettez une révision au calendrier et
appliquez une seule question à chaque ligne :

> Cette règle survivrait-elle à une lecture à voix haute devant un nouveau
> collègue, le premier jour, comme une chose à retenir pour toujours ?

Sinon, elle descend d'un palier ou elle disparaît. Les révisions sont courtes.
Les sauter est la façon dont un fichier de 40 lignes devient un fichier de 150
que plus personne ne lit attentivement, y compris le modèle.

---

**Voir aussi :** [03 · Architecture de la mémoire](03-architecture-memoire.md) ·
[01 · Ce qui est chargé](01-ce-qui-est-charge.md)
