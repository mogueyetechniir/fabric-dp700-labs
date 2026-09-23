# ADR 0001 — Copie binaire vers Bronze

- **Statut** : Accepté
- **Date** : 2026-09-23
- **Contexte d'application** : [Lab 03 — Delta Lake](../labs/03-delta-lake.md), pipeline `pl_ingest_products`

## Contexte

Le fichier `products.csv` est publié sur GitHub. Il doit arriver dans le Lakehouse `lh_delta_lab` pour être transformé ensuite par un notebook Spark. Le lab officiel le téléverse à la main, ce qui n'est ni reproductible ni planifiable.

La zone Bronze doit conserver la donnée **telle que reçue**, pour pouvoir rejouer les transformations et auditer la source.

## Décision

Une activité Copy du pipeline `pl_ingest_products` copie le fichier en format **Binary** (source et destination) vers la section **Files** du Lakehouse : `Files/products/products.csv`.

Le typage et la conversion en table Delta sont faits en aval, par le notebook.

## Alternatives envisagées

| Option | Pourquoi elle est écartée |
|---|---|
| Téléversement manuel | Non reproductible, non planifiable |
| Copy en *DelimitedText* avec mapping vers la section Tables | Le pipeline interprète le schéma dès l'ingestion : un changement de colonne à la source casse ou altère le chargement, et la donnée brute n'est plus conservée |
| Lecture directe de l'URL par le notebook | Mélange ingestion et transformation ; pas de trace dans le Monitoring hub au niveau de l'ingestion |

## Conséquences

- **Positives** : copie fidèle octet par octet ; la source brute reste disponible pour rejouer les traitements ; le pipeline ne dépend pas du schéma.
- **Négatives** : pas de validation du contenu à l'ingestion. *Preview data* et *Mapping* sont indisponibles en Binary. Le contrôle qualité revient au notebook.
- **À surveiller** : le comportement du pipeline quand le dossier `products` n'existe pas encore, et lors d'un second Run (écrasement ou doublon). Les résultats seront consignés dans le journal du lab.
