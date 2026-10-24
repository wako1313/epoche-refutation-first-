# ÉPOCHÉ G2 — Comparaison et registre d'amendements v1.0 → v1.1

**Date :** 24 octobre 2026. **Statut :** candidat / NO_START. **Source historique préservée :** protocole v1.0 SHA256 `bc5b61fd4a848837af4cd32265b23efdcc19c0e983f2bd79b5a8ba6246b03832`.

Cette comparaison est fondée sur la proposition détaillée reçue le 24/10/2026 et le protocole candidat interne v1.0, et ne doit pas être interprétée comme un résultat expérimental.

| Sujet | Proposition nouvelle | Décision v1.1 et justification |
|---|---|---|
| Paramètres | seeds 1000–5000, σ lin=1, p_sparse=.022, p_dense=.12, σ_pair=.5 | **ADOPTÉ comme cible**, encore incomplet sans générateur exact, intercept, amplitudes cubiques et normalisation |
| Moment de génération | « distribution générée avant scellement » puis interdiction de toute instance avant hash | **CORRIGÉ** : données DEV autorisées, instances G2 uniquement après paquet intégral scellé |
| Corpus | 200 instances, 5 graines, six compétiteurs + oracle | **ADOPTÉ** : 6000 runs compétition + 1000 runs oracle descriptif ; appariement par instance |
| A5 | POLYMATH autonome au lieu de MORPHOSIS | **ADOPTÉ** pour mesurer exactement le second exécutant |
| A2 | « 4 partitions de 3-supports » mais supports sont (0,1,2)… | **CORRIGÉ** : quatre dérivées sur triplets de coordonnées ; ne pas confondre avec les partitions groupées G1bis |
| M_outside | U tiré parmi 8 supports de A2, fonction supposée trompeuse | **REFUSÉ** dans G2 : 4 triplets A2 ne sont pas 8 supports, sélectionner des supports déjà testés peut rendre le cubique détectable, et cibler A2 biaise le benchmark ; déplacer les attaques ciblées en G2-adv |
| G2-adv E1 | U tiré parmi triplets non sondés par A2 | **CORRIGÉ ET ADOPTÉ**, seulement pour adversaire coordonné non adaptatif ; les tests groupés peuvent le détecter |
| Définition S_ref | `max` des scores observés dans G2 | **REFUSÉ** : fuite d'évaluation / score dépendant de la compétition ; référence déterministe externe au résultat G2 |
| Pertes M_lin et M_outside | proxy sparse, 0,0150 « estimation haute », BOTTOM 0,0025 | **REFUSÉ pour Bellman** : aucune calibration ; mêmes décisions opérationnelles V10/POLYMATH seulement après DEV |
| MORPHOSIS / EXTERNE | repli au nom V10 | **INACTIFS**, sauf si un vrai benchmark de leur code est ajouté en développement ; aucun nom de décision factice |
| Arrêt/ressources | B=1261, C=2s, « réserve V10 pleine » | **REFUS de la réserve pleine**, coût terminal budget-dépendant ; 2s à confirmer sur DEV, T_norm cohérent à figer |
| A3 | `k` égal aux sondes d'A1 sur la même instance | **ADOPTÉ comme contrôle conditionnel**, nécessite pas de fuite des observations d'A1, et interprétation prudente |
| H1–H3 | marge relative .15, absolues .002 et .0005 ; bootstrap/Holm | **CONSERVÉ**, contraste apparié teste le seuil et non zéro ; H1 sensible au choix de la référence |
| H4 | fréquence ≤2% | **DURCI** : borne unilatérale 95 % ≤2% au niveau instance ; n=200 requiert zéro violation |
| Puissance | 20 cas DEV, σ=.003, 200 suffisent | **PARTIEL** : 20 cas peuvent tester le système, non calibrer cinq modèles et les queues ; puissance H3 approximative n~318 à α=.05/3 |
| Taille | passage automatique à ceil(200*(σ/.003)^2) après pilote | **NON AUTOMATIQUE** : toute modification de n et de puissance exige justification et un nouvel amendement public **avant** toute G2 |
| n=100 + bruit | lancement conditionnel à H1 | **EXPLORATOIRE séparé**, sinon biais de sélection et modèle de bruit non spécifié |
| G2-adv | indépendant, 50×3×2 « 150 exécutions » | **CORRIGÉ** : 300 unités instance×bras ou 1500 runs à cinq graines |
| G2-adv E3 | g_t quadratique puis cubique | **A PRÉCISER** : horloge des appels, référence temporelle, modèle de menace ; aucun claim de détection de dérive |
| Gel SHA / signature | sha256sum, GPG et tiers | **ADOPTÉ comme procédure**, mais aucun hash ni commit d'une **v1.1 finale exécutable** ne constitue encore un scellement ; publier code, seeds et calibration ensemble |

## Amendements encore requis avant scellement

1. Source intégrale du générateur des cinq familles + preuve d'échelle commune des scores, graines effectivement dérivées et test de non-biais M_outside.
2. Version de l'exécutant POLYMATH/V10 en budget réduit avec un comptage unique (appel, cache, CPU).
3. Liste précise de sondes A1/A2/A3/A6 et toutes leurs réalisations d'observation, ancrage, tolérances.
4. Calibration L(d,M,budget) et likelihoods par famille sur DEV indépendante ; supprimer toutes les valeurs proxy.
5. Programme statistique complet, seeds bootstrap, procédure d'échec/timeout, contraintes CPU et matériel.
6. Manifeste SHA256 et commit public **avant la génération des 200 instances**, idéalement dépôt externe immuable.

**Verdict : la v1.1 améliore le protocole candidat mais n'est pas encore un pré-enregistrement figé ni une expérience exécutée.**
