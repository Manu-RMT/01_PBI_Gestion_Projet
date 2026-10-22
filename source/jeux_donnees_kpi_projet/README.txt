JEUX DE DONNÉES CSV — SUIVI DE PROJET / KPI POWER BI
======================================================

Contexte
--------
Données entièrement fictives, créées pour l'entraînement et la construction d'un tableau de bord Power BI.
Date de référence illustrative : 08/10/2026.
Montants en euros. Dates au format ISO YYYY-MM-DD. Encodage UTF-8 avec BOM (Excel-friendly).
Séparateur CSV : virgule.

Fichiers
--------
01_projets.csv                         Référentiel projets et budgets initiaux
02_taches_planning.csv                 Tâches, dates prévues/réelles, avancement et retards
03_jalons.csv                          Jalons et statut
04_historique_pilotage_mensuel.csv     Snapshots mensuels d'avancement et de coûts
05_couts_reels_engages.csv             Lignes de coûts, dépenses et engagements
06_ressources_charge.csv               Charge planifiée/réelle et capacité des ressources
07_risques_actions.csv                 Risques, criticité, statut et actions de mitigation
08_non_conformites.csv                 Non-conformités et coûts de non-qualité
09_livrables.csv                       Livrables, validation et blocages
10_demandes_changement.csv             Demandes de changement et impacts estimés
11_dictionnaire_kpi.csv                KPI, sources et formules suggérées

Clés de jointure
----------------
- project_id : clé commune aux tables.
- task_id, milestone_id, risk_id, nonconformity_id, deliverable_id, change_request_id : identifiants des lignes.
- snapshot_date / month / cost_date : dates d'analyse selon la table.

Conseils Power BI
-----------------
1. Importer chaque CSV via Obtenir les données > Texte/CSV.
2. Utiliser 01_projets.csv comme dimension projet, reliée aux tables via project_id.
3. Créer une table calendrier et la relier aux dates pertinentes; selon les besoins, certaines relations seront inactives.
4. Ne pas additionner des snapshots cumulatifs entre eux : sélectionner un snapshot/date.
5. Séparer les coûts comptabilisés des coûts engagés pour éviter le double comptage.
6. Les données sont synthétiques et certaines valeurs sont volontairement imparfaites pour illustrer des alertes; valider les règles métier avant tout usage réel.
7. Les pourcentages d'avancement au niveau projet doivent idéalement être pondérés (charge, durée, valeur acquise ou règle métier validée), pas une simple moyenne non pondérée.

KPI couverts
------------
Planning, avancement, écarts, jalons, coûts, budget consommé, coût final estimé,
ressources/charge, risques, qualité/non-qualité, livrables, demandes de changement.


Fichiers complémentaires
------------------------
12_incidents.csv                       Incidents, priorité, statut et temps de résolution
13_actions.csv                         Actions, responsables, échéances et clôture
14_controles_qualite.csv               Contrôles qualité et acceptation au premier passage
15_decisions_projet.csv                Décisions attendues, responsables et échéances

KPI supplémentaires couverts
-----------------------------
- Incidents ouverts/non résolus et délai de résolution
- Actions en retard et taux de clôture des actions
- Taux d'acceptation au premier passage et défauts critiques
- Décisions en attente, décisions en retard et décisions par priorité

Ces fichiers utilisent project_id comme clé commune avec 01_projets.csv.
