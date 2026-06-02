# Fonctionnalité : Génération du brouillon de déclaration TVA

## 1. Objectif métier

Permettre la production automatique d’un **brouillon pré-rempli de déclaration de TVA**, mensuelle ou trimestrielle, à partir des écritures comptables analysées et consolidées, afin de :

- Réduire les erreurs de saisie manuelle.
- Sécuriser la préparation de la déclaration.
- Gagner du temps lors du traitement des dossiers.
- Produire un fichier compatible avec les exigences du portail de la DGI marocaine.

Cette fonctionnalité s’inscrit dans le processus global : contrôle → correction → consolidation → validation → export.

---

## 2. Périmètre fonctionnel

### 2.1 Sélection de la période

L’utilisateur peut générer un brouillon pour :

- Une période mensuelle ou trimestrielle.
- Une période définie au niveau du dossier (régime configuré en amont).

Seules les écritures appartenant à la période sélectionnée et prises en compte dans le calcul TVA sont utilisées.

---

### 2.2 Pré-remplissage automatique

Le brouillon est généré à partir des agrégats calculés par le module TVA :

- TVA collectée
- TVA déductible
- Solde de TVA
- Autres champs requis par la structure déclarative marocaine (selon mapping interne)

Le système :

- Mappe automatiquement les agrégats internes vers les champs correspondants de la déclaration.
- Affiche les montants avec possibilité de consultation du détail par écriture.

Aucune saisie manuelle des montants principaux n’est requise pour la génération initiale.

---

### 2.3 Gestion des incohérences bloquantes

Avant génération finale du brouillon :

- Le système vérifie la présence d’anomalies critiques non traitées.
- Si des anomalies critiques existent, un avertissement est affiché.

Règle de gestion :
- Une déclaration comportant des anomalies critiques doit être soit corrigée, soit explicitement validée dans le workflow avant validation experte finale.

---

### 2.4 Consultation du brouillon

L’utilisateur peut :

- Visualiser le brouillon sous forme structurée.
- Accéder au détail des montants par catégorie.
- Identifier les écritures contributrices à chaque agrégat.

Chaque génération de brouillon est historisée.

---

### 2.5 Export compatible DGI

Le système permet :

- L’export du brouillon dans un format compatible avec le portail de la DGI marocaine.
- La génération d’un fichier prêt à être déposé (sans envoi automatique).

Règles :
- Seul un dossier ayant atteint l’étape appropriée dans le workflow peut être exporté.
- L’export est tracé dans le journal d’audit.

---

### 2.6 Intégration au workflow

- Le Collaborateur peut générer le brouillon.
- Le Chargé peut le revoir et valider la cohérence.
- Seul l’Expert peut valider définitivement la déclaration.

Aucune déclaration ne peut être considérée comme "validée" sans action explicite d’un Expert.

---

## 3. Règles de gestion spécifiques

- Le brouillon repose uniquement sur les écritures de la période sélectionnée.
- Toute modification d’écriture déclenche un recalcul des agrégats.
- Une nouvelle version du brouillon est générée après recalcul.
- Chaque version est historisée avec date, utilisateur et rôle.
- L’export ne modifie pas les données comptables.

---

## 4. Critères d’acceptation

- ✅ Le système génère un brouillon cohérent avec les agrégats calculés.
- ✅ Les montants correspondent aux écritures de la période sélectionnée.
- ✅ Le régime (mensuel/trimestriel) est respecté.
- ✅ Le fichier exporté est compatible avec le format requis par la DGI.
- ✅ L’export et les validations sont tracés dans le journal d’audit.
- ✅ Une validation experte est nécessaire avant statut final validé.