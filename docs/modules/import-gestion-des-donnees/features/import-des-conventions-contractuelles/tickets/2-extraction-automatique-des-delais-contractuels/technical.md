## Spécification Technique – Extraction automatique des délais contractuels

### 1. Positionnement dans l’architecture

Ce ticket implémente le composant d’analyse documentaire du module **Import & Gestion des données**, dans le cadre de la fonctionnalité « Import des conventions contractuelles ».

Il intervient dans le pipeline suivant :

1. Upload du document.
2. Stockage sécurisé.
3. Extraction du texte.
4. Analyse des délais.
5. Enregistrement du résultat en base.

---

### 2. Pipeline technique d’analyse

#### 2.1 Extraction du texte

- Utilisation d’un composant d’extraction de texte compatible PDF.
- Gestion des cas :
  - PDF texte natif.
  - PDF nécessitant OCR (si activé ultérieurement).
- Production d’un texte brut normalisé (suppression des caractères parasites, normalisation des espaces).

Le texte extrait n’est pas exposé publiquement mais utilisé pour analyse interne.

---

#### 2.2 Mécanisme de détection

Implémentation d’un moteur de recherche basé sur :

- Expressions régulières ciblant explicitement :
  - "90 jours"
  - "120 jours"
- Recherche insensible à la casse.
- Tolérance minimale aux variations d’espacement.

Le moteur doit :

- Identifier le nombre d’occurrences pour chaque délai.
- Produire un objet résultat structuré :
  - delaiDetecte : 90 | 120 | null
  - statutDetection : "unique" | "multiple" | "none"
  - occurrences : nombre d’occurrences trouvées

Aucune analyse sémantique avancée n’est requise pour le MVP.

---

### 3. Persistance des résultats

Extension ou mise à jour de l’entité `ConventionContractuelle` :

- delaiDetecte (90, 120, null)
- statutDetection (unique, multiple, none)
- analyseAt (datetime)
- analyseVersion (identifiant du moteur ou version interne)

Ces champs sont mis à jour automatiquement après analyse.

Le champ `delaiValide` reste inchangé à ce stade (géré par le processus de validation utilisateur).

---

### 4. Gestion des cas d’erreur

- Si l’extraction de texte échoue :
  - statutDetection = "none"
  - Log technique enregistré.
- Aucune exception non gérée ne doit bloquer le flux global d’upload.
- Les erreurs sont journalisées dans les logs applicatifs pour diagnostic.

---

### 5. Intégration avec les modules aval

- Le module Délais de paiement n’utilise pas `delaiDetecte` directement.
- Il consomme uniquement `delaiValide` après validation utilisateur.
- Un service interne permet de consulter le résultat de détection pour affichage UI.

---

### 6. Sécurité et isolation

- Analyse réalisée dans le contexte du cabinet (multi-tenant).
- Aucun document ou texte extrait ne doit être exposé à d’autres cabinets.
- Journalisation technique interne conforme aux exigences de traçabilité.

---

### 7. Contraintes techniques

- Performance acceptable pour des documents contractuels standards.
- Code modulaire permettant d’étendre ultérieurement la détection à d’autres types de clauses.
- Respect des standards de sécurité et de confidentialité définis au niveau projet.