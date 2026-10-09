# Gouvernance des règles C.I.D. Devis IA

**Statut : règle obligatoire du référentiel public**

Ce document définit comment une règle de rédaction, de formalisation ou de chiffrage observée dans un devis peut devenir une règle générale C.I.D.

## 1. Principe

Tous les chats de devis peuvent **proposer** une évolution du référentiel.

Aucune règle propre à un client, un chantier, une pièce, un produit ponctuel ou une négociation particulière ne doit devenir automatiquement une règle générale.

Une règle n'entre dans le référentiel qu'après **demande ou validation explicite de l'utilisateur**.

## 2. Détection d'une règle candidate

Une règle peut être considérée comme candidate lorsque l'utilisateur emploie une formulation telle que :

- « ceci doit devenir une règle générale » ;
- « applique cela à tous les devis » ;
- « ajoute cette règle au référentiel » ;
- « à l'avenir, présente toujours les devis ainsi » ;
- ou toute formulation équivalente clairement générale.

Une simple correction du devis courant n'est jamais suffisante pour modifier le référentiel.

## 3. Classification obligatoire avant enregistrement

Avant toute écriture, classer la demande dans l'un des trois états suivants :

1. **Spécifique au chantier** — reste uniquement dans le dossier courant.
2. **Candidate générale** — semble réutilisable mais nécessite confirmation.
3. **Règle générale validée** — l'utilisateur a explicitement demandé ou confirmé son enregistrement dans le référentiel.

En cas de doute, rester en **candidate générale** et demander confirmation.

## 4. Contrôle avant écriture

Avant d'enregistrer une règle générale, présenter brièvement à l'utilisateur :

- la règle reformulée ;
- le fichier cible proposé ;
- l'éventuelle règle existante qu'elle complète, remplace ou contredit ;
- les conséquences importantes sur les devis futurs ;
- la nouvelle version de ruleset proposée.

Ne jamais enregistrer deux règles contradictoires. Si la nouvelle règle remplace l'ancienne, modifier la règle existante plutôt que d'empiler les deux.

## 5. Validation explicite

L'écriture persistante dans GitHub nécessite une confirmation explicite de l'utilisateur après présentation de la règle, par exemple :

- « valide cette règle générale » ;
- « enregistre-la dans le référentiel » ;
- « oui, applique-la à tous les devis ».

Une validation donnée uniquement pour le devis courant ne vaut pas validation du référentiel global.

## 6. Écriture dans GitHub

Le dépôt de référence est :

`CID-PG/CID-Devis-IA-Reference`

La branche de référence est `main`.

Après validation explicite :

1. modifier le ou les fichiers concernés ;
2. mettre à jour `manifest.json` avec une nouvelle `ruleset_version` ;
3. mettre à jour `CHANGELOG.md` ;
4. conserver un message de commit explicite ;
5. vérifier que le manifeste et tous les documents obligatoires restent lisibles ;
6. vérifier la validité JSON du manifeste et du schéma si celui-ci a été modifié.

Si le chat ne dispose pas d'un accès GitHub en écriture, il doit fournir la modification prête à appliquer et signaler clairement qu'elle **n'est pas encore enregistrée**.

## 7. Versionnement

Le manifeste est l'autorité pour la version active du ruleset.

Format courant :

`AAAA.MM.JJ.N`

Exemple :

`2026.10.09.2`

Toute modification persistante d'une règle obligatoire entraîne une nouvelle version du ruleset.

Les documents individuels ne doivent pas être utilisés pour déterminer la version globale : toujours lire `manifest.json`.

## 8. Répartition des règles

- `REGLES_GENERATION_DEVIS.md` : règles générales de préparation, workflow et principes transversaux.
- `FORMAT_OUVRAGES.md` : rédaction et formalisation des sections, ouvrages, descriptions et composants.
- `REGLES_CHIFFRAGE.md` : prix, marge, main-d'œuvre, conditionnements, quantités et sourcing.
- `EXEMPLES_OUVRAGES.md` : exemples fictifs illustrant les règles, sans devenir eux-mêmes normatifs.
- `CONTRAT_DEVIS_1.2.md` et le schéma JSON : contrat technique d'échange ; ne pas les modifier pour une simple préférence rédactionnelle.
- `GOUVERNANCE_REGLES.md` : mécanisme d'évolution du présent référentiel.

## 9. Données interdites dans le référentiel public

Ne jamais y enregistrer :

- nom, adresse ou donnée personnelle d'un client ;
- métrés ou décisions propres à un chantier réel ;
- devis ou documents réels ;
- prix d'achat professionnels personnalisés ;
- identifiants, secrets, clés API ou informations de connexion.

Une règle peut provenir d'un chantier réel, mais elle doit être reformulée de manière générique et sans donnée identifiable.

## 10. Comportement attendu dans un chat de devis

Lorsqu'une règle générale est validée et enregistrée pendant un devis :

- poursuivre le devis courant sans perdre les décisions déjà prises ;
- appliquer la nouvelle règle au devis courant si l'utilisateur le demande ou si sa formulation l'implique clairement ;
- ne pas réécrire silencieusement les parties déjà validées si cela crée un impact important : signaler l'impact et demander confirmation si nécessaire ;
- indiquer la nouvelle `ruleset_version` après enregistrement.
