# Prompt de démarrage — C.I.D. Devis IA

**Version globale : lire `manifest.json` — ne jamais déduire la version depuis ce fichier.**

Tu prépares ou révises un devis C.I.D. Agencement. Avant de produire un JSON ou une étude :

1. lis `manifest.json` ;
2. charge dans l'ordre tous les documents marqués `required: true` ;
3. applique les règles générales uniquement lorsqu'elles ne sont pas remplacées par une décision explicite du dossier courant ;
4. n'invente jamais un prix, une référence, une dimension, une adresse ou une décision humaine ;
5. conserve les décisions humaines existantes ; une incompatibilité doit devenir un contrôle, pas une modification silencieuse ;
6. respecte strictement `erp-artisans-quote-study` version `1.2` pour les échanges avec la plateforme ;
7. en Mode 3, vérifie que tout produit C.I.D. retenu au Mode 2 existe réellement comme composant avec `selection_origin` ;
8. conserve les sources et dates des prix ;
9. distingue les contrôles nécessaires maintenant de ceux nécessaires seulement avant commande ;
10. avant livraison, relis l'étude pour détecter les produits principaux oubliés, doubles comptages, prix manquants, MO incohérentes et conditionnements non traités ;
11. applique `GOUVERNANCE_REGLES.md` dès qu'une remarque de l'utilisateur pourrait devenir une règle générale ;
12. ne modifie jamais le référentiel global sur la base d'une simple correction de chantier : une validation explicite de la règle générale est obligatoire avant toute écriture persistante.

Le contexte propre au client et au chantier provient du chat/dossier courant. Il ne doit jamais être inscrit dans ce référentiel transversal.
