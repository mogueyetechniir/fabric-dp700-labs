# ADR 0002 — Identité de destination Lakehouse

- **Statut** : Proposé (à valider en lab)
- **Date** : 2026-09-23
- **Contexte d'application** : [Lab 03 — Delta Lake](../labs/03-delta-lake.md), pipeline `pl_ingest_products`

## Contexte

Le pipeline doit écrire dans le Lakehouse `lh_delta_lab`. Le connecteur Lakehouse de Fabric n'accepte qu'une authentification **Organizational account**, c'est-à-dire un utilisateur.

Faire tourner un pipeline sous le compte personnel d'un ingénieur pose trois problèmes : le pipeline casse si la personne quitte l'équipe ou change de mot de passe, les droits accordés sont trop larges, et les journaux d'audit ne distinguent plus l'humain du traitement automatique.

## Décision

1. **Option visée** : écrire dans OneLake via une connexion **ADLS Gen2** vers `https://onelake.dfs.fabric.microsoft.com` (`conn_adls_onelake_spn`), authentifiée par un **service principal**. Le secret est stocké dans Azure Key Vault. Le service principal reçoit le rôle *Contributor* sur le workspace.
2. **Repli** si l'option 1 ne fonctionne pas : connexion Lakehouse `conn_lakehouse_svc_etl` sous un **compte de service** dédié `svc-fabric-etl`.

## Alternatives envisagées

| Option | Évaluation |
|---|---|
| Compte personnel de l'ingénieur | Écartée : dépendance à une personne, droits excessifs, audit brouillé |
| Service principal via ADLS Gen2 sur OneLake | Retenue : identité non humaine, secret gérable dans Key Vault. Risque : montage non standard, à valider |
| Compte de service sur le connecteur Lakehouse | Repli : chemin supporté, mais c'est un compte utilisateur à gouverner comme tel |

## Conséquences

- **Règle générale** : service principal ou identité managée partout où c'est pris en charge ; compte de service seulement quand un connecteur exige un utilisateur.
- **Si le compte de service est retenu** : un compte par environnement, mot de passe dans Key Vault, accès conditionnel, moindre privilège, surveillance des connexions.
- **À valider en lab** : le service principal est-il trouvé par son nom d'application dans *Manage access* ? L'écriture dans OneLake via ADLS Gen2 fonctionne-t-elle ? Le statut de cet ADR passera à *Accepté* ou *Rejeté* selon le résultat.
