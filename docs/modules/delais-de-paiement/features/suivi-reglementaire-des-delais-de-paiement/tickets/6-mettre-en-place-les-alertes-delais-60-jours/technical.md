## Ticket : Implémentation technique des alertes délais > 60 jours

### 1. Composants impactés

- Module Délais de paiement
- Moteur de règles
- Module Import (données factures)
- Module Conventions (données structurées délais 90/120 jours)
- Base de données (entité AnomalieDelai)

---

### 2. Flux de traitement

#### 2.1 Déclenchement

Le traitement est exécuté :

- À la fin du processus d’import des écritures.
- Lors d’un recalcul manuel (API interne dédiée).

Avant exécution :

- Suppression ou invalidation des anomalies délai existantes du dossier (pour éviter les doublons lors d’un recalcul).

---

### 3. Sélection des données

Requête sur les factures :

- Factures appartenant au dossier courant.
- Statut = ouverte (non soldée ou partiellement soldée).
- Jointure avec :
  - Entité Tiers.
  - Entité Convention active éventuelle.

---

### 4. Algorithme de calcul

Pour chaque facture sélectionnée :

1. Déterminer la date de référence :
   - Si `dateEcheance` non nulle → utiliser `dateEcheance`.
   - Sinon → utiliser `dateFacture`.

2. Calculer :
   ```
   nbJours = dateAnalyse - dateReference
   ```
   (résultat en nombre entier de jours)

3. Déterminer le seuil applicable :
   - Si convention valide détectée → seuil = valeurConvention (90 ou 120).
   - Sinon → seuil = seuilReglementaire (60 par défaut, paramétrable via moteur de règles).

4. Si `nbJours > seuil` → créer une anomalie.

---

### 5. Modèle de données – AnomalieDelai

Champs minimaux :

- id (UUID)
- dossierId
- factureId
- tiersId
- type (ENUM : REGLEMENTAIRE, CONTRACTUELLE)
- seuilApplique (integer)
- nbJoursConstates (integer)
- montant (decimal)
- statut (ENUM : A_TRAITER, VALIDE, JUSTIFIE)
- createdAt (timestamp)
- updatedAt (timestamp)

Index recommandés :

- (dossierId)
- (factureId)
- (statut)

---

### 6. Typologie d’anomalie

Détermination du type :

- Si aucune convention → type = REGLEMENTAIRE.
- Si convention présente → type = CONTRACTUELLE.

La logique doit être indépendante de l’interface utilisateur.

---

### 7. Intégration moteur de règles

- Le seuil réglementaire (60 jours) doit être récupéré via le moteur de règles.
- Les seuils contractuels proviennent des données structurées des conventions.
- Le moteur de règles doit permettre l’évolution future des seuils sans modification du code métier.

---

### 8. Performance

- Traitement optimisé pour environ 5000 lignes par dossier.
- Utilisation de requêtes batch plutôt que traitement unitaire persistant.
- Création des anomalies en lot si possible.

---

### 9. Sécurité et isolation

- Toutes les requêtes filtrées par dossierId.
- Respect de l’isolation logique multi-cabinets.
- Aucune modification des écritures comptables.

---

### 10. Journalisation

- Log technique du lancement du calcul (import ou recalcul manuel).
- Les changements de statut seront gérés dans un autre ticket (workflow), mais la structure doit être compatible avec la journalisation automatique.

---

### 11. Points d’extension futurs

- Ajout d’un niveau de gravité.
- Intégration au scoring global du dossier.
- Ajout d’autres types d’alertes liées aux délais.