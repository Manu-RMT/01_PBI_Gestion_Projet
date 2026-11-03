# Pilotage de projets – Dashboard Power BI

> **Projet de portfolio Data / Business Intelligence**  
> Tableau de bord Power BI consacré au suivi de la performance d'un portefeuille de projets : avancement, planning, budget, risques, qualité, ressources et livrables.

![Aperçu du tableau de bord – Synthèse](images/dashboard-synthese.png)

## Présentation

Ce projet consiste à concevoir un tableau de bord Power BI permettant à une direction de projet ou à un PMO de suivre la santé d'un portefeuille de projets et d'identifier rapidement les points nécessitant une action.

L'objectif est de transformer des données de pilotage opérationnel en indicateurs lisibles et exploitables pour faciliter :
- le suivi de l'avancement réel par rapport à l'avancement prévu ;
- l'analyse des écarts de délais et de coûts ;
- l'estimation du coût final des projets ;
- la surveillance des risques, incidents et non-conformités ;
- le suivi des tâches, jalons et livrables ;
- l'analyse de la charge et de la capacité des ressources.

Le rapport est organisé autour de cinq espaces :

| Page | Contenu |
|---|---|
| **Synthèse** | Vue portefeuille, indicateurs clés, courbe d'avancement, courbe en S, jalons et principaux points de vigilance. |
| **Planning et budget** | Suivi des tâches et jalons, analyse des coûts, budget initial, coûts réels, coûts engagés et prévisions à terminaison. |
| **Risques et qualité** | Risques ouverts et critiques, actions associées, incidents, contrôles qualité et non-conformités. |
| **Ressources et livrables** | Charge prévue/réelle, capacité disponible, taux d'utilisation, surcharge des ressources et suivi des livrables. |
| **Modèle et mesures** | Espace de consultation/documentation du modèle et des indicateurs DAX. |

## Aperçu

La page de synthèse rassemble notamment :
- l'avancement du portefeuille et son écart avec le prévisionnel ;
- le **SPI** (performance des délais) et le **CPI** (performance des coûts) ;
- l'**EAC** (coût estimé à terminaison) et son écart par rapport au budget initial ;
- le nombre de risques critiques ouverts et de tâches en retard ;
- l'évolution mensuelle de l'avancement réel/prévu et des valeurs EV, PV et AC ;
- les jalons à risque, les risques critiques et les signaux de vigilance.

## Modèle de données

Le modèle s'appuie sur une organisation dimensionnelle autour des projets, du calendrier et de tables de faits métier.

### Dimensions et tables de faits identifiées dans les mesures

| Domaine | Tables utilisées |
|---|---|
| Référentiels | `DimProjet`, `Dim_Calendrier` |
| Pilotage et coûts | `FactPilotageMensuel`, `FactCouts` |
| Planning | `FactTaches`, `FactJalons` |
| Ressources | `FactCharge` |
| Risques et qualité | `FactRisques`, `FactControlesQualite`, `FactNonConformites` |
| Suivi opérationnel | `FactIncidents`, `FactActions`, `FactChangements`, `FactDecisionProjet`, `FactLivrables` |

Des tables auxiliaires déconnectées sont également utilisées pour piloter certains visuels (notamment les cascades budgétaires et les KPI de jalons).

> Le modèle détaillé des relations, les cardinalités et le sens de filtrage sont à consulter dans le fichier Power BI. Cette documentation ne prétend pas les déduire uniquement des formules DAX.

## Indicateurs et logique métier

Le modèle contient **131 mesures DAX** dans l'extraction fournie. Elles sont regroupées fonctionnellement comme suit.

### 1. Avancement et pilotage du portefeuille
- Date du dernier reporting.
- Avancement réel et prévu moyen.
- Avancement réel et prévu du portefeuille, pondéré par le budget initial des projets visibles.
- Écart d'avancement en points.
- Interprétation de la courbe en S.

Pour les indicateurs portefeuille, l'approche consiste à retenir, projet par projet, le dernier snapshot disponible à la date sélectionnée ou avant celle-ci, puis à pondérer le taux d'avancement par le budget initial.

### 2. Gestion des coûts et Earned Value Management (EVM)
Les mesures couvrent les principaux indicateurs de la méthode de la valeur acquise :

| Indicateur | Signification | Principe de calcul |
|---|---|---|
| **BAC** | Budget at Completion | Budget initial total des projets visibles. |
| **PV** | Planned Value | Valeur planifiée issue du dernier snapshot retenu pour chaque projet. |
| **EV** | Earned Value | BAC × avancement réel du portefeuille. |
| **AC** | Actual Cost | Coût réel cumulé issu du dernier snapshot retenu pour chaque projet. |
| **SPI** | Schedule Performance Index | EV / PV. Une valeur supérieure à 1 indique une performance supérieure au prévisionnel. |
| **CPI** | Cost Performance Index | EV / AC. Une valeur inférieure à 1 indique une performance de coûts défavorable. |
| **SV** | Schedule Variance | EV − PV. |
| **CV** | Cost Variance | EV − AC. |
| **EAC** | Estimate at Completion | AC + (BAC − EV) / CPI, si le CPI est positif. |
| **ETC** | Estimate to Complete | EAC − AC. |
| **VAC** | Variance at Completion | BAC − EAC. |

Le rapport comprend également le suivi des coûts engagés, de l'exposition financière (AC + coûts engagés), du budget restant et des écarts budgétaires prévisionnels.

### 3. Planning, tâches et jalons
- Nombre total de tâches, tâches terminées, en cours et non démarrées.
- Tâches en retard et tâches critiques en retard.
- Retard moyen des tâches non terminées.
- Jalons échus, respectés, en retard, à risque et à venir.
- Prochain jalon et nombre de jours avant son échéance.
- Taux de respect des jalons.

### 4. Risques, qualité et incidents
- Risques critiques et majeurs ouverts.
- Score moyen et criticité des risques.
- Risques associés à des actions en retard.
- Nombre d'incidents ouverts, incidents résolus, taux et délai moyen de résolution.
- Contrôles qualité, taux accepté au premier contrôle et après reprise.
- Défauts critiques détectés.
- Non-conformités ouvertes/résolues et coût de non-qualité.
- Actions en retard et taux de résolution des actions.
- Décisions et demandes de changement en attente, avec leurs impacts estimés en coût et en délai.

### 5. Ressources et livrables
- Charge prévue et charge réelle en heures.
- Capacité disponible.
- Taux d'utilisation et taux de surcharge.
- Écart entre charge réelle et charge prévue.
- Nombre de ressources en surcharge.
- Nombre de livrables, livrables livrés, en retard et acceptés.
- Retard moyen de livraison.

### 6. Statuts, alertes et restitution
Des mesures complémentaires servent à :
- déterminer un statut de santé projet ;
- fournir des libellés d'interprétation métier ;
- attribuer des couleurs aux KPI selon des seuils ;
- alimenter des visuels HTML personnalisés ;
- alimenter des graphiques en cascade et des KPI de synthèse.

Les seuils d'alerte sont définis dans les mesures DAX et peuvent être adaptés aux règles de gouvernance de l'organisation.

## Technologies

- **Power BI Desktop** – conception du rapport et des visualisations.
- **DAX** – calcul des indicateurs, logique temporelle, statuts et interprétations.
- **Power Query** – préparation et transformation des données (à adapter selon les sources intégrées).
- **Modélisation dimensionnelle** – séparation des dimensions et des faits pour structurer l'analyse.

## Objectif du projet

Ce portfolio illustre la capacité à concevoir un reporting de pilotage de projet allant au-delà de la visualisation : structuration des données, définition d'indicateurs métier, calculs DAX, suivi de la performance et restitution d'informations utiles à la décision.

---

*Projet personnel de démonstration – les indicateurs, seuils et données doivent être adaptés avant toute utilisation dans un contexte opérationnel réel.*
