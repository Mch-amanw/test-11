# Ticket : Générer le brouillon TVA exportable DGI

## 1. Composants concernés

- Module TVA & Déclarations
- Moteur de règles
- Module Workflow
- Module Audit
- Couche d’accès aux données (écritures, périodes, dossiers)

---

## 2. Calcul des agrégats

### 2.1 Source des données

- Sélection des écritures liées au dossier et à la période.
- Filtrage sur les écritures prises en compte pour la TVA.

### 2.2 Logique de calcul

Le service de calcul doit :

1. Charger les écritures validées de la période.
2. Agréger :
   - TVA collectée
   - TVA déductible
3. Calculer le solde (collectée – déductible).
4. Produire un objet AgrégatTVA normalisé.

Le calcul doit être :

- Idempotent.
- Rejouable à tout moment.
- Optimisé pour des volumes d’environ 5000 lignes.

---

## 3. Génération du brouillon (snapshot)

### 3.1 Structure interne

À partir de l’objet AgrégatTVA :

- Application du mapping interne vers la structure déclarative DGI.
- Construction d’un objet `DeclarationTVA` avec :
  - dossierId
  - periodeId
  - regime
  - version
  - statut initial (brouillon)
  - structureDeclaration (snapshot sérialisé)
  - dateGeneration
  - generePar

### 3.2 Versioning

- Incrément automatique du numéro de version par période.
- Les versions précédentes sont conservées.
- Aucune modification d’un snapshot existant n’est autorisée.

---

## 4. Génération du fichier exportable

### 4.1 Source

- Lecture exclusive du champ `structureDeclaration` du snapshot versionné.

### 4.2 Mapping et format

- Utilisation d’une table de correspondance interne entre champs métier et champs DGI.
- Génération d’un fichier conforme à la structure attendue par le portail DGI (format prédéfini au niveau projet).

### 4.3 Contrôles techniques

Avant génération :

- Vérification de l’existence d’un snapshot valide.
- Vérification des permissions utilisateur.
- Vérification du statut compatible avec export.

---

## 5. Sécurité et permissions

- Vérification des rôles côté backend.
- Isolation stricte des données par cabinet.
- Interdiction d’accès à une déclaration hors périmètre du cabinet de l’utilisateur.

---

## 6. Journalisation

Les événements suivants doivent être enregistrés :

- Calcul des agrégats.
- Création d’une nouvelle version de déclaration.
- Génération d’un fichier d’export.

Chaque log inclut :

- userId
- rôle
- dossierId
- periodeId
- version
- horodatage

---

## 7. Contraintes techniques

- Cohérence stricte entre agrégats et fichier exporté.
- Performance adaptée à des dossiers d’environ 5000 écritures.
- Respect de la conformité réglementaire marocaine.
- Architecture compatible avec l’ajout futur d’autres types de déclarations fiscales.