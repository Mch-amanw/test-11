# Spécification technique – Automatiser les contrôles TVA

## 1. Positionnement technique

Ce ticket implémente la couche backend de contrôle TVA dans le module **TVA & Déclarations**, en s’appuyant sur :

- Les écritures issues du module d’import.
- Le moteur de règles pour la configuration des seuils, taux autorisés et niveaux de criticité.
- Le modèle d’entité "Anomalie TVA".
- Le module Workflow pour le blocage en cas d’anomalies critiques.

Le traitement est exécuté côté backend uniquement.

---

## 2. Pipeline de traitement

Pour une période donnée :

1. Charger les écritures validées appartenant au dossier et à la période.
2. Appliquer les contrôles de cohérence HT/TVA/TTC.
3. Appliquer les contrôles de taux.
4. Appliquer les règles de conformité issues du moteur de règles.
5. Générer, mettre à jour ou clôturer les anomalies correspondantes.

Le pipeline est :
- Idempotent (exécutable plusieurs fois sans duplication d’anomalies).
- Déclenché lors d’un recalcul de la période.

---

## 3. Implémentation des contrôles

### 3.1 Contrôle HT / TVA / TTC

Pour chaque écriture :

- Vérifier la présence des champs nécessaires (HT, TVA, TTC, taux).
- Calculer :
  - `expectedTva = HT × taux`
  - `expectedTtc = HT + TVA`
- Comparer avec les montants fournis en tenant compte d’un seuil d’écart issu du moteur de règles.

En cas d’écart supérieur au seuil :
- Création ou mise à jour d’une anomalie avec :
  - typeAnomalie = "calcul" ou "donneesManquantes"
  - criticite issue de la règle associée.

---

### 3.2 Contrôle des taux

- Récupération des taux autorisés depuis le moteur de règles.
- Vérification que le taux de l’écriture appartient à l’ensemble autorisé.
- Vérification de cohérence entre taux et montant TVA.

En cas d’incohérence :
- Création ou mise à jour d’une anomalie avec :
  - typeAnomalie = "taux".

---

### 3.3 Règles de conformité

- Exécution des règles paramétrées dans le moteur de règles.
- Chaque règle renvoie :
  - condition satisfaite (true/false),
  - description,
  - criticité.

Si condition satisfaite :
- Création ou mise à jour d’une anomalie avec :
  - typeAnomalie = "conformite".

---

## 4. Gestion des anomalies

### 4.1 Idempotence

Pour éviter les doublons :

- Identifier une anomalie par un couple unique :
  - ecritureId
  - typeAnomalie
  - règle associée (si applicable)

Lors d’un recalcul :
- Si l’anomalie existe et la condition persiste → mise à jour.
- Si l’anomalie existe mais la condition disparaît → statut = "corrigee" ou équivalent.
- Si nouvelle condition → création.

Aucune suppression physique en base.

---

## 5. Déclenchement du recalcul

Le module doit déclencher les contrôles automatiquement en cas de :

- Modification d’une écriture impactant HT, TVA, TTC ou taux.
- Ajout ou suppression d’écriture.
- Modification d’une règle TVA applicable.

Le recalcul peut être :
- Total sur la période.
- Partiel si seule une écriture est modifiée (optimisation recommandée).

---

## 6. Intégration Workflow

Lors d’un changement de statut vers "validé expert" :

- Le backend vérifie l’absence d’anomalies avec criticite = "bloquante" et statut = "ouverte".
- Si présentes, la transition est refusée.

---

## 7. Performance

- Optimisé pour environ 5000 écritures par dossier.
- Traitement batch côté backend.
- Indexation recommandée sur :
  - periodeId
  - ecritureId
  - typeAnomalie

---

## 8. Sécurité et audit

- Vérification des permissions utilisateur avant déclenchement manuel éventuel.
- Journalisation :
  - Lancement du recalcul.
  - Nombre d’anomalies générées/mises à jour.
  - Utilisateur et horodatage.

- Respect strict de l’isolation des données par cabinet.

---

## 9. Contraintes

- Tous les calculs critiques sont réalisés côté backend.
- Les règles doivent rester configurables via le moteur de règles.
- Le système doit rester conforme à la réglementation fiscale marocaine telle que paramétrée dans les règles.