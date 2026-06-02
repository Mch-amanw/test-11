# Spécification Fonctionnelle – Implémenter le journal d’audit sécurisé

## 1. Objectif du ticket

Mettre en place le **journal d’audit sécurisé** permettant de tracer de manière exhaustive, horodatée et infalsifiable toutes les actions critiques réalisées sur les dossiers dans Comptalyses.

Ce ticket couvre l’implémentation du mécanisme central d’enregistrement des événements d’audit conformément aux spécifications du module *Workflow & Audit* et de la fonctionnalité *Journal d’audit et traçabilité*.

---

## 2. Périmètre fonctionnel

Le journal d’audit doit enregistrer automatiquement les actions suivantes (liste non limitative mais obligatoire pour le MVP) :

- Import de fichiers comptables
- Upload de conventions contractuelles
- Modifications d’écritures
- Propositions et validations de corrections
- Détection et résolution d’anomalies
- Changements de statut du dossier (workflow)
- Validations intermédiaires (Chargé)
- Validation finale (Expert)
- Génération de déclaration TVA
- Export de déclaration TVA
- Actions sensibles déclenchées depuis le module IA
- Tentatives d’actions non autorisées

Chaque action critique réalisée sur un dossier doit générer automatiquement une entrée d’audit.

---

## 3. Informations à tracer

Chaque événement d’audit doit contenir au minimum :

- Identifiant du cabinet
- Identifiant du dossier
- Identifiant de l’utilisateur
- Rôle actif de l’utilisateur
- Horodatage (date et heure)
- Type d’action
- Entité concernée (type + identifiant)
- Détails contextuels (payload)

En cas de modification :

- Ancienne valeur
- Nouvelle valeur

En cas de renvoi ou de validation avec commentaire :

- Commentaire utilisateur associé

Les informations doivent permettre de reconstituer précisément :

- Qui a effectué l’action
- Quand
- Sur quel élément
- Quelle était la modification ou la décision prise

---

## 4. Règles de gestion

- Toute action critique doit obligatoirement générer une entrée d’audit.
- L’horodatage est réalisé côté serveur.
- Le journal est en écriture uniquement (append-only).
- Aucune suppression ou modification d’événement n’est autorisée via l’interface applicative.
- Les tentatives d’actions interdites doivent également être tracées.
- L’enregistrement de l’audit doit être cohérent avec l’action métier (pas d’action validée sans trace correspondante).

---

## 5. Sécurité et confidentialité

- Les événements sont isolés par cabinet (multi-tenant).
- Les utilisateurs ne peuvent consulter que les événements des dossiers auxquels ils ont accès.
- Aucune donnée sensible ne doit être exposée en dehors du contexte autorisé.

Ce ticket met en place la brique centrale garantissant la conformité réglementaire, la traçabilité professionnelle et la responsabilité de l’Expert-comptable.