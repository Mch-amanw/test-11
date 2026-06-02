# Spécification Fonctionnelle – Import des conventions contractuelles

## 1. Objectif métier

Permettre l’import et la gestion des conventions contractuelles clients et fournisseurs afin d’identifier automatiquement les délais de paiement spécifiques (90 ou 120 jours) et d’alimenter le module de suivi des délais de paiement.

Cette fonctionnalité vise à :

- Justifier réglementairement les dépassements du délai standard de 60 jours.
- Réduire les erreurs d’interprétation des conventions.
- Sécuriser les alertes liées aux délais de paiement.
- Garantir la traçabilité des conventions utilisées.

---

## 2. Périmètre

La fonctionnalité couvre :

- Le téléversement de documents contractuels (ex : PDF).
- L’association d’une convention à un tiers (client ou fournisseur) d’un dossier.
- L’extraction automatique des délais contractuels (90 ou 120 jours).
- L’affichage et la validation du délai détecté.
- La gestion de l’historique et de la traçabilité.

Elle ne couvre pas l’analyse complète du contenu juridique des conventions au-delà de l’identification des délais de paiement.

---

## 3. Fonctionnalités détaillées

### 3.1 Upload d’une convention

- L’utilisateur (Collaborateur ou rôle autorisé) peut téléverser un document contractuel.
- Le document doit être rattaché :
  - À un dossier.
  - À un tiers existant (client ou fournisseur).
- Les métadonnées enregistrées incluent :
  - Date d’upload
  - Utilisateur
  - Tiers associé

---

### 3.2 Extraction automatique des délais

Après upload :

- Le système analyse le contenu du document.
- Il recherche explicitement des mentions de délais de paiement de 90 ou 120 jours.
- Si un délai est détecté :
  - Le système propose le délai identifié.
  - Il affiche le résultat à l’utilisateur.

En cas d’absence de détection claire :

- Le délai par défaut de 60 jours reste applicable.
- Le document est néanmoins conservé.

---

### 3.3 Validation utilisateur

- L’utilisateur peut valider le délai détecté.
- Toute modification ou confirmation est tracée.
- Une convention validée devient active pour le calcul des délais de paiement du tiers concerné.

---

### 3.4 Application des règles

- En l’absence de convention valide, le délai réglementaire par défaut est de 60 jours.
- Si une convention valide prévoit 90 ou 120 jours, ce délai s’applique au tiers associé.
- Une convention doit être explicitement liée à un tiers pour être prise en compte.
- Toute modification ultérieure d’un délai contractuel est journalisée.

---

## 4. Critères d’acceptation

1. Un utilisateur autorisé peut téléverser un document et l’associer à un tiers existant.
2. Le système détecte automatiquement une mention explicite de 90 ou 120 jours lorsqu’elle est présente.
3. Le délai détecté est affiché clairement et nécessite validation.
4. En l’absence de convention valide, le délai par défaut de 60 jours est appliqué.
5. Toute action (upload, validation, modification) est visible dans le journal d’audit.
6. Le module délais de paiement utilise correctement le délai validé pour générer ou non des alertes.

---

## 5. Contraintes fonctionnelles

- Respect strict de la confidentialité des documents.
- Isolation logique des données par cabinet.
- Cohérence obligatoire entre tiers déclaré dans la convention et tiers du dossier.
- Aucun calcul d’alerte n’est réalisé dans cette fonctionnalité : elle fournit uniquement le délai contractuel validé aux modules aval.