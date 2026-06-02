# Ticket : Générer le brouillon TVA exportable DGI

## 1. Objectif du ticket

Mettre en œuvre le mécanisme permettant :

- De calculer les agrégats TVA pour une période donnée (mensuelle ou trimestrielle).
- De générer un brouillon structuré de déclaration basé sur ces agrégats.
- De produire un fichier exportable compatible avec le format requis par la DGI marocaine.

Ce ticket couvre la chaîne complète : calcul → structuration → génération du fichier d’export.

---

## 2. Périmètre fonctionnel

### 2.1 Calcul des agrégats TVA

Pour une période sélectionnée dans un dossier :

Le système doit :

- Charger les écritures comptables prises en compte pour la période.
- Consolider les montants de :
  - TVA collectée
  - TVA déductible
- Calculer le solde de TVA pour la période.
- Produire une structure d’agrégats cohérente avec le régime configuré (mensuel ou trimestriel).

Les agrégats doivent être strictement basés sur les écritures appartenant à la période sélectionnée.

---

### 2.2 Génération du brouillon structuré

À partir des agrégats calculés :

- Le système construit une structure déclarative conforme aux champs requis par la déclaration TVA marocaine.
- Les montants sont automatiquement mappés vers les champs correspondants.
- La structure produite représente un snapshot figé de la déclaration à l’instant de génération.

Le brouillon doit être exploitable pour :

- Consultation interne dans l’application.
- Génération d’un fichier d’export.

---

### 2.3 Production du fichier exportable DGI

Le système doit :

- Générer un fichier structuré conforme au format attendu par le portail de la DGI marocaine.
- S’appuyer exclusivement sur le snapshot du brouillon généré (et non recalculer dynamiquement lors de l’export).
- Garantir que les montants du fichier correspondent exactement aux agrégats validés au moment de la génération.

Le module ne gère pas l’envoi au portail DGI ; il produit uniquement un fichier prêt à dépôt.

---

### 2.4 Cohérence avec le workflow

La génération du brouillon peut être déclenchée par les rôles autorisés (Collaborateur, Chargé, Expert).

L’export du fichier doit respecter les règles de workflow définies au niveau du module (statut compatible avec export).

Toutes les actions suivantes doivent être tracées :

- Calcul des agrégats.
- Génération du brouillon.
- Génération du fichier d’export.

---

## 3. Règles de gestion

- Les agrégats sont recalculés automatiquement si les écritures de la période ont été modifiées avant génération.
- Le régime (mensuel ou trimestriel) est déterminé au niveau du dossier et ne peut être modifié lors de la génération.
- Le fichier exporté correspond à une version précise du brouillon.
- Aucune modification directe du fichier exporté n’est possible via l’application.
- Toutes les opérations doivent être historisées dans le journal d’audit.

---

## 4. Dépendances

Ce ticket dépend :

- Du moteur de règles pour l’état des anomalies.
- Du module TVA pour l’accès aux écritures et à la période.
- Du module Workflow pour la gestion des statuts.
- Du module Audit pour la traçabilité.