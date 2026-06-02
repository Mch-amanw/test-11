# Spécification Fonctionnelle – Fonctionnalité : Import Excel Sage

## 1. Objectif métier

Permettre l’import sécurisé des fichiers Excel standardisés exportés depuis Sage afin d’intégrer les écritures comptables d’un dossier dans Comptalyses.

Cette fonctionnalité constitue l’étape initiale obligatoire avant tout traitement (contrôle TVA, analyse des délais de paiement, détection d’anomalies, utilisation du chatbot IA). Elle garantit que seules des données conformes et traçables alimentent les modules aval.

---

## 2. Périmètre

La fonctionnalité couvre :

- Le téléversement d’un fichier Excel exporté depuis Sage.
- La vérification du format attendu (structure des colonnes).
- La validation des données essentielles ligne par ligne.
- La génération d’un rapport d’import détaillé.
- La confirmation explicite de l’intégration dans le dossier.
- L’enregistrement d’une nouvelle version d’import associée au dossier et à une période.

Ne sont pas inclus :
- Les contrôles TVA avancés.
- Les analyses de délais de paiement.
- Le scoring ou la détection métier d’anomalies (réalisés par le moteur de règles).

---

## 3. Description fonctionnelle détaillée

### 3.1 Sélection du dossier et de la période

Avant l’import :
- L’utilisateur sélectionne un dossier existant.
- Il précise la période concernée (mensuelle ou trimestrielle selon le régime TVA).

Règle : un import est toujours rattaché à un dossier et à une période.

---

### 3.2 Upload du fichier Excel

- Upload manuel via interface web sécurisée.
- Seuls les fichiers Excel issus de Sage (format standard attendu) sont acceptés.
- Vérification de l’intégrité du fichier (non corrompu).

Erreurs bloquantes possibles :
- Format de fichier non conforme.
- Fichier illisible ou corrompu.

En cas d’erreur bloquante, l’import est refusé.

---

### 3.3 Vérification du format (validation du schéma)

Le système vérifie :
- La présence des colonnes attendues.
- La cohérence globale de la structure.

Si des colonnes obligatoires sont absentes, l’import est bloqué.

Un message clair indique :
- Les colonnes manquantes.
- Les anomalies de structure détectées.

---

### 3.4 Validation des données ligne par ligne

Pour chaque ligne comptable :

Contrôles minimaux :
- Présence des champs essentiels (date, compte, montants, identifiant tiers si applicable).
- Validité des types (date valide, valeurs numériques pour montants).

Classification des anomalies :
- Erreurs bloquantes globales (structure invalide).
- Erreurs ligne par ligne (ligne rejetée, import global possible).

Les lignes invalides sont :
- Exclues de l’intégration.
- Listées dans le rapport avec le motif de rejet.

---

### 3.5 Rapport d’import

À l’issue de l’analyse, un rapport est affiché à l’utilisateur :

- Nombre total de lignes lues.
- Nombre de lignes valides.
- Nombre de lignes rejetées.
- Liste détaillée des erreurs (par ligne si applicable).

L’utilisateur doit confirmer explicitement l’import pour valider l’intégration des lignes valides.

---

### 3.6 Validation et versioning

Après confirmation :
- Les écritures valides sont enregistrées.
- Une nouvelle version d’import est créée pour le dossier.
- Les versions précédentes restent accessibles en lecture seule.

Règles :
- Toute nouvelle importation sur une même période crée une nouvelle version.
- L’historique des imports (date, utilisateur, période) est conservé.

---

## 4. Règles de gestion

- Un fichier dont la structure est non conforme ne peut pas être intégré.
- Un import doit être explicitement confirmé par l’utilisateur.
- Chaque import est associé à un utilisateur identifié (traçabilité).
- Les données importées ne peuvent pas être modifiées directement : toute correction passe par le module de gestion des écritures.
- Les données d’un cabinet sont isolées logiquement des autres cabinets.

---

## 5. Critères d’acceptation

1. ✅ Un fichier Excel conforme (~5000 lignes) peut être importé sans blocage fonctionnel.
2. ✅ En cas de colonne obligatoire manquante, l’import est refusé avec message explicite.
3. ✅ Les lignes contenant des erreurs de type sont rejetées individuellement et listées.
4. ✅ Le rapport d’import affiche correctement les totaux (lignes lues, valides, rejetées).
5. ✅ Une confirmation utilisateur est nécessaire avant intégration définitive.
6. ✅ Chaque import crée une nouvelle version historisée du dossier.
7. ✅ Les données importées sont accessibles aux modules TVA, délais et moteur de règles.
8. ✅ Toutes les actions sont tracées dans le journal d’audit.

---