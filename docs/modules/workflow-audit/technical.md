# Spécification Technique – Module Workflow & Audit

## 1. Architecture du module

Le module est un composant transverse intégré à l’architecture SaaS multi-clients de Comptalyses.

Il comprend :

- Un moteur de gestion des états (state management)
- Un système de gestion des rôles et permissions (RBAC)
- Un service de journalisation d’audit centralisé
- Des APIs d’intégration avec les autres modules

---

## 2. Gestion des rôles et permissions (RBAC)

### 2.1 Modèle d’accès

- Implémentation d’un modèle **Role-Based Access Control (RBAC)**.
- Attribution d’un ou plusieurs rôles à un utilisateur.
- Vérification systématique des permissions côté backend.

### 2.2 Contrôles techniques

- Middleware d’autorisation sur chaque endpoint sensible.
- Vérification du rôle avant :
  - Validation d’un dossier
  - Export de déclaration
  - Modification d’écritures validées
- Isolation logique des données par cabinet (multi-tenant).

---

## 3. Moteur de workflow

### 3.1 Gestion des états

- Implémentation d’un modèle de type **machine à états finis (FSM)**.
- États persistés en base de données.
- Transitions autorisées définies explicitement.

### 3.2 Contrôle des transitions

- Validation serveur des transitions.
- Vérification du rôle et du statut courant.
- Enregistrement automatique d’un événement d’audit à chaque transition.

---

## 4. Journal d’audit

### 4.1 Stockage

- Table dédiée aux événements d’audit.
- Écriture append-only (aucune mise à jour ni suppression autorisée).
- Indexation par :
  - Dossier
  - Utilisateur
  - Date
  - Type d’action

### 4.2 Format des événements

Chaque événement contient :

- ID unique
- Timestamp (UTC)
- ID utilisateur
- Rôle
- Type d’action
- Entité concernée (type + ID)
- Payload JSON décrivant les changements

### 4.3 Intégrité

- Aucune modification directe autorisée via l’interface applicative.
- Accès restreint en lecture.
- Journalisation centralisée pour cohérence inter-modules.

---

## 5. Intégration inter-modules

- Les autres modules publient des événements au service d’audit.
- API interne pour :
  - Changement de statut de dossier
  - Validation
  - Enregistrement d’action métier
- Couplage faible via interfaces internes clairement définies.

---

## 6. Sécurité

- Authentification obligatoire pour toute action.
- Vérification des permissions côté serveur uniquement.
- Protection contre l’escalade de privilèges.
- Journalisation des tentatives d’accès non autorisées.

---

## 7. Contraintes et exigences

- Performance adaptée à des dossiers d’environ 5000 lignes.
- Traçabilité complète requise pour conformité réglementaire.
- Cohérence transactionnelle entre changement de statut et enregistrement d’audit.
- Architecture évolutive pour intégrer de futurs rôles ou étapes supplémentaires.

---

## 8. Points d’extension futurs

- Paramétrage avancé des workflows par cabinet.
- Signature électronique de validation experte.
- Export du journal d’audit pour contrôle externe.