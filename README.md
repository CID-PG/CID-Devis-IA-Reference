# C.I.D. Devis IA — Référentiel public

Référentiel public des règles utilisées par l'IA pour préparer et réviser les devis de **C.I.D. Agencement**.

## Point d'entrée

Le point d'entrée machine est :

`manifest.json`

Un chat ou un outil IA doit lire le manifeste, puis charger dans l'ordre tous les documents marqués `required: true`.

## Version active

- Ruleset : `2026.10.09.2`
- Contrat JSON : `erp-artisans-quote-study` version `1.2`

## Contenu

- `PROMPT_DEMARRAGE_DEVIS.md`
- `GOUVERNANCE_REGLES.md`
- `REGLES_GENERATION_DEVIS.md`
- `FORMAT_OUVRAGES.md`
- `REGLES_CHIFFRAGE.md`
- `EXEMPLES_OUVRAGES.md`
- `CONTRAT_DEVIS_1.2.md`
- `erp-artisans-quote-study-1.2.schema.json`

## Séparation avec ERP Artisans

Ce dépôt est **indépendant** du dépôt de développement ERP Artisans.

Il ne contient pas de code ERP, de données client, de devis réels, de secrets, de clés API ni de tarifs fournisseurs privés.

GitHub est la source de vérité de ce référentiel. Les évolutions doivent être versionnées et relues avant modification de `main`.

## Confidentialité

Ne jamais publier ici :

- des données personnelles ou coordonnées de clients ;
- des documents de chantier réels ;
- des prix d'achat professionnels personnalisés ;
- des identifiants, jetons ou clés API ;
- des secrets d'infrastructure ou de comptes fournisseurs.


## Évolution des règles

Tous les chats de devis peuvent proposer une règle générale. Une règle n'est inscrite durablement que sur validation explicite de l'utilisateur, selon `GOUVERNANCE_REGLES.md`.

`manifest.json` est l'autorité unique pour connaître la version active du ruleset. `CHANGELOG.md` conserve l'historique des évolutions.
