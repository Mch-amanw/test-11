## Module : Moteur de règles & Anomalies – Spécification technique

### 1. Positionnement dans l’architecture

Module central indépendant interfacé avec :
- Module d’import (source des données comptables)
- Module TVA
- Module Délais de paiement
- Module Workflow
- Module IA
- Module Journal d’audit

Il fonctionne comme un service applicatif interne chargé d’évaluer les règles sur un jeu de données donné.

---

### 2. Modèle conceptuel

#### 2.1 Entités principales

- Rule
  - id
  - name
  - category (TVA / Delais / General)
  - severity (Critique / Majeur / Mineur)
  - isActive
  - parameters (structure configurable)

- RuleExecution
  - id
  - dossierId
  - executionDate
  - versionRules

- Anomaly
  - id
  - dossierId
  - ruleId
  - entityReference (écriture, facture)
  - severity
  - status (Ouverte, Corrigée, Justifiée, Validée)
  - message
  - createdAt
  - updatedAt

---

### 3. Mécanisme d’exécution

- Déclenchement automatique après import ou modification d’écritures.
- Traitement batch optimisé pour environ 5000 lignes.
- Exécution séquentielle ou par catégorie.
- Isolation logique par dossier.

Contraintes :
- Temps d’exécution compatible avec usage interactif.
- Pas de modification directe des données sources.

---

### 4. Paramétrage technique des règles

Les règles doivent être :
- Paramétrables via interface d’administration
- Stockées en base de données
- Interprétées par le moteur (pas codées en dur pour les règles métier ajustables)

Chaque règle peut contenir :
- Des seuils numériques
- Des conditions logiques simples
- Des références à des champs standardisés des écritures

---

### 5. Intégration avec les autres modules

#### 5.1 Module TVA
- Fourniture des anomalies liées aux incohérences de calcul
- Fourniture de données pour le scoring

#### 5.2 Module Délais de paiement
- Analyse des dates d’échéance
- Vérification des conventions importées

#### 5.3 Module Workflow
- Vérification de la présence d’anomalies critiques avant changement d’état

#### 5.4 Assistant IA
- Exposition des anomalies via API interne
- Fourniture des messages explicatifs et des métadonnées de règles

#### 5.5 Journal d’audit
- Traçabilité des exécutions
- Historisation des changements de statut des anomalies

---

### 6. Sécurité et confidentialité

- Isolation stricte des données par cabinet
- Accès contrôlé selon rôle utilisateur
- Journalisation des accès aux anomalies
- Aucun export automatique non autorisé

---

### 7. Performance et évolutivité

- Conçu pour évoluer vers d’autres types de règles fiscales
- Architecture extensible pour ajout de nouvelles catégories
- Versionnement des règles pour assurer traçabilité historique

---

## Conclusion technique

Le Moteur de règles & Anomalies est un composant métier central, conçu comme un service configurable, traçable et extensible, garantissant la conformité fiscale et la qualité des données dans Comptalyses tout en respectant les contraintes de performance et de confidentialité du projet.