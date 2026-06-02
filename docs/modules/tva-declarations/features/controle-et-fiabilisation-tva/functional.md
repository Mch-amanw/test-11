# Fonctionnalité : Contrôle et fiabilisation TVA

## 1. Objectif métier

La fonctionnalité **Contrôle et fiabilisation TVA** vise à analyser automatiquement les écritures comptables importées afin de :

- Vérifier la cohérence des montants HT / TVA / TTC.
- Contrôler l’exactitude des taux de TVA appliqués.
- Identifier les non-conformités potentielles au regard de la réglementation fiscale marocaine.
- Réduire les risques d’erreurs avant la consolidation et la génération du brouillon de déclaration.

Elle constitue la première étape critique du processus de préparation de la déclaration de TVA.

---

## 2. Périmètre fonctionnel

La fonctionnalité s’applique à toutes les écritures de la période sélectionnée (mensuelle ou trimestrielle) issues du module d’import.

### 2.1 Vérification des montants HT / TVA / TTC

Pour chaque écriture concernée :

- Vérification que :
  - `TTC = HT + TVA` (dans la limite d’un écart toléré paramétrable si défini par le moteur de règles).
  - `TVA = HT × taux`.
- Identification des écritures comportant :
  - Montants manquants (HT, TVA ou TTC).
  - Incohérences de calcul.
  - Arrondis anormaux.

Toute incohérence génère une anomalie typée.

---

### 2.2 Vérification des taux de TVA

- Contrôle de la présence d’un taux de TVA lorsque requis.
- Vérification de la cohérence entre le taux appliqué et le montant de TVA calculé.
- Détection des taux incorrects ou incohérents par rapport aux règles configurées.

Les taux applicables sont gérés via le moteur de règles paramétrables, afin de rester conformes à la réglementation marocaine en vigueur.

---

### 2.3 Contrôle de conformité réglementaire marocaine

La fonctionnalité permet :

- D’identifier les écritures susceptibles d’être non conformes aux exigences fiscales marocaines.
- De signaler les cas à risque pouvant impacter la déclaration.
- De classer les anomalies par niveau de criticité (bloquante, majeure, mineure) selon la configuration du moteur de règles.

La fonctionnalité ne prend pas de décision automatique sur la validité fiscale finale : elle assiste l’utilisateur dans l’analyse.

---

### 2.4 Restitution des anomalies

Pour chaque anomalie détectée :

- Description claire et compréhensible.
- Référence de l’écriture concernée.
- Type d’anomalie (calcul, taux, conformité, données manquantes).
- Niveau de criticité.

Les anomalies sont consultables :
- Par liste globale pour la période.
- Par détail au niveau d’une écriture.

---

## 3. Règles de gestion

- Les contrôles s’appliquent uniquement aux écritures incluses dans la période sélectionnée.
- Les règles de détection sont paramétrables via le moteur de règles.
- Toute anomalie critique doit être traitée ou explicitement validée avant validation experte de la déclaration.
- Chaque recalcul (suite à modification d’écriture ou de règle) met à jour automatiquement la liste des anomalies.
- Toutes les détections et résolutions d’anomalies sont tracées dans le journal d’audit.

---

## 4. Critères d’acceptation

- ✅ Pour une écriture cohérente (HT, TVA, TTC corrects), aucune anomalie n’est générée.
- ✅ Pour une écriture avec incohérence de calcul, une anomalie est générée avec le bon type et niveau.
- ✅ Pour une écriture avec taux incorrect, une anomalie "taux incohérent" est visible.
- ✅ La liste des anomalies se met à jour automatiquement après modification d’une écriture.
- ✅ Les anomalies critiques empêchent le passage au statut "validé expert" tant qu’elles ne sont pas traitées ou explicitement validées.

---

## 5. Hors périmètre

- Envoi direct de la déclaration à la DGI.
- Modification automatique des écritures sans action utilisateur.
- Décision automatique de conformité fiscale définitive.