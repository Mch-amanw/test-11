# Spécification Technique – Développer le chatbot contextuel dossier

## 1. Objectif technique

Implémenter la couche d’accès sécurisé et structuré aux données du dossier afin d’alimenter le service d’orchestration IA avec un contexte contrôlé, cohérent et limité au périmètre autorisé.

---

## 2. Architecture cible

Composants impliqués :

- Frontend (interface chat dans le dossier).
- Backend – Service d’orchestration IA.
- Connecteurs internes vers :
  - Tables écritures comptables.
  - Résultats du moteur de règles (anomalies).
  - Module TVA (agrégats et brouillon).
  - Module délais de paiement.
- Module workflow (vérification des rôles).

---

## 3. Construction du contexte IA

### 3.1 Données d’entrée obligatoires

Chaque requête doit inclure :

- `dossierId`
- `userId`
- `role`
- Message utilisateur
- Optionnel : `ecritureId`, `anomalieId`, `periodeTVA`

---

### 3.2 Agrégation des données

Le backend doit :

1. Vérifier l’autorisation d’accès au dossier.
2. Charger uniquement les données pertinentes selon la question :
   - Écriture ciblée.
   - Anomalie spécifique.
   - Sous-ensemble d’agrégats TVA.
   - Liste filtrée des factures en retard.
3. Construire un objet de contexte structuré (JSON interne) contenant :
   - Résumé des données pertinentes.
   - Règles déclenchées le cas échéant.
   - Informations TVA nécessaires.

Les volumes doivent être maîtrisés (pas d’envoi brut des 5000 lignes si non nécessaire).

---

## 4. Accès en lecture seule

- Les connecteurs vers les modules internes doivent être strictement en lecture seule.
- Aucune méthode d’écriture ou modification ne doit être exposée au service IA.
- Les suggestions d’écritures sont générées uniquement sous forme textuelle.

---

## 5. Isolation multi-clients

- Toutes les requêtes doivent être filtrées par `dossierId`.
- Vérification systématique que le dossier appartient au cabinet de l’utilisateur.
- Aucun appel ne doit pouvoir récupérer des données d’un autre cabinet.

---

## 6. Gestion des performances

- Mise en place de requêtes ciblées et indexées.
- Éviter les chargements complets de datasets volumineux.
- Temps de réponse compatible avec une interaction utilisateur (usage conversationnel).

---

## 7. Sécurité

- Authentification via le système central.
- Autorisation basée sur les rôles.
- Chiffrement des communications frontend ↔ backend.
- Journalisation des accès aux données via le service IA.

---

## 8. Journalisation

Pour chaque requête :

- Conserver le message utilisateur.
- Conserver le contexte résumé transmis (sans données excessives sensibles).
- Conserver la réponse générée.
- Associer `userId`, `dossierId`, horodatage.

Ces éléments doivent être intégrés au mécanisme global d’audit.

---

## 9. Contraintes

- Aucun transfert non maîtrisé de données sensibles vers des systèmes externes.
- Respect des exigences de confidentialité élevées du projet.
- Architecture extensible pour intégrer ultérieurement d’autres domaines fiscaux.