# Spécification Technique – Journal d’audit et traçabilité

## 1. Architecture générale

La fonctionnalité repose sur un **service d’audit centralisé** transverse à tous les modules.

Principes :

- Écriture append-only
- Centralisation des événements
- Couplage faible via API interne
- Isolation des données par cabinet (multi-tenant)

---

## 2. Modèle de données

### 2.1 Table principale : auditEvents

Champs obligatoires :

- id (UUID)
- timestamp (UTC)
- cabinetId
- dossierId
- userId
- userRole
- actionType
- entityType
- entityId
- payload (JSON)

Le champ `payload` contient :

- Anciennes valeurs / nouvelles valeurs
- Métadonnées spécifiques à l’action
- Commentaire utilisateur le cas échéant

---

## 3. Écriture des événements

### 3.1 Déclenchement

Les événements sont générés :

- Lors des transitions de workflow (FSM)
- Lors des modifications d’écritures
- Lors des validations
- Lors de la génération et export de déclaration
- Lors des imports
- Lors d’actions sensibles via le module IA
- Lors des tentatives d’accès non autorisées

L’écriture en base est réalisée dans la même transaction que l’action métier lorsque cela est possible afin d’assurer la cohérence.

---

## 4. Garanties d’intégrité

- Aucune opération UPDATE ou DELETE autorisée sur la table audit.
- Accès en écriture réservé au service backend.
- Middleware empêchant toute modification via API publique.
- Journalisation côté serveur uniquement (pas de confiance dans le client).

Indexation recommandée :

- (cabinetId, dossierId)
- (userId)
- (timestamp)
- (actionType)

---

## 5. API de consultation

### 5.1 Endpoints internes

- GET /dossiers/{id}/audit
  - Paramètres : dateFrom, dateTo, userId, actionType

### 5.2 Sécurité

- Vérification RBAC systématique.
- Filtrage automatique par cabinet.
- Interdiction d’accès aux dossiers non autorisés.

---

## 6. Performance et volumétrie

- Prévu pour des dossiers d’environ 5000 lignes.
- Optimisation via indexation et pagination.
- Chargement paginé côté interface.

---

## 7. Intégration inter-modules

Chaque module (Import, TVA, Délais, Écritures, IA, Workflow) appelle le service d’audit via une interface interne normalisée :

- logEvent(actionType, entityType, entityId, payload)

Le service enrichit automatiquement l’événement avec :

- userId
- userRole
- timestamp
- cabinetId

---

## 8. Sécurité et conformité

- Horodatage en UTC côté serveur.
- Aucune dépendance au poste client.
- Journalisation des tentatives d’escalade de privilèges.
- Cohérence transactionnelle entre changement d’état et écriture d’audit.

Cette implémentation garantit une traçabilité robuste, cohérente avec les exigences réglementaires et professionnelles du marché marocain.