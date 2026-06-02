# Spécification Technique – Module Assistant IA Comptable

## 1. Architecture du module

Le module repose sur :

- Une interface frontend de chat intégrée à l’application web.
- Un service backend dédié à l’orchestration des requêtes IA.
- Un connecteur aux données internes du dossier.
- Un accès à une base documentaire de la législation marocaine.

Il s’intègre avec :
- Le moteur de règles.
- Le module TVA.
- Le module délais de paiement.
- Le module workflow (gestion des rôles).

---

## 2. Gestion du contexte

Chaque requête utilisateur inclut :
- Identifiant du dossier.
- Identifiant de l’utilisateur.
- Rôle (Collaborateur, Chargé, Expert).
- Éléments contextuels pertinents (écriture sélectionnée, anomalie ciblée, période TVA).

Le backend construit un contexte structuré transmis au moteur IA.

---

## 3. Accès sécurisé aux données

- Accès en lecture seule aux données comptables.
- Filtrage strict par dossier (isolation logique multi-clients).
- Vérification systématique des permissions avant exposition des données au module IA.

Aucune donnée d’un autre cabinet ne doit être accessible.

---

## 4. Base documentaire réglementaire

- Stockage structuré des textes et références liés à la législation marocaine pertinente.
- Mécanisme de recherche documentaire pour enrichir les réponses.
- Capacité à inclure des références dans les réponses générées.

---

## 5. Journalisation et audit

- Stockage des messages (question, réponse, horodatage, utilisateur, dossier).
- Intégration avec le journal d’audit global.
- Possibilité de restitution des échanges en cas de contrôle interne.

---

## 6. Contraintes techniques

- Respect des exigences de confidentialité élevées du projet.
- Aucune conservation externe non maîtrisée des données sensibles.
- Performances compatibles avec un usage interactif (réponses rapides sur des dossiers ~5000 lignes).
- Architecture évolutive pour intégrer ultérieurement d’autres domaines fiscaux.

---

## 7. Sécurité

- Authentification via le système central de Comptalyses.
- Autorisation basée sur les rôles.
- Chiffrement des échanges réseau.
- Surveillance des accès et journalisation des consultations.

---