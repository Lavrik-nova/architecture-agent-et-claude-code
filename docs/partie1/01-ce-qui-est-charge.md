# 01 · Ce qui est chargé, et pourquoi

L'inventaire complet de ce qui se trouve dans Claude Code avant que j'ouvre un
projet. Chaque entrée dit ce qu'elle fait, ce qu'elle a remplacé, et — quand il y
a lieu — ce que j'ai refusé à sa place.

Rien ici n'a été installé parce que c'était intéressant. Chaque pièce est arrivée
après une panne précise que je peux nommer.

---

## Intelligence du code — un graphe MCP sur la base de code

Un graphe de connaissance de chaque symbole, chaque lien et chaque fichier du
dépôt de travail, interrogé par MCP. Sur le dépôt de l'agent en production, il
contient actuellement :

```
120 fichiers · 3 652 symboles · 6 844 liens · 5,2 Mo · SQLite avec FTS5 + WAL
```

**Ce qu'il a remplacé.** L'exploration par recherche textuelle et lecture de
fichiers. Répondre à « par où passe un message entrant avant d'arriver au
générateur de réponse » demandait une douzaine de recherches et la lecture de
plusieurs fichiers en entier. C'est maintenant deux appels qui rendent les points
d'entrée, les symboles liés et le code pertinent ensemble.

**Pourquoi ça compte plus qu'il n'y paraît.** Le coût n'est pas la recherche —
c'est que chaque fichier lu atterrit dans la fenêtre de contexte et y reste, en
chassant la tâche elle-même. Une requête au graphe rend les trois fonctions qui
comptent au lieu des six fichiers qui les contiennent.

**La règle que je m'impose.** Si l'index existe dans le répertoire de travail, la
recherche textuelle et la lecture de fichiers entiers sont interdites pour
retrouver un symbole. L'index d'abord, puis la lecture du seul endroit qui
compte. C'est inscrit dans le fichier d'instructions, parce que sans cela
l'habitude revient en une semaine.

**Ce que j'ai refusé.** Un second indexeur, recommandé dans une revue d'outils et
sincèrement bon. Deux index sur le même code, c'est deux sources de vérité et une
petite décision à chaque consultation. Consigné dans
[ADR-0003](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/decisions/0003-no-second-code-indexer.md)
🇬🇧 pour que la question ne soit pas rejugée à la prochaine infolettre qui le
recommandera.

---

## Extensions

| Extension | Ce qu'elle fait |
|---|---|
| **Mémoire entre sessions** | Conserve les observations d'une session à l'autre, avec sa propre recherche. Répond à « a-t-on déjà réglé ça ? » sans que j'aie à le reconstituer |
| **Directeur de sécurité** *(la mienne, locale)* | Applique le protocole à deux modes décrit plus bas. Non publiée : elle encode des décisions propres à cette machine |
| **Pont de messagerie** | Rejoint l'opératrice hors du terminal pour les travaux longs |

Que l'extension de sécurité soit locale et écrite à la main est précisément
l'idée. Les extensions générales règlent des problèmes généraux ; les règles
concernant **cette** machine, **ces** identifiants et **ce** système en
production ne se généralisent pas et n'ont pas à être confiées aux valeurs par
défaut de quelqu'un d'autre.

---

## Compétences — la couche de déclenchement

Trois compétences personnelles, toujours disponibles. Ce qui importe n'est pas
qu'elles existent, mais **ce qui est chargé en contexte pour qu'elles
fonctionnent** : la description seule, une à trois lignes. Le corps se charge
quand un déclencheur s'active et ne coûte rien au repos.

| Compétence | S'active sur |
|---|---|
| **Tri des connaissances** | Invocation explicite uniquement. Elle déplace des fichiers et modifie des pages durables : elle ne part jamais de sa propre initiative |
| **Recherche vers implémentation** | Les choix d'architecture coûteux — mémoire, récupération, évaluation, orchestration. Retrouver un patron éprouvé avant d'en inventer un |
| **Lentille d'heuristiques** | Manuelle. Une source d'hypothèses pour relire un plan, explicitement pas une autorité |

**Un détail qui décide si tout cela fonctionne.** Les mots déclencheurs sont
écrits dans la langue que l'opératrice tape réellement. Une compétence décrite
uniquement en anglais ne s'active pas de façon fiable sur une demande écrite en
français ou en ukrainien, et **l'échec est silencieux** — aucune erreur, la
compétence ne se déclenche simplement pas, et tout le monde conclut que l'idée
était mauvaise. Chaque description porte donc son vocabulaire de déclenchement
dans la langue de travail, plus une clause explicite de *ce pour quoi elle n'est
pas faite*, sans quoi une compétence trop largement décrite s'active sur tout.

---

## La chaîne de validation — où l'humain se place

Trois commandes qui forment une machine à états, avec un humain dans la
transition :

```
découverte client  →  [ approbation humaine ]  →  construction
   DÉCOUVERTE                                       CONSTRUCTION
```

La découverte lit la matière brute, produit un dossier structuré et une offre,
puis **s'arrête**. Elle ne peut pas avancer d'elle-même. L'approbation est une
commande distincte que seule une personne lance ; elle valide l'état et fusionne
le travail préparé vers la production. Ce n'est qu'ensuite que la construction
peut démarrer.

**Pourquoi une machine à états plutôt qu'une règle.** « Demander avant de
construire » dans un fichier d'instructions est une demande, et les demandes sont
respectées jusqu'au moment où elles dérangent. Une machine à états ne se laisse
pas convaincre : la commande de construction lit le contrat approuvé, ou elle n'a
rien à lire.

---

## Points d'accroche — quatre moments du cycle

Les accroches sont le seul mécanisme ici que le modèle ne peut pas discuter.
Elles s'exécutent de façon déterministe, hors de la conversation.

| Moment | Ce qui tourne |
|---|---|
| **Avant une écriture** | Script de garde. Vérifie la cible avant que quoi que ce soit soit écrit |
| **Après une écriture** | Script de capture, qui alimente la couche mémoire |
| **À l'arrêt** | Deux : un rappel de discipline, et un compteur de coût qui consigne ce que la session a consommé |
| **Au démarrage d'une session** | Signale l'état de la base de connaissances et si de la matière non traitée attend |

**L'accroche de démarrage est la plus petite et la plus utile.** Elle lit l'index
des connaissances et dit, en une ligne, sa taille et combien d'éléments attendent
d'être triés. Sans elle, le dossier d'arrivée se remplit en silence : la personne
qui l'a rempli est occupée, et l'assistant n'a aucune raison d'aller voir.

**Ce que je refuse délibérément de mettre en accroche :** un appel de modèle qui
classe chaque demande entrante pour décider quel contexte injecter. Cela ajoute
de la latence, du coût et un nouveau point de panne **à chaque tour**, en échange
de ce qu'une description de compétence fait déjà à l'intérieur de la requête
existante. Les accroches sont faites pour les vérifications déterministes. Le
jugement appartient à la conversation.

---

## Mémoire — trois couches, trois métiers

| Couche | Contient | Lue |
|---|---|---|
| **Fichier d'instructions** | Les invariants : commandes, contraintes d'architecture, frontières | Chaque session, toujours |
| **Mémoire opérationnelle** | Journaux de session, profil, décisions, dossiers ouverts. SQLite avec recherche plein texte | À la demande, plus un fichier de démarrage à l'ouverture |
| **Wiki d'ingénierie** | Connaissance triée, avec sa provenance. Un index d'une ligne par page | L'index au démarrage ; une page seulement en travaillant sur son sujet |

La séparation est toute la conception. Les confondre est l'échec classique : un
seul fichier de connaissance qui grossit, soit chargé en permanence — donc au
coût sans limite —, soit jamais, auquel cas c'est un journal intime.

Le détail dans
[03 · Architecture de la mémoire](03-architecture-memoire.md).

---

## Protocole à deux modes

Toute action est réversible ou elle ne l'est pas.

**Réversible** : lecture, analyse, rédaction, modification à l'intérieur du
projet, opérations locales de gestion de version. Cela avance sans confirmation.

**Irréversible ou tournée vers l'extérieur** : publication, déploiement,
suppression récursive, changement d'intégration continue, envoi de quoi que ce
soit vers l'extérieur, écriture hors du projet, tout ce qui touche aux
identifiants ou aux données de production. Cela s'arrête et attend.

L'arrêt n'est pas une question polie. Il exige une phrase d'autorisation explicite
qu'on ne peut pas produire par réflexe — « oui », « ok » et « vas-y » sont ce
qu'on tape en pensant à autre chose.

Le détail dans
[05 · Les garde-fous](https://github.com/Lavrik-nova/claude-code-and-agent-architecture/blob/main/docs/part1/05-gates.md) 🇬🇧.

---

## Une brèche que je n'ai pas refermée

La configuration des permissions contient actuellement **357 règles
d'autorisation et aucune règle d'interdiction.** C'est une liste blanche qui a
grossi par accumulation, une invite à la fois — exactement le mode de
prolifération contre lequel ce dépôt met en garde ailleurs.

Ce n'est pas dangereux en soi : le protocole à deux modes et la garde d'écriture
se tiennent devant les opérations qui comptent. Mais ce n'est pas la conception
que je défendrais, et une liste d'interdits pour les chemins d'identifiants et
les écritures hors projet est le prochain chantier plutôt qu'un travail fait.

Je l'écris ici plutôt que de l'omettre. Un document de configuration qui ne
présente que ce qui a bien tourné est une brochure.

---

**Suite :** [02 · L'économie du contexte](02-economie-du-contexte.md)
— comment ces pièces s'empêchent de se chasser les unes les autres.
