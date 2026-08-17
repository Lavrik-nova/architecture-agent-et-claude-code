# Claude Code, configuré — et l'agent qui en est sorti

**Partie I** — ce que je charge dans Claude Code avant de construire quoi que ce
soit : le graphe de code, les sous-agents critiques, le harnais, et les trois
couches de mémoire qui empêchent une décision de s'évaporer entre deux sessions.

**Partie II** — le système que j'ai bâti avec : un administrateur IA qui absorbe
le flux entrant d'un manufacturier québécois dont le catalogue compte des
centaines d'articles. Pas un robot à scénarios. Une pile de raisonnement.

Écrit pour les ingénieurs qui construisent des agents avec Claude Code. On
suppose que vous savez ce qu'est un harnais et on ne l'explique pas.

[![Licence : MIT](https://img.shields.io/badge/Licence-MIT-black.svg)](LICENSE)
[![English](https://img.shields.io/badge/Also%20in-English-1f6feb.svg)](https://github.com/Lavrik-nova/claude-code-and-agent-architecture)

---

## Le chiffre, avant tout le reste

Un dépôt qui dit « production » sans montrer un chiffre demande un crédit qu'il
n'a pas gagné. Voici le premier chiffre du projet, avec ses limites :

> **62,8 % de réponses substantielles dès le premier tour.**
> 121 questions réelles gelées, rejouées contre la version en production.
> Quatre juges indépendants qui n'avaient pas vu la course.
> **Prix et codes promo : 8,7 %** — la pire catégorie, et celle qui coûte des ventes.
>
> Une seule course, un juge modèle, aucune ligne de base sans agent.
> **C'est une référence, pas un verdict.**

Le détail, les trois incidents datés qui ont façonné l'architecture, et ce que
ces chiffres ne prouvent pas :
**[09 · Ce que la mesure a trouvé](docs/partie2/09-ce-que-la-mesure-a-trouve.md)**.

C'est le document que je lirais en premier si c'était le dépôt de quelqu'un
d'autre.

---


> 🇬🇧 **Note sur la langue.** L'édition française est en cours. Les documents
> marqués d'un drapeau pointent vers l'original anglais, complet et à jour, le
> temps que la rédaction française les rattrape. Traduits en premier : la page
> d'accueil et le document 9, parce que ce sont les deux que je donnerais à lire
> si je n'avais qu'une minute.

## Partie I · L'assistant configuré

Ce qui est réellement installé, pourquoi chaque pièce est là, et ce que j'ai
refusé.

| № | Document | |
|---|---|---|
| 1 | [Ce qui est chargé, et pourquoi](docs/partie1/01-ce-qui-est-charge.md) | L'inventaire complet : graphe de code, extensions, compétences, quatre points d'accroche, la chaîne de validation, trois couches de mémoire |
| 2 | [L'économie du contexte](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/02-context-economy.md) 🇬🇧 | Trois paliers de chargement et la règle de placement. Où va une règle, et ce qu'elle coûte d'y rester |
| 3 | [Architecture de la mémoire](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/03-memory-architecture.md) 🇬🇧 | Arrivée → tri → wiki → index → journal en ajout seul. Pourquoi le refus est le produit principal |
| 4 | [La compétence de tri](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/04-adjudication-skill.md) 🇬🇧 | La compétence entière, telle qu'elle tourne — pas sa description |
| 5 | [Les garde-fous](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/05-gates.md) 🇬🇧 | Réversible ou non. Où vit un vrai contrôle, et où un garde-fou n'est que du théâtre |
| 6 | [Comment je sais que ça marche](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/06-how-i-know-it-works.md) 🇬🇧 | Critères fixés d'avance — et ce que je n'ai pas mesuré |
| 7 | [L'ordre d'installation](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/07-installation-order.md) 🇬🇧 | Cinq étapes. La première consiste à ne rien faire pendant une semaine, exprès |
| 8 | [Avant de construire un agent](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/08-before-you-build.md) 🇬🇧 | Neuf questions auxquelles je réponds avant la première ligne — et avant d'en ajouter un second |

## Partie II · L'administrateur IA

Étude d'architecture anonymisée. Aucun nom de client, aucun nom de produit,
aucune invite reproduite mot pour mot : la structure et l'intention au complet,
les formulations remplacées par un exemple neutre.

| № | Document | |
|---|---|---|
| 1 | [Le problème](docs/partie2/01-le-probleme.md) | Le flux entrant, sa forme, et ce qu'il coûte quand un humain l'absorbe |
| 2 | [Pourquoi un robot à scénarios échoue ici](docs/partie2/02-pourquoi-les-scenarios-echouent.md) | Quatre classes de message réel qui brisent tout arbre de décision |
| 3 | [La pile de raisonnement](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part2/03-reasoning-stack.md) 🇬🇧 | Couche par couche : verrou de langue, intention, fiches de principe, sélection des faits, contrat d'incertitude, escalade |
| 4 | [La connaissance produit](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part2/04-product-knowledge.md) 🇬🇧 | Comment les faits sont rangés, retrouvés, révisés et activés — et pourquoi le filtre vit dans le SQL |
| 5 | [Une mémoire qui reste orientée](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part2/05-memory-and-updates.md) 🇬🇧 | Comment la base se met à jour et comment l'agent reste juste pendant qu'elle bouge |
| 6 | [Limites et passage à l'humain](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part2/06-limits-and-handoff.md) 🇬🇧 | Quand le système doit s'arrêter, et pourquoi avouer son ignorance vaut ce que ça coûte |
| 7 | [Attraper une mauvaise réponse](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part2/07-catching-bad-answers.md) 🇬🇧 | Ce qui est consigné à chaque échange, ce qui déclenche une alerte, comment une panne est disséquée |
| 8 | [Ce qui est délibérément absent](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part2/08-deliberately-absent.md) 🇬🇧 | Les architectures refusées, avec leurs raisons |
| 9 | ⭐ [Ce que la mesure a trouvé](docs/partie2/09-ce-que-la-mesure-a-trouve.md) | **Les chiffres, trois incidents datés, et ce qu'ils ne prouvent pas** |

## Également ici

- **[Journal des décisions](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/decisions/README.md) 🇬🇧** — chaque choix d'architecture
  avec les options qui ont perdu et la raison.
- **[Gabarits](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/tree/main/templates) 🇬🇧** — fichier d'instructions, page de wiki, index,
  journal, compétence de tri, fiche de décision. À copier.

---

## Une seule phrase traverse les deux parties

> Dire à un modèle de ne pas faire quelque chose, c'est une demande.
> Ce n'est pas un contrôle.

Dans la partie I, c'est ce qui donne leur forme aux permissions et aux garde-fous.
Dans la partie II, c'est pourquoi le filtre de révision est posé à l'intérieur de
la requête SQL plutôt que dans l'invite — pour qu'aucun appelant ne puisse le
contourner, y compris une version future du code écrite par quelqu'un qui a
oublié la règle.

Trois correctifs par retouche d'invite ont été annulés sur ce projet avant que la
phrase soit écrite. Un cas est raconté en entier dans
[09](docs/partie2/09-ce-que-la-mesure-a-trouve.md).

---

## Ce qui n'est pas ici

**Aucune comparaison contrôlée** de cette configuration contre son absence. Il y a
une mesure, un journal des décisions et des critères fixés d'avance — pas une
expérience. Là où un nombre apparaît, sa méthode et ses limites sont écrites à
côté.

**Aucun système multi-agents que j'aurais construit.** J'argumente dans ce dépôt
sur les conditions qui justifient un second agent, et je sais reconnaître qu'un
argument n'est pas une pratique. Un relecteur externe me l'a fait remarquer et il
avait raison ; c'est écrit ici plutôt que passé sous silence.

**Aucun nom de client, aucun nom de produit, aucune donnée client** — sous aucune
forme, y compris anonymisée.

---

## Si vous ne retenez qu'une chose

Tout ce qui précède tient dans une phrase : **un contrôle doit être quelque chose
que le système ne peut pas refuser.** Pas une règle dans une invite, pas une
habitude, pas une note dans un fichier que plus personne n'ouvre depuis la
troisième semaine. Du code, un drapeau, ou un test.

J'ai longtemps cru que la version bien formulée suffisait. Elle a tenu chaque
fois, jusqu'à ce que l'entrée devienne étrange — et là elle n'a plus tenu, sans
qu'aucune alarme se déclenche. C'est cette partie-là qui coûte cher.

Si vous ne prenez qu'une chose d'ici, prenez la plus petite.

**Gelez vingt vraies questions cette semaine.** Pas des questions inventées :
vingt que votre système a réellement reçues. Rejouez-les. Puis faites corriger
les réponses par quelque chose qui ne les a pas produites — un autre modèle avec
un contexte neuf, un collègue, n'importe qui qui n'était pas dans la pièce.

Le chiffre ne vous plaira pas. C'est exactement là qu'est sa valeur. Le mien
disait 62,8 %, et l'utile n'était pas le 62,8 : c'était de découvrir que la
question posée juste avant l'achat échouait neuf fois sur dix, bien au chaud dans
une moyenne qui ressemblait à du progrès.

Vingt questions et un correcteur honnête, c'est un mardi après-midi. Vous en
apprendrez plus sur votre système qu'en un mois de plus à construire par-dessus.

---

Il me manque encore des choses, et elles sont nommées plutôt que cachées :
aucune comparaison contrôlée contre l'absence de configuration, aucune liste
d'interdits dans mes propres permissions, et aucun système multi-agents que
j'aurais construit — j'argumente sur les conditions qui justifient un second
agent, et un argument n'est pas une pratique.

Si quelque chose ici est faux, je préfère sincèrement le savoir. Le journal des
décisions existe pour qu'un mauvais choix puisse être retrouvé et annulé, et cela
ne fonctionne que si quelqu'un regarde.

**[nova@lavrikgeo.com](mailto:nova@lavrikgeo.com)** · [lavrikgeo.com](https://lavrikgeo.com)

## Licence

[MIT](LICENSE).
