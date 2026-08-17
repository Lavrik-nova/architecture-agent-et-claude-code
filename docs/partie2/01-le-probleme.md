# 01 · Le problème

Un manufacturier québécois. Un catalogue de plusieurs centaines d'articles, vendu
en direct et par détaillants. Une clientèle bilingue qui change de langue au
milieu d'une phrase, et une garantie à vie qui produit un flux constant de
réclamations sur des produits achetés il y a des années.

Le flux entrant arrive par un module de clavardage sur le site et par une
messagerie. Ce n'est pas une file de soutien au sens informatique : il n'y a pas
de catégories de billets, pas de champ de priorité, et aucun client n'a lu la
documentation.

---

## Ce que le flux contient réellement

Les catégories ci-dessous reviennent constamment. Aucune n'est exotique ; la
difficulté est qu'elles arrivent mélangées, dans une seule phrase, chez quelqu'un
qui ignore dans laquelle il se trouve.

| Classe | Forme typique |
|---|---|
| **Choix de produit** | « Il me faudrait quelque chose pour un portable de 15 pouces et une boîte à lunch, pour un ado, qui va passer l'hiver » |
| **Disponibilité et points de vente** | « L'avez-vous en marine, et y a-t-il un magasin proche de chez nous » |
| **Garantie** | « La fermeture éclair a lâché. Acheté il y a peut-être quatre ans, pas de facture » |
| **Réparation et pièces** | « Est-ce que ça se répare ou je remplace » |
| **Spécifications** | « Est-ce qu'un 17 pouces entre ? Les dimensions exactes ? » |
| **Mélangée** | Deux des cas ci-dessus dans un seul message, le second sous-entendu |

La dernière ligne décide de toute l'architecture. Un message qui contient une
question de choix **et** une contrainte de disponibilité **et** une condition
jamais énoncée — l'ado, l'hiver — est le cas normal, pas le cas limite.

---

## Le volume, et ce qu'il déplace

**62,8 % des questions reçoivent une réponse substantielle dès le premier tour**,
mesuré sur 121 questions réelles gelées et jugées par quatre correcteurs
indépendants. La méthode, la ventilation par catégorie et les limites de ce
chiffre sont dans
[09 · Ce que la mesure a trouvé](09-ce-que-la-mesure-a-trouve.md).

La forme économique mérite d'être dite, parce que c'est elle qui rend le projet
utile plutôt qu'intéressant.

**Sans le module, ce flux devient des appels téléphoniques.** Chacune de ces
questions continue d'être posée — le client la pose au téléphone, ou il ne la
pose pas et achète ailleurs. Les deux issues coûtent cher et une seule est
visible.

- Un appel sur des dimensions ou une admissibilité à la garantie occupe un
  employé plusieurs minutes, plus le coût d'interruption sur ce qu'il faisait.
  À l'échelle d'une semaine, cela se compte en dizaines d'heures.
- **Les appels qui n'ont pas lieu sont pires.** Un client qui n'obtient pas de
  réponse au moment où il décide ne dépose pas de plainte : il ferme l'onglet.
  Cette perte n'apparaît dans aucun système, et c'est exactement pour cela
  qu'elle est sous-estimée.

La mesure qui compte n'est donc pas « messages répondus ». C'est **les
conversations qui seraient devenues un appel ou une vente perdue, et qui ne le
sont pas devenues.**

---

## Pourquoi ce n'est pas un problème de billets de soutien

Trois propriétés rendent le manuel habituel inapplicable.

**Le catalogue est physique et il bouge.** Des produits sont discontinués, des
couleurs vont et viennent, des spécifications sont corrigées. La réponse à « en
avez-vous » a une durée de vie qui se compte en jours, et un agent qui récite
avec assurance un article discontinué est pire qu'un agent qui dit devoir
vérifier.

**La garantie est un jugement, pas une consultation.** L'admissibilité dépend du
mode de bris, de l'âge, de l'usage et de la gamme. Une table de règles couvre une
partie du chemin ; le reste exige de lire ce que le client a réellement décrit,
ce qui n'est presque jamais formulé en vocabulaire de garantie.

**La langue n'est pas un réglage.** Un client québécois écrit le français sans
accents, passe à l'anglais pour un terme technique, et répond « ok » d'une façon
qui ne porte aucun signal de langue. Se tromper là-dessus n'est pas un défaut
cosmétique : répondre dans la mauvaise langue se lit comme une machine, et la
conversation s'arrête.

---

## L'exigence, énoncée une fois

> Répondre juste, dans la langue du client, à partir de faits que l'entreprise
> possède réellement — et lorsque ces conditions ne sont pas réunies, le dire et
> passer la main, plutôt que de produire une réponse fluide et fausse.

La seconde moitié est la moitié coûteuse. Un système qui répond toujours est
facile. Un système qui sait quand il ne devrait pas répondre exige presque toute
l'architecture décrite dans cette partie.

---

**Suite :** [02 · Pourquoi un robot à scénarios échoue ici](02-pourquoi-les-scenarios-echouent.md)
