# Spécification Technique – Suivi réglementaire des délais de paiement

## 1. Composants impactés

- Module Délais de paiement (moteur de calcul des délais).
- Moteur de règles (paramétrage des seuils).
- Module d’import (données factures).
- Module conventions (délais contractuels structurés).
- Module workflow (gestion des statuts).
- Journal d’audit.

---

## 2. Logique de traitement

### 2.1 Sélection des données
- Récupération des factures ouvertes issues des écritures importées.
- Association à l’entité Tiers.
- Récupération de la convention active éventuelle associée au tiers.

### 2.2 Calcul du délai
- Date de référence : date d’analyse du dossier.
- Calcul :
  - Si date d’échéance disponible → calcul basé sur date d’échéance.
  - Sinon → calcul basé sur date de facture.
- Résultat : nombre de jours écoulés (entier).

### 2.3 Détermination du seuil applicable
- Si convention valide détectée → seuil = 90 ou 120 jours.
- Sinon → seuil = 60 jours.
- Les valeurs de seuil sont paramétrables via le moteur de règles.

### 2.4 Génération d’anomalie
Création d’un objet `AnomalieDelai` si :
- Nombre de jours > seuil applicable.

Attributs minimaux :
- Identifiant facture
- Identifiant tiers
- Type (REGLEMENTAIRE / CONTRACTUEL)
- Seuil appliqué
- Nombre de jours constaté
- Montant concerné
- Statut (A_TRAITER, VALIDE, JUSTIFIE)
- Horodatage de création

---

## 3. Intégration Workflow

- Exposition des anomalies via API interne.
- Mise à jour du statut par action utilisateur (validation / justification).
- Déclenchement automatique d’une entrée dans le journal d’audit à chaque changement de statut.
- Vérification bloquante : le dossier ne peut être validé à l’étape suivante si des anomalies sont en statut A_TRAITER.

---

## 4. Performance

- Traitement optimisé pour environ 5000 lignes par dossier.
- Calcul déclenché :
  - À l’import initial.
  - À la demande (recalcul manuel possible après modification des écritures ou conventions).

---

## 5. Sécurité et confidentialité

- Accès contrôlé selon rôle.
- Isolation logique des données par cabinet.
- Aucune modification des écritures comptables par cette fonctionnalité.
- Journalisation complète des actions utilisateurs.

---

## 6. Évolutivité

- Seuils entièrement paramétrables via moteur de règles.
- Structure compatible avec l’ajout futur de nouveaux types d’alertes (ex : pénalités de retard, reporting avancé).