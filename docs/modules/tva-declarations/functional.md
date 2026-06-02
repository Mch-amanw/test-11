# Module : TVA & Déclarations

## 1. Rôle du module dans le projet

Le module **TVA & Déclarations** est le cœur métier de Comptalyses. Il a pour objectif de :

- Contrôler et fiabiliser les données liées à la TVA issues des fichiers Excel exportés depuis Sage.
- Détecter automatiquement les anomalies de calcul ou de conformité réglementaire marocaine.
- Consolider les montants de TVA collectée et déductible.
- Générer un brouillon de déclaration de TVA (mensuelle ou trimestrielle) compatible avec les exigences de la DGI marocaine.
- Intégrer le processus de validation obligatoire via le workflow (Collaborateur → Chargé → Expert).

Ce module s’appuie sur le moteur de règles pour la détection des anomalies et sur le module Workflow pour la validation finale.

---

## 2. Périmètre fonctionnel

### 2.1 Analyse et contrôle des écritures TVA

À partir des écritures importées :

#### Vérifications automatiques
- Cohérence des montants HT / TVA / TTC.
- Vérification du calcul de la TVA (montant = base × taux).
- Vérification de la cohérence des taux appliqués.
- Identification des écritures comportant des montants manquants ou incohérents.

#### Contrôle de conformité réglementaire
- Vérification de la cohérence globale des montants au regard des règles fiscales marocaines.
- Identification des écarts susceptibles d’impacter la déclaration.

Les anomalies détectées sont classées par type et niveau de risque.

---

### 2.2 Détection d’anomalies via moteur de règles

Le module exploite le moteur de règles paramétrables pour :

- Détecter des incohérences spécifiques liées à la TVA.
- Identifier des taux incorrects.
- Repérer des écarts entre écritures similaires.
- Signaler toute situation définie comme anomalie dans la configuration.

Chaque anomalie comporte :
- Une description explicite.
- Les écritures concernées.
- Un niveau de criticité.

---

### 2.3 Scoring de fiabilité TVA

Le module attribue un **score de fiabilité TVA** au dossier sur la période analysée.

Ce score reflète notamment :
- Le nombre d’anomalies détectées.
- Leur niveau de criticité.
- La cohérence globale des calculs.

Ce score aide à prioriser les contrôles avant validation experte.

---

### 2.4 Consolidation des montants TVA

Pour une période donnée (mensuelle ou trimestrielle) :

- Agrégation de la TVA collectée.
- Agrégation de la TVA déductible.
- Calcul du solde de TVA.
- Mise en évidence des ajustements ou corrections éventuelles.

Les agrégats sont consultables via une interface synthétique avec possibilité de détail par écriture.

---

### 2.5 Génération du brouillon de déclaration

Le module génère un **brouillon structuré de déclaration de TVA** conforme aux exigences marocaines :

- Adapté au régime mensuel ou trimestriel.
- Pré-remplissage automatique des champs à partir des agrégats calculés.
- Mise en évidence des incohérences bloquantes.

Le brouillon peut être :
- Consulté à l’écran.
- Exporté dans un format compatible avec le portail de la DGI marocaine.

---

### 2.6 Intégration au workflow

- Le Collaborateur prépare et corrige les anomalies.
- Le Chargé valide la cohérence du dossier.
- L’Expert valide définitivement la déclaration.

Règle impérative :
- Aucune déclaration ne peut être considérée comme prête à l’envoi sans validation explicite par un Expert.

Toutes les actions (génération, modification, validation, export) sont tracées dans le journal d’audit.

---

### 2.7 Gestion des corrections

Le module permet :
- La visualisation des écritures impactant la déclaration.
- La proposition d’écritures correctives.
- La mise à jour des agrégats après correction.
- L’export des corrections vers Sage.

Chaque modification est historisée.

---

## 3. Règles de gestion spécifiques

- La déclaration est basée uniquement sur les écritures validées dans la période sélectionnée.
- Toute anomalie critique doit être traitée ou explicitement validée avant passage au statut "validé expert".
- Le calcul des agrégats est recalculé automatiquement après chaque modification d’écriture.
- Le régime (mensuel ou trimestriel) est défini au niveau du dossier.
- Toutes les étapes doivent être traçables pour audit.

---

# technicalSpec

# Module : TVA & Déclarations –