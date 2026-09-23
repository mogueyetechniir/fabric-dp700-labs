# fabric-dp700-labs

Labs pratiques de préparation à la certification **Microsoft DP-700 — Fabric Data Engineer Associate**.

## Objectif

Documenter, lab par lab, la construction d'une plateforme de données sur Microsoft Fabric
(Lakehouse, Data Pipelines, Notebooks, Delta Lake), avec les choix d'architecture justifiés.

## Structure

| Dossier | Contenu |
|---|---|
| [`docs/labs/`](docs/labs/) | Comptes rendus des labs |
| [`docs/decisions/`](docs/decisions/) | Décisions d'architecture (ADR) |
| [`docs/images/`](docs/images/) | Schémas et captures |
| [`fabric/`](fabric/) | Items Fabric synchronisés via l'intégration Git du workspace `ws_dp700_lab` |
| [`scripts/`](scripts/) | Automatisation (API Fabric, Terraform) — à venir |
| [`.github/workflows/`](.github/workflows/) | Contrôles automatiques |

## Labs

| # | Lab | Statut |
|---|---|---|
| 03 | [Delta Lake](docs/labs/03-delta-lake.md) | En cours |

## Décisions d'architecture

- [ADR 0001 — Copie binaire vers Bronze](docs/decisions/0001-copie-binaire-bronze.md)
- [ADR 0002 — Identité de destination Lakehouse](docs/decisions/0002-identite-destination-lakehouse.md)

## Licence

[MIT](LICENSE)
