## Spécification Technique – Ticket : Développer le module d’import Excel

### 1. Architecture et responsabilités

Implémentation d’un service dédié d’import Excel intégré au module « Import & Gestion des données ».

Composants principaux :

- ExcelParsingService
- SchemaValidationService
- LineValidationService
- ImportReportBuilder

Ce service s’intègre dans le pipeline défini dans la spécification technique de la fonctionnalité « Import Excel Sage ».

---

### 2. Parsing du fichier Excel

#### 2.1 Lecture

- Utilisation d’une librairie robuste de lecture Excel.
- Support des formats standards Excel compatibles avec les exports Sage.
- Lecture structurée de la feuille principale.

#### 2.2 Mapping des colonnes

- Définition d’un schéma de référence configurable (liste des colonnes obligatoires).
- Mécanisme de correspondance tolérant :
  - Insensible à l’ordre des colonnes.
  - Tolérance limitée aux variations mineures de libellés.

En cas d’échec du mapping obligatoire :
- Lancement d’une exception fonctionnelle bloquante.

---

### 3. Validation du schéma

- Comparaison des colonnes détectées avec le schéma de référence.
- Identification des colonnes manquantes.
- Retour d’un objet d’erreur structuré en cas de non-conformité.

Aucun traitement ligne par ligne n’est exécuté si la validation du schéma échoue.

---

### 4. Validation ligne par ligne

Pour chaque ligne :

1. Parsing des champs (date, compte, montants, tiers).
2. Conversion vers types internes normalisés.
3. Validation des champs obligatoires.
4. Capture des erreurs via un mécanisme centralisé.

Structure recommandée :

- Objet LineValidationResult :
  - lineNumber
  - status (VALID / REJECTED)
  - errorMessages[]
  - mappedData (si valide)

Les lignes valides sont conservées en mémoire ou en stockage temporaire jusqu’à confirmation.
Les lignes rejetées sont stockées avec leur motif.

---

### 5. Construction du rapport d’import

Création d’un objet ImportReport :

- totalLines
- validLinesCount
- rejectedLinesCount
- rejectedLinesDetails[] (lineNumber, errorMessages)

Ce rapport est retourné à la couche applicative pour affichage.

---

### 6. Gestion des erreurs

#### 6.1 Erreurs bloquantes

- Fichier illisible.
- Format invalide.
- Colonnes obligatoires absentes.

Implémentation via exceptions métier spécifiques (ex: InvalidSchemaException).

#### 6.2 Erreurs non bloquantes

- Gérées via accumulation dans LineValidationResult.
- Ne stoppent pas le traitement global.

---

### 7. Performance et volumétrie

- Traitement optimisé pour environ 5000 lignes.
- Boucle de traitement efficace sans opérations lourdes par ligne.
- Éviter les accès base de données dans la boucle de validation.
- Possibilité d’exécution asynchrone selon l’architecture globale.

Objectif : traitement stable et prévisible sur la volumétrie cible.

---

### 8. Sécurité et isolation

- Aucune persistance définitive sans validation globale.
- Respect de l’isolation multi-tenant via contexte dossier/cabinet transmis au service.
- Aucune exposition directe des fichiers bruts.

---

### 9. Extensibilité

- Schéma de référence configurable pour supporter futures évolutions d’exports Sage.
- Architecture modulaire permettant d’ajouter de nouveaux formats d’import ultérieurement.

Ce ticket pose les fondations techniques du moteur d’import Excel, utilisé par l’ensemble des dossiers du système.