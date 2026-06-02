# Ticket : Implémenter le moteur de règles paramétrables

## 1. Objectif du ticket

Mettre en place le cœur du moteur de règles paramétrables permettant :

- De définir des règles métier configurables.
- De les activer ou désactiver au niveau du cabinet.
- De les exécuter automatiquement sur les données d’un dossier.
- De générer des anomalies structurées à partir des règles déclenchées.

Ce ticket couvre l’implémentation du moteur d’évaluation et de son cycle d’exécution, conformément aux spécifications du module « Moteur de règles & Anomalies ».

---

## 2. Périmètre fonctionnel couvert

Le moteur doit permettre :

1. La prise en compte des règles actives d’un cabinet.
2. L’évaluation des règles sur les données d’un dossier (écritures, factures, balances).
3. La génération d’anomalies associées à une règle et à une entité cible.
4. La gestion du cycle d’exécution (création d’un historique d’exécution).
5. La mise à jour des anomalies existantes lors d’une réexécution.

Ne sont pas couverts dans ce ticket :
- L’interface utilisateur complète d’administration (hors besoins minimaux d’intégration).
- Le traitement détaillé des statuts d’anomalies (géré par une fonctionnalité dédiée).
- Le scoring global du dossier.

---

## 3. Fonctionnement attendu du moteur

### 3.1 Chargement des règles

Lors d’une exécution sur un dossier :

- Le moteur charge toutes les règles actives associées au cabinet.
- Les règles sont filtrées par catégorie si nécessaire (TVA, Délais, General).
- Chaque règle est évaluée dans sa version courante.

Une règle inactive ne doit jamais être exécutée.

---

### 3.2 Évaluation des règles

Pour chaque règle active :

- Le moteur parcourt les entités concernées du dossier.
- Il applique les conditions définies dans l’expression de la règle.
- Si la condition est vérifiée, une anomalie est générée.

L’évaluation doit :

- Se baser uniquement sur les champs standardisés issus de l’import.
- Respecter les paramètres configurés (seuils, tolérances, conditions logiques).
- Ne jamais modifier les données sources.

---

### 3.3 Génération des anomalies

Lorsqu’une règle est déclenchée :

Une anomalie est créée avec :

- La référence du dossier.
- La règle concernée (id + version).
- L’entité ciblée (type + identifiant).
- Le niveau de gravité défini par la règle.
- Un message explicatif structuré.
- Un statut initial : « Ouverte ».

Les anomalies doivent être historisées et liées à une exécution donnée.

---

### 3.4 Réexécution et mise à jour

En cas de nouvelle exécution (après import ou modification d’écriture) :

- Une nouvelle entrée d’exécution est créée.
- Les anomalies sont recalculées.
- Les anomalies qui ne sont plus valides doivent être clôturées automatiquement (selon logique définie au niveau technique).
- Les nouvelles anomalies sont créées.

Les anomalies historiques ne doivent pas être supprimées.

---

## 4. Déclenchement du moteur

Le moteur doit pouvoir être déclenché :

- Automatiquement après import de données.
- Automatiquement après modification d’écriture.
- Manuellement via un appel applicatif interne.

Chaque exécution doit être traçable.

---

## 5. Contraintes fonctionnelles

- Isolation stricte par cabinet et par dossier.
- Aucune correction automatique des écritures.
- Traçabilité complète des règles utilisées lors de chaque exécution.
- Compatibilité avec un volume cible d’environ 5000 lignes par dossier.

---

## 6. Résultat attendu

À l’issue de ce ticket, le système dispose d’un moteur opérationnel capable :

- D’interpréter des règles paramétrées en base.
- D’exécuter ces règles sur un dossier.
- De produire des anomalies structurées et traçables.
- De s’intégrer avec les modules Import, Workflow, Journal d’audit et Assistant IA via les entités persistées.

Ce moteur constitue la brique centrale de détection d’anomalies dans Comptalyses.