# Spécification Fonctionnelle – Base législative marocaine intégrée

## 1. Objectif métier

La fonctionnalité **Base législative marocaine intégrée** permet au chatbot de l’Assistant IA Comptable d’appuyer ses réponses sur des références réglementaires marocaines pertinentes (notamment en matière de TVA et de délais de paiement).

L’objectif est de :

- Renforcer la fiabilité des réponses fournies par l’assistant.
- Permettre la citation explicite de références légales.
- Apporter un éclairage réglementaire structuré et pédagogique.
- Sécuriser les utilisateurs (collaborateurs, chargés, experts) dans leurs décisions.

Cette base documentaire constitue un support d’aide à la décision et non une validation juridique officielle.

---

## 2. Périmètre fonctionnel

### 2.1 Couverture réglementaire (MVP)

La base couvre, dans le cadre du MVP :

- Les règles relatives à la TVA marocaine (principes généraux, taux, déductibilité).
- Les règles sur les délais de paiement (60 jours, conventions 90/120 jours).
- Les éléments réglementaires directement liés aux contrôles effectués par Comptalyses.

La couverture est limitée aux thématiques du MVP et peut être enrichie ultérieurement.

---

### 2.2 Recherche et restitution de références

Lorsqu’un utilisateur pose une question réglementaire ou demande une justification (ex : "Sur quelle base cette anomalie est-elle détectée ?") :

- L’assistant interroge la base documentaire.
- Il sélectionne les références pertinentes.
- Il reformule la règle de manière pédagogique.
- Il peut mentionner explicitement la référence légale (ex : article, texte applicable).

Les réponses doivent :

- Distinguer clairement le texte réglementaire et l’explication simplifiée.
- Indiquer qu’il s’agit d’une information à titre d’assistance.

---

### 2.3 Citation structurée

Lorsqu’une référence est utilisée :

- Elle doit être identifiable (nom du texte, article ou section si applicable).
- Elle doit être contextualisée (lien avec la situation du dossier).
- Elle ne doit pas être présentée comme une validation automatique.

Exemple de restitution attendue :
- Explication simplifiée.
- Mention de la référence réglementaire pertinente.

---

### 2.4 Intégration avec l’explication des anomalies

Pour les anomalies détectées par le moteur de règles :

- L’assistant peut compléter l’explication métier par une base réglementaire.
- La référence réglementaire renforce la compréhension du risque.

Cette intégration doit rester cohérente avec les règles paramétrables du moteur.

---

### 2.5 Limites et responsabilités

- La base documentaire ne remplace pas une consultation juridique.
- Les réponses générées par l’IA restent consultatives.
- L’expert conserve l’entière responsabilité de la validation finale.

---

## 3. Règles de gestion

- La base documentaire est utilisée uniquement dans le cadre du dossier actif.
- Les références citées doivent appartenir au corpus validé et intégré.
- Aucune référence externe non maîtrisée ne doit être utilisée.
- Les réponses doivent distinguer clairement :
  - La règle réglementaire.
  - L’interprétation ou l’explication pédagogique.
- Toute interaction impliquant une référence légale est journalisée avec la réponse générée.

---

## 4. Critères d’acceptation

- ✅ Le chatbot est capable de citer une référence réglementaire marocaine pertinente en réponse à une question sur la TVA.
- ✅ Les références sont présentées de manière identifiable et contextualisée.
- ✅ Les réponses distinguent texte réglementaire et explication.
- ✅ Les citations restent limitées au périmètre documentaire validé.
- ✅ Les échanges incluant des références sont tracés dans le journal d’audit.

---