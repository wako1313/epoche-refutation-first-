# ÉPOCHÉ — Second audit G-DET v0.2 (DEV)

**Date :** 2026-10-24. **G2 NO_START.** Audit d'un certificat conditionnel de consommation physique, pas scellement confirmatoire, pas validation externe.

## Preuve source

Sources épinglées et contrôlées :
- `synapse/engine.py` SHA256 `597eab44979dd586faaf7a676dc59e09199935ba50c4ba9b8b67b1571dc25d18`
- `src/bench.py` SHA256 `d50195f3d004fea1a8c4cf4653c75c556998d997320b29b1fc4de341d6100042`

`Engine(tabU,Config(steps=90,candidate_limit=12,restart=True))` sur fonctions statiques n=56, cache permanent, un seul `run()`.

Lecture de code par un **second chemin automatisé indépendant du script d'audit initial** : `Engine.__init__` appelle une fois `env.fitness` ; `Engine.step` effectue une compréhension de fitness sur un pool de **1 état courant + 12 voisins + 1 candidat de redémarrage par itération**. Le cache persistant contient l'état courant avant chaque itération (initialisé et ensuite choisi dans le pool précédemment évalué), soit au moins 90 cache hits.

```
MAX_LOGICAL = 1 + 90*(1 + 12 + 1) = 1261
U_cert     = 1 + 90*(12 + 1)     = 1171 physical evaluations
```

Le libellé ancien « un redémarrage par exécution » est erroné : **un par itération** lorsque `restart=True`. L'environnement de saturation initial atteint `1171`.

## Nouveaux contrôles

- **96** essais adversariaux : fonction constante, plateaux et valeurs quantifiées, maxima multiples, valeurs extrêmes, collision de tous les voisins, voisins dupliqués, redémarrage fixe, extrêmes de l'état ; tous ≤1171.
- **100** relectures des 20 instances DEV antérieures ×5 graines ; toutes ≤1171.
- **196/196** simulations : 1261 requêtes logiques et 90 redémarrages à chaque fois ; maximum de nouvelles évaluations observé **1158**.
- **Contrôle négatif :** cache désactivé ⇒ **1261 évaluations physiques** ; la garantie de 1171 est alors fausse et ne doit pas être appliquée.
- **CPU : contre-exemple statique déterministe** coûtant artificiellement 0,00205 s CPU par évaluation : **1171 évaluations physiques, 2,566 s CPU** ⇒ **une borne physique ne certifie pas le plafond CPU de 2 secondes**.
- Temps CPU sur les 100 cas DEV : médiane **0,0786 s**, maximum **0,1483 s** (observations seulement, dépendantes de la machine).
- Rétro-contrôle : la porte `B−32 >= 1171` aurait refusé les **900/900** exécutions DEV des caps 1158, 1100 et 1042.
- **12 tests automatisés sur 12**, vérifiés après extraction neuve de l'archive.

La seconde procédure n'est **pas** une revue par un second scientifique humain indépendant ; cette condition reste ouverte si elle est exigée.

## Portée de G-DET

`B−k >= 1171+δ`. Si `B=1261` et `k=32`, la marge résiduelle est **58 évaluations**. Une sonde D₂ à coût k=128 est refusée à B=1261 (elle exige B≥1299+δ). Dans ce régime, la porte dépend seulement du budget et du coût fixes : elle n'apprend pas une classe ni ne garantit H4 pour les branches POLYMATH.

**Non couvert :** plafond CPU, perturbation de RNG par une vraie sonde, cache à éviction, objectif dynamique, différence d'implémentation ou de configuration, routage vers d'autres solveurs. Aucune vraie sonde D₂ validée, aucun holdout G2 généré.

## Artefacts

Archive autonome de la conversation : `EPOCHE_GDET_v02_SECOND_AUDIT_DEV_2026_10_24.zip`. SHA256 :
`a378911d7ab444b9d5a9d68e3b6ec9421c93eb254e320e2d7f7ccce09bea0986`.

Archive de **34 fichiers**, dont 33 couverts par un manifeste SHA256 ; contient sources vendorizées, script de revue AST, campagne adversariale, 196 traces CSV, résultats JSON, tests et rapport intégral. **Les scripts et les traces ne sont pas encore transférés intégralement dans GitHub**. Cette page publique n'est pas le dépôt intégral des artefacts.

**Verdict : GO conditionnel sur U_cert relatif aux sources hachées, NO-GO CPU 2 s comme garantie, NO_START G2.**
