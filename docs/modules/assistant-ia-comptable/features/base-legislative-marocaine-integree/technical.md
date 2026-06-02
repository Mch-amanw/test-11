# Spécification Technique – Base législative marocaine intégrée

## 1. Architecture de la base documentaire

La fonctionnalité repose sur :

- Un stockage structuré des documents réglementaires pertinents.
- Une indexation permettant la recherche ciblée par thème (TVA, délais de paiement, etc.).
- Une couche d’accès dédiée utilisée par le backend du module Assistant IA.

La base est intégrée à l’architecture du module Assistant IA et ne constitue pas un service externe public.

---

## 2. Modélisation des données

Chaque élément documentaire doit inclure au minimum :

- Identifiant unique.
- Type de texte (ex : loi, article, disposition).
- Thématique (TVA, délais de paiement, etc.).
- Contenu textuel structuré.
- Métadonnées (référence, version si applicable).

Les documents sont stockés de manière sécurisée et séparés des données comptables des dossiers.

---

## 3. Mécanisme de recherche

Lors d’une requête utilisateur :

1. Le backend identifie si une dimension réglementaire est requise.
2. Il interroge la base documentaire via un mécanisme de recherche interne.
3. Les extraits pertinents sont injectés dans le contexte transmis au moteur IA.

Le moteur IA génère une réponse enrichie incluant :

- Reformulation pédagogique.
- Référence structurée issue du corpus.

---

## 4. Intégration avec le moteur IA

- Les extraits documentaires sont fournis comme contexte contrôlé.
- Le modèle ne doit pas inventer de références hors corpus.
- Un mécanisme de contrôle limite les réponses aux documents effectivement fournis.

La logique d’orchestration est assurée par le service backend du module Assistant IA.

---

## 5. Sécurité et confidentialité

- La base documentaire est accessible en lecture seule par le module IA.
- Aucune donnée comptable n’est stockée dans la base réglementaire.
- Isolation logique respectée entre cabinets.
- Journalisation des requêtes ayant mobilisé des références réglementaires.

---

## 6. Contraintes techniques

- Temps de réponse compatible avec un usage interactif.
- Corpus limité au périmètre MVP (TVA et délais de paiement).
- Architecture évolutive permettant l’ajout ultérieur d’autres domaines fiscaux.
- Aucun appel à des sources externes non maîtrisées en production.

---