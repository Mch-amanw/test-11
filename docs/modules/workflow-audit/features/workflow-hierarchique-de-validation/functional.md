# Spécification Fonctionnelle – Workflow hiérarchique de validation

## 1. Objectif métier

Mettre en place un processus structuré de validation hiérarchique des dossiers comptables selon le schéma :

**Collaborateur → Chargé de dossier → Expert-comptable**

Ce workflow vise à :

- Sécuriser la préparation des déclarations de TVA
- Garantir un contrôle intermédiaire avant validation finale
- Assurer la responsabilité professionnelle de l’Expert
- Empêcher tout export ou envoi de déclaration sans validation experte explicite
- Renforcer la traçabilité et la conformité réglementaire

---

## 2. Périmètre de la fonctionnalité

La fonctionnalité couvre :

- La gestion des statuts d’un dossier
- Les transitions entre les rôles
- Les validations intermédiaires et finales
- Les renvois pour correction
- Le blocage de l’export tant que la validation finale n’est pas obtenue

Elle s’applique à tout dossier faisant l’objet d’une préparation de déclaration TVA.

---

## 3. États du dossier

Un dossier peut prendre les états suivants :

1. **En préparation (Collaborateur)**
2. **En revue (Chargé)**
3. **En validation finale (Expert)**
4. **Validé**
5. **Renvoyé pour correction**

L’état courant est visible par les utilisateurs autorisés.

---

## 4. Règles de gestion

### 4.1 Règles générales

- Un dossier doit suivre l’ordre hiérarchique défini.
- Une transition vers l’étape suivante nécessite une action explicite de validation.
- Seul le rôle correspondant peut effectuer la validation à son niveau.
- Toute validation doit être associée à l’utilisateur authentifié.

### 4.2 Validation par le Collaborateur

- Le Collaborateur prépare le dossier (analyse anomalies, corrections, etc.).
- Il peut soumettre le dossier au Chargé.
- Une fois soumis, le dossier passe à l’état **En revue**.

### 4.3 Validation par le Chargé

- Le Chargé peut :
  - Valider et transmettre à l’Expert (→ En validation finale)
  - Renvoyer au Collaborateur (→ Renvoyé pour correction)
- En cas de renvoi, un commentaire explicatif est obligatoire.

### 4.4 Validation par l’Expert

- L’Expert peut :
  - Valider définitivement le dossier (→ Validé)
  - Renvoyer au Chargé ou au Collaborateur avec commentaire obligatoire
- Seule la validation par l’Expert autorise l’export de la déclaration TVA.

### 4.5 Modifications après validation

- Toute modification significative après validation intermédiaire peut nécessiter une nouvelle validation.
- Toute modification après validation finale invalide automatiquement le statut **Validé** et replace le dossier dans un état nécessitant validation.

### 4.6 Blocage de l’export

- L’export de la déclaration TVA est strictement interdit tant que le dossier n’est pas au statut **Validé**.
- Aucun contournement manuel n’est autorisé.

---

## 5. Critères d’acceptation

- ✅ Un Collaborateur ne peut pas valider un dossier au niveau Chargé ou Expert.
- ✅ Un Chargé ne peut pas effectuer la validation finale.
- ✅ L’Expert est le seul à pouvoir autoriser l’export.
- ✅ Un dossier ne peut pas sauter une étape.
- ✅ Un renvoi impose un commentaire obligatoire.
- ✅ Toute validation est enregistrée avec utilisateur, rôle et horodatage.
- ✅ Une modification après validation finale invalide le statut validé.

---

## 6. Cas d’usage principaux

1. Préparation par Collaborateur → Validation Chargé → Validation Expert → Export autorisé.
2. Préparation → Rejet par Chargé → Correction → Nouvelle soumission.
3. Validation finale → Modification d’écriture → Dossier automatiquement réouvert.

---

## 7. Contraintes spécifiques

- Le processus doit être simple, explicite et non ambigu.
- Les actions doivent être traçables via le journal d’audit.
- Le système doit empêcher toute escalade de privilèges.

---

## 8. Impact sur la responsabilité professionnelle

La validation experte constitue un acte formel engageant la responsabilité professionnelle. La plateforme doit garantir que cette validation est explicite, traçable et non falsifiable.