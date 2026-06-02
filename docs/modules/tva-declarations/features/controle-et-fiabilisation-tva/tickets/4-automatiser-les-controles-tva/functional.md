# Ticket : Automatiser les contrôles TVA

## 1. Objectif du ticket

Mettre en œuvre l’automatisation des contrôles TVA sur les écritures comptables importées afin de :

- Vérifier la cohérence des montants HT / TVA / TTC.
- Contrôler l’exactitude des taux de TVA appliqués.
- Détecter les cas de non-conformité potentielle au regard des règles fiscales marocaines configurées.
- Générer automatiquement des anomalies structurées exploitables dans le module TVA & Déclarations.

Ce ticket couvre l’implémentation effective des contrôles décrits dans la fonctionnalité « Contrôle et fiabilisation TVA ».

---

## 2. Périmètre fonctionnel

Le traitement s’applique à toutes les écritures comptables de la période sélectionnée (mensuelle ou trimestrielle) appartenant à un dossier.

### 2.1 Contrôle de cohérence HT / TVA / TTC

Pour chaque écriture comportant des montants :

- Vérification que :
  - `TTC = HT + TVA`
  - `TVA = HT × taux`
- Prise en compte d’un éventuel seuil d’écart toléré si défini dans le moteur de règles.
- Détection des cas suivants :
  - Montant HT, TVA ou TTC manquant.
  - Écart de calcul supérieur au seuil autorisé.
  - Arrondi anormal.

En cas d’écart, une anomalie de type « calcul » ou « données manquantes » est générée.

---

### 2.2 Contrôle des taux de TVA

Pour chaque écriture soumise à TVA :

- Vérification de la présence d’un taux lorsque requis.
- Vérification que le taux appliqué correspond à un taux autorisé selon la configuration issue du moteur de règles.
- Vérification de la cohérence entre le taux déclaré et le montant de TVA calculé.

En cas d’incohérence :
- Génération d’une anomalie de type « taux ».

---

### 2.3 Contrôle de conformité réglementaire (basé sur règles)

Application des règles paramétrées dans le moteur de règles pour :

- Identifier des situations définies comme non conformes.
- Détecter des combinaisons de données incohérentes (ex. taux inattendu sur certaines écritures selon configuration).
- Signaler des cas à risque pour la déclaration.

Chaque règle déclenchée génère une anomalie de type « conformité » avec un niveau de criticité défini par la règle.

---

### 2.4 Génération et mise à jour des anomalies

Pour chaque anomalie détectée :

- Création ou mise à jour d’une entité Anomalie TVA liée à :
  - L’écriture concernée.
  - La période.
  - Le dossier.
- Attribution automatique d’un niveau de criticité selon la règle correspondante.
- Rédaction d’une description explicite compréhensible par un collaborateur comptable.

En cas de correction ultérieure de l’écriture :
- L’anomalie est automatiquement recalculée.
- Si la condition n’est plus vérifiée, l’anomalie est clôturée (statut mis à jour, sans suppression physique).

---

## 3. Intégration au processus global

- Les contrôles sont exécutés automatiquement lors du calcul ou recalcul de la période.
- Les anomalies critiques détectées peuvent bloquer la validation experte via le module Workflow.
- Toutes les détections et mises à jour d’anomalies sont tracées dans le journal d’audit.

---

## 4. Hors périmètre du ticket

- Paramétrage avancé du moteur de règles (supposé existant).
- Interface détaillée de gestion des anomalies (couverte par d’autres tickets).
- Envoi de la déclaration à la DGI.

Ce ticket porte exclusivement sur l’automatisation backend des contrôles TVA et la génération des anomalies associées.