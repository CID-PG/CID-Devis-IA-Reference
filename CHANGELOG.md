# Historique du ruleset C.I.D. Devis IA

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
