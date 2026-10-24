# ÉPOCHÉ v0.3 — Correction de la RVoI et audit du cache (DEV, 24 octobre 2026)

**STATUT : DEV EXPÉRIMENTAL SEULEMENT — G2 NON SCELLÉ — aucun holdout G2 généré.**

## Découverte déterminante

La précédente alerte **4/20 instances en violation de H4** après un diagnostic fictivement parfait était, dans les conditions mesurées, due à un **défaut d'ordonnancement du wrapper DEV de V10**. Le wrapper réduisait systématiquement les pas du moteur de 90 à **87** après 32 sondages sur la base du nombre nominal de requêtes, alors que l'ordonnanceur disposait encore du **cache partagé** et du budget réel de 1 261 évaluations physiques.

Après 32 appels physiques payés, une exécution historique V10 préservant **90 pas** a été tentée sur les mêmes 20 instances ×5 graines :

| Contrôle | Résultat |
|---|---:|
| 800 exécutions V10 directes : 20 instances ×5 graines ×8 budgets | Terminées |
| Réplication exacte des 100 résultats V10 à 1 261 évaluations | 100/100 |
| Qualité monotone sur les 8 budgets | 100/100 trajectoires |
| Convexité discrète sur les 8 budgets | **7/100**, aucune des 5 anciennes répétitions H4 |
| Violation H4, ancienne règle 87 pas | **5/100 répétitions**, 4/20 instances |
| Violation H4, 32 sondes puis 90 pas | **0/100 répétitions**, 0/20 instances |
| Dépassements physiques des 1 261 évaluations | **0/100** |
| Maximum d'appels physiques après sondes + 90 pas | **1 190** |
| Requêtes logiques (avec cache) | 1 293 |

Les scores du V10 à 90 pas sont strictement identiques aux scores de base sans sonde sur ces 100 traces. Les cinq anciennes dégradations étaient liées à la coupure conservatrice des trois derniers pas du *wrapper*, pas à un dépassement du budget physique.

### Oracle de classe parfait — toujours artificiel

Le routage reçoit un **label de classe impossible à obtenir gratuitement dans un vrai diagnostic**. Les 32 évaluations sont néanmoins réellement facturées ; les moteurs V10 (90 pas) et POLYMATH (reconstruction) sont exécutés depuis leurs sources historiques.

- Loss V10 plein : **0,09006524**.
- Loss oracle idéal corrigé : **0,08857897**.
- Gain moyen DEV : **+0,00148627**.
- Intervalle bootstrap exploratoire IC95 par instance (10 000 rééchantillonnages) : **[+0,00031691 ; +0,00318629]**.
- Statuts : **100/100 OK**, 0/100 violations de score H4, 0/20 instances H4.

**Attention :** zéro violation sur 20 instances ne prouve pas H4 ≤2 % ; la borne supérieure Clopper–Pearson unilatérale à 95 % est encore **≈13,91 %**. Le gain moyen reste inférieur à la marge historique H2 de +0,002 et ne provient d'aucune sonde réellement discriminante.

## Révision mathématique — pas de pénalité dupliquée

Avec un risque terminal **budgété** `r(b,B)=min_d E[L(d,M;B)]`, la bonne valeur myope d'une sonde de coût k est :

`RVoI_B(S|b) = r(b,B) − c(S) − E_o[r(b_post,B−k)]`.

Elle se décompose exactement en :

`[r(b,B) − E_o r(b_post,B)] − c(S) − E_o[r(b_post,B−k)−r(b_post,B)]`.

Le dernier terme est le **coût d'opportunité conditionnel**. **Ne pas l'ajouter à nouveau** à une DP qui utilise déjà `r(b_post,B−k)`. La convexité de Q(B) n'est pas nécessaire : la non-négativité du coût d'opportunité suit de l'inclusion des ensembles de politiques faisables sous budget croissant, à condition de pouvoir ne pas dépenser tout le budget.

Plus important : dans ce pilote, les **cache hits** rendent le seul scalaire B insuffisant pour décrire les ressources restantes si l'on veut prédire fidèlement la suite de V10. Le cache et les paramètres d'ordonnancement doivent aussi être contrôlés.

Le prototype `budget_aware_dp.py` emploie des pertes terminales dépendant du budget mesuré sans doubler les charges de sonde, et **7/7 tests passent**. Il n'est **pas** une solution complète à H4 : la minimisation du risque moyen n'impose pas une borne sur la probabilité de régression extrême.

## Archive reproductible et limites

- Archive livrée dans la conversation : `EPOCHE_v03_RVOI_budget_cache_DEV_2026_10_24.zip`.
- SHA256 archive : `a9385c067d5536b28e1383e099f504afc0c7e498f0e0381b262c30c4693677d6`.
- **82 fichiers ZIP**, dont un manifeste de **81 fichiers contrôlés individuellement**.
- Inclut `audit_budget_worker.py`, `merge_results.py`, `replay_perfect_policy_budget_fixed.py`, `budget_aware_dp.py`, `test_v03.py`, les véritables moteurs V10/POLYMATH vendorizés, les 800 lignes de profils budgétaires, 100 lignes de rejeu, et le rapport intégral.
- Le code et les traces de cette archive **ne sont pas encore transférés intégralement sur GitHub** ; le présent document est une synthèse vérifiable par comparaison avec l'archive fournie dans la conversation.

**Décision technique :** recalibrer la continuation V10 sur le nombre physique d'évaluations restant et son cache ; ne pas implémenter un supplément global `Δ_term` qui compterait deux fois le coût. Construire ensuite une vraie sonde pertinente et recalibrer POLYMATH sur budgets variables. **G2 demeure NO_START.**
