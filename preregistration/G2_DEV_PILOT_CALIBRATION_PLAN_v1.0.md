# ÉPOCHÉ — Plan de préparation du pilote et de calibration (candidat)

**Date :** 2026-10-24. **État :** pré-G2, aucun pilote complet attesté, aucune instance G2 confirmatoire générée.

## Principe de séparation des données

Trois corpus **strictement indépendants** :
1. **DEV-smoke** : 20 instances stratifiées (4×5 familles), cinq graines, uniquement tests d'intégration et chronométrie.
2. **DEV-calibration** : corpus additionnel dédié aux vraisemblances des syndromes, pertes terminales V10/POLYMATH, coûts selon budget restant, seuil fixe A2 et score de référence. Planifier sa taille sur la stabilité des estimations ; **20 instances totales ne suffisent pas** à une calibration fiable de cinq familles et des événements rares.
3. **G2-holdout** : 200 instances i.i.d. communes aux bras, **créées seulement après scellement**. Ne pas réutiliser les instances déjà utilisées pour préparer les générateurs ou calibrer les politiques.

Les graines maîtresses de familles 1000,2000,3000,4000,5000 sont des propositions de la spécification G2. Les valeurs 6000–6020 sont proposées pour DEV-smoke. Le générateur doit transformer ces valeurs en graines distinctes par famille/instance et le protocole final publier cette dérivation.

## Arbitre des décisions

**Décisions effectivement admissibles aujourd'hui :** V10 authentique et POLYMATH recon **si et seulement si leurs matrices de perte/cout ont été mesurées** pour cinq familles et plusieurs budgets terminaux. MORPHOSIS, EXTERNE, BOTTOM restent **INACTIFS**, sans perte inventée.

Le code public G1bis donne un Bellman exact **sur états et observations abstraits**. Il ne constitue pas encore l'intégration avec les environnements à n=56, le compteur partagé d'appels, les retours de scores V10/POLYMATH, ni une vraisemblance empirique. Ce raccordement est à implémenter et tester, sans prétendre que l'expérience G2 a été exécutée.

## Étapes d'intégration et assertions

1. **Reconstruction du générateur**, objectifs exacts en ±1, densité sans remise, poids, amplitude et échelle, hash du code ; même objet f pour les sept bras.
2. **Interface f comptabilisée** : retourne fitness, compteur `evals_real`, `cache_hits`, temps CPU ; journal de tous les sondages, même ceux réutilisés. Tests : 8 sommets d'une dérivée coordonnée sont évalués ≤8 fois avec cache ; un appel réutilisé ne coûte pas à nouveau.
3. **V10 véritable**, chemin de code et SHA figés. Valider qu'il obtient strictement les mêmes traces que l'archive en budget intégral avant de proposer un budget réduit. Sinon NO_START.
4. **POLYMATH** : mesurer coûts complets et scores pour chaque famille, en configuration autonome et en résolution après sondes, avec budgets restants variables.
5. **Tests de sondes** : clarté de A2 (quatre triplets) et des sondes A1/A6 (bibliothèque figée avant G2), ancres, précision en arithmétique flottante, et cas G1bis adversariaux.
6. **Vraisemblances** : estimer `P(o|M,S)` sur des cas disjoints et normalisés ; modèle explicite pour les sondes groupées et les faux négatifs par annulation, pas de probabilité 0,95/0,02 supposée universelle. Calibration croisée / sensibilité des priors.
7. **Pertes** : mesurer `E[Q_loss|M,d,budget]`, `E[E|M,d,budget]`, `E[T|M,d,budget]` avec intervalles pour les cinq familles. Ne pas ajouter les coûts deux fois dans Bellman.
8. **Référence de qualité** : fixer une borne supérieure analytique calculable à partir des coefficients du générateur et normaliser les scores indépendamment des bras G2 ; preuve que la transformation est la même pour tous les solveurs. Tester l'effet de l'offset sur H1.
9. **CPU et puissance** : profiler sur la machine retenue, contrôler `C_max=2` s et `T_norm=2` s ou rédiger une modification **avant scellement**. Évaluer la variance **des contrastes appariés**, non des scores isolés, notamment H3 Δ=0,0005.
10. **Vérifications de sûreté** : une solution issue du diagnostic est vérifiée par f et conservée comme incumbent ; V10 à budget complet n'est pas disponible gratuitement après des sondes. Le repli est conditionné au budget restant ; H4 est une mesure statistique a posteriori, pas un certificat.
11. **Pipeline final** : script unique DEV-pilot, tests source/code, procédure de non-consultation G2, CI, analyse verrouillée, dépendances, seeds, manifeste et commit.

## Contrôles d'arrêt

- Échec du générateur ; perte terminale non calibrée ; budget de repli impossible ; syndrome non reproductible ; fichier hors-modèle dépendant du résultat des bras ; scores normalisés a posteriori ; CPU non mesuré ; versions sources non fixées : **NO_START**.
- Le nombre d'instances de calibration peut être ajusté **avant G2**, avec amendement documenté. Après génération G2 : **aucun tuning, aucune sélection de bras, aucune modification de seuils**.

## État documentaire

Les archives locales antérieures contiennent des sources V10 et POLYMATH, et G1bis comporte 13 tests synthétiques positifs. Ces résultats préexistants **ne constituent pas** des estimations de pertes sur M_lin et M_outside à n=56 ; ces cases restent vides jusqu'à mesure.

**Prochaine action technique autorisée :** implémenter et valider le banc DEV-smoke avec les vrais solveurs. Ce n'est pas une expérience confirmatoire et doit rester étiqueté DEV.
