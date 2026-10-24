# ÉPOCHÉ — Statut public des campagnes

**Date :** 2026-10-24. **Statut : G2 NO_START, G2-adv NO_START.**

## Documents accessibles

- Protocole G2 candidat v1.1 : [G2_PROTOCOL_CANDIDATE_v1.1.md](G2_PROTOCOL_CANDIDATE_v1.1.md).
- Comparaison version v1.0/v1.1 : [COMPARISON_AND_AMENDMENTS_v1.1.md](COMPARISON_AND_AMENDMENTS_v1.1.md).
- Plan pilote/calibration : [G2_DEV_PILOT_CALIBRATION_PLAN_v1.0.md](G2_DEV_PILOT_CALIBRATION_PLAN_v1.0.md).
- Protocole adversarial candidat v0.2 : [G2_ADV_PROTOCOL_CANDIDATE_v0.2.md](G2_ADV_PROTOCOL_CANDIDATE_v0.2.md).
- Empreintes des **quatre documents candidats** : [SHA256_CANDIDATES_v1.1.txt](SHA256_CANDIDATES_v1.1.txt).
- v1.0 historique : [G2_PROTOCOL_OVERVIEW_CANDIDATE_v1.0.md](G2_PROTOCOL_OVERVIEW_CANDIDATE_v1.0.md) et [SHA256_PROTOCOLE_CANDIDAT.txt](SHA256_PROTOCOLE_CANDIDAT.txt) (hash du document source original, qui n'est pas encore entièrement archivé ici).

## Conditions non remplies

1. Code déterministe complet du générateur cinq familles, normalisation et graines dérivées.
2. Interopérabilité authentique V10 + POLYMATH avec compteur partagé d'évaluations, budget réduit et garde d'incumbent.
3. Bibliothèque de sondes et vraisemblances `P(o|M,S)` calibrées sur un corpus de développement.
4. Matrice mesurée de pertes V10/POLYMATH pour les **cinq familles**, y compris linéaire et hors-modèle.
5. Références `S_ref(i)` externes aux résultats G2, coût CPU et limite de temps reproductible.
6. Pilote DEV-smoke (20 instances) et calibration DEV indépendante achevés et audités.
7. Paquet final (code, paramètres, données dev, scripts statistiques) intégralement haché, publié et horodaté avant la première génération de holdout G2.

**Interdit :** qualifier les SHA des documents candidats de scellement confirmatoire définitif ou générer le jeu des 200 instances G2 avant satisfaction des conditions 1–7.

Les sources historiques G1bis, V10 et POLYMATH sont disponibles dans les archives de recherche, mais elles n'ont pas encore été intégrées et calibrées dans un banc G2 conforme.

## Règle de gouvernance

Tout amendement aux règles confirmatoires doit apparaître dans un nouveau document et un nouvel engagement public **avant l'ouverture du holdout**. H1–H4 ne peuvent pas être modifiés après inspection des résultats. G2-adv est indépendant et ne corrige pas a posteriori les résultats G2.
