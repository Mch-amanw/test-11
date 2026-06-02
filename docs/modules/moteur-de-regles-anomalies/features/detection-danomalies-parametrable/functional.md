# Fonctionnalité : Détection d’anomalies paramétrable

## 1. Objectif métier

Permettre au cabinet de configurer, activer et exécuter des règles métier afin de détecter automatiquement des anomalies comptables et fiscales (TVA, montants, incohérences générales) dans les dossiers importés.

Cette fonctionnalité vise à :
- Fiabiliser les données comptables avant validation.
- Réduire les risques d’erreurs de TVA et de non-conformité réglementaire marocaine.
- Standardiser les contrôles internes du cabinet.
- Fournir une base exploitable pour le scoring du dossier et l’assistant IA.

---

## 2. Périmètre fonctionnel

La fonctionnalité couvre :

- La configuration des règles métier paramétrables.
- L’activation / désactivation des règles au niveau du cabinet.
- L’exécution automatique des règles sur un dossier.
- La génération et la restitution des anomalies détectées.
- La classification des anomalies par gravité.

Elle ne couvre pas :
- La correction automatique des écritures (le moteur ne modifie jamais les données).
- La gestion détaillée du cycle de vie des anomalies (traitée par la fonctionnalité dédiée du module).

---

## 3. Configuration des règles

Chaque règle paramétrable doit permettre :

- Définition d’un nom et d’une description.
- Catégorisation : TVA / Délais / Générale.
- Attribution d’un niveau de gravité : Critique / Majeur / Mineur.
- Activation / désactivation.
- Paramétrage de seuils (ex : tolérance d’écart sur un calcul).
- Définition de conditions logiques basées sur les champs standards des écritures (montants HT, TVA, TTC, dates, comptes, etc.).

Les règles sont définies au niveau du cabinet et s’appliquent à tous les dossiers (MVP).

---

## 4. Types d’anomalies détectées

### 4.1 Anomalies TVA
- Incohérence entre HT / TVA / TTC.
- Taux de TVA incohérent.
- Écart de calcul supérieur au seuil paramétré.

### 4.2 Anomalies de montants
- Montants incohérents ou négatifs inattendus.
- Déséquilibres identifiables via règles définies.

### 4.3 Anomalies générales
- Données essentielles manquantes.
- Incohérences détectables via conditions paramétrées.

---

## 5. Exécution des règles

- Les règles sont exécutées automatiquement après import des données.
- Une réexécution est déclenchée après modification d’écritures.
- L’exécution porte sur l’ensemble des données du dossier.

Chaque anomalie détectée contient :
- La référence de l’écriture ou de l’entité concernée.
- La règle déclenchée.
- Le niveau de gravité.
- Un message explicatif structuré.

---

## 6. Restitution des résultats

L’utilisateur doit pouvoir :

- Consulter la liste des anomalies par dossier.
- Filtrer par catégorie ou gravité.
- Identifier les écritures concernées.
- Visualiser un indicateur global de risque (basé sur la gravité et le volume d’anomalies).

Les anomalies critiques peuvent bloquer la progression du workflow tant qu’elles ne sont pas traitées ou validées.

---

## 7. Règles de gestion

- Une règle inactive n’est jamais exécutée.
- Toute modification de règle est tracée (audit).
- Les anomalies sont générées à chaque exécution et historisées.
- Le moteur ne modifie jamais automatiquement les écritures.
- Toute anomalie critique doit être traitée ou validée avant validation finale par l’expert.

---

## 8. Critères d’acceptation

- Il est possible de créer, modifier, activer et désactiver une règle.
- Après import d’un fichier (~5000 lignes), les règles actives sont exécutées automatiquement.
- Les anomalies détectées sont visibles et correctement associées aux écritures.
- Une modification d’écriture déclenche une nouvelle exécution.
- Les anomalies critiques empêchent le passage à l’étape finale du workflow.
- Toutes les actions de configuration sont tracées dans le journal d’audit.

---

# technicalSpec

## 1. Composants impactés

- Service "RuleEngine" (cœur d’exécution des règles).
- Base de données (entités Rule, RuleExecution, Anomaly).
- Module Import (déclencheur post-import).
- Module Gestion des écritures (déclencheur post-modification).
- Module Workflow (vérification des anomalies critiques).
- Module Journal d’audit (traçabilité).
- API interne exposée au module IA.

---

## 2. Modèle de données

### 2.1 Entité Rule
- id
- name
- description
- category (TVA / Delais / General)
- severity (Critique / Majeur / Mineur)
- isActive
- parameters (JSON structuré)
- createdAt / updatedAt

### 2.2 Entité RuleExecution
- id
- dossierId
- executionDate
- rulesVersion

### 2.3 Entité Anomaly
- id
- dossierId
- ruleId
- entityReference (id écriture ou facture)
- severity
- status
- message
- createdAt
- updatedAt

---

## 3. Mécanisme d’exécution

- Déclenchement automatique via événements :
  - Fin d’import.
  - Modification d’écriture.
- Exécution batch sur environ 5000 lignes.
- Isolation stricte par dossier.
- Traitement optimisé pour rester compatible avec un usage interactif.

Les règles sont interprétées dynamiquement à partir des paramètres stockés (pas de codage en dur pour les règles configurables).

---

## 4. Paramétrage technique des règles

- Les paramètres sont stockés en format structuré (ex : JSON).
- Les conditions doivent être basées uniquement sur les champs standardisés issus de l’import.
- Validation côté serveur lors de la création ou modification d’une règle.
- Versionnement implicite via horodatage et enregistrement des exécutions.

---

## 5. Intégrations

### 5.1 Module Workflow
- API interne permettant de vérifier la présence d’anomalies critiques ouvertes avant changement d’état.

### 5.2 Assistant IA
- Endpoint interne exposant :
  - Liste des anomalies.
  - Métadonnées de la règle (description, catégorie, gravité).

### 5.3 Journal d’audit
- Log des actions suivantes :
  - Création / modification / suppression logique de règle.
  - Activation / désactivation.
  - Exécution des règles.

---

## 6. Contraintes techniques

- Isolation des données par cabinet (multi-tenant).
- Aucune modification automatique des données sources.
- Respect des exigences de confidentialité.
- Architecture extensible pour ajouter de nouvelles catégories de règles.

---

## 7. Performance et évolutivité

- Optimisation pour traitement de 5000 lignes par dossier.
- Conception extensible pour intégrer ultérieurement d’autres règles fiscales.
- Possibilité d’ajouter de nouveaux types de paramètres sans refonte du moteur.

---