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
