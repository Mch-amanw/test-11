# Fonctionnalité : Suivi réglementaire des délais de paiement

## 1. Objectif métier
Permettre au cabinet de contrôler automatiquement le respect des délais de paiement réglementaires marocains (60 jours), en tenant compte des conventions contractuelles valides autorisant des délais de 90 ou 120 jours.

Cette fonctionnalité vise à :
- Identifier rapidement les factures en dépassement.
- Sécuriser la conformité réglementaire du dossier.
- Aider les collaborateurs à prioriser les actions.
- Alimenter le scoring de risque global du dossier.

---

## 2. Périmètre
La fonctionnalité couvre :
- L’analyse des factures ouvertes (non soldées ou partiellement soldées) issues des données importées.
- Le calcul automatique du nombre de jours écoulés.
- L’application du seuil réglementaire ou contractuel.
- La génération d’alertes en cas de dépassement.

Elle ne couvre pas :
- La modification des écritures comptables.
- La modification des conventions contractuelles.

---

## 3. Règles de gestion

1. **Seuil réglementaire par défaut** :
   - Toute facture dépassant 60 jours déclenche une alerte réglementaire si aucune convention valide n’est associée au tiers.

2. **Prise en compte des conventions** :
   - Si une convention valide (90 ou 120 jours) est détectée pour le tiers concerné, le seuil applicable remplace le seuil de 60 jours.
   - Le système applique automatiquement le seuil contractuel détecté.

3. **Dépassement contractuel** :
   - Toute facture dépassant le seuil contractuel (90 ou 120 jours) déclenche une alerte contractuelle.

4. **Base de calcul** :
   - Le nombre de jours est calculé entre la date de facture (ou date d’échéance si disponible) et la date d’analyse du dossier.
   - Seules les factures ouvertes sont prises en compte.

5. **Typologie des alertes** :
   - Alerte réglementaire (>60 jours sans convention).
   - Alerte contractuelle (>90 ou >120 jours selon convention).

6. **Traçabilité** :
   - Toute validation ou justification d’un dépassement est enregistrée dans le journal d’audit.

7. **Workflow** :
   - Les anomalies doivent être consultables par le Collaborateur.
   - Le Chargé et l’Expert doivent pouvoir les valider ou les justifier avant validation finale du dossier.

---

## 4. Comportement attendu

- Affichage d’une liste des factures en dépassement avec :
  - Tiers concerné
  - Date facture / échéance
  - Nombre de jours constaté
  - Seuil applicable
  - Type d’alerte
- Indicateurs synthétiques :
  - Nombre total de factures en dépassement
  - Montant total concerné
  - Répartition clients / fournisseurs

---

## 5. Critères d’acceptation

- ✅ Une facture à 65 jours sans convention déclenche une alerte réglementaire.
- ✅ Une facture à 75 jours avec convention 90 jours ne déclenche pas d’alerte.
- ✅ Une facture à 95 jours avec convention 90 jours déclenche une alerte contractuelle.
- ✅ Les alertes sont visibles selon les rôles définis.
- ✅ Toute validation ou justification modifie le statut de l’anomalie et génère une entrée dans le journal d’audit.
- ✅ Aucun dépassement ne peut être ignoré sans action explicite (validation ou justification).

---

## 6. Contribution au projet

Cette fonctionnalité renforce la conformité réglementaire, réduit le risque juridique lié aux retards de paiement et alimente le score de risque global du dossier exploité par le cabinet.