# Spécification Technique – Implémentation du moteur de règles paramétrables

## 1. Composant à implémenter

Création du composant central : `RuleExecutionEngine`.

Il est responsable de :

- Charger les règles actives d’un cabinet.
- Évaluer les règles sur un dossier.
- Générer les anomalies.
- Enregistrer l’exécution.

Architecture cible :

- Couche Service métier
- Couche Moteur d’évaluation
- Couche Persistance

---

## 2. Interfaces principales

### 2.1 Service principal

`RuleExecutionEngine`

Méthode principale :

executeRules(dossierId: UUID, triggeredBy: TriggerType): RuleExecutionResult

Responsabilités :
- Vérifier l’existence du dossier.
- Charger les règles actives (par cabinetId).
- Créer une entité RuleExecution.
- Évaluer chaque règle.
- Persister les anomalies.
- Retourner un résumé (nombre d’anomalies par gravité, durée d’exécution).

---

## 3. Chargement des données

Pour un dossier donné :

- Charger les écritures normalisées.
- Charger les factures et balances si nécessaires.
- Charger les conventions analysées (si règles Délais).

Toutes les données doivent être isolées par cabinetId.

Optimisation :
- Chargement en mémoire contrôlé.
- Indexation sur dossierId.

---

## 4. Interprétation des règles

### 4.1 Structure attendue

Chaque règle contient :

- parameters (JSON)
- expressionDefinition (JSON logique ou DSL interne)

Le moteur doit :

- Parser l’expression.
- Résoudre dynamiquement les champs référencés.
- Évaluer les opérateurs supportés (=, !=, >, <, >=, <=).
- Supporter une logique AND / OR.

Les champs utilisables doivent être validés contre une whitelist (htAmount, tvaAmount, ttcAmount, rate, dueDate, invoiceDate, accountNumber, etc.).

Aucune exécution de code dynamique arbitraire n’est autorisée.

---

## 5. Génération et mise à jour des anomalies

### 5.1 Création

Pour chaque entité déclenchant la règle :

- Créer une entité Anomaly.
- Renseigner ruleId + ruleVersion.
- status = Ouverte.
- severity héritée de la règle.

### 5.2 Réexécution

Stratégie recommandée :

- Identifier les anomalies précédentes liées au dossier.
- Marquer comme clôturées automatiquement celles dont la condition n’est plus vérifiée.
- Conserver l’historique (ne pas supprimer physiquement).

Lien entre Anomaly et RuleExecution via ruleVersion et timestamps.

---

## 6. Entité RuleExecution

À chaque exécution :

- Création d’une entrée RuleExecution.
- Champs : dossierId, executionTimestamp, triggeredBy, executionDurationMs, ruleSetVersion.

Le champ ruleSetVersion correspond à la version globale des règles au moment de l’exécution.

---

## 7. Déclencheurs

Le moteur doit être invoqué par :

- Événement post-import.
- Événement post-modification d’écriture.
- Endpoint interne POST /dossiers/{id}/rules/execute.

Les appels doivent être sécurisés par contrôle de rôle.

---

## 8. Performance

Contraintes :

- Traitement d’environ 5000 lignes.
- Temps cible < 5 secondes.

Optimisations possibles :

- Précompilation des expressions.
- Évaluation en mémoire.
- Réduction des accès base pendant boucle d’évaluation.

---

## 9. Sécurité

- Isolation stricte par cabinetId.
- Validation des champs autorisés dans les expressions.
- Journalisation des exécutions.
- Aucune modification des données sources.

---

## 10. Intégration inter-modules

Le moteur doit produire des données exploitables par :

- Workflow (requête anomalies critiques ouvertes).
- Module TVA (filtrage catégorie TVA).
- Assistant IA (accès anomalies + métadonnées règle).
- Journal d’audit (log exécution).

---

## 11. Livrable technique attendu

À l’issue de ce ticket :

- Le service RuleExecutionEngine est implémenté.
- Les règles paramétrées en base peuvent être interprétées.
- Les anomalies sont générées et persistées.
- Les exécutions sont historisées.
- Le moteur est intégrable avec les autres modules via API interne.

Ce composant constitue le noyau technique du module « Moteur de règles & Anomalies ».