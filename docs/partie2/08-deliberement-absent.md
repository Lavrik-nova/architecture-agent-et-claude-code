# 08 · Ce qui est délibérément absent

Les architectures refusées, avec leurs raisons. C'est le document que je lirais en
premier dans le dépôt de quelqu'un d'autre, parce que ce qu'un système fait est
surtout déterminé par ce sur quoi il a été bâti, et ce sur quoi il a été bâti est
surtout déterminé par ce qui a été refusé.

---

## Un moteur vectoriel

**Ce qu'il aurait apporté.** Une récupération sémantique sur la base de
connaissance. Des questions formulées dans le vocabulaire du client rejoindraient
des pages écrites dans le vocabulaire du catalogue, sans passerelle explicite de
mots-clés.

**Pourquoi non.** À l'échelle actuelle — un ensemble organisé de pages sur un
catalogue de quelques centaines d'articles — le recoupement déterministe de
mots-clés avec une discipline de révision bat un moteur vectoriel non réglé, et il
possède trois propriétés que le moteur n'a pas : il ne peut pas tomber à
l'exécution, une mauvaise récupération s'explique en regardant les données, et une
personne non ingénieure peut l'inspecter et le corriger.

**Le coût honnête.** La classe 2 de
[02 · Pourquoi les scénarios échouent](02-pourquoi-les-scenarios-echouent.md) —
*« est-ce que ça survit à un adolescent ? »* — est exactement ce que les
embarquements traitent bien. C'est aujourd'hui pris en charge par la couche de
raisonnement, ce qui fonctionne et coûte plus cher par requête.

**À revoir quand.** Une panne de récupération précise et consignée que le
recoupement de mots-clés ne résout pas, une fois les mots-clés corrigés. La
fonction de pointage est isolée précisément pour que cet échange ne coûte rien
structurellement le jour où la preuve arrive.

---

## Une mémoire qui s'écrit elle-même

**Ce qu'elle aurait apporté.** L'agent inscrivant sa propre connaissance durable à
partir de l'exploitation, sans étape de révision. Apprentissage plus rapide,
aucune minute humaine par semaine.

**Pourquoi non.** Ce n'est pas un système plus mûr ; c'est un système avec un mode
de panne plus discret. Un élément faux entre, il est récupéré, il façonne des
réponses, et les réponses qu'il a façonnées paraissent cohérentes — donc il se
renforce. Quand quelqu'un s'en aperçoit, la base porte plusieurs générations
d'erreur assurée et il n'y a plus de point de retour propre.

La révision coûte quelques minutes par semaine. Le coût de son absence est
illimité et n'est pas détectable tôt. À cette échelle, l'arbitrage n'est pas
serré.

**À revoir quand.** Il existera un jeu d'évaluation assez solide pour attraper un
mauvais élément avant qu'il atteigne la récupération. La révision se remplace par
de la mesure, pas par de l'optimisme.

---

## Une note de confiance sur la réponse produite

**Ce qu'elle aurait apporté.** Un nombre par réponse, un seuil, une règle
d'escalade. Simple à bâtir et simple à expliquer.

**Pourquoi non.** Un modèle à qui l'on demande de produire une réponse puis de
noter sa propre confiance produit une réponse fluide et une note assurée, parce
que les deux viennent du même processus. La défaillance que la note est censée
attraper est exactement celle qu'elle ne peut pas voir.

**La version honnête de cet argument.** Le contrat d'incertitude pose la question
plus tôt, contre les entrées, et attachée à chaque règle plutôt que globale — 24
règles distinctes sur 25 fiches. C'est une meilleure position. Ce n'est pas une
chose de nature différente : le contrat est exécuté par le modèle et non par une
barrière, donc le même angle mort subsiste, plus étroit. Voir
[03 · La pile de raisonnement](03-pile-de-raisonnement.md).

Une vérification de couverture déterministe — tous les `known_facts` des fiches
actives sont-ils présents ? — en ferait un contrôle plutôt qu'un contrat. C'est
une quarantaine de lignes sur des données que le système possède déjà, et elle
n'est pas écrite. **C'est le plus grand écart entre ce que cette architecture
affirme et ce qu'elle impose.**

---

## Un seuil de confiance global

**Ce qu'il aurait apporté.** Un seul bouton de réglage pour tout le système.

**Pourquoi non.** Une information manquante ne signifie pas la même chose selon le
domaine. Une question de stock tolère une réponse nuancée ; une question de
garantie ne doit pas en produire, parce qu'une affirmation de garantie nuancée est
lue comme un engagement malgré la nuance. Un seul bouton finit réglé sur le cas le
plus strict — rendant le système inutile ailleurs — ou sur la moyenne, donc faux
là où ça compte.

L'`uncertainty_rule` vit sur la fiche. Vingt-cinq petites décisions locales valent
mieux qu'une décision globale qui ne peut pas être juste.

---

## Un appel de modèle dans la détection de langue

**Ce qu'il aurait apporté.** Un meilleur traitement des entrées véritablement
ambiguës.

**Pourquoi non.** La langue est évaluée à chaque tour. Un appel de modèle là ajoute
de la latence partout et introduit de la variance dans une question qui a une
bonne réponse. Le détecteur déterministe est de l'arithmétique, il ne peut pas
tomber, et il se corrige en ajoutant une expression à une liste — c'est ainsi que
les cas de français québécois sans accents ont été réglés en minutes plutôt que
par du réglage d'invite.

---

## Les correctifs de comportement au niveau de l'invite

**Ce qu'ils auraient apporté.** Le correctif le plus rapide possible pour tout
comportement observé : ajouter une phrase à l'invite.

**Pourquoi non.** Essayé et annulé trois fois. Cela tient jusqu'à l'entrée
inhabituelle suivante, puis lâche en silence, et chaque tentative laisse dans
l'invite une phrase que personne n'ose retirer parce que personne ne se souvient
de ce qu'elle protégeait.

La règle est maintenant imposée au critique de code lui-même : *ne jamais proposer
de corriger un comportement en ajoutant des mots à une invite ; un contrôle doit
être du code, un drapeau ou un test.*

C'est la même phrase qui donne sa forme à la configuration de la partie I. Elle a
été apprise ici, en production, au prix de trois retours en arrière.

---

## Un second agent

**Ce qu'il aurait apporté.** Des agents spécialistes par domaine — garantie,
choix, disponibilité — coordonnés par un aiguilleur.

**Pourquoi non.** La justification d'un second agent est l'isolation du contexte :
une sous-tâche qui exige de lire bien plus que l'agent principal ne devrait
porter, et qui renvoie une courte conclusion. Cette condition n'est pas remplie
ici. La sélection des faits garde déjà l'ensemble de travail petit, et les
domaines partagent l'essentiel de leur connaissance.

Ce qu'un second agent ajoute automatiquement : une façon pour deux composants de
tenir des croyances contradictoires sur le même client, un nouvel endroit où le
travail se duplique, et une nouvelle façon de se bloquer mutuellement. Ces coûts
sont certains ; le bénéfice était spéculatif.

**À dire franchement :** je n'ai pas construit de système multi-agents. J'argumente
sur les conditions qui en justifient un, et un argument n'est pas une pratique. Un
relecteur externe me l'a fait remarquer et il avait raison.

---

## Le motif commun aux sept

Chaque refus suit la même forme, et elle mérite d'être nommée parce que c'est la
partie transférable :

> **Une capacité était disponible. La défaillance qu'elle aurait empêchée n'avait
> pas eu lieu. La défaillance qu'elle aurait introduite était structurelle.**

Adopter une technique parce qu'elle est bonne n'est pas la même chose que
l'adopter parce qu'elle résout un problème qu'on a. Chaque entrée ci-dessus est
consignée avec une condition de réexamen, pour que la question soit tranchée
plutôt que simplement remise — et pour que le prochain article qui en recommande
une ne relance pas la discussion à zéro.

---

**Retour :** [Sommaire de la partie II](../../README.md#partie-ii--ladministrateur-ia) ·
[09 · Ce que la mesure a trouvé](09-ce-que-la-mesure-a-trouve.md)
