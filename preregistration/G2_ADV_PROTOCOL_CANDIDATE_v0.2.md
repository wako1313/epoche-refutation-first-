# ÉPOCHÉ — G2-adv, protocole adversarial candidat v0.2

**Date :** 2026-10-24. **Statut : DRAFT / NON SCELLÉ / NON EXÉCUTÉ.**  
**Distinct de G2 principal :** aucun résultat G2-adv ne modifiera rétroactivement les paramètres, les seuils ou le verdict H1–H4 de G2.

## 1. Question, modèle de menace

Un adversaire **hors ligne** connaissant le plan fixe A2, ou la stratégie publique d'ÉPOCHÉ A1, peut-il construire une fonction hors de la classe cible qui passe les diagnostics et induit une mauvaise décision terminale ?

L'adversaire choisit **avant l'exécution** une fonction f et ne la modifie pas ensuite. Il peut connaître le code, les distributions de tirage, les sondes fixes et les modèles de vraisemblance ; il ne connaît ni les tirages privés des sondes aléatoires ni les observations à venir.

Ne pas confondre : (a) une dérivée de coordonnée nulle sur un triplet ; (b) un test de retournement de trois blocs ; (c) une impossibilité d'identification à partir de toutes les observations effectives. La preuve d'aveuglement à un test coordonné ne s'étend pas automatiquement aux sondes groupées.

## 2. Sous-expériences proposées

**E1 — Cubique isolée ciblant A2.** `g=q+epsilon*chi_U`, où `U` est tiré parmi les triplets **absents de la bibliothèque de tests coordonnés A2** (non parmi les supports déjà testés). Le module d'attaque confirme formellement `D_S chi_U=0` pour chaque triplet S de A2. q, epsilon, U et la normalisation sont générés sur graines adversariales distinctes. Cela garantit l'aveuglement des syndromes A2 ciblés, **pas** le passage d'un autre type de sonde ni une perte algorithmique inévitable.

**E2 — Adversaire à coût borné.** Chercher sur une famille de fonctions de degré ≥3 une fonction maximisant la probabilité de non-détection selon la **politique d'acquisition** connue, sous budget d'attaque spécifié (nombre d'appels et CPU). Rapporter tous les essais, la dispersion de coûts et les solutions échouées. Un recuit est une baseline, pas une garantie d'optimalité. Aucune recherche sur des instances réservées après gel.

**E3 — Changement de régime.** `g_t` quadratique jusqu'au changement, puis ajoute des interactions cubiques ; changement et indexation temporelle **dans les appels à f**, avec budget total 1261 et une horloge d'évaluation fixée. On doit définir la référence de qualité sur la phase finale pour éviter de comparer deux fonctions différentes ; protocole d'optimum dynamique avant toute inférence. Il s'agit d'un test hors distribution primaire.

## 3. Plan et unité d'échantillonnage

Par défaut : 50 **instances indépendantes** par E1/E2/E3, communes aux deux bras A1/A2, cinq graines d'agent par instance si le budget le permet. Cela correspond à **300 combinaisons instance×bras** ou **1 500 exécutions avec cinq graines**, et non « 150 exécutions ». Les essais utilisés pour construire les attaques ne constituent pas des échantillons confirmatoires.

Pour A1 adaptatif, construire f **avant** l'exécution d'A1 : l'attaquant ne peut pas choisir le monôme après avoir vu les résultats de la politique. Les sondes prédictives que l'attaquant simule appartiennent aux données de développement et leur coût est journalisé. Le jeu G2-adv est scellé séparément ; il n'est pas utilisé pour calibrer A1.

Le protocole final doit fixer : définition et amplitude des fonctions; seed master; mode de sélection d'epsilon; paramètres d'attaque; stratégie et répartition des sondes; terminal decisions admissibles; limite CPU; nombre de graines; définition de la réussite d'attaque.

## 4. Mesures et hypothèses candidates

**Métrique primaire** : fréquence de **mauvais routage dommageable** au niveau instance (une décision a entraîné une perte de qualité terminale > epsilon fixé face au meilleur choix de routage calibré), avec borne de confiance, plutôt que `decision != argmin_M L` calculé seulement à partir de pertes moyennes par famille. Rapporter séparément le simple rejet structurel raté et le regret réellement encouru.

- H1adv : risque de mauvais routage dommageable A1 inférieur d'au moins 10 points de pourcentage à A2 sur E1.
- H2adv : probabilité de mauvais routage dommageable A1 ≤0,25 sur E2.
- H3adv : probabilité A1 ≤0,15 sur E3, sous définition du résultat dynamique précisément fixée.

Ces seuils sont **candidats** ; le plan statistique doit spécifier tests appariés binaires/IC, correction des trois comparaisons, gestion des cinq graines, puissance, attaques échouées et frontières d'égalité **avant le scellement**. Avec seulement 50 instances, une marge de 10 points peut manquer de puissance.

Échec H1adv : conclusion adversariale négative ; H2adv : blocage de revendication de robustesse et **suspension discrétionnaire de G3** si adoptée avant le gel de gouvernance ; H3adv : exclusion des régimes changeants des claims. Ne pas réécrire G2 primaire à partir de G2-adv.

## 5. Livrables

`PROTOCOLE_G2ADV_FINAL.md` à rédiger après le développement, générateurs E1/E2/E3, scripts de test adversariaux, traces de **tous** les essais d'attaque et des 300/1500 exécutions, rapport des échecs, manifeste SHA256 et scellement tiers **avant génération du jeu adversarial confirmatoire**.

**NO_START :** plan encore incomplet ; ne pas l'appeler « protocole gelé ». Le théorème d'impossibilité relatif aux seules sondes coordonnées ne suffit pas à caractériser un adversaire du diagnostic à trois groupes.
