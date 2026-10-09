# C.I.D. Agencement — Règles de chiffrage

**Ruleset : 2026.10.09.1**

Ce document synthétise les règles de chiffrage transversales applicables aux devis. Une règle particulière explicitement fixée dans un dossier prévaut sur ce document.

## TVA

Les taux courants utilisés dans la plateforme sont **5,5 %**, **10 %** et **20 %**. Ne pas choisir automatiquement un taux uniquement à partir du type de travaux si le dossier ne permet pas de le déterminer avec certitude.

## Main-d'œuvre

- MO1 est la catégorie principale courante.
- MO2, MO3, MA/MOA et autres catégories peuvent être utilisées lorsqu'elles existent dans la configuration.
- Les heures sont définies ouvrage par ouvrage.
- Le tarif horaire n'est pas universel : utiliser la valeur active du dossier ou la valeur explicitement confirmée.
- Une modification de tarif global ne doit jamais recalculer silencieusement un devis existant.

## Marge

- Ne pas appliquer une marge ou un multiplicateur historique sans règle active du dossier.
- Conserver séparément prix d'achat HT, prix de vente HT, source et date.
- Si le prix manque, créer un contrôle plutôt qu'inventer une valeur.

## Sourcing

Ordre de priorité :

1. base tarifaire professionnelle active du fournisseur ;
2. référence déjà validée si elle reste adaptée et le prix est encore pertinent ;
3. prix public Internet en dernier recours, avec source, date, référence exacte et lien.

## Conditionnements

Pour tout conditionnement fixe, calculer le nombre entier nécessaire en arrondissant au supérieur. Conserver si utile : besoin net, nombre de conditionnements, quantité achetée et reliquat.

### Produit standard stockable

Imputer au chantier la consommation réelle (avec pertes raisonnables si justifiées). Le reliquat reste du stock réutilisable.

### Produit spécifique au chantier

Le conditionnement complet nécessaire peut être imputé lorsque le reliquat n'a pas de valeur réaliste de réemploi.

## Quantités et métrés

- Partir du besoin net mesuré.
- Ajouter les pertes uniquement lorsqu'elles sont justifiées.
- Appliquer ensuite le conditionnement.
- Éviter les doubles comptages.
- Conserver les dimensions sources lorsqu'elles sont nécessaires au contrôle.

## Produits Mode 2

Un produit fourni par C.I.D. et retenu au Mode 2 doit être présent dans le Mode 3 comme composant chiffré, avec sa provenance `selection_origin`. Un produit fourni par le client ou existant n'est pas acheté par C.I.D., mais peut générer main-d'œuvre et fournitures de pose.

## Contrôle final

Avant finalisation, vérifier au minimum : fournitures principales, accessoires, consommables, conditionnements, pertes, main-d'œuvre, TVA, références, prix, source/date, éléments conservés/hors prestation et contrôles restants.
