# Spécification Fonctionnelle – Chatbot connecté aux dossiers

## 1. Objectif métier

La fonctionnalité **Chatbot connecté aux dossiers** permet aux utilisateurs (Collaborateur, Chargé de dossier, Expert) d’interagir avec un assistant IA contextualisé au dossier comptable actif.

L’objectif est de :

- Expliquer les anomalies détectées par le moteur de règles.
- Répondre aux questions relatives aux écritures, à la TVA et aux délais de paiement.
- Proposer des pistes d’écritures correctives.
- Faciliter la compréhension des agrégats et du brouillon de déclaration TVA.

Le chatbot agit comme un assistant d’aide à la décision. Il ne modifie jamais directement les données comptables.

---

## 2. Périmètre fonctionnel

### 2.1 Accès contextuel au dossier actif

Le chatbot est accessible depuis un dossier spécifique.

Il peut exploiter uniquement :
- Les écritures comptables importées (HT, TVA, TTC, dates, comptes).
- Les anomalies détectées par le moteur de règles.
- Les données liées aux délais de paiement.
- Les agrégats et brouillons générés par le module TVA.

Aucune donnée d’un autre dossier ou cabinet ne peut être consultée.

---

### 2.2 Explication des anomalies

Pour chaque anomalie détectée, l’utilisateur peut :

- Demander une explication en langage naturel.
- Obtenir la règle déclenchée.
- Comprendre l’impact potentiel (risque TVA, non-conformité, retard réglementaire).

L’explication doit être :
- Pédagogique.
- Reliée explicitement aux données du dossier.
- Distincte d’une validation formelle.

---

### 2.3 Proposition d’écritures correctives

Le chatbot peut :

- Suggérer une logique de correction.
- Décrire une écriture comptable corrective possible (comptes, sens débit/crédit, principe).
- Expliquer l’impact attendu sur la TVA ou la conformité.

Règles :
- Aucune écriture n’est créée automatiquement.
- L’utilisateur doit passer par le module de gestion des écritures pour effectuer une correction.
- Les propositions sont indicatives et doivent être validées via le workflow standard.

---

### 2.4 Réponses aux questions liées au dossier

Le chatbot répond aux questions telles que :

- "Pourquoi ce montant de TVA est-il différent du mois précédent ?"
- "Quelles factures dépassent le délai légal ?"
- "Explique-moi le calcul de la TVA collectée."

Les réponses doivent :
- Être basées sur les données du dossier actif.
- Être cohérentes avec les résultats des modules TVA et délais de paiement.
- Être adaptées au rôle de l’utilisateur.

---

### 2.5 Historique des échanges

- Chaque conversation est associée au dossier.
- Les messages sont historisés avec l’utilisateur et l’horodatage.
- Les échanges peuvent être consultés ultérieurement à des fins d’audit.

---

## 3. Règles de gestion

- Le chatbot fonctionne en lecture seule sur les données comptables.
- Il respecte strictement les permissions liées aux rôles (Collaborateur, Chargé, Expert).
- Toute validation officielle reste soumise au workflow standard.
- Les réponses ont un caractère consultatif et pédagogique.
- Les échanges sont tracés dans le journal d’audit.

---

## 4. Critères d’acceptation

- ✅ Depuis un dossier actif, l’utilisateur peut poser une question et recevoir une réponse contextualisée.
- ✅ Une anomalie sélectionnée peut être expliquée de manière claire et reliée à la règle déclenchée.
- ✅ Le chatbot peut proposer une écriture corrective sans modifier les données.
- ✅ Aucune donnée d’un autre dossier ou cabinet n’est exposée.
- ✅ Les échanges sont enregistrés et consultables.
- ✅ Les réponses respectent le périmètre TVA et délais de paiement du MVP.

---