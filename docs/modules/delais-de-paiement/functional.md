# Module : Délais de paiement

## 1. Rôle du module dans le projet
Le module **Délais de paiement** a pour objectif d’analyser les balances âgées clients et fournisseurs afin de contrôler le respect des délais réglementaires marocains (60 jours), tout en prenant en compte les conventions contractuelles spécifiques autorisant des délais de 90 ou 120 jours.

Il contribue à :
- La conformité réglementaire du cabinet et de ses clients
- La détection automatique des dépassements de délais
- La génération d’alertes exploitables dans le workflow
- L’alimentation du moteur de règles et du scoring de risque

Ce module s’intègre au workflow global (Collaborateur → Chargé → Expert) et alimente le journal d’audit.

---

## 2. Fonctionnalités principales

### 2.1 Analyse des balances âgées
- Lecture des données issues de l’import Excel (écritures clients et fournisseurs).
- Identification des factures ouvertes (non soldées ou partiellement soldées).
- Calcul automatique du nombre de jours écoulés entre :
  - La date de facture
  - La date d’échéance (si disponible)
  - La date d’analyse (date de clôture ou date courante du dossier)
- Regroupement par tiers (client / fournisseur).
- Visualisation synthétique :
  - Répartition par tranches (0–30, 31–60, 61–90, 91–120, >120 jours).

---

### 2.2 Contrôle réglementaire des délais
- Règle standard : alerte automatique si délai > 60 jours.
- Exception :
  - Si une convention valide 90 ou 120 jours est détectée pour le tiers concerné.
  - Dans ce cas, le seuil d’alerte est ajusté au délai contractuel.
- Détection automatique des dépassements contractuels (ex : > 90 ou > 120 jours selon convention).

Chaque dépassement génère :
- Une anomalie dans le dossier.
- Un niveau de gravité (paramétrable via moteur de règles).

---

### 2.3 Gestion des conventions contractuelles
- Association des conventions importées aux tiers concernés.
- Prise en compte automatique des délais contractuels extraits (90 ou 120 jours).
- Si aucune convention valide n’est détectée : application du seuil réglementaire de 60 jours.

Le module ne modifie pas les conventions mais consomme les données structurées issues du module d’import et d’extraction documentaire.

---

### 2.4 Génération d’alertes et suivi
- Liste des factures en dépassement.
- Indication du type de dépassement :
  - Réglementaire (>60 jours sans convention)
  - Contractuel (>90 ou >120 jours selon convention)
- Indicateurs synthétiques :
  - Nombre de factures en dépassement
  - Montant total concerné
  - Répartition clients / fournisseurs
- Contribution au score de risque global du dossier.

Les alertes sont visibles par :
- Collaborateur (analyse)
- Chargé (validation intermédiaire)
- Expert (validation finale)

---

### 2.5 Intégration au workflow
- Les anomalies liées aux délais doivent être revues avant validation du dossier.
- Les décisions (validation malgré dépassement, justification) sont historisées.
- Aucune déclaration ne peut être finalisée sans passage par les étapes de validation prévues.

---

## 3. Règles de gestion

1. Toute facture dépassant 60 jours sans convention valide déclenche une alerte réglementaire.
2. Si une convention 90 ou 120 jours est détectée, le seuil applicable remplace le seuil de 60 jours.
3. Toute facture dépassant le seuil contractuel déclenche une alerte contractuelle.
4. Les calculs sont réalisés sur la base des données importées (aucune modification automatique des écritures).
5. Toute validation ou justification d’un dépassement est tracée dans le journal d’audit.
6. Les règles de seuil peuvent être paramétrables via le moteur de règles.

---