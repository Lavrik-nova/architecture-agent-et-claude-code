# 07 · Attraper une mauvaise réponse

Une mauvaise réponse, dans ce système, ne lève aucune exception. Elle est bien
formée, fluide, dans la bonne langue, et indiscernable d'une bonne jusqu'à ce que
quelqu'un agisse dessus.

Tout ce qui suit existe parce que c'est vrai.

---

## Le critique adverse

Les changements apportés à la couche de raisonnement sont relus par un critique
dont les consignes sont écrites pour rendre l'éloge difficile. Voici l'invite
réelle, identifiants du projet retirés :

```text
Tu es un critique de code adverse sur un agent de service à la clientèle en
production. Ton travail est d'essayer de CASSER le changement, pas de le louer.

Règles que tu dois suivre :
- Ne signale que des défauts que tu peux pointer avec un fichier et une ligne
  du diff.
- Pour chacun, donne la défaillance concrète : entrées ou état -> mauvais
  comportement.
- Si tu n'es pas certain, écris PLAUSIBLE, pas CONFIRMÉ. Deviner coûte plus
  cher que se taire.
- Tu n'as PAS lu le registre des décisions du projet. Si quelque chose paraît
  faux mais pourrait être une décision délibérée du propriétaire, dis-le
  explicitement au lieu de l'affirmer.
- Ne propose jamais de corriger un comportement en ajoutant des mots à une
  invite. Sur ce projet, cela a été annulé trois fois ; un contrôle doit être
  du code, un drapeau ou un test.
- Dis franchement si le changement est sain. Un critique qui trouve toujours
  quelque chose est du bruit.

Réponds en JSON : {"findings":[{"severity":"haute|moyenne|basse",
"verdict":"CONFIRMÉ|PLAUSIBLE","file":"","line":0,"claim":"","failure":"",
"fix":""}],"verdict_overall":""}
```

Cinq de ces lignes ont chacune été écrites après une défaillance précise.

**« Pointer un fichier et une ligne. »** Sans cela, un critique produit des
opinions d'architecture qu'on ne peut ni appliquer ni vérifier. Un constat qu'on
ne peut pas localiser n'est pas un constat.

**« PLAUSIBLE, pas CONFIRMÉ. »** Un relecteur qui affirme tout avec la même
assurance oblige à tout vérifier, ce qui coûte plus que la relecture n'a fait
économiser. Graduer la confiance rend la sortie triable.

**« Tu n'as PAS lu le registre des décisions. »** Le critique est délibérément
aveugle. Un relecteur qui détient l'historique rationalise : il voit un choix
étrange et suppose qu'il était voulu. Un aveugle demande pourquoi. Il se trompe
plus souvent, et ses erreurs coûtent peu ; ses trouvailles justes sont la raison
d'être de la relecture. C'est cette configuration qui a attrapé un test qui
exerçait une réimplémentation au lieu du vrai code — voir
[05](05-memoire-et-mises-a-jour.md).

**« Ne jamais proposer d'ajouter des mots à une invite. »** Annulé trois fois.
Ajouter une phrase à une invite paraît le correctif le plus rapide et tient
jusqu'à l'entrée inhabituelle suivante, puis lâche en silence. La règle est
maintenant imposée au relecteur pour qu'il ne puisse même pas suggérer la chose
tentante.

**« Un critique qui trouve toujours quelque chose est du bruit. »** Sans
permission explicite d'approuver, un critique invente des constats pour justifier
l'appel, et l'équipe apprend à l'ignorer. La permission est ce qui garde le signal
vivant.

---

## Ce qui est consigné à chaque échange

| Consigné | Sert à |
|---|---|
| Message, langue résolue, langue de session | Les pannes de langue — les plus fréquentes et les plus visibles |
| Intention et étiquettes détectées | Les erreurs de classement systématiques |
| Fiches de principe activées | Quelle règle a gouverné ce cas, pour remonter un mauvais motif jusqu'à sa fiche |
| Faits retenus | Si la bonne connaissance était atteignable |
| Issue du contrat d'incertitude et raison d'arrêt | Où le système est aveugle |
| Réponse envoyée, ou transfert | La reconstitution |

L'intérêt de consigner *les fiches et les faits* plutôt que la seule réponse :
quand une réponse est fausse, la question est de savoir quelle couche a lâché. De
mauvais faits désignent la récupération ou la base. De bons faits avec une
mauvaise réponse désignent la fiche ou la génération. Sans l'état intermédiaire,
chaque enquête repart de zéro et se termine généralement par une retouche
d'invite — le correctif que le critique a l'interdiction de suggérer, pour de
bonnes raisons.

---

## Ce qui déclenche une alerte

Pas le taux d'erreur. Les erreurs sont ici majoritairement silencieuses, donc un
faible taux d'erreur n'apprend rien et rassure à tort.

**Des raisons d'arrêt qui se regroupent.** La même information manquante bloquant
de nombreuses conversations est une lacune précise : soit un fait que la base
devrait posséder, soit une règle plus stricte qu'elle n'a besoin de l'être. Les
deux se réparent et ni l'une ni l'autre n'est visible dans un agrégat.

**La langue qui bascule en pleine conversation.** Détectable exactement, et cela
signifie que le détecteur a rencontré une tournure qu'il ne connaît pas. C'est
ainsi qu'ont été trouvés les cas de français québécois sans accents.

**Un contrat d'incertitude qui cesse d'arrêter une classe qu'il arrêtait avant.**
Une barrière qui a discrètement cessé d'arrêter est pire qu'une barrière qui tombe
bruyamment, parce que le système a l'air de s'être amélioré.

**Une récupération qui ne rend rien.** Une requête qui n'a rencontré aucune page
approuvée est soit du vocabulaire que la base ne porte pas, soit une page qui
devrait exister. Les deux méritent une lecture individuelle.

---

## Comment une défaillance est disséquée

Ordre fixe, parce que sauter à la fin est exactement ainsi que naissent les
retouches d'invite.

1. **Reproduire avec la même entrée.** Si ça ne se reproduit pas, la variance
   elle-même est le constat.
2. **Lire l'état intermédiaire consigné.** Quelles fiches se sont activées, quels
   faits ont été retenus, ce que le contrat a conclu.
3. **Localiser la couche.** Langue, intention, fiches, faits, contrat,
   génération. La consignation transforme cette étape en consultation plutôt
   qu'en débat.
4. **Corriger à cette couche.** Une fiche est une donnée : on la modifie et on la
   réapprouve. Une panne de récupération, ce sont des mots-clés ou une page
   manquante. Une panne de génération, c'est une frontière de fiche trop lâche.
5. **Ajouter le cas au jeu de tests.** Pas à l'invite.

L'étape 5 est celle qui compose. Chaque défaillance disséquée devient un cas que
les changements futurs devront passer, et l'ensemble grossit jusqu'à décrire tout
ce que le système est connu pour bien faire.

---

## La règle qui court sous tout cela

> Un contrôle doit être du code, un drapeau ou un test.

Trois retours en arrière l'ont enseignée ici, et c'est la même phrase qui donne sa
forme à la configuration de la partie I. Une contrainte qui dépend de la
coopération du modèle n'est pas une contrainte — c'est une préférence, valable
jusqu'à ce que l'entrée devienne inhabituelle.

Un cas où le critique a bloqué une couche dont les 44 tests étaient verts est
raconté en entier dans
[09 · Ce que la mesure a trouvé](09-ce-que-la-mesure-a-trouve.md).

---

**Suite :** [08 · Ce qui est délibérément absent](08-deliberement-absent.md)
