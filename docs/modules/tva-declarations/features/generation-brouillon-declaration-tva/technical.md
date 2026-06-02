# Fonctionnalité : Génération du brouillon de déclaration TVA

## 1. Composants impactés

- Module TVA (agrégats et calculs)
- Moteur de règles (état des anomalies)
- Module Gestion des écritures (déclenchement recalcul)
- Module Workflow (statuts et validations)
- Module Audit (journalisation)

---

## 2. Modèle de données spécifique

Entité principale :

### DéclarationTVA
- id
- dossierId
- periodeId
- regime (mensuel / trimestriel)
- version
- statut (brouillon / en revue / validé expert)
- dateGeneration
- generePar (userId)
- structureDeclaration (snapshot structuré des champs déclaratifs)

Relations :
- Une DéclarationTVA est liée à une Période.
- Plusieurs versions peuvent exister pour une même période.

---

## 3. Pipeline de génération

### 3.1 Étapes techniques

1. Vérification des permissions (backend).
2. Chargement des agrégats TVA pour la période.
3. Vérification de la présence d’anomalies critiques.
4. Mapping agrégats → structure déclarative DGI.
5. Création d’un snapshot immuable (versionnée).
6. Enregistrement en base avec incrément de version.
7. Journalisation de l’événement.

Le snapshot garantit que la déclaration exportée correspond exactement aux données à un instant donné.

---

## 4. Mécanisme de versioning

- Chaque recalcul déclenche la création d’une nouvelle version.
- Les versions précédentes sont conservées pour audit.
- Le numéro de version est incrémental par période.

Aucune modification directe d’une version existante n’est autorisée (immutabilité logique).

---

## 5. Export DGI

### 5.1 Mapping

- Table de correspondance interne entre champs agrégés et champs requis par la DGI.
- Structure d’export conforme au format attendu (structure prédéfinie).

### 5.2 Génération du fichier

- Génération dynamique à partir du snapshot stocké.
- Contrôle de cohérence avant génération.
- Journalisation de l’export (userId, rôle, timestamp, version).

Le module ne gère pas la transmission au portail DGI.

---

## 6. Gestion des performances

- Optimisation des requêtes d’agrégation.
- Réutilisation des agrégats déjà calculés lorsque possible.
- Traitement adapté à des volumes d’environ 5000 lignes.

---

## 7. Sécurité

- Vérification systématique des rôles côté backend.
- Isolation des données par cabinet.
- Accès restreint aux snapshots selon rôle.
- Journalisation complète des générations et exports.

---

## 8. Contraintes

- Conformité stricte à la réglementation fiscale marocaine.
- Compatibilité avec le format requis par la DGI.
- Cohérence obligatoire avec le régime configuré au niveau du dossier.