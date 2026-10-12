# C.I.D. Agencement — Règles de chiffrage

**Version globale active : lire `manifest.json`**

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

## Déposes, tri et traitement des déchets — RG-002

Pour tout ouvrage comprenant une dépose avec évacuation des déchets :

1. Distinguer les matériaux : gravats, plaques de plâtre, isolants, bois, métaux, etc.
2. Estimer leurs quantités depuis les **dimensions et épaisseurs réelles**, en séparant les volumes de matière, le foisonnement après dépose et les poids si nécessaires. Ne pas assimiler tout le volume d'un caisson à ses parois.
3. Déterminer les filières de traitement appropriées ; rechercher le tarif à jour auprès de l'organisme compétent pour le chantier.
4. Chiffrer chargement, manutention, poids/volume, capacité du véhicule et trajets.
5. **Mutualiser** les évacuations entre les ouvrages pour limiter les trajets redondants.
6. Distinguer les coûts de **traitement**, de **transport** et de **main-d'œuvre**.

Ne pas prévoir automatiquement une location de benne : elle doit être justifiée par la configuration ou validée explicitement. Si l'évacuation n'est pas incluse, la signaler sans l'imputer arbitrairement.

## Raccordements et accessoires des équipements — RG-004

Pour tout ouvrage de fourniture, pose ou remplacement d'un équipement, contrôler les raccordements aux réseaux, adaptateurs, fixations, étanchéité, joints et finitions nécessaires, en tenant compte de l'existant et des accessoires déjà inclus. **Détailler et chiffrer individuellement les fournitures significatives**, sans doublons.

Le principe s'applique à **toutes les familles d'équipements**, sans imposer les mêmes raccords à un évier, une douche ou une plaque de cuisson. Constituer progressivement des **kits métier de référence**, par famille ; ils servent de base prévisionnelle **adaptable**, et non de liste obligatoire à facturer. Les premiers kits fictifs (meuble vasque et sèche-serviettes électrique) figurent dans `EXEMPLES_OUVRAGES.md`. Aucun autre kit n'est réputé validé.

## Primaire d'accrochage avant carrelage — RG-006

Pour chaque ouvrage de carrelage au sol ou au mur, **vérifier systématiquement la nécessité** d'un primaire selon le support, l'état/porosité, la colle, les systèmes d'étanchéité et les prescriptions du fabricant. Ne pas ajouter automatiquement un primaire inutile ou incompatible.

Si nécessaire, identifier séparément le produit, sa marque/référence, surface, consommation, prix et source. Mutualiser les achats. **Éviter les doubles imputations** lorsqu'une application de primaire compatible sur la même surface figure déjà dans un ouvrage de préparation ou d'étanchéité.

## Prix complémentaires et historique — RG-008

Avant de rechercher un prix neuf, examiner les **catalogues professionnels actifs**, la **base complémentaire C.I.D.** disponible et les références validées antérieurement. La base complémentaire enrichit les catalogues sans les remplacer.

Pour un prix exploitable, conserver autant que possible : désignation, marque, références fabricant/fournisseur, unité, prix d'achat HT, organisme/fournisseur, source, date et conditionnement. Lors d'une actualisation, **conserver l'ancien prix et sa date dans l'historique**, sans écrasement.

Un prix historique n'est pas automatiquement un prix courant : contrôler la fraîcheur, la provenance et la pertinence ; si nécessaire, rechercher une actualisation ou créer un contrôle. Une base actualisée ne doit jamais **recalculer rétroactivement un devis établi**.

L'enregistrement effectif d'un nouveau tarif dans les fichiers de prix est une opération maîtrisée, distincte d'une recherche. Le référentiel GitHub **public** n'accueille ni tarifs privés ni données de client/chantier.

## Finitions des revêtements de sol — RG-009

Pour **toute pose de revêtement de sol**, contrôler selon le chantier :

- plinthes, fixations et accessoires ;
- seuils de porte, profils de transition et de rive ;
- joints périphériques, de fractionnement/dilatation et finitions compatibles ;
- sous-couches, primaires et préparations éventuellement nécessaires.

Choisir des finitions compatibles avec le revêtement et solliciter un choix esthétique seulement s'il est significatif. Métrer les longueurs à partir des périmètres, ouvertures et rives réellement concernées. **Détailler les composants significatifs**, leurs références, prix et conditionnements, en mutualisant les achats pertinents.

Ne pas ajouter de plinthes si un revêtement mural ou une finition validée assure cette fonction ; ne pas ajouter une sous-couche déjà intégrée ou incompatible avec le fabricant ; **ne pas combler par défaut les jeux de dilatation au mastic**. Vérifier les doublons entre ouvrages.
