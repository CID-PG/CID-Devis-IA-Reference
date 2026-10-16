# Historique du ruleset C.I.D. Devis IA

## 2026.10.16.1 — Contrat 1.3 et contrôles structurés

- passage du contrat actif `erp-artisans-quote-study` de **1.2 à 1.3** ;
- séparation explicite de `user_response`, `ai_instruction`, `history` et `user_action_state` dans les contrôles ;
- ajout de `interaction_type` et `response_type` pour éviter que l'interface devine l'action attendue depuis un texte libre ;
- ajout de l'état **En attente IA** via `delegated_to_ai` ;
- ajout de `field_states` sur les composants : `active`, `ignored`, `delegated_to_ai` ;
- un champ ignoré n'est plus considéré comme manquant et ne doit pas être recherché par l'IA ;
- préparation de l'UX PA HT / unité avec pertinence locale du champ ;
- conservation des contrats 1.2 dans le dépôt à titre historique, mais le manifeste pointe désormais vers 1.3.

## 2026.10.12.1 — Lot des neuf règles RG-001 à RG-009

- RG-001 : désignations courtes terminées par deux-points, descriptions en tirets, référence fabricant prioritaire côté client, références revendeurs conservées en interne.
- RG-002 : tri et quantification réaliste des déchets, traitement, transport et mutualisation.
- RG-003 : deux niveaux de révision JSON au choix (rapide/partielle ou complète), contrôles de validité et audit global avant finalisation.
- RG-004 : règle transversale d'accessoires/raccordements pour tous les équipements, kits évolutifs et adaptables.
- RG-005 : kit métier sèche-serviettes et trois Wago à deux entrées en base de chiffrage.
- RG-006 : vérifier la nécessité du primaire en carrelage, sans application inutile ni doublon.
- RG-007 : composants chiffrables détaillés, vrais kits et consommables mineurs comme exceptions.
- RG-008 : consultation des prix complémentaires et historisation des tarifs, sans prix privés sur GitHub.
- RG-009 : contrôle des plinthes, seuils, sous-couches, profils et joints des ouvrages de sols.
- Prompt d'entrée et exemples fictifs actualisés ; contrat JSON **1.2** inchangé.

## 2026.10.09.2 — Gouvernance des règles

- ajout de `GOUVERNANCE_REGLES.md` ;
- tous les chats de devis peuvent proposer une règle générale ;
- aucune règle n'est promue automatiquement depuis un chantier ;
- validation explicite obligatoire avant écriture persistante ;
- toute modification persistante d'une règle obligatoire incrémente `ruleset_version` ;
- `manifest.json` devient l'unique autorité de version globale ;
- procédure prévue pour les chats sans accès GitHub en écriture.

## 2026.10.09.1 — Première publication

- publication initiale du référentiel public ;
- règles générales, format des ouvrages, chiffrage, contrat 1.2, schéma et exemples.
