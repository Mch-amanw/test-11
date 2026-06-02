## Module : Moteur de règles & Anomalies

### 1. Rôle du module dans le projet
Le Moteur de règles & Anomalies est le cœur analytique de Comptalyses. Il permet de détecter automatiquement les incohérences comptables et fiscales à partir des données importées (écritures comptables, balances âgées, conventions contractuelles), selon un ensemble de règles métier paramétrables.

Il alimente :
- Le module TVA (contrôles de cohérence et conformité)
- Le module Délais de paiement (détection des dépassements réglementaires)
- Le scoring de risque du dossier
- L’Assistant IA (explication des anomalies)
- Le workflow (blocage ou validation conditionnelle selon le niveau d’anomalie)

---

### 2. Objectifs fonctionnels

- Automatiser la détection d’anomalies comptables et fiscales
- Assurer la conformité avec la réglementation marocaine (TVA, délais de paiement)
- Réduire les erreurs humaines dans le traitement des dossiers
- Fournir un cadre de règles évolutif et paramétrable
- Générer un score de risque synthétique par dossier

---

### 3. Typologie des règles

#### 3.1 Règles TVA
- Cohérence HT / TVA / TTC
- Vérification des taux de TVA appliqués
- Détection de taux incohérents par rapport à la nature de l’opération
- Détection d’écarts de calcul (arrondis anormaux, incohérences systématiques)

#### 3.2 Règles Délais de paiement
- Détection automatique des factures > 60 jours
- Neutralisation automatique si convention valide 90/120 jours détectée
- Signalement des dépassements réglementaires

#### 3.3 Règles d’anomalies générales
- Incohérences de montants
- Données manquantes essentielles
- Anomalies détectables via règles paramétrables définies par le cabinet

---

### 4. Paramétrage des règles

Le module doit permettre :
- L’activation / désactivation d’une règle
- Le paramétrage de seuils (ex : tolérance d’écart)
- La catégorisation des règles (TVA, délais, autres)
- La définition d’un niveau de gravité :
  - Critique
  - Majeur
  - Mineur

Les règles sont globales au cabinet (dans le cadre du MVP) et appliquées à tous les dossiers.

---

### 5. Exécution des contrôles

- Les règles sont exécutées automatiquement après import des données.
- Une réexécution peut être déclenchée après modification d’écritures.
- Chaque anomalie détectée est associée à :
  - L’écriture ou la facture concernée
  - La règle déclenchée
  - Le niveau de gravité
  - Un message explicatif structuré

---

### 6. Restitution des résultats

Le module fournit :
- Une liste détaillée des anomalies
- Un regroupement par type et par gravité
- Un indicateur global de risque du dossier
- Des données exploitables par le chatbot pour explication

Les anomalies critiques peuvent bloquer le passage à l’étape suivante du workflow tant qu’elles ne sont pas traitées ou validées.

---

### 7. Gestion du cycle de vie des anomalies

Chaque anomalie peut avoir un statut :
- Ouverte
- Corrigée
- Justifiée
- Validée

Toute action sur une anomalie (correction, justification, validation) doit être tracée dans le journal d’audit.

---

### 8. Règles de gestion

- Toute anomalie critique doit être traitée ou validée avant validation finale par l’expert.
- Les règles appliquées doivent être traçables pour chaque dossier.
- Les modifications d’écritures entraînent une réévaluation des règles.
- Le moteur ne modifie jamais automatiquement les écritures : il signale uniquement.
- Toutes les détections doivent être historisées.

---

## Conclusion fonctionnelle

Le Moteur de règles & Anomalies constitue la couche de contrôle automatisé du système. Il centralise la logique métier de détection et garantit la cohérence fiscale et réglementaire des dossiers traités dans Comptalyses.