# Spécification Technique – Import des conventions contractuelles

## 1. Positionnement dans l’architecture

Cette fonctionnalité appartient au module **Import & Gestion des données** et interagit avec :

- Le module Délais de paiement
- Le moteur de règles
- Le module Workflow
- Le module Journal d’audit

Elle fournit un délai contractuel structuré et validé par tiers.

---

## 2. Upload et stockage des documents

### 2.1 Upload

- Upload via interface sécurisée (canal chiffré).
- Vérification du type de fichier autorisé (ex : PDF).
- Contrôle d’intégrité du fichier.

### 2.2 Stockage

- Stockage sécurisé dans un espace isolé par cabinet (multi-tenant).
- Enregistrement des métadonnées en base :
  - Identifiant du dossier
  - Identifiant du tiers
  - Identifiant utilisateur
  - Date et heure
  - Statut (en attente, validé)
  - Délai détecté
  - Niveau de confiance si disponible

Les documents ne sont pas exposés directement aux modules externes.

---

## 3. Extraction automatisée des délais

### 3.1 Pipeline d’analyse

1. Récupération du document uploadé.
2. Extraction du texte.
3. Recherche ciblée de mentions « 90 jours » ou « 120 jours ».
4. Retour d’un résultat structuré :
   - Délai détecté (90 ou 120)
   - Indicateur de détection

Aucune interprétation juridique avancée n’est réalisée.

### 3.2 Gestion des cas ambigus

- Si plusieurs délais sont détectés, le système marque le statut comme nécessitant validation.
- Si aucun délai explicite n’est trouvé, le champ délai reste vide jusqu’à validation.

---

## 4. Modèle de données

Extension des entités :

- **ConventionContractuelle**
  - id
  - dossierId
  - tiersId
  - cheminDocument
  - delaiDetecte (90, 120, null)
  - delaiValide (60, 90, 120)
  - statut
  - createdAt
  - createdBy
  - validatedAt
  - validatedBy

Le module Délais de paiement interroge `delaiValide` pour ses calculs.

---

## 5. Intégration avec les modules aval

- Exposition d’un service interne permettant de récupérer le délai contractuel applicable par tiers.
- Si aucune convention validée n’existe, le service retourne automatiquement 60 jours.
- Le moteur de règles peut utiliser cette donnée comme paramètre d’analyse.

---

## 6. Sécurité et audit

- Contrôle d’accès basé sur les rôles.
- Journalisation des événements : upload, détection, validation, modification.
- Isolation stricte des données par cabinet.
- Aucune suppression physique définitive sans traçabilité (logique de conservation conforme aux exigences de confidentialité).

---

## 7. Contraintes techniques

- Robustesse face à des documents de formats variables.
- Performance acceptable même en cas d’analyses successives sur plusieurs conventions.
- Architecture extensible pour supporter ultérieurement d’autres types de clauses contractuelles.