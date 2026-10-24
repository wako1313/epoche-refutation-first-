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
