## Spécification Fonctionnelle – Extraction automatique des délais contractuels

### 1. Objectif du ticket

Mettre en place un mécanisme d’analyse automatisée des documents contractuels importés (clients/fournisseurs) afin d’identifier les mentions explicites de délais de paiement de **90 jours** ou **120 jours**.

Ce mécanisme doit :

- Analyser le contenu textuel des documents téléversés.
- Identifier les mentions explicites de délais 90 ou 120 jours.
- Proposer un délai détecté à l’utilisateur.
- Alimenter le processus de validation utilisateur sans appliquer automatiquement le délai.

Ce ticket couvre uniquement la détection automatique du délai, et non la validation finale ni l’application dans le module Délais de paiement.

---

### 2. Périmètre fonctionnel

La fonctionnalité intervient immédiatement après l’upload d’une convention contractuelle.

Elle comprend :

- L’extraction du texte du document.
- La recherche ciblée de mentions explicites de type :
  - « 90 jours »
  - « 120 jours »
- La production d’un résultat structuré comprenant :
  - Le délai détecté (90 ou 120)
  - Un indicateur de détection (ex : détecté / non détecté / ambigu)

Elle ne comprend pas :

- L’analyse juridique complète du document.
- L’interprétation de clauses complexes.
- La validation définitive du délai (réalisée par l’utilisateur).

---

### 3. Comportement attendu

#### 3.1 Déclenchement

- L’analyse est déclenchée automatiquement après l’upload réussi d’un document contractuel.
- L’utilisateur n’a pas à lancer manuellement l’analyse.

#### 3.2 Détection d’un délai unique (90 ou 120 jours)

Si une mention explicite et unique de 90 ou 120 jours est détectée :

- Le système propose le délai correspondant.
- Le statut de la convention reste en attente de validation.
- L’utilisateur visualise clairement le délai proposé.

#### 3.3 Détection multiple ou ambiguë

Si plusieurs mentions de délais sont détectées (ex : 60 et 120 jours) ou si le contexte rend la détection ambiguë :

- Le système marque la convention comme nécessitant validation explicite.
- Le délai détecté peut être affiché, mais sans présélection automatique.

#### 3.4 Aucune détection

Si aucune mention explicite de 90 ou 120 jours n’est détectée :

- Aucun délai détecté n’est proposé.
- Le champ reste vide en attente d’une éventuelle saisie manuelle.
- Le délai réglementaire par défaut (60 jours) ne doit pas être enregistré ici, mais sera géré par le module aval.

---

### 4. Contraintes fonctionnelles

- L’analyse doit rester limitée aux délais 90 et 120 jours (MVP).
- Le résultat de la détection ne doit jamais activer automatiquement un délai sans validation utilisateur.
- Toute détection doit être traçable dans le journal d’audit.
- Le mécanisme doit fonctionner pour des documents contractuels standards utilisés par les cabinets marocains.

---

### 5. Traçabilité

Chaque analyse doit générer une trace incluant :

- Identifiant de la convention.
- Date et heure d’analyse.
- Résultat de la détection (90, 120, multiple, none).
- Identifiant technique du moteur d’analyse utilisé.

Ces informations sont stockées pour audit et diagnostic.