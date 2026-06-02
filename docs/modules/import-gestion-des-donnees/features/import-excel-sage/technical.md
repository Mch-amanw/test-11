# Spécification Technique – Fonctionnalité : Import Excel Sage

## 1. Composants impactés

- Interface Web (écran d’import).
- Service d’upload sécurisé.
- Service de parsing Excel.
- Service de validation de schéma.
- Service de validation ligne par ligne.
- Base de données (entités Import, Écriture comptable, Dossier).
- Module Journal d’audit.
- Services internes exposant les données normalisées.

---

## 2. Pipeline technique détaillé

### 2.1 Upload

- Transmission via canal sécurisé.
- Vérification du type MIME et de l’extension.
- Stockage temporaire avant validation.

---

### 2.2 Lecture et parsing

- Lecture structurée des feuilles pertinentes.
- Mapping des colonnes vers le modèle interne attendu.
- Gestion robuste des variations mineures de format (ordre de colonnes, libellés proches).

---

### 2.3 Validation du schéma

- Vérification de la présence des colonnes obligatoires.
- Comparaison avec un schéma de référence configurable.
- En cas d’échec : arrêt du pipeline et retour d’erreur.

---

### 2.4 Validation ligne par ligne

Pour chaque ligne :

- Parsing des dates vers un format interne standard.
- Conversion des montants en types numériques.
- Vérification des champs obligatoires.
- Attribution d’un statut : valide / rejetée.

Les lignes rejetées sont stockées avec :
- Numéro de ligne.
- Motif du rejet.

---

### 2.5 Stockage et versioning

Après confirmation utilisateur :

- Création d’un enregistrement Import (métadonnées : dossier, période, utilisateur, date).
- Création des entités Écriture comptable associées à la version.
- Les versions précédentes restent intactes (lecture seule).

Aucun traitement analytique n’est déclenché avant validation complète de l’import.

---

## 3. Performance

- Optimisation pour environ 5000 lignes par fichier.
- Traitement potentiellement asynchrone pour éviter le blocage de l’interface.
- Retour d’un état d’avancement si traitement long.

---

## 4. Sécurité

- Isolation logique des données par cabinet (multi-tenant).
- Vérification des permissions : seuls les utilisateurs autorisés peuvent importer.
- Journalisation des événements : upload, validation, confirmation.
- Aucun accès externe direct aux fichiers bruts.

---

## 5. Intégrations internes

Une fois validées, les données sont exposées via services internes pour :

- Module TVA (agrégats et contrôles).
- Module Délais de paiement.
- Moteur de règles.
- Assistant IA (accès contrôlé aux données du dossier).

La fonctionnalité agit exclusivement comme couche d’ingestion et de normalisation.

---

## 6. Contraintes techniques

- Robustesse face aux variations mineures d’exports Sage.
- Maintien de la traçabilité complète (versioning).
- Respect strict des exigences de confidentialité et d’isolation des données.

---

"contextSummary": "La fonctionnalité Import Excel Sage permet d’ingérer les écritures comptables exportées depuis Sage dans un dossier Comptalyses. Elle couvre l’upload sécurisé, la validation du format, le contrôle ligne par ligne des données essentielles et la génération d’un rapport d’import. Chaque import est rattaché à un dossier et à une période, crée une nouvelle version historisée et nécessite une confirmation explicite de l’utilisateur. Les données validées sont ensuite mises à disposition des modules TVA, délais de paiement, moteur de règles et assistant IA, sans réaliser d’analyse métier à ce stade. La traçabilité, le versioning et l’isolation des données par cabinet sont des exigences centrales."}
}]}