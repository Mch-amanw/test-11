# Spécification Technique – Implémenter le journal d’audit sécurisé

## 1. Architecture

Implémentation d’un **service d’audit centralisé** transverse aux modules :

- Import
- TVA
- Délais de paiement
- Gestion des écritures
- Workflow
- IA

Tous les modules doivent appeler ce service via une interface interne standardisée.

---

## 2. Modèle de données

Création d’une table dédiée `auditEvents`.

### 2.1 Champs obligatoires

- `id` (UUID, clé primaire)
- `timestamp` (UTC, généré côté serveur)
- `cabinetId`
- `dossierId`
- `userId`
- `userRole`
- `actionType`
- `entityType`
- `entityId`
- `payload` (JSON)

### 2.2 Contenu du payload (JSON)

Peut inclure selon le cas :

- oldValue
- newValue
- metadata spécifique à l’action
- commentaire utilisateur
- message d’erreur (en cas de tentative non autorisée)

---

## 3. Service d’audit

### 3.1 Interface interne

Méthode standard :

`logEvent(actionType, entityType, entityId, payload)`

Le service enrichit automatiquement l’événement avec :

- userId (issu du contexte d’authentification)
- userRole
- cabinetId
- dossierId
- timestamp (UTC)

Aucune donnée critique ne doit provenir du client.

---

## 4. Déclenchement des événements

Les événements sont déclenchés :

- Après validation réussie d’une action métier
- Lors des transitions de workflow (FSM)
- Lors des modifications d’écritures
- Lors des validations et exports
- Lors des imports
- Lors d’erreurs d’autorisation (middleware RBAC)

Lorsque possible, l’écriture de l’événement d’audit doit être réalisée dans la **même transaction** que l’action métier afin de garantir la cohérence.

---

## 5. Garanties d’intégrité

- Table en mode append-only (aucune opération UPDATE ou DELETE autorisée côté application).
- Accès en écriture réservé au backend.
- Middleware empêchant toute modification via API publique.
- Indexation recommandée :
  - (cabinetId, dossierId)
  - (userId)
  - (timestamp)
  - (actionType)

---

## 6. Sécurité

- Vérification RBAC avant toute action métier.
- Journalisation des tentatives d’accès non autorisées.
- Isolation stricte des données par cabinet.
- Horodatage généré exclusivement côté serveur.

---

## 7. Performance et volumétrie

- Conçu pour supporter des dossiers d’environ 5000 lignes.
- Pagination prévue pour consultation ultérieure.
- Indexation optimisée pour requêtes par dossier et par période.

Ce ticket implémente la brique technique centrale garantissant la traçabilité, l’intégrité et la conformité du système Comptalyses.