# Spécification technique : Contrôle et fiabilisation TVA

## 1. Positionnement dans l’architecture

La fonctionnalité est intégrée au module **TVA & Déclarations** et interagit avec :

- Le module d’import (source des écritures).
- Le moteur de règles (définition et paramétrage des contrôles).
- Le module Gestion des écritures (corrections).
- Le module Workflow (blocage en cas d’anomalies critiques).
- Le module Audit (journalisation des événements).

Elle s’insère dans le pipeline de calcul avant la consolidation des agrégats TVA.

---

## 2. Données utilisées

Entrées principales :

- Écritures comptables (HT, TVA, TTC, taux, date, période).
- Paramètres issus du moteur de règles (règles de calcul, seuils, criticité).

Sorties :

- Entités "Anomalie TVA" liées à :
  - Une écriture.
  - Une période.
  - Un type d’anomalie.
  - Un niveau de criticité.

---

## 3. Mécanisme de contrôle

### 3.1 Pipeline de traitement

1. Chargement des écritures de la période.
2. Application des règles de cohérence HT/TVA/TTC.
3. Vérification du calcul TVA = HT × taux.
4. Application des règles de conformité issues du moteur de règles.
5. Génération ou mise à jour des anomalies.

Le traitement est :
- Idempotent (rejouable sans effet de bord).
- Déclenché automatiquement lors d’un recalcul.

---

### 3.2 Recalcul automatique

Un recalcul est déclenché en cas de :

- Modification d’une écriture impactant la TVA.
- Ajout ou suppression d’écriture.
- Modification d’une règle TVA applicable.

Les anomalies précédentes sont :
- Soit mises à jour si toujours valides.
- Soit clôturées si la condition n’est plus vérifiée.

---

## 4. Modèle d’anomalie (conceptuel)

Champs principaux :

- id
- dossierId
- periodeId
- ecritureId
- typeAnomalie (calcul, taux, conformité, données manquantes)
- criticite (bloquante, majeure, mineure)
- description
- statut (ouverte, corrigée, validée)
- createdAt / updatedAt
- userAction (si validée manuellement)

Les anomalies sont historisées et ne sont jamais supprimées physiquement (traçabilité).

---

## 5. Intégration avec le workflow

- Lors du passage au statut "validé expert", le backend vérifie l’absence d’anomalies critiques ouvertes.
- Si des anomalies bloquantes existent, la transition est refusée.
- Toute validation manuelle d’anomalie est tracée avec utilisateur, rôle et horodatage.

---

## 6. Performance

- Optimisation pour traitement d’environ 5000 lignes par dossier.
- Calculs effectués côté backend.
- Recalcul partiel privilégié lorsque seules certaines écritures sont modifiées.

---

## 7. Sécurité et confidentialité

- Vérification systématique des permissions côté backend.
- Isolation stricte des données par cabinet.
- Journalisation des contrôles et actions utilisateurs.

---

## 8. Contraintes

- Conformité stricte à la réglementation fiscale marocaine.
- Paramétrage flexible via moteur de règles pour anticiper les évolutions légales.
- Aucun calcul critique ne doit dépendre exclusivement du frontend.