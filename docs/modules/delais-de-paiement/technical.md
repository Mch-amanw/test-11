# Spécification Technique – Module Délais de paiement

## 1. Positionnement dans l’architecture

Le module Délais de paiement s’intègre dans l’architecture modulaire :
- Consomme les données du module d’import (écritures Excel).
- Consomme les conventions structurées issues du module d’extraction documentaire.
- S’appuie sur le moteur de règles pour l’évaluation des seuils et la génération d’anomalies.
- Expose ses résultats au module workflow et au journal d’audit.

---

## 2. Composants techniques

### 2.1 Moteur de calcul des délais
- Calcul du nombre de jours entre date de facture / échéance et date d’analyse.
- Gestion des cas :
  - Factures soldées (exclusion ou traitement spécifique selon paramétrage).
  - Factures partiellement réglées.
- Optimisé pour traiter ~5000 lignes par dossier.

---

### 2.2 Moteur de contrôle réglementaire
- Application dynamique des seuils :
  - 60 jours par défaut.
  - 90 ou 120 jours si convention associée.
- Paramétrage via moteur de règles (configuration centralisée).
- Génération d’objets "anomalie délai" stockés en base.

---

### 2.3 Modèle de données (conceptuel)
- Entité Facture (issue des écritures importées).
- Entité Tiers (client / fournisseur).
- Entité Convention (délai contractuel).
- Entité AnomalieDelai :
  - Identifiant facture
  - Type (réglementaire / contractuel)
  - Seuil applicable
  - Nombre de jours constaté
  - Statut (à traiter, validé, justifié)

---

### 2.4 Intégration avec le workflow
- Exposition des anomalies via API interne.
- Mise à jour du statut selon actions utilisateur.
- Journalisation automatique des actions (validation, justification).

---

## 3. Performance et contraintes

- Traitement fluide pour des fichiers d’environ 5000 lignes.
- Calcul exécuté à l’import ou à la demande (recalcul possible après modification).
- Isolation logique des données par cabinet.
- Respect strict de la confidentialité des données dossier.

---

## 4. Sécurité et traçabilité

- Accès aux données limité selon rôle (Collaborateur, Chargé, Expert).
- Toute action sur une anomalie est tracée.
- Aucune modification directe des écritures comptables par ce module.

---

## 5. Évolutivité

- Architecture permettant l’ajout futur de contrôles complémentaires (ex : pénalités, reporting réglementaire avancé).
- Paramétrage extensible via moteur de règles.

---