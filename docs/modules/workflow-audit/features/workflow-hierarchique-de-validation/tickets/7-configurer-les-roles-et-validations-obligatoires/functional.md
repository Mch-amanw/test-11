## Spécification Fonctionnelle – Configuration des rôles et validations obligatoires

### 1. Objectif du ticket

Mettre en place la configuration opérationnelle des rôles **Collaborateur**, **Chargé de dossier** et **Expert-comptable**, ainsi que les règles de validation obligatoires associées au workflow hiérarchique.

Ce ticket vise à garantir :

- L’application stricte du schéma hiérarchique Collaborateur → Chargé → Expert
- L’impossibilité pour un utilisateur d’agir hors de son périmètre
- Le blocage total de l’export de déclaration tant que la validation experte n’est pas réalisée
- La traçabilité des validations

Ce ticket constitue la base de sécurisation du processus métier.

---

### 2. Définition des rôles

Trois rôles applicatifs doivent être disponibles dans le système :

#### 2.1 Collaborateur
Peut :
- Préparer le dossier
- Modifier les écritures (selon règles du module Gestion des écritures)
- Soumettre le dossier au Chargé

Ne peut pas :
- Valider au niveau Chargé
- Valider au niveau Expert
- Exporter une déclaration TVA

---

#### 2.2 Chargé de dossier
Peut :
- Consulter et analyser le dossier
- Valider le travail du Collaborateur
- Transmettre à l’Expert
- Renvoyer au Collaborateur avec commentaire obligatoire

Ne peut pas :
- Effectuer la validation finale
- Exporter une déclaration TVA

---

#### 2.3 Expert-comptable
Peut :
- Consulter l’ensemble du dossier
- Valider définitivement le dossier
- Renvoyer au Chargé ou au Collaborateur avec commentaire obligatoire
- Autoriser l’export de la déclaration TVA

Seul ce rôle permet l’activation de l’export.

---

### 3. Règles de validation obligatoires

#### 3.1 Validation hiérarchique

- Un dossier ne peut progresser que dans l’ordre hiérarchique défini.
- Chaque validation doit être déclenchée par une action explicite.
- L’identité de l’utilisateur validant doit être associée à l’action.

---

#### 3.2 Blocage avant validation finale

Tant que le dossier n’est pas au statut **Validé par l’Expert** :

- L’export de la déclaration TVA est strictement interdit.
- Les actions liées à l’envoi ou génération finale sont désactivées.
- Aucun contournement manuel n’est autorisé.

---

#### 3.3 Séparation stricte des responsabilités

- Un utilisateur ne peut pas effectuer une validation à un niveau supérieur à son rôle.
- Un utilisateur ne peut pas s’auto-valider à plusieurs niveaux si cela contourne la séparation des responsabilités.
- Toute tentative d’action non autorisée doit être bloquée.

---

### 4. Traçabilité des validations

Chaque validation doit :

- Être enregistrée dans le journal d’audit
- Inclure : utilisateur, rôle, date/heure, statut précédent, statut suivant
- Être non modifiable après enregistrement

---

### 5. Impact métier

Cette configuration :

- Sécurise la responsabilité professionnelle de l’Expert
- Réduit les risques d’erreur ou d’envoi non contrôlé
- Formalise la chaîne de contrôle interne du cabinet
- Constitue un prérequis indispensable à l’utilisation du module TVA

Ce ticket ne couvre pas la personnalisation avancée des rôles ni la gestion multi-rôles par utilisateur (évolutions futures).