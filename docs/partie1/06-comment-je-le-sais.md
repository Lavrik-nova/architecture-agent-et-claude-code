# 06 · Comment je sais que ça marche

Le document inconfortable. La plupart des textes sur une configuration de ce genre
se terminent par un chiffre qui sonne décisif et qui se révèle, à l'examen, être
l'impression de l'auteur habillée d'un signe de pourcentage.

Voici ce que je vérifie réellement, ce que je n'ai pas mesuré, et pourquoi la
distinction mérite d'être tenue.

---

## Critères fixés d'avance

Écrits avant qu'on croie la configuration efficace, parce qu'un critère inventé
après coup sera toujours satisfait.

| Critère | Comment il se vérifie | Pourquoi celui-là |
|---|---|---|
| **Une compétence se déclenche sans qu'on le demande** | Elle s'est activée à la première demande naturelle de son domaine, dans la langue de travail | Si cela échoue, rien d'autre ne compte — la connaissance existe et n'est jamais atteinte |
| **Les explications répétées cessent** | Comparé aux notes de la semaine de référence, sur la même durée | La plainte d'origine. Si elle persiste, la couche d'instructions est mauvaise |
| **Une décision survit un mois** | Demander un choix vieux d'un mois ; le raisonnement consigné revient, pas une improvisation | C'est la différence entre une mémoire et un journal intime |
| **Des refus ont réellement lieu** | Le journal contient des refus avec leurs raisons, pas seulement des acceptations | Un tri qui accepte tout a cessé de trancher |
| **Le palier toujours chargé reste borné** | Nombre de lignes à chaque révision | La croissance y est silencieuse et composée |

---

## Ce que ces critères ont attrapé

**La panne de langue des déclencheurs.** Une compétence dont la description était
en anglais ne s'est pas déclenchée sur des demandes formulées dans la langue de
travail. Aucune erreur, aucun avertissement — la compétence ne s'activait
simplement pas, et la conclusion raisonnable aurait été « cette approche ne
fonctionne pas ». Le critère l'a attrapée parce qu'il demande si la compétence
s'est déclenchée, pas si elle est bonne.

Correctif : vocabulaire de déclenchement écrit dans la langue que l'utilisatrice
tape réellement, plus une clause explicite de *ce pour quoi ce n'est pas fait*.
C'est aujourd'hui la première chose que je vérifie sur toute nouvelle compétence.

**Le fichier toujours chargé à 149 lignes.** Un plafond de 60 avait été fixé la
veille, par écrit, par moi. Le lendemain, je proposais d'y ajouter des règles
d'aiguillage. La règle a attrapé l'auteur de la règle, ce qui est la seule
véritable épreuve d'une règle.

**Un garde-fou appliqué à une prémisse non vérifiée.** J'ai plaidé contre la
construction d'une protection au motif que la chose qu'elle protégeait n'existait
pas encore. La prémisse était fausse — la chose existait, sous une forme que je
n'avais pas vérifiée. Le garde-fou était sain ; l'entrée ne l'était pas. La
correction consignée n'a pas été « faire plus attention », mais une modification du
garde-fou lui-même : *vérifier que la chose est réellement absente fait partie du
garde-fou, ce n'est pas une étape qui le précède.*

Cette dernière est la plus utile de ce document. Un critère qui ne fait jamais que
confirmer que la configuration fonctionne n'est pas un critère.

---

## Ce que je n'ai pas mesuré

**Aucune comparaison contrôlée.** Je n'ai pas fait tourner les mêmes tâches avec
et sans cette configuration sur un jeu de cas figé pour comparer les résultats.
C'est l'expérience qui justifierait un pourcentage, et elle n'existe pas.

**Aucun taux de régression.** J'ignore à quelle fréquence la configuration produit
un résultat pire que son absence — par exemple une compétence qui se déclenche sur
quelque chose qu'elle aurait dû laisser tranquille. Je sais que c'est arrivé ;
j'ignore combien de fois.

**Aucun écart de coût.** Le compteur de session consigne la consommation, mais je
n'ai pas attribué la différence à la configuration plutôt qu'au changement de
nature des tâches.

**Aucun décompte des mauvaises décisions évitées.** Le chiffre le plus précieux et
le moins mesurable. Toute méthode a ce problème et la plupart des publications le
règlent discrètement en n'en parlant pas.

Il existe en revanche **une mesure du côté du système en production** — 121
questions gelées, quatre juges indépendants, 62,8 % de réponses substantielles au
premier tour. Elle ne mesure pas cette configuration ; elle mesure l'agent. La
distinction est faite exprès. Voir
[09 · Ce que la mesure a trouvé](../partie2/09-ce-que-la-mesure-a-trouve.md).

---

## Pourquoi les lacunes sont énoncées plutôt que comblées par des adjectifs

Deux raisons, l'une de principe et l'autre pratique.

**De principe.** Ce dépôt soutient qu'une matière non vérifiée ne doit pas être
présentée comme un fait — c'est la règle que la compétence de tri impose aux
écrits de tout le monde. L'appliquer à mes propres affirmations n'est pas de la
modestie, c'est de la cohérence. Un document qui exige de la provenance des
articles et n'en offre aucune pour lui-même défend une idée à laquelle il ne croit
pas.

**Pratique.** Un ingénieur qui lit un texte de ce genre cherche l'endroit où
l'auteur a cessé d'être rigoureux. Donnez-lui un pourcentage sans expérience
derrière et il le trouvera en dix secondes ; tout le reste devient suspect.
Énoncez la lacune et le reste du document tient.

---

## Ce qui ferait de ceci une vraie mesure

Concret et pas encore fait :

1. **Un jeu de cas figé** — trente à quarante demandes représentatives, écrites une
   fois, réutilisées. Assez variées pour qu'une configuration chanceuse ne puisse
   pas toutes les passer.
2. **Deux passages par cas**, avec et sans la configuration, sur la même version de
   modèle.
3. **Jugés contre des critères écrits d'avance**, pas lus et notés à l'impression.
4. **Consigné par passage :** résultat, coût, et si une compétence s'est déclenchée
   alors qu'elle n'aurait pas dû.

C'est une journée de travail et c'est l'étape honnête suivante. D'ici là, ce que
j'ai est un journal de décisions, un comportement en production et des critères
fixés d'avance — ce qui est plus que la plupart, et moins qu'une preuve.

---

**Voir aussi :** [07 · L'ordre d'installation](07-ordre-installation.md) — d'où
vient la semaine de référence
