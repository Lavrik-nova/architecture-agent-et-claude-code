# 06 · Limites et passage à l'humain

L'essentiel de l'ingénierie de ce système ne porte pas sur le fait de répondre.
Il porte sur le fait de savoir quand ne pas le faire.

Un agent qui produit toujours une réponse est simple à bâtir et coûteux à
exploiter, parce que le prix de ses mauvaises réponses est payé par quelqu'un
d'autre — le client qui agit sur une spécification fausse, l'employé qui doit
retirer un engagement de garantie qui n'aurait jamais dû être donné.

---

## Trois façons de s'arrêter, et elles ne sont pas interchangeables

| Issue | Quand | Ce que le client voit |
|---|---|---|
| **Question ciblée** | Une seule information manquante trancherait | Une question, pas un formulaire |
| **Réponse nuancée** | Assez pour être utile, pas assez pour être complet | La réponse, plus la limite énoncée |
| **Passage à l'humain** | La décision n'appartient pas au système | Un transfert clair, avec le contexte conservé |

Laquelle s'applique est décidé par l'`uncertainty_rule` de la fiche qui gouverne —
par fiche, pas globalement. Voir
[03 · La pile de raisonnement](03-pile-de-raisonnement.md).

**Pourquoi par fiche.** Une information manquante sur une question de stock et une
information manquante sur une question de garantie ne sont pas la même situation.
L'une tolère une réponse nuancée ; l'autre ne doit pas en produire, parce qu'une
réponse de garantie nuancée est lue comme un engagement malgré la nuance. Un seuil
de confiance global ne peut pas exprimer cela. Les systèmes bâtis sur un seul
seuil finissent réglés sur le cas le plus strict — donc inutiles partout ailleurs
— ou réglés sur la moyenne, donc faux là où ça compte.

---

## La question ciblée, et pourquoi il n'y en a qu'une

La bonne réponse à un message sous-déterminé est souvent une question de
clarification. Le mode de panne consiste à les poser toutes.

Un message avec quatre inconnues et une réplique en forme de formulaire perd le
client à la deuxième question. La contrainte de conception est que le système
demande **la seule information qui change le plus la réponse**, et travaille avec
ce qu'il a pour le reste — en énonçant l'hypothèse qu'il a faite, pour que le
client la corrige au passage.

C'est la différence entre un système qui clarifie et un système qui interroge, et
c'est pourquoi le champ `known_facts` d'une fiche compte : il dit de quelles
entrées la règle dépend vraiment, ce qui permet de classer ce qui manque au lieu
de l'énumérer.

---

## L'ambiguïté est un état, jamais une exclusion silencieuse

Une règle empruntée au volet rapports du même système, parce qu'elle se généralise :

```python
"""status(task) -> "répondu" | "ouvert" | "ambigu". Une tâche est RÉPONDUE
seulement si SON PROPRE fil contient un envoi strictement postérieur à
l'arrivée. C'est par fil et par horodatage — JAMAIS un seuil d'envoi global —
de sorte qu'une réponse tardive au client B ne peut pas masquer un client A
resté sans réponse. Aucun lien établi = "ambigu" → jamais exclu en silence."""
```

Deux règles à en extraire.

**« Ambigu » est un troisième état visible.** Quand le système ne peut pas établir
si quelque chose a été traité, il dit *ambigu* et l'achemine pour tri. Il ne
devine pas, et il ne laisse pas tomber l'élément. Tout ce qui ne peut pas être
classé doit finir devant une personne — le silence est la seule issue jamais
acceptable, parce que le silence ressemble en tout point à un succès.

**Jamais un seuil global.** L'implémentation tentante compare à un seul horodatage
pour tout le monde. Elle est plus simple et elle cache exactement le cas qui
compte : une réponse tardive à un client masquant un autre client resté sans
réponse. La vérification fil par fil demande plus de travail et c'est la seule
version qui ne peut pas dissimuler une défaillance.

Les deux règles relèvent du même instinct que le filtre SQL de
[04](04-connaissance-produit.md) : rendre le cas dangereux structurellement
impossible plutôt que compter sur le fait qu'on le remarque.

---

## Pourquoi avouer son ignorance vaut ce que ça coûte

Le coût est réel. « Je dois vérifier » satisfait moins qu'une réponse, et une
partie de ces conversations auraient été réglées par une bonne devinette.

C'est quand même le bon arbitrage, pour une raison propre à ce domaine.

**Une mauvaise réponse ici n'est pas une réponse médiocre — c'est un engagement.**
Une admissibilité à la garantie énoncée, une dimension citée, une disponibilité
confirmée : le client agit dessus. En retirer une coûte plus de bonne volonté que
de ne l'avoir jamais offerte, et coûte à l'employé l'appel que le module existait
pour éviter, cette fois avec un client contrarié au bout du fil.

**La panne est invisible de l'intérieur.** Une réponse fluide et fausse ne génère
aucune erreur, aucune alerte, et dans la plupart des cas aucune plainte — le
client ne revient simplement pas. Un système optimisé sur le taux de complétion
dérive vers l'erreur assurée, et ses indicateurs s'améliorent tout du long.

La contrainte est donc inversée : **le système est optimisé pour ne pas se
tromper, et c'est le taux de complétion qui a le droit de souffrir.** La mesure
qui compte n'est pas combien de conversations se sont terminées par une réponse,
mais combien se sont terminées correctement — y compris celles qui se sont
correctement terminées par un transfert.

---

## Ce qu'un bon transfert emporte

Un transfert qui perd le contexte est une deuxième conversation, et le client se
répète. Ce qui est conservé :

- L'échange complet, dans la langue où il a eu lieu
- Ce que le système a établi — intention, faits retenus, contrainte qui l'a bloqué
- Pourquoi il s'est arrêté : quelle règle, quelle information manquante

Le dernier point est celui qu'on omet d'habitude, et c'est celui qui rend le
transfert utile. Un employé qui reçoit *« admissibilité à la garantie
indéterminée : date d'achat inconnue, mode de bris compatible avec l'usure »*
part d'une position. Celui qui reçoit *« escaladé »* part de zéro.

C'est aussi la matière première de l'amélioration. Une raison d'arrêt qui revient
est une lacune précise et actionnable — un fait que la base devrait posséder, ou
une règle plus stricte qu'elle n'a besoin de l'être. Les raisons d'arrêt sont la
façon dont le système vous dit où il est aveugle.

---

**Suite :** [07 · Attraper une mauvaise réponse](07-attraper-une-mauvaise-reponse.md)
