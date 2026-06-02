# Spécification Fonctionnelle – Intégrer la base documentaire légale marocaine

## 1. Objectif du ticket

Ce ticket vise à mettre en place l’infrastructure fonctionnelle permettant :

- L’intégration d’un corpus réglementaire marocain validé (TVA et délais de paiement – périmètre MVP).
- L’indexation structurée des textes.
- La consultation de ces textes par le module Assistant IA.
- La restitution de références réglementaires identifiables dans les réponses du chatbot.

Il constitue la brique fondamentale permettant à l’assistant de produire des réponses justifiées et appuyées sur des références légales issues d’un corpus maîtrisé.

---

## 2. Périmètre fonctionnel

### 2.1 Intégration du corpus réglementaire (MVP)

Le système doit permettre :

- L’import de textes réglementaires validés (format structuré).
- Leur stockage dans une base documentaire dédiée.
- Leur catégorisation par thématique (TVA, délais de paiement).
- L’identification claire de chaque élément documentaire (référence, article, section).

Le corpus est limité au périmètre suivant :
- Règles relatives à la TVA marocaine pertinentes pour le moteur de règles.
- Règles relatives aux délais de paiement (60 jours, conventions 90/120 jours).

Aucun texte hors périmètre MVP ne doit être intégré à ce stade.

---

### 2.2 Structuration et indexation

Chaque texte intégré doit être :

- Structuré en unités exploitables (article, disposition, section).
- Indexé selon :
  - Thématique
  - Mots-clés
  - Type de texte

L’indexation doit permettre une recherche pertinente par sujet réglementaire.

---

### 2.3 Consultation par le module Assistant IA

Le module Assistant IA doit pouvoir :

- Interroger la base documentaire via une couche d’accès dédiée.
- Récupérer les extraits pertinents.
- Utiliser ces extraits pour enrichir les réponses.

La consultation est :

- En lecture seule.
- Restreinte au corpus validé.
- Journalisée lorsqu’une référence est mobilisée dans une réponse.

---

### 2.4 Restitution des références

Les références issues de la base doivent pouvoir être restituées dans un format structuré :

- Nom du texte ou de la loi.
- Article ou section.
- Thématique.

La restitution permet au chatbot de :

- Distinguer clairement l’explication pédagogique et la référence réglementaire.
- Mentionner explicitement la source issue du corpus intégré.

---

## 3. Règles de gestion

- Seuls les textes validés et intégrés dans la base peuvent être utilisés.
- Aucune référence externe non intégrée ne peut être citée.
- La base documentaire ne contient aucune donnée comptable.
- Toute utilisation d’un extrait réglementaire dans une réponse est traçable.
- L’intégration du corpus respecte le périmètre TVA et délais de paiement du MVP.

---

## 4. Hors périmètre

- Enrichissement automatique par scraping ou connexion à des sources externes.
- Mise à jour dynamique automatique des textes.
- Extension à d’autres domaines fiscaux.

Ces éléments pourront faire l’objet de tickets ultérieurs.