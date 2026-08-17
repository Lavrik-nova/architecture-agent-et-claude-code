# 04 · La connaissance produit

D'où viennent les faits, comment on les retrouve, et pourquoi la ligne la plus
importante de ce document est une clause `WHERE`.

---

## Deux dépôts, deux métiers

| Dépôt | Contient | Nature |
|---|---|---|
| **Connaissance de raisonnement** | Des pages de connaissance produit et politique, indexées par mots-clés, révisées et activées une par une | Organisée, lente à changer |
| **Mémoire opérationnelle** | Ce que l'exploitation a appris — motifs, corrections, leçons — typé et porté par canal | Cumulative, révisée avant usage |

Ils sont séparés parce que leurs modes de panne sont opposés. La connaissance
organisée se périme. La mémoire apprise admet une chose fausse puis la répète avec
assurance. Les fusionner revient à appliquer une seule discipline de révision à
deux problèmes, et à perdre sur les deux.

---

## La récupération, et la ligne qui compte

```python
def retrieve(query: str, channel: str = "both", limit: int = MAX_PAGES_INJECTED) -> list:
    """Rendre les pages APPROUVÉES + ACTIVES les plus pertinentes pour ce canal.

    Le filtre de révision et d'activation vit dans le SQL, de sorte qu'aucun
    appelant ne peut le contourner. Le pointage est un recoupement de mots-clés
    déterministe — aucun appel de modèle, aucun réseau."""
    ensure_wiki_tables()
    qtok = _tokens(query)
    rows = db.get_conn().execute(
        "SELECT * FROM wiki_pages"
        " WHERE review_status = ? AND activation_state = ?"
        "   AND (channel_scope = 'both' OR channel_scope = ?)",
        (REVIEW_APPROVED, STATE_ACTIVE, channel),
    ).fetchall()
    ...
```

### Pourquoi le filtre est dans le SQL et non chez l'appelant

C'est la décision de conception que je défendrais le plus fermement de tout le
projet.

L'implémentation évidente consiste à ramener les pages et à laisser le code
appelant écarter celles qui ne sont pas approuvées. Ça marche. C'est aussi une
règle qui ne tient qu'aussi longtemps que chaque appelant s'en souvient — y
compris des appelants qui n'existent pas encore, écrits par quelqu'un qui n'a
jamais lu ce fichier, sous pression, en fin de livraison.

Mettre le filtre dans la requête rend la page non approuvée **inatteignable**. Il
n'existe aucun chemin de code qui la rende, parce qu'il n'existe aucun chemin de
code qui la sélectionne. Le commentaire dans les sources dit exactement cela :
*de sorte qu'aucun appelant ne peut le contourner.*

La généralisation : dire à **un futur programmeur** de ne pas faire quelque chose
est aussi une demande. Un contrôle est quelque chose que le système ne peut pas
faire.

### Ce que ce filtre ne couvre pas

Un relecteur a insisté là-dessus, et il avait raison. Mettre le prédicat dans la
requête bouche exactement un trou : l'appelant qui oublie. Il ne bouche pas :

- **Le SQL direct**, depuis un script ou une console
- **Un second chemin de récupération** écrit plus tard sans le prédicat
- **Une réplique de lecture ou une exportation** consommée en aval
- **Une interface d'administration** qui a légitimement besoin des lignes non
  approuvées

Il n'existe actuellement **aucun test qui affirme qu'une nouvelle requête sur
cette table porte le prédicat.** Ce test est ce qui transformerait une bonne
habitude en invariant, et il n'existe pas. D'ici là, c'est un chemin protégé et
non une garantie au niveau du schéma — distinction qui mérite d'être tenue,
puisque tout l'argument pour mettre le filtre dans le SQL était que les habitudes
ne survivent pas à une échéance.

La version forte est une vue qui n'expose que les lignes approuvées et actives, la
table de base n'étant atteignable que par le chemin d'administration. C'est une
migration, pas une retouche, et elle est sur la liste plutôt que faite.

### Pourquoi le pointage est déterministe

Recoupement de mots-clés, trié par recoupement puis par fraîcheur. Aucun appel
d'embarquement vectoriel, aucun réseau, aucun modèle.

Trois conséquences, toutes voulues :

- **Ça ne peut pas tomber.** Une étape de récupération qui fait un appel réseau a
  un mode de panne ; celle-ci n'en a pas.
- **C'est explicable.** Quand une mauvaise page remonte, la raison est visible
  dans les mots-clés, et le correctif est une modification de données plutôt
  qu'un exercice de réglage.
- **C'est remplaçable.** La fonction de pointage est isolée. Un moteur vectoriel
  peut la remplacer plus tard sans toucher à un seul appelant — ce qui est le bon
  moment pour en ajouter un : quand une panne de récupération précise l'exige, et
  pas une étape plus tôt.

À l'échelle actuelle — un ensemble organisé de pages sur un catalogue de quelques
centaines d'articles — le recoupement de mots-clés avec une discipline de révision
bat un moteur vectoriel que personne n'a réglé, et il reste inspectable par une
personne qui n'est pas ingénieure.

---

## Portée par canal, et l'exclusion qu'on remarque mal

Les deux dépôts portent une `channel_scope`, et la mémoire opérationnelle porte
quelque chose de plus tranchant : **certains types de mémoire sont entièrement
exclus de certains canaux.**

```python
excluded_types = _CHANNEL_EXCLUDED_TYPES.get(channel, frozenset())
...
if r["mem_type"] in excluded_types:
    continue  # p. ex. un motif de dossier est exclu du canal clavardage
```

Un motif appris en traitant un dossier individuel est utile quand un employé
regarde cette classe de dossier. Il n'est **pas** approprié à montrer à un
visiteur anonyme sur un site public, où il peut laisser deviner la forme de la
situation de quelqu'un d'autre.

L'exclusion se fait par type, au niveau de la requête, et non par un jugement au
moment de répondre. Même raisonnement que pour le filtre SQL : une frontière qui
dépend de la bonne décision prise à chaque fois n'est pas une frontière.

---

## Le stockage est illimité ; l'injection ne l'est pas

```python
"""Le stockage est illimité ; `limit` ne borne que ce qui est injecté
dans une invite."""
```

Une ligne de documentation qui porte une distinction importante.

**Retenir** tout ne coûte presque rien et se révèle parfois décisif : l'élément
qui explique une panne rare vaut d'être gardé des années. **Injecter** tout coûte
cher et nuit activement, parce que le contexte non pertinent dilue le contexte
pertinent.

Les systèmes qui confondent les deux finissent par supprimer de la connaissance
pour économiser des jetons — le mauvais arbitrage, fait pour une vraie raison.
Gardez le dépôt ; bornez l'injection.

---

## Ce qui arrive quand un fait manque

Le contrat d'incertitude de [03](03-pile-de-raisonnement.md) attrape ce cas avant
la génération. La suite est décidée par l'`uncertainty_rule` de la fiche qui
gouverne, et non par un réglage global — parce que « je n'ai pas cela » ne
signifie pas la même chose pour une question de stock et pour une question de
garantie.

La seule issue jamais permise : combler la lacune avec de la connaissance
générale. Une spécification plausible que l'entreprise ne possède pas est la
forme la plus coûteuse de mauvaise réponse, parce qu'elle est indiscernable d'une
bonne jusqu'à ce qu'un client agisse dessus.

---

**Suite :** [05 · Une mémoire qui reste orientée](05-memoire-et-mises-a-jour.md)
