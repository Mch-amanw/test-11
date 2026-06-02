# Spécification Technique – Chatbot connecté aux dossiers

## 1. Composants impactés

- Frontend (interface de chat intégrée au dossier).
- Service backend d’orchestration IA.
- Connecteurs vers :
  - Moteur de règles.
  - Module TVA.
  - Module délais de paiement.
  - Module workflow (rôles et permissions).
- Base de données (stockage des conversations).

---

## 2. Gestion du contexte applicatif

Chaque requête envoyée au service IA inclut :

- Identifiant du dossier.
- Identifiant de l’utilisateur.
- Rôle utilisateur.
- Éventuellement : identifiant d’une écriture ou d’une anomalie sélectionnée.
- Période TVA concernée si applicable.

Le backend construit un objet de contexte structuré contenant uniquement les données nécessaires à la requête.

---

## 3. Accès aux données

- Accès en lecture seule aux tables comptables et résultats d’analyse.
- Filtrage strict par identifiant de dossier.
- Vérification des permissions avant toute exposition au module IA.

Aucune écriture ne peut être créée ou modifiée via l’API du chatbot.

---

## 4. Orchestration IA

Le service backend :

1. Récupère les données pertinentes du dossier.
2. Construit un contexte limité et structuré.
3. Interroge le moteur IA.
4. Retourne une réponse enrichie et contrôlée au frontend.

Des garde-fous doivent être mis en place pour :
- Empêcher toute génération d’instructions exécutables.
- Encadrer les suggestions d’écritures comme texte explicatif uniquement.

---

## 5. Stockage et audit

Pour chaque message :
- Contenu de la question.
- Réponse générée.
- Identifiant utilisateur.
- Identifiant dossier.
- Horodatage.

Les données sont stockées dans la base interne de Comptalyses et intégrées au journal d’audit global.

---

## 6. Contraintes techniques

- Temps de réponse compatible avec un usage interactif.
- Gestion efficace de dossiers d’environ 5000 lignes.
- Isolation logique stricte en environnement SaaS multi-clients.
- Aucun transfert non maîtrisé de données sensibles.

---

## 7. Sécurité

- Authentification via le système central.
- Vérification systématique des autorisations.
- Chiffrement des communications réseau.
- Journalisation des accès et consultations.

---