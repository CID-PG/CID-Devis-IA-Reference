# C.I.D. Agencement — Exemples d'ouvrages

**Version globale active : lire `manifest.json`**

Ces exemples sont fictifs et servent uniquement de référence de structure et de rédaction.

## 1. Produit principal choisi au Mode 2

### Ouvrage

**Fourniture et pose d'un revêtement de sol COREtec**

Préparation courante du support, fourniture et pose du revêtement, découpes périphériques et réalisation des finitions prévues.

### Composants internes attendus

- COREtec — produit principal retenu ;
- profils/seuils nécessaires ;
- consommables adaptés ;
- MO1.

Le composant COREtec doit conserver :

```json
"selection_origin": {
  "product_need_id": "need-sol-zone",
  "candidate_id": "candidate-coretec"
}
```

Le simple fait d'écrire « COREtec » dans le nom de l'ouvrage ne remplace pas le composant produit.

## 2. Produit fourni par le client

### Ouvrage

**Pose d'un meuble vasque fourni par le client**

Mise en place, réglage, fixation et raccordements sur les attentes prévues.

### Composants internes

- fixations adaptées ;
- petits raccords nécessaires ;
- MO1.

Le meuble n'est pas valorisé comme achat C.I.D.

## 3. Dépose avec évacuation à confirmer

**Dépose du revêtement existant**

Dépose et manutention du revêtement existant.

Si l'évacuation des déchets n'est pas encore confirmée, créer un contrôle séparé plutôt que l'inclure silencieusement dans la description.

## 4. Raccordement sur attentes existantes

**Raccordement d'un équipement sanitaire sur attentes existantes**

Adaptation et raccordement sur les alimentations et évacuations existantes prévues, avec essais de fonctionnement.

Ne pas chiffrer une création complète de réseau si le dossier confirme la réutilisation des attentes existantes.

## 5. Kit métier de référence — meuble vasque (RG-004)

Base prévisionnelle pour un meuble vasque à raccorder sur attentes existantes :

- 1 m de tube PVC Ø32 mm ;
- 2 coudes PVC 90° Ø32 mm ;
- 2 coudes laiton filetés 12/17 ;
- 2 flexibles sanitaires 12/17 mâle/femelle de 15 cm.

Vérifier les raccords et flexibles déjà livrés, le siphon, les dimensions et les attentes. Adapter les composants à l'installation ; les éléments inutiles ou inclus ne sont pas facturés deux fois. Les consommables stockables suivent la règle de consommation réelle.

## 6. Kit métier de référence — sèche-serviettes électrique (RG-005)

Sur une alimentation existante adaptée, base prévisionnelle :

- 1 sortie de câble appropriée ;
- 1 boîte d'encastrement ou de raccordement, **si nécessaire** ;
- **3 bornes Wago à levier à deux entrées compatibles avec les fils souples**, comme base de chiffrage ;
- les fixations spécifiques adaptées au support et à l'appareil ;
- du câble complémentaire uniquement si nécessaire.

Vérifier classe de l'appareil, éventuel fil pilote, branchement effectivement prescrit, normes et contraintes de salle de bain. Adapter le nombre de raccordements ; ne pas présumer un conducteur de terre sur un appareil qui n'en prévoit pas. Ne pas créer automatiquement une nouvelle ligne électrique ni compter deux fois les fournitures déjà incluses.

## 7. Primaire de carrelage (RG-006)

Vérifier la nécessité technique d'un primaire de support et ne l'ajouter que s'il est requis. Si la même surface a été primairisée dans un ouvrage de préparation ou d'étanchéité compatible, ne pas compter une deuxième application fictive.

## 8. Finitions de sol (RG-009)

Sur un ouvrage de sol, vérifier séparément plinthes, seuils, profils de finition, sous-couche éventuelle, joints périphériques et fixations. Ne pas ajouter de sous-couche distincte si elle est intégrée, ni de plinthes lorsque les rives sont déjà terminées autrement.
