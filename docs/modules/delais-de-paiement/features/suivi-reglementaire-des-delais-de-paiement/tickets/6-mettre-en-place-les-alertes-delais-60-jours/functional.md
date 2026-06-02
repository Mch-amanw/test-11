## Ticket : Mettre en place les alertes délais > 60 jours

### 1. Objectif du ticket
Mettre en œuvre le mécanisme opérationnel d’analyse des balances âgées et de génération automatique des alertes liées aux dépassements de délais de paiement, conformément aux règles réglementaires marocaines (60 jours) et aux conventions contractuelles valides (90 ou 120 jours).

Ce ticket couvre la détection et la création des anomalies de délai au niveau des factures ouvertes.

---

### 2. Périmètre fonctionnel

Le ticket inclut :

- L’analyse des factures ouvertes issues des données importées.
- Le calcul du nombre de jours écoulés à la date d’analyse du dossier.
- L’application automatique du seuil réglementaire ou contractuel.
- La génération d’une alerte (anomalie) en cas de dépassement.
- L’enregistrement de ces anomalies dans le dossier.

Le ticket n’inclut pas :

- L’interface détaillée de visualisation (traitée dans un autre ticket si nécessaire).
- La modification des écritures comptables.
- La modification des conventions contractuelles.

---

### 3. Règles de gestion appliquées

#### 3.1 Factures concernées

- Seules les factures ouvertes (non soldées ou partiellement soldées) sont analysées.
- Les factures totalement soldées sont exclues du contrôle.

#### 3.2 Base de calcul du délai

- Si une date d’échéance est disponible, le calcul est basé sur la date d’échéance.
- Sinon, le calcul est basé sur la date de facture.
- Le nombre de jours est calculé entre cette date de référence et la date d’analyse du dossier.

#### 3.3 Détermination du seuil applicable

- Par défaut : seuil réglementaire de 60 jours.
- Si une convention valide (90 ou 120 jours) est détectée pour le tiers concerné :
  - Le seuil contractuel remplace le seuil de 60 jours.

#### 3.4 Déclenchement des alertes

Une alerte est générée si :

- Le nombre de jours constaté est strictement supérieur au seuil applicable.

Type d’alerte :

- **RÉGLEMENTAIRE** : dépassement du seuil de 60 jours sans convention valide.
- **CONTRACTUELLE** : dépassement du seuil contractuel (90 ou 120 jours).

Chaque alerte est rattachée :

- À la facture concernée.
- Au tiers associé.
- Au dossier en cours.

---

### 4. Données produites

Pour chaque anomalie de délai créée :

- Identifiant de la facture.
- Identifiant du tiers.
- Type d’alerte (RÉGLEMENTAIRE / CONTRACTUELLE).
- Seuil appliqué.
- Nombre de jours constaté.
- Montant concerné.
- Statut initial : A_TRAITER.
- Horodatage de création.

Ces anomalies alimentent :

- La liste des anomalies du dossier.
- Le module workflow (traitement ultérieur).
- Le scoring global du dossier (via agrégation future).

---

### 5. Intégration au processus global

- Le calcul est exécuté :
  - Lors de l’import initial des écritures.
  - Lors d’un recalcul manuel déclenché après modification des écritures ou des conventions.
- Les anomalies générées sont consultables par les utilisateurs autorisés selon leur rôle.
- Les anomalies devront être traitées (validation ou justification) dans le cadre du workflow avant validation finale du dossier.

---

### 6. Contraintes fonctionnelles

- Aucun dépassement ne doit être ignoré automatiquement.
- Aucune modification des écritures comptables n’est réalisée par ce traitement.
- Les règles de seuil restent paramétrables via le moteur de règles.
- Le traitement doit rester performant pour un volume d’environ 5000 lignes par dossier.