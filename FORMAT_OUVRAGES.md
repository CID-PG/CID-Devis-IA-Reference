# C.I.D. Agencement — Format des ouvrages

**Version globale active : lire `manifest.json`**  
**Statut : actif — version initiale à enrichir au fil des validations utilisateur**

Ce document décrit la forme attendue d'un ouvrage de devis. Il complète les règles générales sans les remplacer.

## 1. Structure

Un devis est organisé en **sections / zones**, puis en **ouvrages**. Un ouvrage contient :

- un libellé client ;
- une description client ;
- une quantité et une unité ;
- un mode de calcul et un taux de TVA ;
- des composants internes : produits, services, main-d'œuvre, consommables, sous-ouvrages ou commentaires ;
- les contrôles et notes internes nécessaires.

La présentation client privilégiée reste **ouvrage + description**. Les composants servent au chiffrage et ne sont exposés que lorsqu'ils apportent une information commerciale utile.

## 2. Libellé de l'ouvrage

Le libellé doit être court, lisible et décrire la prestation, pas le calcul interne.

- Produit fourni par C.I.D. : préférer **« Fourniture et pose de … »** lorsque la fourniture fait partie de la prestation.
- Produit fourni par le client ou existant : préférer **« Pose de … »**, **« Repose de … »** ou **« Adaptation de … »** selon le cas.
- Dépose : utiliser **« Dépose de … »** et préciser séparément si évacuation/traitement des déchets est inclus.
- Préparation : créer un ouvrage distinct si la préparation du support constitue une prestation significative.

Le nom commercial d'un produit peut apparaître lorsque le modèle, le décor ou la gamme constitue un choix explicite du client. Sinon, rester générique dans le libellé et conserver la référence détaillée dans le composant produit.

## 3. Description client

La description explique **ce qui sera réalisé**. Elle doit rester commerciale et compréhensible.

Inclure lorsque pertinent :

- préparation ou adaptation du support ;
- fourniture et mise en œuvre ;
- découpes, ajustements et réglages ;
- raccordements prévus ;
- finitions incluses ;
- limites explicites de la prestation si elles sont importantes pour le client.

Éviter dans la description client :

- références fournisseur sans intérêt commercial ;
- calculs de marge ;
- nombre de vis, cartouches ou petits raccords ;
- notes internes ;
- formulations comme « à confirmer » qui doivent plutôt devenir un contrôle interne, sauf si la réserve doit réellement être portée à la connaissance du client.

## 4. Composants internes

Les composants doivent permettre de reconstruire le coût et la logique technique de l'ouvrage.

Ordre recommandé :

1. produit(s) principal(aux) ;
2. accessoires spécifiques ;
3. produits de préparation / consommables significatifs ;
4. main-d'œuvre ;
5. commentaires ou sous-ouvrages si nécessaire.

Un produit principal retenu au Mode 2 et fourni par C.I.D. doit apparaître comme composant `product`. Son origine de sélection est conservée par `selection_origin`.

## 5. Main-d'œuvre

La main-d'œuvre est un composant séparé (`labor`). Les heures sont définies ouvrage par ouvrage. Le tarif provient de la règle active du dossier ou de la configuration locale validée ; il ne doit pas être inventé.

## 6. Incertitudes

Une incertitude ne doit pas être cachée dans le texte. Créer un contrôle structuré avec l'échéance correcte : sélection produit, revue détaillée, transfert ERP ou commande.

Une donnée à vérifier avant commande ne doit pas bloquer artificiellement la préparation du devis si le chiffrage peut avancer.

## 7. Exemples de formulation

### Revêtement fourni par C.I.D.

**Libellé :** Fourniture et pose d'un revêtement de sol COREtec  
**Description :** Préparation courante du support, fourniture et pose du revêtement, découpes périphériques et réalisation des finitions prévues.

Le composant interne doit contenir le COREtec retenu, puis les profils/accessoires nécessaires et la main-d'œuvre.

### Produit fourni par le client

**Libellé :** Pose d'un meuble vasque fourni par le client  
**Description :** Mise en place, réglage, fixation et raccordements prévus sur les attentes existantes.

Le meuble n'est pas acheté par C.I.D. ; la main-d'œuvre et les fournitures nécessaires à la pose restent chiffrées.
