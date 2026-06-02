# Spécification Fonctionnelle – Module Import & Gestion des données

## 1. Rôle du module

Le module **Import & Gestion des données** constitue la porte d’entrée des informations comptables dans Comptalyses. Il permet :

- L’import des fichiers Excel exportés depuis Sage.
- L’import des documents contractuels (conventions clients/fournisseurs).
- La validation du format et de la cohérence minimale des données.
- La préparation et la structuration des données pour les modules d’analyse (TVA, délais de paiement, moteur de règles, IA).

Ce module garantit que seules des données valides, traçables et sécurisées alimentent les traitements du système.

---

## 2. Import des fichiers comptables Sage

### 2.1 Périmètre

- Import de fichiers Excel standardisés exportés depuis Sage.
- Volume cible : environ 5000 lignes comptables par dossier.
- Import par dossier (période mensuelle ou trimestrielle selon le cas TVA).

### 2.2 Fonctionnalités

- Upload manuel du fichier Excel via interface sécurisée.
- Vérification automatique du format attendu (structure des colonnes).
- Contrôle de complétude des champs essentiels (dates, comptes, montants HT/TVA/TTC si présents).
- Détection des erreurs bloquantes (format invalide, colonnes manquantes, fichier corrompu).
- Affichage d’un rapport d’import :
  - Nombre total de lignes lues
  - Nombre de lignes valides
  - Nombre de lignes rejetées
  - Liste des erreurs détectées
- Confirmation explicite de l’import par l’utilisateur.

### 2.3 Règles de gestion

- Un fichier invalide (structure non conforme) ne peut pas être intégré.
- Chaque import est rattaché à un dossier et à une période.
- Un historique des imports est conservé (date, utilisateur, version du fichier).
- Les données importées ne doivent pas être modifiées directement sans passer par le module de gestion des corrections.

---

## 3. Import des conventions contractuelles

### 3.1 Périmètre

- Upload de documents contractuels clients/fournisseurs.
- Objectif : identifier les délais de paiement spécifiques (90/120 jours).

### 3.2 Fonctionnalités

- Téléversement de documents (format documentaire standard, ex : PDF).
- Association du document à un client ou fournisseur du dossier.
- Extraction automatique des délais contractuels (90 ou 120 jours) lorsque détectables.
- Affichage du délai détecté et possibilité de validation par l’utilisateur.
- Enregistrement du document et du délai associé.

### 3.3 Règles de gestion

- En l’absence de convention valide, le délai réglementaire par défaut de 60 jours s’applique.
- Une convention doit être explicitement associée à un tiers (client ou fournisseur).
- Toute modification d’un délai contractuel est tracée dans le journal d’audit.

---

## 4. Structuration et préparation des données

### 4.1 Normalisation

- Transformation des données Excel en un format interne structuré.
- Uniformisation des champs nécessaires aux modules :
  - Dates
  - Comptes
  - Montants
  - Identifiants de tiers

### 4.2 Préparation pour les modules aval

Les données importées alimentent :

- Le module TVA (contrôles HT/TVA/TTC, taux).
- Le module Délais de paiement (analyse des balances âgées).
- Le moteur de règles (détection d’anomalies).
- L’assistant IA (accès contrôlé aux données du dossier).

Le module garantit la disponibilité, la cohérence et la traçabilité des données transmises.

---

## 5. Traçabilité et audit

- Journalisation de chaque import (date, heure, utilisateur, dossier concerné).
- Conservation des versions successives des fichiers importés.
- Historique des rejets et corrections liés à l’import.

---

## 6. Contraintes fonctionnelles

- Respect strict de la confidentialité des données.
- Isolation logique des données par cabinet.
- Aucun traitement analytique avancé (TVA, scoring, alertes) n’est réalisé dans ce module : il prépare uniquement les données.