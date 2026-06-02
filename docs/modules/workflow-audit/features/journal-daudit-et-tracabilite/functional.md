# Spécification Fonctionnelle – Journal d’audit et traçabilité

## 1. Objectif métier

La fonctionnalité **Journal d’audit et traçabilité** vise à garantir une traçabilité complète, infalsifiable et exploitable de toutes les actions réalisées sur un dossier dans Comptalyses.

Elle permet :

- De sécuriser la responsabilité professionnelle de l’Expert-comptable
- De justifier les décisions prises (corrections, validations, exports)
- De répondre aux exigences de contrôle interne et réglementaire
- De reconstituer l’historique complet d’un dossier à tout moment

Cette fonctionnalité constitue un élément central de la gouvernance et de la conformité de la plateforme.

---

## 2. Périmètre fonctionnel

Le journal d’audit couvre l’ensemble des actions significatives réalisées sur un dossier, notamment :

### 2.1 Actions tracées

Sont obligatoirement historisées :

- Import de fichiers comptables
- Upload de conventions contractuelles
- Modifications d’écritures
- Propositions et validations de corrections
- Détections et résolutions d’anomalies
- Changements de statut du dossier
- Validations intermédiaires (Chargé) et finales (Expert)
- Génération et export de déclaration TVA
- Interactions sensibles déclenchées depuis le module IA
- Tentatives d’actions non autorisées

Toute action considérée comme critique dans le workflow doit générer une entrée d’audit.

---

## 3. Informations enregistrées

Chaque événement d’audit contient au minimum :

- Identifiant du dossier
- Identifiant de l’utilisateur
- Rôle actif au moment de l’action
- Date et heure (horodatage)
- Type d’action
- Entité concernée (écriture, déclaration, dossier, document…)
- Ancienne valeur / nouvelle valeur (si modification)
- Commentaire associé si applicable (ex : motif de renvoi)

Les informations doivent permettre de reconstituer précisément le contexte et la portée de l’action.

---

## 4. Consultation du journal

### 4.1 Accès

- L’accès au journal est restreint selon le rôle.
- Les utilisateurs ne peuvent consulter que les dossiers auxquels ils ont accès.
- L’Expert dispose d’un accès complet à l’historique du dossier.

### 4.2 Visualisation

- Affichage chronologique des événements.
- Possibilité de filtrer par :
  - Type d’action
  - Utilisateur
  - Période
- Visualisation détaillée d’un événement (avant/après si applicable).

Aucune suppression ni modification d’entrée n’est autorisée.

---

## 5. Règles de gestion

- Toute transition de statut doit générer automatiquement une entrée d’audit.
- Toute modification d’écriture après validation intermédiaire ou finale doit être tracée et peut invalider l’état validé du dossier.
- Une déclaration ne peut être exportée sans génération préalable d’une trace d’audit.
- Le journal est en écriture uniquement (append-only).
- Aucune entrée ne peut être supprimée ou altérée via l’interface applicative.
- Les actions doivent être tracées même si elles échouent (ex : tentative d’accès non autorisée).

---

## 6. Critères d’acceptation

- ✅ Toute action critique déclenche automatiquement une entrée d’audit.
- ✅ Les informations affichées permettent de comprendre qui a fait quoi, quand, sur quel élément.
- ✅ Un Expert peut reconstituer l’historique complet d’un dossier.
- ✅ Aucune suppression ou modification d’événement n’est possible.
- ✅ Les tentatives d’actions interdites sont visibles dans le journal.
- ✅ Les filtres (utilisateur, date, type) fonctionnent correctement.

---

## 7. Hors périmètre (MVP)

- Export externe du journal pour audit réglementaire.
- Signature électronique des validations.
- Paramétrage personnalisé du niveau de traçabilité par cabinet.

Ces éléments pourront être intégrés dans des évolutions ultérieures.