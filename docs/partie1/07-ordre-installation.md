# 07 · L'ordre d'installation — cinq étapes

Chaque étape dépend de la précédente. Les faire dans le désordre produit une
configuration qui a l'air complète et qu'on ne peut pas évaluer.

Le temps de travail total est d'environ trois heures, réparties sur une semaine
qui commence par ne rien faire.

---

## Étape 0 · Référence — une semaine, passive

**Ne configurez rien encore.**

Travaillez normalement pendant une semaine et tenez une note courante de ce qui
va réellement mal. Pas ce que vous imaginez qui ira mal — ce qui arrive.
Entrées typiques :

```
lun.  réexpliqué la cible de déploiement pour la troisième fois
mar.  aplati un appel du patron dépôt en requête directe, deux fois
mer.  quatre questions de clarification pour renommer une variable
jeu.  perdu le raisonnement derrière la décision de schéma de lundi
```

Deux raisons pour lesquelles cette étape n'est pas optionnelle.

**On ne peut pas mesurer une amélioration sans un avant.** Toute affirmation que
vous ferez ensuite — à vous-même, à un client, à un employeur — repose sur cette
note ou sur rien.

**La moitié des problèmes que vous imaginez n'apparaîtront pas.** Une
configuration écrite contre des problèmes imaginés est la première source de
surcharge dans ces installations, et elle est particulièrement difficile à retirer
ensuite, parce que personne ne peut prouver qu'elle est inutile.

---

## Étape 1 · La spécification du projet — 45 minutes

L'assistant lit le fichier d'instructions au début de chaque session. Deux couches
y appartiennent, et la plupart des installations les confondent.

**La couche philosophique** — des principes d'ingénierie généraux. Partagés entre
projets, changent rarement, réellement transportables.

**La couche spécification** — les commandes, les contraintes d'architecture et les
conventions de *ce* dépôt. Non transportable. Non devinable. C'est la partie qui
manque d'habitude, et c'est celle qui rapporte.

### Procédure

1. Lancer `/init` et laisser produire un fichier de départ à partir du dépôt réel.
2. Supprimer tout ce qui n'est pas une commande, une contrainte d'architecture ou
   une convention.
3. Appliquer le test d'admission de
   [02 · L'économie du contexte](02-economie-du-contexte.md) à chaque ligne
   survivante.
4. S'arrêter à 60 lignes. Au-delà, vous décrivez des préférences.

### Une couche spécification qui mérite sa place

```markdown
## Commandes
- Tests :  npm run test:unit
- Lint :   npm run lint   (ne jamais corriger le style à la main — le lint le possède)

## Architecture
- Le patron dépôt vit dans /src/repositories.
  Les services n'interrogent pas la base directement.
- /src/legacy est gelé. Signaler avant de toucher ; ne jamais remanier au passage.

## Conventions
- Toute nouvelle route reçoit un schéma dans /src/schemas avant son gestionnaire.
- Demander avant d'ajouter une dépendance.
```

Douze lignes. Chacune est vérifiable, et chaque infraction coûte cher. La dernière
ligne fait le plus de travail en pratique : elle est précise, elle est vérifiable,
et c'est une règle plutôt qu'un sentiment.

**Ce qui n'y appartient pas :** tout ce qu'un lecteur décrirait comme un conseil.

---

## Étape 2 · Le dépôt de connaissance — 15 minutes

```
connaissance/
├── arrivee/        la matière arrive ici ; pas encore de la connaissance
│   └── liens.md    une adresse par ligne, pour ce qui n'est pas un fichier
├── wiki/           pages denses avec leur provenance
├── archives/       originaux après traitement — déplacés, jamais supprimés
├── index.md        une ligne par page. Le seul fichier lu couramment.
└── journal.md      ajout seul : ce qui a été pris, refusé, pourquoi
```

Créez-le vide. Un dépôt vide avec la bonne forme vaut mieux qu'un dépôt plein avec
la mauvaise, parce que la forme détermine ce que vous accepterez d'y mettre.

Deux conventions à fixer maintenant, tant que c'est peu coûteux :

- **`index.md` contient une ligne par page et rien d'autre.** Dès que du contenu
  se met à vivre dans l'index, vous avez reconstruit le palier toujours chargé.
- **`journal.md` est en ajout seul.** C'est la piste d'audit. S'il peut être
  modifié, on ne peut pas s'y fier, et s'y fier est sa seule fonction.

---

## Étape 3 · Le tri — 30 minutes

Installez une compétence qui possède la boucle d'arrivée. Version complète dans
[04 · La compétence de tri](04-competence-de-tri.md).

### Le premier passage est un étalonnage

Donnez-lui cinq éléments réels. Attendez-vous à trois ou quatre refus. Si tout est
accepté, la compétence est complaisante plutôt qu'utile — resserrez la formulation
jusqu'à ce qu'elle refuse, et lisez ses raisons plutôt que ses verdicts.

Deux détails qui comptent plus qu'ils n'en ont l'air :

- **Écrivez les mots déclencheurs dans la langue que vous tapez réellement.** Une
  compétence décrite uniquement en anglais ne se déclenche pas de façon fiable sur
  une demande écrite dans une autre langue, et l'échec est silencieux.
- **Mettez `disable-model-invocation: true`** sur les compétences qui ne doivent
  tourner que sur demande. Le tri en fait partie : il déplace des fichiers et
  modifie des pages durables.

---

## Étape 4 · Les garde-fous — une heure

Triez chaque action possible en deux boîtes : **réversible** et **non**. Puis
posez de vrais contrôles sur la seconde.

C'est un document en soi — voir [05 · Les garde-fous](05-garde-fous.md) — mais la
partie qui appartient à l'installation est l'exercice de tri. Faites-le sur
papier, une fois, explicitement. La plupart des gens n'ont jamais écrit la liste
et sont surpris par sa brièveté, et par ce qui s'y trouve.

---

## Étape 5 · La mesure — 30 minutes

Avant de croire que la configuration fonctionne, écrivez à quoi ressemble le fait
de fonctionner. Faites-le maintenant, pendant que vous êtes encore capable d'être
déçue.

**Mauvais critère :** « les réponses semblent plus cohérentes ».
**Mauvais critère :** « l'assistant semble se souvenir davantage ».

Les deux sont infalsifiables, et les deux paraîtront vrais quel que soit le
résultat.

**Des critères praticables** sont observables et fixés d'avance — la liste est
dans [06 · Comment je sais que ça marche](06-comment-je-le-sais.md).

Si un critère échoue, la première hypothèse honnête est que le *déclencheur* est
mauvais, pas que l'idée l'est. Les descriptions sont le point de panne le plus
fréquent et le moins coûteux à corriger.

---

## Ce que j'ai délibérément laissé de côté

**Une accroche qui classe chaque demande avec un modèle.** De la latence et du
coût à chaque tour, plus un composant qui peut tomber à chaque tour, pour faire ce
qu'une description fait déjà.

**Un moteur vectoriel.** Pour une base personnelle ou d'équipe qui se compte en
dizaines de pages, un fichier d'index et une recherche plein texte suffisent et
restent inspectables. L'infrastructure de récupération doit apparaître quand une
panne de récupération démontrée l'exige, et pas une étape plus tôt.

**Une mémoire qui s'écrit elle-même.** Un assistant qui réécrit sa propre
connaissance durable sans révision n'est pas un système plus mûr — c'est un
système avec un mode de panne plus discret. La révision humaine des écritures
durables coûte peu à cette échelle, et c'est là que l'essentiel de la valeur est
attrapé.

---

**Suite :** [08 · Avant de construire un agent](08-avant-de-construire.md)
