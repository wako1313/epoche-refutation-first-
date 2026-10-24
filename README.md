# ÉPOCHÉ — Refutation-First Computation

Projet **Calcul Éidétique** : diagnostic de structure, valeur de l'information, abstention décisionnelle et routage de solveurs dans les limites d'un budget d'évaluations et de CPU.

## G2 : VERSION CANDIDATE v1.1 — NON SCELLÉE — NO_START

La proposition du 24 octobre 2026 a été comparée au protocole v1.0 ; une version consolidée **v1.1** est publiée sans effacer l'historique. Aucun résultat confirmatoire G2 n'est revendiqué.

- [Protocole intégral G2 candidat v1.1](preregistration/G2_PROTOCOL_CANDIDATE_v1.1.md)
- [Comparaison des versions et registre des modifications](preregistration/COMPARISON_AND_AMENDMENTS_v1.1.md)
- [Pilote de développement et calibration préalable](preregistration/G2_DEV_PILOT_CALIBRATION_PLAN_v1.0.md)
- [Protocole adverse séparé G2-adv v0.2](preregistration/G2_ADV_PROTOCOL_CANDIDATE_v0.2.md)
- [État exact des conditions bloquantes](preregistration/STATUS_NON_SEALED.md)

## Provenance du candidat précédent

- [Synthèse G2 v1.0](preregistration/G2_PROTOCOL_OVERVIEW_CANDIDATE_v1.0.md)
- [SHA256 historique du fichier original complet v1.0](preregistration/SHA256_PROTOCOLE_CANDIDAT.txt)

**Ne pas confondre :** les SHA/commits publics de ces candidats prouvent seulement l'existence de textes préparatoires. Le **gel confirmatoire de G2** exige le protocole final complet **et** le code exécutable, les jeux de développement/calibration, les paramètres, versions, graines, likelihoods, statistiques et empreintes, publiés **avant toute génération d'instances du holdout G2**.

Les archives historiques V10/POLYMATH et G1bis existent, mais leur seule présence ne constitue ni la calibration sur les cinq familles ni un routage G2 exécutable. Les décisions MORPHOSIS/EXTERNE/BOTTOM ne reçoivent aucune perte inventée.

## Gouvernance

H1–H4 sont des conditions strictes d'autorisation de G3. G2-adv est une investigation **distincte** ; les résultats négatifs sont obligatoirement publiés. Aucune revendication de « nouveau paradigme démontré », de robustesse universelle ou de supériorité contre GOMEA/MaxSAT/MIP à ce stade.


## Résultats techniques du 24 octobre 2026 (DEV uniquement)

- [Rapport du pilote DEV](dev/DEV_PILOT_2026_10_24_RESULTS.md) : 300 exécutions équilibrées de V10 / sondes fixes+V10 / ÉPOCHÉ-DP, 20 exécutions POLYMATH seul et profilage incomplet (90/200).
- [Moyennes appariées des 20 instances](dev/DEV_20_INSTANCE_PAIRED_MEANS.csv).
- [Amendement candidat v1.2, non scellé](preregistration/G2_AMENDMENT_CANDIDATE_v1.2.md).

Le vrai contrôleur ÉPOCHÉ-DP a choisi V10 sur les 100 exécutions DEV ; aucun gain de score par rapport à V10 dans cette expérience. L'archive complète du code et des traces est disponible comme artefact de conversation, identifiée par le SHA256 `0acc91e6da6b2d84e6ff3bb5dd230af86bad0eb91cd3fb7622891ebe059a0b72`, **mais pas encore intégralement chargée sur GitHub**. Aucun holdout G2 n'a été généré.


## Contrôles DP et falsification DEV de H2 (24 octobre 2026)

- [Contrôles à croyance concentrée, simulateur de sonde parfaite et résultats appariés](dev/DP_CONSISTENCY_ORACLE_DEV_2026_10_24.md).
- [Amendement H2 v1.3 : CANDIDAT NON SCELLÉ](preregistration/G2_H2_AMENDMENT_CANDIDATE_v1.3.md).
- [Spécification D₂, sans développement de sonde](dev/D2_PARSIMONY_SPEC_CANDIDATE.md).

Croyance 99 % sparse → POLYMATH ; 99 % dense → V10 (tests réussis). Un diagnostic **artificiellement parfait** avec 32 évaluations réellement facturées produit sur DEV +0,001431 de gain de loss moyen (20 instances, cinq graines) si linéaire et sparse sont favorisés, mais présente quatre instances ayant au moins une violation H4. Aucun gain de véritable sonde n'est démontré et **les 200 instances G2 n'ont pas été générées**.

Archive de conversation reproductible (non importée intégralement dans GitHub) : `EPOCHE_G1bis_H2_contrepreuves_DEV_2026_10_24.zip`, SHA256 `22ba1ee58a9a3032ebd6a03122781ac0c3696537b51a82063fca2bbba2d2bcf7`, manifeste de 80 fichiers vérifiés et huit tests réussis.

## v0.3 — Résultat DEV du coût d'opportunité (24 octobre 2026)

- [Rapport de contre-épreuve V10/cache, preuve algébrique et pilote corrigé](dev/RVOI_v03_BUDGET_CACHE_DEV_RESULTS_2026_10_24.md).
- Les 800 profils V10 montrent 100/100 trajectoires monotones, mais seulement **7/100 convexes**.
- Les **4/20 instances H4** du test artificiel précédent retombent à **0/20** lorsque l'on maintient **90 itérations V10** et que l'on comptabilise les évaluations physiques partagées avec le cache (max. 1190/1261).
- L'oracle artificiellement informé des familles améliore la loss moyenne DEV de **+0,001486**, sans sonde discriminante réelle ; **aucune garantie H4 ≤2 % ni H2 +0,002 n'est validée**.
- La RVoI budgétée doit faire apparaître le coût d'opportunité **une seule fois**, via le risque terminal au budget restant.
- Archive de reproduction complète (code + 800 profils + 100 replays) : `EPOCHE_v03_RVOI_budget_cache_DEV_2026_10_24.zip`, SHA256 `a9385c067d5536b28e1383e099f504afc0c7e498f0e0381b262c30c4693677d6`, disponible dans la conversation, **pas encore importée intégralement sur GitHub**.

**G2 reste NO_START ; les seuils H1–H4 ne sont pas changés.**
