## Spécification Fonctionnelle – Ticket : Développer le module d’import Excel

### 1. Objectif du ticket

Implémenter le cœur fonctionnel du module d’import Excel Sage, incluant :

- Le parsing du fichier Excel.
- La validation du format standard attendu.
- La validation des données ligne par ligne.
- La gestion structurée des erreurs.
- La prise en charge de volumes d’environ 5000 lignes par dossier.

Ce ticket couvre le traitement interne du fichier après upload, conformément à la fonctionnalité « Import Excel Sage ».

---

### 2. Périmètre fonctionnel

Ce ticket inclut :

- Lecture du fichier Excel téléversé.
- Vérification de la conformité du schéma (colonnes obligatoires).
- Validation des champs essentiels pour chaque ligne.
- Classification des erreurs (bloquantes / ligne par ligne).
- Génération d’un rapport d’import exploitable par l’interface.

Ce ticket n’inclut pas :

- L’interface utilisateur complète.
- Les contrôles métier TVA avancés.
- Les analyses de délais de paiement.
- Le déclenchement des traitements aval.

---

### 3. Validation du format standard Sage

Le système doit :

- Vérifier la présence des colonnes obligatoires définies par le schéma de référence.
- Tolérer des variations mineures (ordre des colonnes, libellés proches si mappables).
- Refuser l’import si une colonne obligatoire est absente.

En cas d’erreur bloquante :

- Le fichier n’est pas intégré.
- Un message explicite est généré précisant les colonnes manquantes ou anomalies de structure.

---

### 4. Parsing et validation ligne par ligne

Pour chaque ligne du fichier :

- Lecture et mapping vers le modèle interne.
- Vérification de la présence des champs essentiels (date, compte, montants, identifiant tiers si applicable).
- Validation du type des données (date valide, montants numériques, champs non vides si obligatoires).

Chaque ligne reçoit un statut :

- **Valide** : intégrable après confirmation utilisateur.
- **Rejetée** : exclue de l’intégration avec motif explicite.

Les erreurs ligne par ligne n’empêchent pas l’import global si la structure est conforme.

---

### 5. Gestion des erreurs

Le système distingue :

1. **Erreurs bloquantes globales**
   - Fichier illisible ou corrompu.
   - Structure non conforme.
   - Colonnes obligatoires absentes.

2. **Erreurs ligne par ligne**
   - Donnée invalide.
   - Champ obligatoire manquant.
   - Type incorrect.

Les erreurs ligne par ligne sont :

- Enregistrées avec numéro de ligne et motif.
- Restituées dans le rapport d’import.

---

### 6. Rapport d’import

À l’issue du traitement, le module doit produire un objet rapport contenant :

- Nombre total de lignes lues.
- Nombre de lignes valides.
- Nombre de lignes rejetées.
- Liste détaillée des erreurs par ligne.

Ce rapport sera exploité par l’interface pour affichage et confirmation utilisateur.

---

### 7. Gestion du volume (~5000 lignes)

Le module doit être dimensionné pour :

- Traiter efficacement des fichiers d’environ 5000 lignes.
- Garantir un temps de traitement compatible avec une utilisation interactive.
- Éviter les blocages ou erreurs liés à la volumétrie cible.

La robustesse et la stabilité sur ce volume sont des exigences prioritaires.

---

### 8. Cohérence avec les règles de gestion

- Aucun traitement analytique métier (TVA, scoring, alertes) n’est réalisé à ce stade.
- Aucune écriture n’est persistée définitivement sans validation complète du processus d’import.
- Toutes les anomalies détectées doivent être traçables et exploitables par le journal d’audit ultérieurement.