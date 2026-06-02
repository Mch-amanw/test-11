Spécification Technique

## 1. Positionnement dans l’architecture

Le module TVA & Déclarations est un module métier central interagissant avec :

- Le module d’import (source des écritures comptables).
- Le moteur de règles (détection d’anomalies).
- Le module Gestion des écritures (corrections).
- Le module Workflow (validation).
- Le module Audit (journalisation).

Il fonctionne dans l’architecture SaaS multi-clients avec isolation logique des données par cabinet.

---

## 2. Modèle de données (conceptuel)

Entités principales :

- Dossier
- Période (mensuelle/trimestrielle)
- Écriture comptable
- Anomalie TVA
- Agrégat TVA
- Déclaration TVA (brouillon)
- Validation (avec rôle et horodatage)

Relations :
- Une période appartient à un dossier.
- Une déclaration est liée à une période.
- Les anomalies sont liées aux écritures et à la période.
- Les validations sont liées à la déclaration.

---

## 3. Traitement et calculs

### 3.1 Pipeline de calcul

1. Chargement des écritures validées pour la période.
2. Application des règles TVA via le moteur de règles.
3. Génération des anomalies.
4. Calcul des agrégats (TVA collectée, déductible, solde).
5. Génération du brouillon structuré.

Chaque étape est idempotente et recalculable.

---

### 3.2 Mécanisme de recalcul

- Recalcul automatique déclenché après :
  - Modification d’écriture.
  - Ajout ou suppression d’écriture.
  - Modification de règle applicable.
- Les versions successives du brouillon sont historisées.

---

## 4. Génération du brouillon et export DGI

- Génération d’une structure de déclaration conforme aux champs requis par la DGI marocaine.
- Mapping entre agrégats internes et champs déclaratifs.
- Export dans un format compatible avec le portail DGI (structure prédéfinie).

Le module ne réalise pas l’envoi direct mais prépare un fichier prêt à dépôt.

---

## 5. Sécurité et contrôle d’accès

- Accès conditionné par rôle (Collaborateur, Chargé, Expert).
- Seul le rôle Expert peut changer le statut en "validé".
- Vérification systématique des permissions côté backend.
- Isolation stricte des données par cabinet.

---

## 6. Journalisation et audit

Le module enregistre :

- Génération de brouillon.
- Modifications des écritures impactant la TVA.
- Recalculs.
- Changements de statut.
- Export de déclaration.

Chaque événement inclut :
- Identifiant utilisateur.
- Rôle.
- Horodatage.
- Référence du dossier et de la période.

---

## 7. Contraintes techniques

- Traitement performant pour environ 5000 lignes par dossier.
- Recalcul optimisé pour éviter une latence excessive.
- Conformité stricte à la réglementation fiscale marocaine.
- Architecture évolutive pour intégrer d’autres types de déclarations fiscales.

---