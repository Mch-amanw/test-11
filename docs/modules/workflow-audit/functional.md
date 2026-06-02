# Spécification Fonctionnelle – Module Workflow & Audit

## 1. Rôle du module dans le projet

Le module **Workflow & Audit** structure et sécurise le processus de traitement des dossiers au sein de Comptalyses. Il garantit :

- Une progression contrôlée des dossiers selon un circuit hiérarchique défini
- Une validation experte obligatoire avant toute génération ou envoi de déclaration
- Une traçabilité complète des actions réalisées sur les données
- Le respect des exigences de confidentialité et de conformité réglementaire

Il constitue le socle de gouvernance interne de la plateforme.

---

## 2. Gestion des rôles

### 2.1 Rôles définis

Le module gère trois rôles principaux :

- **Collaborateur** : prépare le dossier, analyse les anomalies, propose des corrections.
- **Chargé de dossier** : contrôle le travail du collaborateur, valide ou renvoie pour correction.
- **Expert-comptable** : valide définitivement le dossier et autorise l’export de la déclaration.

### 2.2 Principes de permissions

- Les droits sont déterminés par rôle.
- Un utilisateur ne peut pas valider un dossier à un niveau supérieur à son rôle.
- Seul l’Expert peut autoriser l’export de la déclaration TVA.
- Les actions sensibles (validation, export, modification après validation) sont strictement contrôlées.

---

## 3. Workflow de traitement d’un dossier

### 3.1 États du dossier

Un dossier évolue selon des statuts structurés :

1. **En préparation (Collaborateur)**
2. **En revue (Chargé)**
3. **En validation finale (Expert)**
4. **Validé**
5. **Renvoyé pour correction** (retour possible à l’étape précédente)

### 3.2 Règles de transition

- Le passage à l’étape suivante nécessite une action explicite de validation.
- Un Chargé ou un Expert peut renvoyer le dossier à l’étape précédente avec commentaire obligatoire.
- Toute modification significative après validation intermédiaire peut nécessiter une nouvelle validation.
- Une déclaration ne peut être exportée que si le dossier est au statut **Validé** par l’Expert.

---

## 4. Journal d’audit

### 4.1 Périmètre des actions tracées

Le journal d’audit enregistre notamment :

- Import de fichiers
- Modifications d’écritures
- Propositions et validations de corrections
- Changements de statut du dossier
- Validations intermédiaires et finales
- Génération et export de déclaration
- Interactions sensibles avec le système

### 4.2 Informations enregistrées

Chaque entrée d’audit inclut :

- Identifiant utilisateur
- Rôle de l’utilisateur au moment de l’action
- Date et heure
- Type d’action
- Élément concerné (dossier, écriture, déclaration)
- Ancienne valeur / nouvelle valeur (si applicable)

### 4.3 Consultation du journal

- Accès restreint selon rôle.
- Visualisation chronologique par dossier.
- Filtrage par type d’action ou utilisateur.
- Aucune suppression d’entrée autorisée.

---

## 5. Règles de gestion clés

- Toute déclaration TVA doit être validée par un Expert avant export.
- Aucune action critique ne doit être possible sans authentification préalable.
- Toute modification post-validation experte doit générer une nouvelle trace et invalider l’état validé si nécessaire.
- Le journal d’audit est infalsifiable et non modifiable.
- Les droits sont appliqués de manière stricte et cohérente avec le rôle actif.

---

## 6. Confidentialité et conformité

- Les utilisateurs ne peuvent accéder qu’aux dossiers autorisés.
- Toutes les actions sont traçables pour répondre aux exigences réglementaires et aux contrôles internes.
- Le module contribue à sécuriser la responsabilité professionnelle de l’Expert-comptable.

---

## 7. Intégration avec les autres modules

Le module Workflow & Audit interagit avec :

- Le module d’import (traçabilité des imports)
- Le module TVA (validation et génération de déclaration)
- Le module délais de paiement (validation des alertes)
- Le module gestion des écritures (historique des corrections)
- Le module IA (traçabilité des actions déclenchées depuis l’assistant)

Il agit comme couche transverse de contrôle et de gouvernance.