# Spécification Fonctionnelle – Comptalyses

## 1. Présentation du projet
Comptalyses est une solution SaaS d’assistance comptable destinée aux cabinets comptables marocains. Elle vise à automatiser et fiabiliser le traitement des fichiers comptables, en mettant l’accent sur :

- Le contrôle et la fiabilisation de la TVA déductible
- Le suivi des délais de paiement
- La préparation des déclarations de TVA
- L’assistance via un chatbot intelligent connecté aux dossiers

Le projet démarre avec un cabinet pilote.

---

## 2. Objectifs métier

- Réduire les erreurs liées à la TVA (montants, taux, conformité réglementaire marocaine)
- Sécuriser la préparation des déclarations de TVA
- Assurer le respect des délais de paiement réglementaires (60 jours, sauf conventions 90/120 jours)
- Détecter automatiquement les anomalies comptables
- Fournir un outil d’assistance intelligent aux collaborateurs et experts
- Mettre en place un workflow structuré avec validation experte obligatoire avant dépôt

---

## 3. Utilisateurs cibles

- Collaborateurs comptables (préparation des dossiers)
- Chargés de dossier (validation intermédiaire)
- Experts-comptables (validation finale et envoi)

---

## 4. Périmètre fonctionnel du MVP

### 4.1 Import et gestion des données

- Import de fichiers Excel standardisés exportés depuis Sage
- Volume cible : environ 5000 lignes comptables par dossier
- Import de conventions clients/fournisseurs (documents contractuels)
- Lecture automatique des délais contractuels (90/120 jours)
- Accès aux données internes des dossiers pour analyse

---

### 4.2 Contrôle et fiabilisation de la TVA

#### Vérifications automatiques :
- Cohérence HT / TVA / TTC
- Vérification des taux de TVA
- Conformité à la réglementation marocaine

#### Détection d’anomalies (basée sur règles paramétrables)
- Incohérences de montants
- Taux incorrects
- Toute anomalie détectable via règles définies

#### Scoring
- Attribution d’un score de risque par dossier (fiabilité TVA / conformité)

---

### 4.3 Suivi des délais de paiement

- Analyse des balances âgées clients et fournisseurs
- Détection automatique des factures dépassant 60 jours
- Exception automatique si convention valide 90/120 jours détectée
- Génération d’alertes réglementaires le cas échéant
- Suivi analytique des retards

---

### 4.4 Préparation des déclarations de TVA

- Gestion TVA mensuelle et trimestrielle marocaine
- Génération d’un brouillon pré-rempli
- Export compatible avec le portail de la DGI marocaine
- Validation obligatoire par l’expert avant envoi

---

### 4.5 Gestion des corrections

- Possibilité de modification des écritures
- Proposition d’écritures correctives
- Export des corrections vers Sage
- Historique des modifications

---

### 4.6 Workflow et rôles

Workflow structuré :
1. Collaborateur prépare
2. Chargé de dossier valide
3. Expert valide et autorise l’envoi

Fonctionnalités associées :
- Gestion des rôles et permissions
- Journal d’audit complet (traçabilité des actions et validations)

---

### 4.7 Assistant IA (Chatbot)

Fonctionnalités :
- Accès aux données du dossier
- Explication des anomalies détectées
- Proposition d’écritures correctives
- Réponses aux questions liées aux dossiers
- Appui sur une base documentaire de la législation marocaine
- Capacité à citer les références légales pertinentes

---

## 5. Règles de gestion principales

- Toute déclaration doit être validée par un expert avant envoi.
- Les factures dépassant 60 jours doivent déclencher une alerte sauf présence d’une convention valide 90/120 jours.
- Les anomalies sont détectées via un système de règles paramétrables.
- Les conventions doivent être importées sous forme documentaire et analysées.
- Toutes les actions (corrections, validations, exports) doivent être tracées.
- Respect strict de la confidentialité des données.

---