# Spécification Technique – Intégrer la base documentaire légale marocaine

## 1. Architecture cible

La base documentaire est intégrée dans l’architecture du module Assistant IA.

Composants concernés :

- Base de données dédiée (ou schéma dédié) pour le corpus réglementaire.
- Service backend d’accès documentaire.
- Mécanisme d’indexation et de recherche interne.
- Interface d’administration minimale pour l’import initial (si nécessaire).

Aucun service externe public n’est utilisé en production.

---

## 2. Modélisation des données

Création d’une entité `RegulatoryDocument` avec les champs minimum suivants :

- `id` (UUID)
- `title` (nom du texte)
- `type` (loi, article, disposition, etc.)
- `theme` (TVA, délais_paiement)
- `referenceCode` (ex : numéro d’article)
- `content` (texte structuré)
- `version` (optionnel, si applicable)
- `createdAt`
- `updatedAt`

Possibilité de structurer les documents en sous-unités (articles liés à un texte parent) via une relation parent/enfant.

Les données sont stockées séparément des données comptables des dossiers.

---

## 3. Indexation et recherche

Mise en place d’un mécanisme de recherche interne :

- Indexation textuelle du champ `content`.
- Filtrage par `theme`.
- Filtrage par `type`.

La recherche doit permettre :

1. Réception d’une requête structurée (mots-clés, thème).
2. Retour d’une liste d’extraits pertinents (contenu + métadonnées).

Les résultats doivent être limités en nombre (extraits les plus pertinents) afin de maîtriser le contexte transmis au moteur IA.

---

## 4. Service d’accès documentaire

Implémentation d’un service backend dédié exposant :

- Méthode de recherche par thème et mots-clés.
- Méthode de récupération d’un document par identifiant.

Contraintes :

- Lecture seule en production.
- Aucune modification via le module IA.
- Vérification systématique des permissions utilisateur avant exposition des extraits.

---

## 5. Intégration avec l’orchestrateur IA

Lorsqu’une requête nécessite une dimension réglementaire :

1. L’orchestrateur identifie le besoin réglementaire.
2. Il appelle le service documentaire.
3. Il injecte les extraits sélectionnés dans le contexte envoyé au moteur IA.

Un mécanisme de contrôle doit :

- Empêcher l’injection de références hors corpus.
- S’assurer que seules les références réellement retournées par le service peuvent être citées.

---

## 6. Journalisation

Pour chaque requête mobilisant la base réglementaire :

- Enregistrement de :
  - Identifiant utilisateur
  - Identifiant dossier
  - Identifiants des documents utilisés
  - Horodatage

Ces éléments sont intégrés au journal d’audit du module Assistant IA.

---

## 7. Sécurité

- Accès en lecture seule.
- Isolation logique respectée entre cabinets (même si le corpus est commun, l’accès se fait via le contexte dossier).
- Aucun stockage de données comptables dans cette base.
- Chiffrement des échanges entre service documentaire et orchestrateur.

---

## 8. Contraintes techniques

- Performance compatible avec un usage interactif (temps de recherche court).
- Corpus limité au périmètre MVP.
- Architecture extensible pour intégrer ultérieurement d’autres thèmes fiscaux.

Ce ticket fournit la brique technique fondamentale permettant l’usage fiable et contrôlé de références réglementaires dans les réponses du chatbot.