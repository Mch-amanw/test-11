# Spécification Technique – Module Import & Gestion des données

## 1. Positionnement dans l’architecture

Le module s’inscrit dans l’architecture modulaire SaaS de Comptalyses et interagit avec :

- Le module TVA
- Le module Délais de paiement
- Le moteur de règles
- Le module Workflow
- Le module IA
- Le module Journal d’audit

Il agit comme couche d’ingestion et de préparation des données.

---

## 2. Traitement des fichiers Excel Sage

### 2.1 Pipeline d’import

1. Upload sécurisé du fichier.
2. Vérification du type et de l’intégrité du fichier.
3. Lecture structurée des feuilles pertinentes.
4. Validation du schéma (présence des colonnes attendues).
5. Parsing ligne par ligne.
6. Validation des types (dates, numériques, textes).
7. Stockage des données valides en base.
8. Génération d’un rapport d’import.

### 2.2 Gestion des erreurs

- Classification des erreurs :
  - Erreurs bloquantes (structure invalide).
  - Erreurs ligne par ligne (données incohérentes).
- Stockage des lignes rejetées avec motif.
- Aucun traitement aval déclenché tant que l’import n’est pas validé.

### 2.3 Performance

- Optimisation pour des fichiers d’environ 5000 lignes.
- Traitement asynchrone possible pour éviter le blocage de l’interface.

---

## 3. Stockage des données

### 3.1 Modèle logique

- Entité Dossier
- Entité Import (métadonnées : date, utilisateur, version)
- Entité Écriture comptable
- Entité Tiers (client/fournisseur)
- Entité Convention contractuelle

Les données sont liées par des identifiants internes.

### 3.2 Versioning

- Chaque import crée une nouvelle version associée au dossier.
- Les versions précédentes restent accessibles en lecture seule.

---

## 4. Gestion des documents contractuels

### 4.1 Upload et stockage

- Stockage sécurisé des documents.
- Association à un tiers via identifiant interne.
- Conservation des métadonnées (date d’upload, utilisateur, statut de validation).

### 4.2 Extraction des délais

- Mécanisme d’extraction automatisée (analyse de contenu documentaire).
- Détection ciblée des mentions 90 jours / 120 jours.
- Retour d’un résultat structuré (délai détecté, niveau de confiance).

---

## 5. Sécurité

- Upload via canal sécurisé.
- Isolation logique des données par cabinet (multi-tenant).
- Contrôle d’accès basé sur les rôles.
- Journalisation des accès et imports.

---

## 6. Intégrations internes

- Exposition des données normalisées via services internes pour :
  - Calcul TVA
  - Analyse des délais
  - Moteur de règles
  - Assistant IA

- Aucun accès direct externe aux données brutes.

---

## 7. Contraintes

- Conformité aux exigences de confidentialité.
- Robustesse face à des variations mineures de format des exports Sage.
- Architecture évolutive pour supporter d’autres formats d’import à l’avenir.