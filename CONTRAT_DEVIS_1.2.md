# Évolution du contrat Devis ERP Artisans — version 1.2

**Statut : CONTRAT FONCTIONNEL V3 VALIDÉ CÔTÉ PLATEFORME / À INTÉGRER CÔTÉ ERP ARTISANS**

**Objet :** faire évoluer le contrat `erp-artisans-quote-study` 1.1 pour couvrir l’ensemble du cycle d’échange entre l’IA, l’utilisateur et la future plateforme Devis ERP Artisans, sans limiter le contrat à la seule étude détaillée finale.

---

## 1. Contexte

Le contrat 1.1 couvre correctement une étude de devis détaillée et révisée :

- filiation par `exchange_id`, `revision`, `parent_revision`, `parent_sha256` ;
- structure métier `study.sections[] → works[] → components[]` ;
- contrôles `checks` ;
- décisions humaines `review` ;
- liens produit, prix, fournisseur, conditionnement, statuts et traçabilité.

Le nouveau processus métier validé introduit trois phases successives :

1. **Mode 1 — Définition du chantier**
   - questionnaire conditionnel préparé par l’IA ;
   - réponses structurées de l’utilisateur ;
   - confirmations de calculs/déductions IA ;
   - branches de questions affichées dynamiquement selon les réponses.

2. **Mode 2 — Produits spécifiques**
   - besoins produit issus du Mode 1 ;
   - une recommandation principale et quelques alternatives pertinentes ;
   - photos distantes, liens fournisseur/fabricant/fiches techniques ;
   - décisions : retenir, refuser, demander une autre proposition, proposer un produit par URL ;
   - conservation des produits refusés et de leurs motifs.

3. **Mode 3 — Revue détaillée**
   - étude complète par sections / ouvrages / composants ;
   - main-d’œuvre ;
   - quantités ;
   - conditionnements ;
   - consommables ;
   - fournisseurs ;
   - prix ;
   - contrôles restants ;
   - revue par exception avant transfert vers ERP Artisans / Dolibarr.

La plateforme locale est un prototype fonctionnel destiné à terme à être intégré dans ERP Artisans. L’objectif du contrat 1.2 est donc de modéliser correctement le processus cible, sans surinvestir dans une compatibilité complexe entre deux produits voués à converger.

---

## 2. Décision d’architecture

Le format reste :

`erp-artisans-quote-study`

La version devient :

`1.2`

L’enveloppe 1.1 est conservée :

- `format`
- `format_version`
- `exchange_id`
- `revision`
- `parent_revision`
- `parent_sha256`
- `exchange_kind`
- `metadata`
- `target`
- `study`
- `checks`
- `review`

Un nouveau bloc est ajouté :

`workflow`

Le contrat cible est donc :

```text
erp-artisans-quote-study 1.2
│
├── metadata
├── target
│
├── workflow
│   ├── current_phase
│   ├── intake
│   │   └── topics
│   │       └── questions conditionnelles
│   └── product_selection
│       └── needs
│           ├── candidates
│           └── decision
│
├── study
│   └── sections
│       └── works
│           └── components
│
├── checks
└── review
```

---

## 3. Phases du workflow

`workflow.current_phase` prend l’une des valeurs suivantes :

- `technical_intake`
- `product_selection`
- `detailed_review`
- `ready_for_erp`

Correspondance fonctionnelle :

| Valeur | Interface |
|---|---|
| `technical_intake` | Mode 1 — Définition du chantier |
| `product_selection` | Mode 2 — Produits spécifiques |
| `detailed_review` | Mode 3 — Revue détaillée |
| `ready_for_erp` | Étude prête pour transfert / conversion ERP |

La phase pilote l’interface par défaut, mais n’interdit pas la navigation vers les phases déjà parcourues.

---

# 4. Mode 1 — `workflow.intake`

## 4.1 But

Permettre à l’IA de préparer en amont un configurateur conditionnel complet afin de limiter les allers-retours.

L’IA doit :

- analyser la dictée chantier ;
- structurer les thèmes pertinents ;
- calculer ce qui peut l’être ;
- ne pas poser une question dont la réponse existe déjà ;
- préparer les branches conditionnelles utiles ;
- différencier questions, confirmations et informations ;
- n’afficher que les branches devenues pertinentes.

Le Mode 1 ne doit pas encore traiter les références détaillées, visserie, silicone, colle, raccords standards, marges ou petites fournitures.

---

## 4.2 Structure `intake`

```json
{
  "ai_understanding": "Résumé initial produit par l’IA.",
  "user_correction": "",
  "topics": []
}
```

### `ai_understanding`

Résumé général de ce que l’IA a compris du chantier.

### `user_correction`

Correction ou complément libre fourni par l’utilisateur.

### `topics`

Thèmes réellement pertinents pour le chantier.

Exemples :

- Évier
- Douche
- WC
- Porte
- Sol
- Peinture
- Chauffe-eau

L’IA ne doit pas générer des thèmes sans rapport avec le chantier.

---

## 4.3 Thème

Structure :

```json
{
  "id": "topic-douche",
  "label": "Douche",
  "ai_summary": "Résumé initial du thème.",
  "resolved_summary": "",
  "user_notes": "",
  "questions": []
}
```

Le statut visuel du thème (`non commencé`, `en cours`, `complet`, `attention`) n’est pas stocké comme autorité : il est calculé par l’interface à partir des questions applicables.

---

## 4.4 Question

Chaque question possède un identifiant stable.

Champs :

- `id`
- `label`
- `question`
- `type`
- `required`
- `allow_to_check`
- `priority`
- `help`
- `options`
- `visibility`
- `proposed_value`
- `response`

### Types autorisés

- `single_choice`
- `multiple_choice`
- `yes_no`
- `number`
- `measure`
- `dimensions`
- `short_text`
- `long_text`
- `date`
- `confirmation`
- `information`

Aucun langage ou script exécutable ne doit être introduit dans le JSON.

---

## 4.5 Réponse

États autorisés :

- `unanswered`
- `answered`
- `confirmed`
- `to_check`
- `not_applicable`

Interprétation :

- `unanswered` : aucune décision utilisateur ;
- `answered` : valeur fournie explicitement par l’utilisateur ;
- `confirmed` : valeur proposée/calculée par l’IA et confirmée ;
- `to_check` : la question a été traitée mais la donnée reste à vérifier ;
- `not_applicable` : la branche conditionnelle ne s’applique plus.

Une question masquée par une condition ne bloque jamais la complétude du thème.

---

## 4.6 Questions conditionnelles

Opérateurs V1 :

- `equals`
- `not_equals`
- `in`

Groupes :

- `all`
- `any`

Exemple simple :

```json
{
  "question_id": "evier-egouttoir",
  "operator": "equals",
  "value": "oui"
}
```

Exemple groupé :

```json
{
  "all": [
    {
      "question_id": "evier-fourniture",
      "operator": "equals",
      "value": "cid"
    },
    {
      "question_id": "evier-robinetterie",
      "operator": "equals",
      "value": "cid"
    }
  ]
}
```

Aucune expression JavaScript, SQL, PHP ou pseudo-code n’est admise.

---

## 4.7 Mesures

Une mesure simple peut porter :

- `value`
- `unit`

Une dimension peut porter :

- `width`
- `height`
- `depth`

avec une unité commune.

Précision autorisée :

- `measured`
- `approximate`
- `to_check`

---

## 4.8 Valeur proposée par l’IA

Une question de type `confirmation` peut porter `proposed_value`.

Exemple :

```json
{
  "type": "confirmation",
  "question": "Surface calculée",
  "proposed_value": {
    "value": 16.24,
    "unit": "m²"
  },
  "response": {
    "state": "unanswered",
    "value": null,
    "note": ""
  }
}
```

Si l’utilisateur confirme :

`state = confirmed`

S’il corrige :

`state = answered`

La distinction doit être conservée.

---

## 4.9 Priorité / échéance métier

Valeurs de `priority` :

- `required_for_product_selection`
- `required_for_detailed_review`
- `required_for_erp_export`
- `required_before_order`
- `optional`

Cette graduation remplace l’idée qu’une donnée inconnue bloquerait nécessairement tout le processus.

Exemple :

Une mesure définitive de paroi peut être `required_before_order` sans empêcher la réalisation du devis.

---

# 5. Mode 2 — `workflow.product_selection`

## 5.1 But

Présenter uniquement les produits spécifiques qui constituent de vrais choix pour le client ou pour l’entreprise.

Exemples :

- receveur ;
- paroi ;
- colonne de douche ;
- meuble vasque ;
- robinetterie ;
- WC ;
- porte ;
- poignée visible ;
- parquet ;
- radiateur ;
- volet roulant.

Ne doivent pas être exposés ici :

- visserie ;
- silicone ;
- colle ;
- petits raccords ;
- rails ;
- consommables annexes ;
- conditionnements techniques sans choix client.

---

## 5.2 Besoin produit

Structure :

```json
{
  "id": "need-douche-receveur",
  "topic_id": "topic-douche",
  "label": "Receveur de douche",
  "criteria_summary": "1200 × 900 environ, extra-plat, blanc.",
  "supply_mode": "cid",
  "required": true,
  "candidates": [],
  "decision": {}
}
```

### `supply_mode`

Valeurs :

- `cid`
- `client`
- `existing`
- `not_required`

Un produit fourni par le client ou conservé reste présent dans le dossier afin d’alimenter les contraintes de pose et le Mode 3.

---

## 5.3 Produit candidat

Un candidat peut être proposé par l’IA ou provenir d’un lien fourni par l’utilisateur.

Champs principaux :

- `id`
- `origin`
- `label`
- `brand`
- `reference`
- `supplier`
- `supplier_reference`
- `manufacturer_reference`
- `image_url`
- `links`
- `pricing`
- `key_features`
- `ai_reason`
- `warnings`

### `origin`

- `ai`
- `user`

### Liens

Types :

- `supplier`
- `manufacturer`
- `technical_sheet`
- `installation_manual`
- `other`

Les URL peuvent rester distantes. L’application n’est pas obligée de télécharger les images ou documents.

---

## 5.4 Décision produit

États :

- `to_decide`
- `retained`
- `retained_with_reservation`
- `new_search_requested`
- `user_product_to_analyze`
- `supplied_by_client`
- `existing_kept`
- `excluded`

Un produit retenu au Mode 2 devient une décision humaine autoritaire.

L’IA ne peut pas le remplacer silencieusement dans une révision suivante.

---

## 5.5 Produits refusés

Les produits rejetés restent conservés.

La décision peut stocker :

- `rejected_candidate_ids`
- `reasons`
- `request_scope`
- `notes`

`request_scope` distingue explicitement :

- `product` : rechercher un autre produit pour le même besoin ;
- `need` : redéfinir le besoin lui-même avant de proposer de nouveaux produits ;
- `null` : aucune demande de nouvelle recherche active.

Exemple : passer d'un carrelage à un revêtement COREtec correspond à `request_scope = "need"`. L'IA doit alors réexaminer les conséquences techniques du besoin, et pas seulement remplacer une référence produit.

Motifs standards :

- `too_expensive`
- `wrong_finish`
- `wrong_dimensions`
- `technical_incompatibility`
- `appearance`
- `availability`
- `other`

L’IA doit utiliser ces motifs pour éviter de reproposer inutilement les mêmes produits.

---

## 5.6 Produit proposé par l’utilisateur

Structure :

```json
{
  "state": "user_product_to_analyze",
  "user_product": {
    "url": "https://...",
    "notes": "Je préfère celui-ci."
  }
}
```

L’interface doit proposer un bouton **Coller le lien** utilisant le presse-papiers lorsque le navigateur l’autorise, avec saisie manuelle en secours.

Lors de la révision suivante, l’IA peut créer un candidat `origin = user` après analyse.

---

## 5.7 Produit retenu sous réserve

Exemple :

```json
{
  "state": "retained_with_reservation",
  "candidate_id": "candidate-paroi-001",
  "notes": "À confirmer après mesure finie de la largeur."
}
```

Une réserve nécessitant un contrôle futur doit être reflétée dans `checks`.

---

# 6. Mode 3 — `study`, `checks`, `review`

Le Mode 3 utilise directement le socle historique du contrat.

Il ne crée pas une troisième structure parallèle.

Correspondance :

```text
workflow.intake              → Mode 1
workflow.product_selection   → Mode 2
study + checks + review      → Mode 3
```

Le Mode 3 est une **revue par exception** :

- l’IA prépare l’étude complète ;
- les lignes fiables n’exigent pas une validation manuelle ligne par ligne ;
- les incertitudes et blocages sont mis en évidence ;
- l’utilisateur conserve la possibilité de modifier n’importe quel ouvrage ou composant.

## 6.1 Traçabilité obligatoire des produits retenus au Mode 2

Tout composant de type `product` peut porter un champ `selection_origin`. Il vaut `null` lorsque le composant n’est pas issu directement d’un choix du Mode 2. Lorsqu’un produit retenu au Mode 2 est matérialisé dans le Mode 3, il doit contenir :

```json
"selection_origin": {
  "product_need_id": "need-sol-wc-r2",
  "candidate_id": "candidate-coretec-oak"
}
```

Règle métier : pour tout besoin `supply_mode = "cid"` dont la décision vaut `retained` ou `retained_with_reservation`, le produit retenu doit être présent dans au moins un composant `product` non exclu du Mode 3 et ce composant doit référencer le besoin et le candidat retenu.

Si cette matérialisation manque, la plateforme crée un contrôle bloquant avant `erp_export` du type **« Produit retenu absent du devis »**. L’IA doit alors ajouter le produit principal à l’ouvrage concerné, sans supprimer silencieusement les autres composants déjà présents.

Exceptions normales : `supplied_by_client`, `existing_kept`, `excluded` et `not_required` ne déclenchent pas cette obligation d’achat C.I.D.

---

# 7. `study` pendant les Modes 1 et 2

En version 1.2, `study.sections` peut être vide ou partiel pendant les premières phases.

Exemple :

```json
{
  "study": {
    "title": "Réfection salle de bain",
    "need": "…",
    "internal_notes": "",
    "commercial": {
      "proposal_date": null,
      "valid_until": null,
      "payment_terms_code": null,
      "payment_method_code": null
    },
    "sections": []
  }
}
```

L’étude détaillée complète est normalement construite avant ou pendant `detailed_review`.

---

# 8. Conditionnements Mode 3

Le bloc `packaging` est étendu.

Champs :

- `need_quantity`
- `consumption_quantity`
- `package_quantity`
- `package_unit`
- `purchase_package_count`
- `purchased_quantity`
- `remainder_quantity`
- `charge_mode`

### `charge_mode`

- `need`
- `full_package`
- `to_confirm`

Exemple produit spécifique :

```text
besoin chantier       = 9 ml
conditionnement        = 2,40 m / pièce
achat                  = 4 pièces
quantité achetée       = 9,60 m
reliquat               = 0,60 m
charge_mode            = full_package
```

Exemple produit stockable :

```text
besoin chantier        = 0,5 cartouche
achat physique         = 1 cartouche éventuelle
imputation chantier    = 0,5 cartouche
charge_mode            = need
```

---

# 9. Évolution des `checks`

Les niveaux historiques restent :

- `blocking`
- `warning`
- `information`

Deux informations sont ajoutées.

## 9.1 Cible

```json
{
  "target": {
    "type": "component",
    "id": "component-paroi"
  }
}
```

Types de cible :

- `global`
- `topic`
- `question`
- `product_need`
- `candidate`
- `work`
- `component`

Pour `global`, `id` vaut `null`.

## 9.2 Échéance

`required_before` :

- `product_selection`
- `detailed_review`
- `erp_export`
- `order`
- `none`

Exemple :

Une mesure définitive peut être requise avant `order` mais ne pas bloquer `erp_export`.

---

## 9.3 Traitabilité des contrôles dans le Mode 3

Un contrôle affiché dans **À traiter maintenant** doit toujours offrir une voie de traitement exploitable :

- cible `work` ou `component` : ouverture directe dans **Tout le devis** ;
- cible `question` ou `topic` : affichage du contenu source dans le Mode 3, résolution automatique si la source est désormais complète, et accès secondaire au Mode 1 si une correction amont reste nécessaire ;
- cible globale : bloc de décision dans le Mode 3 avec possibilité de confirmer ou de renvoyer explicitement à l’IA ;
- contrôle global de temps de main-d’œuvre : affichage des lignes MO par ouvrage, heures modifiables et confirmation globale ;
- contrôle explicitement renvoyé à l’IA : conservation d’une consigne dans `decision`.

Le bouton **Préparer le retour à l’IA** doit distinguer les points ayant reçu une décision, ceux explicitement renvoyés à l’IA et ceux encore sans traitement. Les points sans traitement sont signalés avant export.

---

# 10. Autorité des décisions humaines

Une révision IA ne peut jamais modifier silencieusement :

- une réponse Mode 1 `answered` ;
- une réponse Mode 1 `confirmed` ;
- un produit Mode 2 `retained` ;
- un produit Mode 2 `retained_with_reservation` ;
- un produit explicitement rejeté ;
- une décision humaine du `review`.

Si l’IA détecte une contradiction ou une incompatibilité, elle crée ou met à jour un `check`.

Elle ne remplace pas automatiquement la décision humaine.

---

# 11. Révisions et filiation

Le mécanisme 1.1 est conservé :

- `exchange_id`
- `revision`
- `parent_revision`
- `parent_sha256`
- `exchange_kind = full`

Règles :

- le même échange conserve son `exchange_id` ;
- `revision` augmente d’une unité ;
- `parent_revision` référence la révision reçue ;
- `parent_sha256` est l’empreinte exacte du fichier précédent ;
- le document complet est renvoyé à chaque échange ;
- aucun patch n’est utilisé.

---

# 12. Identifiants stables

Doivent rester stables entre révisions tant que l’objet existe :

- topic ;
- question ;
- besoin produit ;
- candidat ;
- section ;
- ouvrage ;
- composant ;
- check.

Un nouvel objet reçoit un nouvel identifiant.

Un identifiant supprimé ne doit jamais être réutilisé pour représenter un autre objet.

---

# 13. Compatibilité historique

Décision du 8 octobre 2026 : **aucune compatibilité historique ne doit être développée dans la nouvelle plateforme**.

La V3 travaille uniquement avec le contrat `erp-artisans-quote-study` **1.2**.

Sont explicitement hors périmètre :

- migration automatique `1.1 → 1.2` ;
- import des anciens formats `cid-devis-echanges` ;
- chaînes de migration entre les versions historiques de la plateforme.

Si une ancienne étude doit être reprise, le chat/assistant ayant produit le fichier doit la relire et produire un nouveau document conforme au contrat 1.2.

L’objectif est de ne pas introduire de dette technique de compatibilité dans un outil transitoire destiné à converger ultérieurement vers ERP Artisans.

# 14. Exigences d’interface associées

Le contrat ne doit pas imposer toute l’UX, mais les comportements suivants font partie du besoin fonctionnel :

- une seule application avec les trois modes ;
- navigation interne Mode 1 / Mode 2 / Mode 3 ;
- mobile-first ;
- thème clair / sombre / système, préférence mémorisée ;
- sauvegarde automatique ;
- liens et images distants autorisés ;
- presse-papiers pour les liens produit lorsque le navigateur l’autorise ;
- une modification d’un paramètre local TVA/MO ne modifie jamais silencieusement le devis courant ; la plateforme détecte les occurrences concernées et propose explicitement une propagation en masse ;
- aucune obligation d’être totalement hors ligne ;
- l’application peut être servie depuis un NAS en HTTP/HTTPS sans serveur métier dédié.

---

# 14 bis. Arbitrages plateforme V3 validés

Décisions complémentaires du 8 octobre 2026 :

- la plateforme reste **strictement locale** : aucun NAS, serveur métier ou mécanisme de synchronisation n’est développé ;
- IndexedDB stocke les dossiers de travail et une mini-base de paramètres métier locaux ;
- cette configuration locale contient au minimum les taux de TVA `5,5 %`, `10 %`, `20 %` et les catégories de main-d’œuvre (`MO1`, `MO2`, `MO3`, `MA`, etc.) avec leurs valeurs de chiffrage ;
- un export/import JSON de la configuration locale est prévu comme sauvegarde manuelle, sans synchronisation ;
- le Mode 3 fonctionne en **revue par exception** : aucune validation obligatoire ligne par ligne ;
- une ligne fiable peut être compacte mais reste toujours dépliable, éditable, supprimable, remplaçable ou marquable `À revoir par l’IA` ;
- un produit précédemment retenu peut toujours être remplacé ultérieurement ; si ce changement peut affecter le Mode 3, les éléments concernés sont marqués à revoir/recalculer par l’IA sans suppression automatique de l’ancien contenu ;
- une validation humaine représente une **décision courante**, jamais un verrou définitif.


### Ergonomie des décisions produit

- les besoins produits traités se replient automatiquement après décision ;
- la fiche compacte conserve un résumé du choix et reste réouvrable ;
- le repli ne doit pas provoquer de saut de navigation : la fiche traitée reste replacée sous l'en-tête ;
- toute décision reste modifiable ; les propositions écartées restent consultables et peuvent être reconsidérées ;
- une demande de nouvelle recherche conserve le motif rapide et une `notes` libre affichée comme **Précision pour l’IA** ;
- les compteurs d'avancement restent visibles pendant le défilement dans les trois modes.

---

# 15. Cas de recette minimum

## Cas A — Mode 1 conditionnel

Thème Évier :

- fourniture C.I.D. ;
- pose encastrée ;
- égouttoir = oui ;
- apparition immédiate de la position d’égouttoir ;
- modification égouttoir = non ;
- sous-question devient non applicable sans perte silencieuse de l’historique.

Attendu :

- complétude recalculée correctement ;
- aucune question masquée ne bloque.

## Cas B — Confirmation IA

Dimensions pièce données :

- 2,90 × 5,60 m ;
- IA propose 16,24 m² ;
- utilisateur confirme.

Attendu :

`response.state = confirmed`

## Cas C — Produit retenu

Mode 2 :

- IA propose un receveur ;
- utilisateur le retient.

Attendu :

- décision `retained` ;
- l’IA ne peut pas le remplacer silencieusement à la révision suivante.

## Cas D — Produit utilisateur

- utilisateur clique « Mon produit » ;
- colle une URL ;
- export vers l’IA.

Attendu :

`user_product_to_analyze`

La révision suivante peut créer un candidat `origin = user`.

## Cas E — Nouvelle recherche

- produit refusé ;
- motif `too_expensive` ;
- plafond 450 € HT en note.

Attendu :

- candidat rejeté conservé ;
- nouvelle recherche demandée ;
- le même produit ne doit pas être proposé à nouveau sans justification.

## Cas F — Réserve avant commande

- paroi retenue ;
- largeur exacte à relever.

Attendu :

- décision `retained_with_reservation` ;
- check `required_before = order` ;
- devis peut continuer.

## Cas G — Mode 3 / conditionnement

- besoin plinthe 9 m ;
- longueur pièce 2,40 m.

Attendu :

- `purchase_package_count = 4`
- `purchased_quantity = 9.6`
- `remainder_quantity = 0.6`

---

# 16. Critères de validation du contrat 1.2

Le contrat sera considéré prêt pour intégration ERP Artisans lorsque :

1. le JSON Schema Draft 2020-12 est valide ;
2. un dossier peut évoluer Mode 1 → Mode 2 → Mode 3 avec le même `exchange_id` ;
3. les IDs restent stables entre révisions ;
4. les décisions humaines ne sont pas écrasées silencieusement ;
5. les questions conditionnelles fonctionnent sans code exécutable ;
6. les produits refusés restent traçables ;
7. un produit utilisateur peut être transmis par URL ;
8. les réserves peuvent être différées jusqu’à la bonne échéance ;
9. le Mode 3 peut être complet sans que les Modes 1/2 aient été historiquement utilisés ;
10. la plateforme refuse clairement tout format autre que 1.2 et demande une régénération du dossier au format courant.

---

# 17. Travail attendu côté Codex / ERP Artisans

Ce document ne demande pas une intégration immédiate en production.

Le chantier ERP Artisans devra au minimum :

1. ajouter et versionner le schéma 1.2 ;
2. ne pas ajouter de dette de compatibilité historique à la nouvelle plateforme locale ;
3. étendre la persistance du JSON de travail ;
4. préserver les nouveaux blocs `workflow` ;
5. faire évoluer export/import/révisions ;
6. ajouter les tests de contrat ;
7. décider dans un chantier séparé comment intégrer les interfaces Mode 1 / 2 / 3 dans l’ERP.

La plateforme locale reste le laboratoire fonctionnel et UX jusqu’à l’intégration future dans ERP Artisans.

---

## 18. Précision UX validée — contrôles « maintenant » / « plus tard »

Décision du 8 octobre 2026 : dans la revue détaillée, les contrôles encore `pending` ne doivent pas être présentés sous un compteur unique ambigu.

La plateforme distingue visuellement :

- **À traiter maintenant** : contrôles requis avant `product_selection`, `detailed_review` ou `erp_export` ;
- **Vérifications pour plus tard** : contrôles requis avant `order` ou sans échéance bloquante immédiate.

Conséquences :

- une réserve `required_before = order` n'empêche pas à elle seule une étude d'être prête pour ERP Artisans ;
- une validation manuelle d'un ouvrage dans le Mode 3 ne résout pas automatiquement les vérifications prévues avant commande ;
- ces vérifications restent visibles et traçables jusqu'à leur résolution explicite ;- une modification de produit ou une demande « À revoir par l'IA » peut rendre l'étude non prête pour ERP tant que le contrôle associé, requis avant `erp_export`, n'a pas été résolu ;
- la préparation d'un retour vers l'IA reste possible afin que l'IA puisse justement traiter ces points.