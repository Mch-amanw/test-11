## Spécification Technique – Configuration des rôles et validations obligatoires

### 1. Implémentation du modèle RBAC

#### 1.1 Définition des rôles

Création de trois rôles système persistés :

- COLLABORATOR
- MANAGER (Chargé de dossier)
- EXPERT

Stockage en base :

- Table `roles`
- Table de liaison `user_roles`

Chaque utilisateur possède au moins un rôle.

---

### 2. Permissions associées

Implémentation d’un mapping explicite rôle → actions autorisées.

Actions principales concernées :

- SUBMIT_TO_REVIEW
- VALIDATE_REVIEW
- RETURN_FOR_CORRECTION
- VALIDATE_FINAL
- EXPORT_TVA

Les permissions sont vérifiées exclusivement côté backend.

---

### 3. Middleware d’autorisation

Mise en place d’un middleware ou interceptor backend qui :

1. Vérifie l’authentification.
2. Vérifie le rôle de l’utilisateur.
3. Vérifie la compatibilité entre :
   - Rôle
   - Statut courant du dossier
   - Action demandée

En cas d’échec :

- Retour HTTP 403 (ou équivalent)
- Enregistrement d’un événement d’audit de tentative non autorisée

---

### 4. Blocage technique de l’export

L’endpoint d’export TVA doit :

- Vérifier `status == VALIDATED`
- Vérifier `role == EXPERT`

Double protection :

- Contrôle RBAC
- Vérification explicite du statut du dossier

Aucune logique d’activation uniquement côté frontend.

---

### 5. Intégration avec la machine à états (FSM)

Chaque action de validation déclenche :

- Vérification du rôle
- Vérification de la transition autorisée
- Mise à jour atomique du statut
- Création d’un événement d’audit dans la même transaction

La transition est annulée si l’écriture d’audit échoue.

---

### 6. Prévention d’escalade de privilèges

- Interdiction de modifier le champ `status` directement via API générique.
- Tous les changements de statut passent par un service dédié.
- Journalisation des tentatives d’accès non autorisées.
- Vérification côté serveur uniquement (aucune confiance accordée au frontend).

---

### 7. Journalisation

Chaque validation génère un enregistrement dans la table `audit_events` contenant :

- event_id
- dossier_id
- previous_status
- new_status
- user_id
- role
- timestamp_utc
- optional_comment

Stockage en append-only.

---

### 8. Multi-tenant

- Vérification que l’utilisateur appartient au même cabinet que le dossier.
- Isolation stricte des données.

---

### 9. Contraintes

- Performance indépendante du volume d’écritures.
- Cohérence transactionnelle stricte.
- Conception extensible pour ajout futur de nouveaux rôles.

Ce ticket ne couvre pas :

- Paramétrage personnalisé des workflows par cabinet
- Signature électronique
- Gestion avancée de délégation