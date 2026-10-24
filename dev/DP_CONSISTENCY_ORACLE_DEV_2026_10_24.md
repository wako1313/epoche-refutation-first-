# ÉPOCHÉ — Contrôle du véritable DP et essai d'oracle de classe parfait (DEV, 24/10/2026)

**STATUT : DEV ONLY / G2 NO_START.** Aucune instance confirmatoire G2 n'a été produite.

Le noyau **réel** `epoche_dp.StructuralDP`, la calibration exploratoire `calibration_dev.json` (3 instances par classe, insuffisante pour G2), `synapse.engine.Engine` V10 et `polymath.recover` historiques ont été exécutés. Deux terminaux seulement : V10 et POLYMATH.

## Tests de cohérence

| Croyance concentrée (99 %) | Décision Bellman | Résultat |
|---|---|---|
| M_quad_s | STOP → POLYMATH | PASS |
| M_quad_d | STOP → V10 | PASS |
| M_lin | STOP → POLYMATH | PASS |

Même résultat avec et sans les 3 sondes G1bis. Deux cas ont réellement exécuté les terminaux après cette sélection : instance sparse 8000 → POLYMATH, score 0,95222643, 605 appels, 0,139 s CPU ; instance dense 9000 → V10, score 0,81053601, 1149 appels, 0,064 s CPU.

Un modèle d'observation **artificiellement parfait** révélant l'appartenance à `{sparse}` ou `{linéaire,sparse}` fait bien acheter une sonde de 32 évaluations puis router respectivement vers POLYMATH ou V10. Ce test **ne prouve pas l'existence d'une sonde réelle aussi informative**. Les 32 évaluations physiques sont payées, tandis que le label est injecté séparément par le générateur.

À coût de 128 évaluations, la DP refuse la sonde parce que V10 est supposé exiger **1150 évaluations terminales** et que 1261−128=1133. Ceci révèle une limite du modèle de coût terminal fixe ; re-calibrer V10 à budget réduit avant toute conclusion.

## 200 exécutions d'oracle synthétique payant (DEV)

20 instances DEV stratifiées **4 par famille** ×5 graines ×2 versions du label, score référence analytique fixé à 1 et loss `1−score_final + 0,002 E/1261 + 0,002 CPU/2,0`.

| Variante | Loss moyenne | Gain moyen vs V10 complet | Bootstrap IC95 par instance |
|---|---:|---:|---|
| V10 historique seul | 0,09006524 | — | — |
| Parfait sparse uniquement, coût 32 | 0,08899228 | +0,00107296 | [−0,000195 ; +0,002907] |
| Parfait linéaire OU sparse, coût 32 | 0,08863396 | +0,00143128 | [+0,000129 ; +0,003234] |

Repondération exploratoire sur les probabilités **G2 envisagées** (10 %,25 %,25 %,25 %,15 %) : gains +0,00136925 et +0,00154806, IC bootstrap stratifié [0,000266 ; 0,002819] et [0,000437 ; 0,003001]. Chaque famille ne compte que 4 instances dans ce DEV ; ne pas généraliser les intervalles à une confirmation G2.

**Limite H4 critique :** malgré le label parfait, 5 des 100 répétitions, sur **4 des 20 instances**, subissent une dégradation de score >0,001 face à V10 complet, parce que 32 évaluations ont amputé son budget. Le critère H4 est défini au niveau de l'instance (≥1 violation sur cinq graines). L'effet moyen positif ne suffit pas.

## Audit de la borne « H2 impossible »

L'ancienne opération `0,25×0,00222−32×(0,002/1261) ≈ 0,000504` est correcte **uniquement sous l'ancienne matrice de pertes, en ne traitant que sparse**. Elle ne borne pas le banc DEV actuel, dont le gain de *qualité* observé en calibration exploratoire sparse est 0,0807911−0,0728572≈0,007934 et qui comprend 10 % d'instances linéaires.

La DP actuelle prédit, avec son propre modèle de pertes préliminaire et un classificateur parfait à 32 appels, des améliorations ~0,001919 (sparse seule) / ~0,001959 (linéaire+sparse). Les mesures DEV donnent 0,001073 / 0,001431 (non repondérées). **H2 +0,002 ne passe pas sur cet oracle DEV, mais n'est pas mathématiquement impossible : les IC recouvrent des gains supérieurs.**

## D₂ sans implémentation

32 paires à quatre évaluations = 128 appels. Sous Bernoulli indépendant idéal `p_s=0,022`, `p_d=0,12`, classer « dense » dès ≥2 paires non nulles produit 15,60 % de faux denses sparse et 8,97 % de faux sparse dense, soit **12,29 % d'erreur équilibrée**, et non <5 %. Les cubiques peuvent aussi influencer D₂. En outre, 128 appels empêchent la réservation naïve de V10 à 1150 évaluations.

**Décision : pas d'implémentation de D₂ avant calibration des vraies pertes à budget réduit et un diagnostic de pouvoir discriminant.**

## Provenance / intégrité

Archive technique intégrale (fournie dans le chat) : `EPOCHE_G1bis_H2_contrepreuves_DEV_2026_10_24.zip` ; SHA256 `22ba1ee58a9a3032ebd6a03122781ac0c3696537b51a82063fca2bbba2d2bcf7`. Contient 80 fichiers contrôlés, sources et environnement de développement vendorizés, 200 résultats individuels synthétiques étiquetés, 8 tests PASS, analyses bootstrap et scripts.

**Note de provenance :** l'archive binaire complète est fournie dans la conversation, mais n'est pas encore téléversée dans ce dépôt GitHub. Les constats publiés ici ne constituent ni scellement G2, ni revue indépendante, ni validation de nouveauté.
