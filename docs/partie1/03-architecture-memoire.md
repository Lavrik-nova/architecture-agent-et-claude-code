# 03 · Architecture de la mémoire

> Le difficile, dans un système de connaissance, n'est pas de noter les choses.
> C'est de refuser d'en noter la plupart, et de pouvoir défendre chaque refus.

## La boucle

```
arrivée/  →  tri  →  wiki/  →  index.md
   │         │                    │
   │         └──→ journal.md ←────┘
   │              (ajout seul)
   └──→ archives/   (originaux, jamais supprimés)
```

Cinq pièces mobiles. Chacune existe parce que son absence produit une défaillance
précise et observée.

| Pièce | Rôle | Défaillance si absente |
|---|---|---|
| `arrivée/` | Sas. La matière arrive et n'est pas encore de la connaissance | La matière brute est collée directement dans des pages durables ; la provenance est perdue en une semaine |
| Le tri | Un jugement, porté une fois, consigné | Tout est gardé. Le dépôt grossit et sa qualité moyenne baisse jusqu'à ce que plus personne ne le consulte |
| `wiki/` | Des pages denses, avec leur provenance | Beaucoup de pages minces. Des contraintes liées se dispersent et se contredisent en silence |
| `index.md` | Une ligne par page. La seule partie lue couramment | Le dépôt ne peut pas être consulté à bas prix, donc il ne l'est pas |
| `journal.md` | Ajout seul. Ce qui a été pris, ce qui a été refusé, pourquoi | Aucun moyen d'auditer le dépôt sans le lire en entier |
| `archives/` | Originaux, déplacés et non supprimés | Une affirmation contestée ne peut pas être retracée jusqu'à sa source |

## Le tri : trois issues, pas deux

Chaque élément se résout en exactement une de celles-ci.

**Un patron qui fonctionne.** Quelqu'un l'a fait tourner en production et dit
comment. Il porte ses contraintes — ce qui le rend sûr, récupérable, observable —
et il dit où il ne s'applique **pas**. On le garde.

**Une correction utile.** Elle contredit quelque chose que le dépôt contient déjà.
On la garde, et on n'écrase pas l'ancienne page en silence. On marque
l'affirmation remplacée, on dit laquelle l'emporte et pourquoi. Le raisonnement
vaut plus que la conclusion, parce que la prochaine contradiction en aura besoin.

**Rien.** Un réchauffé, une liste d'outils, du matériel promotionnel, ou la
redite de quelque chose qu'on possède déjà. **On le dit nommément, en une ligne,
et on n'ajoute rien.**

Cette troisième issue est celle qui porte la structure. Deux tiers de la bonne
matière y finissent. Un dépôt qui ne refuse jamais n'est pas organisé, et l'effort
d'organisation est exactement ce qui rend le tiers restant digne de confiance.

## Provenance : trois champs, non négociables

Chaque page et chaque entrée porte :

```yaml
source:      auteur + lieu de publication
as_of:       la date de la matière, pas la date du classement
source_type: primaire | secondaire
```

`primaire` — documentation officielle, le dépôt ou les écrits de l'auteur, un
compte rendu de production avec des preuves.
`secondaire` — la redite de l'un de ces éléments par quelqu'un d'autre.

**La matière secondaire ne tranche jamais une décision à elle seule.** Elle peut
soulever une hypothèse, pointer vers une source primaire, ou corroborer. Elle ne
peut pas être la raison d'un changement d'architecture. Cette seule règle élimine
l'essentiel des dégâts que les billets de blogue assurés causent à une base de
connaissance.

`as_of` compte plus qu'il n'en a l'air. Un patron juste pour une génération de
modèle est souvent un échafaudage bâti autour d'une limite qui n'existe plus. Une
connaissance sans date ne peut pas être mise à la retraite, donc elle ne l'est
jamais.

## Typer la connaissance : quatre sortes, pas une

« Ne consigner que des faits confirmés » sonne rigoureux et détruit
discrètement de la valeur. Une observation non confirmée n'est pas sans valeur —
elle vaut exactement ce que vaut une observation non confirmée, à condition d'être
étiquetée comme telle.

| Type | Définition | A le droit de porter une décision ? |
|---|---|---|
| `observation` | Quelque chose vu une fois. Aucun mécanisme proposé | Non |
| `hypothèse` | Un mécanisme proposé, pas encore éprouvé | Seulement pour concevoir un test |
| `confirmé` | Vérifié contre le système lui-même, ou une source primaire | Oui |
| `politique` | Une décision prise, qui lie désormais | Oui — c'est la décision elle-même |

La règle est qu'un élément non confirmé ne doit jamais être *présenté* comme un
fait — pas qu'il faut le jeter. Jeter les hypothèses oblige à les redécouvrir.

## Pourquoi pas un seul fichier de connaissance qui grossit

Le fichier unique en ajout seul est la conception la plus courante et elle échoue
de façon prévisible.

- Il est soit toujours chargé — palier 1, et il grossit sans limite — soit jamais,
  auquel cas c'est un journal intime.
- L'ajout sans structure fait s'accumuler les contradictions en silence. La ligne
  40 et la ligne 900 se contredisent ; rien ne fait remonter le conflit.
- Il n'y a pas d'unité à remplacer, à dater ou à retirer, donc rien ne l'est
  jamais.

Le remède n'est pas une base de données. C'est *des pages avec leur provenance*,
*un index bon marché à lire*, et *un journal qui consigne les refus*. Du markdown
ordinaire dans le dépôt survit ; les systèmes de documentation qui vivent ailleurs
ne survivent pas.

## Où passe la frontière

La connaissance vit dans le dépôt. **Les données client n'entrent pas dans le
dépôt de connaissance, sous aucune forme, à aucune étape** — ni comme exemple, ni
anonymisées, ni « juste pour une page ». Les patrons tirés du travail avec des
données client sont de la connaissance ; les données ne le sont pas, et les deux
sont faciles à confondre à onze heures du soir.

Si la matière en arrivée contient des données personnelles ou clients, elle est
traitée sous les règles applicables à ces données — plus strictes que celles-ci —
et seul le patron traverse vers le dépôt.

---

**Voir aussi :** [02 · L'économie du contexte](02-economie-du-contexte.md) ·
[04 · La compétence de tri](04-competence-de-tri.md) ·
[05 · Les garde-fous](05-garde-fous.md)
