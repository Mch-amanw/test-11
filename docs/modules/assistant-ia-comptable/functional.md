# Spécification Fonctionnelle – Module Assistant IA Comptable

## 1. Rôle du module dans le projet

Le module **Assistant IA Comptable** fournit un chatbot intelligent intégré à Comptalyses, destiné à assister les collaborateurs, chargés de dossier et experts-comptables dans l’analyse des dossiers.

Il permet :
- D’expliquer les anomalies détectées par le moteur de règles.
- De répondre aux questions relatives aux écritures comptables et aux agrégats TVA.
- De proposer des pistes de correction.
- D’apporter un éclairage réglementaire basé sur la législation marocaine.

Ce module agit comme un assistant d’aide à la décision. Il ne valide ni ne modifie automatiquement les données sans action explicite de l’utilisateur.

---

## 2. Fonctionnalités principales

### 2.1 Interface conversationnelle

- Chat accessible depuis un dossier.
- Historique des échanges lié au dossier.
- Compréhension des questions en langage naturel.
- Réponses contextualisées en fonction du dossier actif.

Exemples d’interactions :
- "Pourquoi cette écriture est-elle signalée ?"
- "Explique-moi l’écart de TVA sur ce fournisseur."
- "Quelles factures dépassent le délai légal ?"

---

### 2.2 Accès aux données du dossier

L’assistant peut exploiter :
- Les écritures importées (HT, TVA, TTC, dates, comptes).
- Les anomalies détectées par le moteur de règles.
- Les résultats du module TVA (agrégats, brouillon de déclaration).
- Les données liées aux délais de paiement.

Règles :
- Accès strictement limité au dossier courant.
- Respect des permissions liées au rôle utilisateur.

---

### 2.3 Explication des anomalies

Pour chaque anomalie détectée :
- Reformulation claire du problème.
- Explication de la règle déclenchée.
- Indication de l’impact potentiel (TVA, conformité, risque).

L’assistant ne modifie pas directement les écritures mais peut :
- Suggérer une correction.
- Expliquer la logique d’une écriture corrective.

---

### 2.4 Assistance réglementaire (Maroc)

- Réponses aux questions relatives à la TVA marocaine.
- Explication des règles sur les délais de paiement (60 jours, conventions 90/120 jours).
- Capacité à citer ou mentionner les références légales pertinentes.

Les réponses doivent :
- Être formulées de manière pédagogique.
- Distinguer clairement information réglementaire et interprétation.

---

### 2.5 Aide à la préparation de la déclaration TVA

- Explication des montants agrégés dans le brouillon.
- Détail des calculs (bases, taux, montants).
- Réponse aux questions sur la cohérence de la déclaration.

Le module ne valide pas la déclaration : la validation reste soumise au workflow (validation experte obligatoire).

---

### 2.6 Traçabilité

- Enregistrement des conversations dans le journal du dossier.
- Association des échanges aux utilisateurs.
- Conservation des interactions à des fins d’audit.

---

## 3. Règles de gestion spécifiques

- L’assistant n’effectue aucune modification automatique des données.
- Toute action corrective doit être validée via les modules standards (gestion des écritures, workflow).
- Les réponses doivent être contextualisées au dossier actif.
- L’accès aux données respecte strictement les rôles (Collaborateur, Chargé, Expert).
- Les échanges sont considérés comme des éléments consultatifs et non comme validation réglementaire officielle.

---

## 4. Limites du MVP

- Périmètre limité à la TVA marocaine et aux délais de paiement.
- Base documentaire centrée sur la réglementation marocaine pertinente au MVP.
- Pas d’intégration directe avec des services externes de dépôt ou de télétransmission.

---