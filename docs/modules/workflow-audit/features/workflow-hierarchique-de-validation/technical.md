# Spécification Technique – Workflow hiérarchique de validation

## 1. Architecture

La fonctionnalité repose sur :

- Le moteur de workflow (machine à états finis – FSM)
- Le système RBAC
- Le service d’audit centralisé
- Les APIs de validation et de transition

Elle s’intègre au module transversal Workflow & Audit.

---

## 2. Modélisation des états

### 2.1 États persistés

Champ `status` au niveau du dossier avec valeurs possibles :

- PREPARATION
- IN_REVIEW
- FINAL_REVIEW
- VALIDATED
- RETURNED

Les transitions autorisées sont définies explicitement côté serveur.

### 2.2 Table de transitions

Les transitions sont contrôlées par :

- Statut courant
- Rôle de l’utilisateur
- Action demandée

Toute tentative de transition non autorisée génère :

- Refus côté backend (HTTP 403 ou équivalent)
- Événement d’audit de tentative non autorisée

---

## 3. Contrôle d’accès (RBAC)

- Vérification systématique du rôle côté backend.
- Middleware d’autorisation sur endpoints :
  - Soumission au Chargé
  - Validation Chargé
  - Validation Expert
  - Renvoi pour correction
- Interdiction d’accès direct à l’API d’export si statut ≠ VALIDATED.

---

## 4. Invalidation automatique

### 4.1 Détection des modifications critiques

Les actions suivantes déclenchent une réouverture automatique si le dossier est VALIDATED :

- Modification d’écriture
- Suppression ou ajout d’écriture
- Modification impactant les agrégats TVA

### 4.2 Mécanisme technique

- Hook ou événement métier déclenché lors d’une modification.
- Si `status = VALIDATED`, alors :
  - Passage automatique à un état nécessitant revalidation.
  - Enregistrement d’un événement d’audit.

---

## 5. Journalisation

Chaque transition génère automatiquement un événement d’audit contenant :

- ID dossier
- Ancien statut
- Nouveau statut
- ID utilisateur
- Rôle
- Timestamp UTC
- Commentaire (si requis)

Les événements sont stockés en append-only.

---

## 6. Intégration avec module TVA

- L’API d’export vérifie obligatoirement : `status == VALIDATED`.
- Vérification effectuée exclusivement côté serveur.
- Toute tentative d’export hors statut valide est refusée et auditée.

---

## 7. Cohérence transactionnelle

- La transition d’état et l’écriture dans le journal d’audit doivent être atomiques (transaction unique).
- En cas d’échec d’écriture d’audit, la transition est annulée.

---

## 8. Sécurité

- Authentification obligatoire.
- Vérification des permissions côté serveur uniquement.
- Protection contre manipulation du statut via API directe.
- Journalisation des tentatives d’accès non autorisées.

---

## 9. Performance

- Les transitions doivent être légères et indépendantes du volume des écritures (~5000 lignes).
- Aucune recomputation lourde ne doit être déclenchée hors cas de modification critique.

---

## 10. Évolutivité

- Conception permettant l’ajout futur d’étapes intermédiaires.
- Possibilité d’ajouter de nouveaux rôles sans refonte majeure de la FSM.
- Paramétrage potentiel futur par cabinet.