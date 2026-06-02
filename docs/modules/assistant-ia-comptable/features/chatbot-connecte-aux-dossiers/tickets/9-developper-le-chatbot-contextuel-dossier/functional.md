# Spécification Fonctionnelle – Développer le chatbot contextuel dossier

## 1. Objectif du ticket

Mettre en place le mécanisme permettant au chatbot d’accéder aux données du dossier actif afin de fournir des réponses contextualisées, notamment pour :

- Expliquer les anomalies détectées par le moteur de règles.
- Répondre aux questions liées aux écritures comptables.
- Analyser les agrégats TVA et les brouillons de déclaration.
- Proposer des pistes d’écritures correctives (sans modification automatique).

Ce ticket couvre la capacité du chatbot à exploiter les données internes du dossier en lecture seule, dans le respect strict des rôles et de la confidentialité multi-clients.

---

## 2. Périmètre fonctionnel

### 2.1 Accès aux écritures comptables

Le chatbot doit pouvoir exploiter, pour le dossier actif :

- Les lignes d’écritures importées (HT, TVA, TTC).
- Les dates (facture, échéance si disponible).
- Les comptes comptables associés.
- Les informations clients/fournisseurs.

Ces données peuvent être utilisées pour :

- Expliquer une incohérence de montants.
- Justifier un écart de TVA.
- Décrire le traitement comptable observé.

---

### 2.2 Accès aux anomalies détectées

Le chatbot doit pouvoir :

- Récupérer la liste des anomalies détectées par le moteur de règles.
- Identifier la règle déclenchée.
- Expliquer la logique de détection.
- Relier explicitement l’anomalie aux écritures concernées.

L’explication doit inclure :

- La description pédagogique de l’anomalie.
- La règle métier correspondante.
- L’impact potentiel (TVA, conformité, risque).

---

### 2.3 Accès aux données TVA

Le chatbot doit pouvoir exploiter :

- Les agrégats calculés (TVA collectée, déductible, crédit, etc.).
- Les bases de calcul.
- Les taux appliqués.
- Les données du brouillon de déclaration.

Il doit être en mesure de :

- Expliquer un montant agrégé.
- Détailler la composition d’un total.
- Répondre à des questions comparatives simples (ex : écart entre périodes si disponible).

---

### 2.4 Accès aux délais de paiement

Le chatbot doit pouvoir :

- Identifier les factures dépassant le délai réglementaire de 60 jours.
- Prendre en compte les exceptions liées aux conventions 90/120 jours si détectées.
- Expliquer pourquoi une facture est signalée ou non.

---

### 2.5 Proposition d’écritures correctives (indicatives)

À partir d’une anomalie ou d’une question spécifique, le chatbot peut :

- Décrire une écriture corrective possible (comptes concernés, sens débit/crédit, principe général).
- Expliquer l’impact attendu sur la TVA ou la conformité.

Contraintes :

- Aucune écriture n’est créée automatiquement.
- Aucune modification n’est effectuée sur les données.
- Les propositions sont purement textuelles et consultatives.

---

## 3. Règles de gestion

- Le chatbot fonctionne strictement en lecture seule.
- Il ne peut accéder qu’au dossier actif.
- Les données exposées dépendent du rôle utilisateur (Collaborateur, Chargé, Expert).
- Aucune donnée d’un autre dossier ou cabinet ne doit être accessible.
- Les réponses doivent être explicitement contextualisées au dossier en cours.
- Les propositions d’écritures sont des suggestions, non des validations officielles.

---

## 4. Hors périmètre

- Création ou modification automatique d’écritures.
- Validation de la déclaration TVA.
- Accès à des données externes au dossier actif.

---

## 5. Impact utilisateur

Ce ticket rend le chatbot réellement opérationnel en tant qu’assistant d’analyse dossier, en lui permettant de produire des réponses pertinentes et contextualisées, et non génériques.

Il constitue une brique centrale pour la valeur métier du module Assistant IA Comptable.