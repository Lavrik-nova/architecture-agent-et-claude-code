# 08 · Avant de construire un agent

> Un agent est une boucle avec des outils. Presque tout ce qui décide s'il
> fonctionne se trouve à l'extérieur de la boucle. Voici la liste que je remplis
> avant d'en écrire la première ligne — et avant d'en ajouter un second à un
> système existant.

---

## Partie 1 · Les trois couches, et laquelle est cassée

Les systèmes d'agents ont trois couches. Diagnostiquer la mauvaise est l'erreur la
plus coûteuse disponible, parce que les pannes de chaque couche se réparent à peu
de frais à son propre niveau et presque pas depuis une autre.

| Couche | Ce que c'est | Symptôme quand c'est elle |
|---|---|---|
| **Harnais** | Tout ce qui entoure le modèle : assemblage du contexte, outils, permissions, bac à sable, mémoire, reprise | Le modèle est capable mais mal informé, trop autorisé, ou incapable de reprendre |
| **Boucle** | Le cycle lui-même : appel, demande d'outil, résultat, recommencer | Tourne sans fin, s'arrête trop tôt, répète une action identique, perd l'objectif |
| **Graphe** | Comment plusieurs agents sont câblés ensemble | Les agents se contredisent, dupliquent le travail, ou s'attendent |

**Un diagnostic en soixante secondes.** Donnez la tâche échouée à une personne
disposant exactement du contexte qu'avait l'agent.

- La personne échoue aussi → **harnais**. L'information n'était pas là.
- La personne réussit facilement → **boucle**. L'information était là et s'est
  perdue, ou le cycle s'est terminé de travers.
- La personne réussit mais demande « qui était censé faire ça ? » → **graphe**.

La plupart des équipes attaquent le graphe en premier, parce qu'ajouter un agent
ressemble à du progrès. C'est la plus coûteuse des trois à modifier et la moins
susceptible d'être la cause.

---

## Partie 2 · Liste de vérification

Neuf questions. Une question sans réponse n'est pas un obstacle en soi — une
question sans réponse *non remarquée* l'est.

### 1. Quelle est la référence déterministe ?

Que fait la version la plus bête possible — un script, une table de
correspondance, une requête ? Construisez-la d'abord et consignez son résultat.
Sans référence, « l'agent fonctionne » n'est pas testable, et vous ne saurez pas
s'il a battu le script.

### 2. Quelle est l'unité de succès ?

Pas « l'utilité ». Un résultat précis et vérifiable : le billet a été acheminé à
la bonne file ; la facture correspond ; la suite de tests passe sans que les tests
aient été modifiés. Si le succès ne peut pas se vérifier sans qu'un humain lise la
sortie et se forme une impression, vous n'êtes pas prête à construire.

### 3. Que lit l'agent, et qui l'a décidé ?

C'est dans la récupération que vit l'essentiel de la qualité. Tout envoyer coûte
cher et donne un résultat *pire* — la matière pertinente se dilue. Bien
sélectionner représente en général une réduction d'un facteur dix et une
amélioration en même temps. Décidez de la politique de sélection avant que la
boucle existe.

### 4. Jusqu'où peut-il atteindre, et avec quels privilèges ?

Énumérez les outils, et pour chacun : ce qu'il peut lire, ce qu'il peut changer, et
si le changement est réversible. Tout ce qui est irréversible exige un vrai
contrôle, pas une instruction — voir [05 · Les garde-fous](05-garde-fous.md).

### 5. Que se passe-t-il s'il s'arrête à mi-chemin ?

Chaque action est dans un de quatre états : pas commencée, commencée, terminée,
terminée-mais-le-résultat-est-perdu. Le quatrième est celui qui fait mal. Décidez
maintenant quelles actions doivent être idempotentes, et comment une reprise sait
ce qui a déjà eu lieu.

### 6. Quels sont les plafonds, et que se passe-t-il au plafond ?

Nombre maximal d'appels de modèle, d'appels d'outils, de profondeur de
délégation, de temps écoulé, d'argent. Un abandon est une issue définie avec un
transfert défini — décidez d'avance ce que l'utilisateur voit. Un abandon sans
plan est une panne avec des étapes en plus.

### 7. Quelles pannes de mémoire peut-on avaler sans danger ?

Celle-ci est régulièrement ratée parce qu'on applique une règle unique partout.

- **Échouer en mode ouvert** convient à la personnalisation. Une préférence
  manquante dégrade l'expérience ; elle ne corrompt rien.
- **Échouer en mode fermé** est exigé pour l'autorisation, l'isolation des
  locataires, les clés d'idempotence et les politiques obligatoires. Si le système
  ne peut pas confirmer la frontière, il doit s'arrêter plutôt que continuer
  permissivement.

Classez chaque lecture de mémoire dans l'une des deux avant d'écrire le repli.

### 8. Que regarderez-vous quand il se comportera mal ?

Si vous ne pouvez pas reconstituer un passage — ce qui a été récupéré, ce qui a
été appelé, ce qui est revenu, ce que ça a coûté — vous ne déboguez pas, vous
devinez. Décidez ce que contient une trace, et décidez ce qui ne doit jamais y
apparaître.

### 9. Faut-il vraiment construire cela ?

La dernière question, et celle qui gagne le plus à être posée à voix haute. Nommez
le **résultat** qui change si cette chose existe. Si la réponse est une capacité
plutôt qu'un résultat — « alors il pourrait aussi… » — ce n'est pas prêt à être
construit.

Dès qu'un assistant peut entretenir un système, le coût marginal d'un processus
automatisé de plus paraît nul, et le système grossit jusqu'à ce que personne ne
puisse dire à quoi sert chaque partie. Reconstruire ce qui fonctionne déjà achète
de la maîtrise et coûte l'entretien qu'un fournisseur absorbait. Parfois cet
échange est juste. Il ne l'est jamais automatiquement.

---

## Partie 3 · Avant d'ajouter un second agent

Un second agent est la solution la plus souvent envisagée et l'une des moins
souvent justes. Elle se justifie si l'une de ces conditions est vraie, pas
autrement :

- **Isolation du contexte.** La sous-tâche exige de lire bien plus que l'agent
  principal ne devrait porter, et seule une courte conclusion doit revenir. C'est
  la raison la plus forte et souvent la seule réelle.
- **Des perspectives véritablement indépendantes**, où la valeur vient du fait que
  les perspectives ne sont pas corrélées — un relecteur adverse, pas un second
  ouvrier.
- **Du parallélisme sur des éléments indépendants**, où les éléments n'interagissent
  réellement pas.

Ne justifient pas : le fait que les étapes soient conceptuellement distinctes ; le
fait que le schéma soit plus joli ; l'envie d'un personnage spécialiste par tâche.
Cela produit un coût de coordination sans gain de capacité.

**Ce qu'un second agent ajoute automatiquement :** une façon pour deux composants
de tenir des croyances contradictoires, un nouvel endroit où le travail se
duplique, et une nouvelle façon de se bloquer. Budgétez-les avant de l'ajouter.

**À dire franchement :** je n'ai pas construit de système multi-agents. Ce qui
précède est un jugement tiré de l'observation et de la conception d'un système à
agent unique, pas d'une pratique de coordination. Un relecteur externe l'a relevé
et il avait raison ; c'est écrit ici plutôt que passé sous silence.

---

## Partie 4 · Comment vous le saurez

Fixez ceci avant de commencer, pour ne pas corriger votre propre copie après coup.

| Question | Consigné avant de construire |
|---|---|
| Qu'a obtenu la référence déterministe ? | |
| Quelle est la cible, et sur quel jeu de cas ? | |
| Quelle régression vous ferait revenir en arrière ? | |
| Que coûte une demande résolue ? | |
| Qui relit les échecs, et à quelle fréquence ? | |

Une colonne vide n'est pas une formalité. C'est la différence entre un système que
vous pouvez défendre et une démonstration que vous pouvez exécuter.

---

**Voir aussi :** [07 · L'ordre d'installation](07-ordre-installation.md) ·
[Sommaire de la partie II](../../README.md#partie-ii--ladministrateur-ia)
