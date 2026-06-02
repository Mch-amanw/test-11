## Spécification Technique – Détection d’anomalies paramétrable

### 1. Positionnement
La fonctionnalité de Détection d’anomalies paramétrable est une capacité opérationnelle du module « Moteur de règles & Anomalies ». Elle permet la configuration, l’exécution et la gestion technique des règles métier appliquées aux données comptables importées.

Elle s’appuie sur :
- Les données issues du module d’import (écritures, factures, balances âgées)
- Les conventions contractuelles analysées
- Le référentiel TVA marocain

---

### 2. Architecture technique

#### 2.1 Composants
- RuleConfigurationService : gestion CRUD des règles
- RuleExecutionEngine : moteur d’évaluation des règles
- AnomalyPersistenceService : persistance et mise à jour des anomalies
- RuleVersioningService : gestion du versionnement
- RuleApiController : exposition API interne pour UI et autres modules

Architecture logique :
- Couche API
- Couche Services métier
- Couche moteur d’évaluation
- Couche persistance (base de données)

---

### 3. Modèle de données

#### 3.1 Entité Rule
- id (UUID)
- name (string)
- code (string unique)
- category (enum: TVA, Delais, General)
- severity (enum: Critique, Majeur, Mineur)
- isActive (boolean)
- parameters (JSON)
- expressionDefinition (JSON ou DSL interne)
- createdAt (datetime)
- updatedAt (datetime)
- version (integer)
- cabinetId (UUID)

#### 3.2 Entité RuleExecution
- id (UUID)
- dossierId (UUID)
- executionTimestamp (datetime)
- ruleSetVersion (integer)
- triggeredBy (enum: Import, ManualRerun, AfterCorrection)
- executionDurationMs (integer)

#### 3.3 Entité Anomaly
- id (UUID)
- dossierId (UUID)
- ruleId (UUID)
- ruleVersion (integer)
- entityType (enum: Ecriture, Facture, Balance)
- entityId (string)
- severity (enum)
- status (enum: Ouverte, Corrigee, Justifiee, Validee)
- message (string)
- metadata (JSON)
- createdAt (datetime)
- updatedAt (datetime)
- resolvedBy (UUID nullable)

Indexation obligatoire sur :
- dossierId
- ruleId
- status
- severity

---

### 4. Mécanisme d’évaluation des règles

#### 4.1 Format des règles
Les règles sont définies via :
- Paramètres configurables (seuils, taux, tolérances)
- Expression logique structurée (DSL interne ou JSON logique)

Exemple simplifié :
{
  "conditions": [
    { "field": "tvaAmount", "operator": "!=", "formula": "htAmount * rate" },
    { "field": "difference", "operator": ">", "value": 2 }
  ],
  "logic": "AND"
}

Les champs utilisables sont limités à un référentiel standardisé (htAmount, tvaAmount, ttcAmount, dueDate, invoiceDate, rate, accountNumber, etc.).

---

### 5. Déclenchement

#### 5.1 Déclenchement automatique
- Après import d’un fichier
- Après modification d’écritures
- Après modification des paramètres d’une règle

#### 5.2 Déclenchement manuel
- Bouton "Relancer les contrôles" accessible selon rôle

Chaque exécution génère une entrée RuleExecution.

---

### 6. Logique d’exécution

- Chargement des règles actives pour le cabinet
- Isolation par dossier
- Exécution par catégorie ou en pipeline complet
- Traitement optimisé pour environ 5000 lignes
- Évaluation en mémoire avec itération contrôlée

Contraintes :
- Temps cible < 5 secondes pour 5000 lignes
- Aucune modification des données sources

Les anomalies existantes sont :
- Recalculées si la règle est modifiée
- Fermées automatiquement si la condition n’est plus vérifiée

---

### 7. Gestion du versionnement

Chaque modification d’une règle :
- Incrémente le champ version
- Ne modifie pas les anomalies historiques
- Permet traçabilité via ruleVersion dans Anomaly

Les exécutions stockent la versionRules utilisée.

---

### 8. API interne

#### 8.1 Gestion des règles
- POST /rules
- PUT /rules/{id}
- GET /rules
- PATCH /rules/{id}/activate

#### 8.2 Exécution
- POST /dossiers/{id}/rules/execute
- GET /dossiers/{id}/anomalies

#### 8.3 Gestion anomalies
- PATCH /anomalies/{id}/status
- GET /anomalies?severity=&status=

Accès contrôlé par rôle.

---

### 9. Intégration inter-modules

- Workflow : blocage si anomalies critiques ouvertes
- Module TVA : récupération anomalies catégorie TVA
- Module Délais : règles spécifiques échéances
- Assistant IA : accès aux anomalies + métadonnées + expression rule
- Journal d’audit : journalisation des créations, modifications, changements de statut

---

### 10. Sécurité

- Isolation stricte par cabinetId
- Vérification des permissions avant exécution
- Journalisation des accès et modifications
- Aucune exposition directe des expressions internes aux utilisateurs non autorisés

---

### 11. Scalabilité et évolutivité

- Architecture extensible pour nouvelles catégories fiscales
- Ajout de nouveaux opérateurs logiques sans refonte majeure
- Support futur de règles basées sur données historiques

---

## Conclusion technique

La Détection d’anomalies paramétrable repose sur un moteur d’évaluation interprétant des règles configurables, versionnées et isolées par cabinet. Elle garantit performance, traçabilité, conformité réglementaire marocaine et intégration complète avec le workflow et l’assistant IA, tout en respectant les exigences strictes de sécurité et de confidentialité du projet Comptalyses.