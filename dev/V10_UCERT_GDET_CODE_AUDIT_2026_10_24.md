# ÉPOCHÉ — Audit du code V10 : borne déterministe de G-DET

**Date :** 2026-10-24. **Statut : AUDIT DEV, NON SCELLÉ, G2 = NO_START.** La borne porte sur **cette** implémentation et **cette** configuration. Le protocole EPOCHE_G2_MARGIN_GATE_v0.1 reste au stade candidat.

## 1. Sources réellement auditées

- **V10 authentique** : `synapse/engine.py`, SHA256 `597eab44979dd586faaf7a676dc59e09199935ba50c4ba9b8b67b1571dc25d18`.
- **Oracle/Adapter DEV** : `src/bench.py`, SHA256 `d50195f3d004fea1a8c4cf4653c75c556998d997320b29b1fc4de341d6100042`.
- `Engine(..., mode='tabu', Config(steps=90,candidate_limit=12,restart=True))` et une exécution unique de `run()`.
- `bench.Adapter.advance(step)=False` : fonction d'objectif statique et déterministe ; `bench.Oracle(cache=True)` conserve sans éviction ses valeurs indexées par l'état entier.

## 2. Preuve de la borne de consommation physique

Dans le moteur `Engine.__init__`, exactement **une** requête de fitness est utilisée pour l'état initial (lignes 41–45). `_pool` (lignes 77–84) crée à chaque itération : l'état courant, **≤12** voisins (limite contrôlée par `Adapter.neighbors`) et **1** état de redémarrage. `Engine.step` (lignes 98–105) évalue ce pool **une** fois ; après, le moteur n'appelle plus directement la fitness. Le nombre maximal de requêtes **logiques** est donc :

```
1 + 90*(1 stay + 12 neighbors + 1 restart) = 1261.
```

**Invariant de cache :** à chaque itération, l'état courant fait partie du cache. Au départ, l'initialisation a déjà évalué cet état ; ensuite, le nouvel état est obligatoirement un des candidats évalués lors de l'itération précédente (`state=candidates[chosen]`), et la fonction demeure statique. Il y a ainsi au moins **un cache hit par itération**. Par conséquent, le majorant physique **certifié conditionnellement** est :

```
U_cert = 1 + 90*(12+1) = 1171 évaluations.
```

Le majorant est **serré** au regard de ces seules hypothèses : un environnement synthétique produisant 12 nouveaux voisins et un nouveau redémarrage par étape **atteint exactement 1171 évaluations**, malgré 1261 requêtes logiques.

## 3. Porte G-DET

Condition `B−k >= U_cert+delta`, `delta >= 0` en **évaluations physiques**.

| B | k | Marge maximale delta | G-DET |
|---:|---:|---:|---|
| 1261 | 32 | 58 | ADMISSIBLE si delta≤58 |
| 1261 | 90 | 0 | ADMISSIBLE si delta=0 |
| 1261 | 91 | impossible | REFUS |
| 1261 | 128 | impossible | REFUS (D₂ 32 paires ×4) |
| 1158 | 32 | impossible | REFUS |

G-DET est donc **instanciable**, contrairement à l'hypothèse où l'on ne disposerait que d'un maximum empirique. La borne 1171 est issue du code et non des 100 essais.

## 4. Rejeu de traçoires et tests

- **9/9** tests automatisés réussis, empreintes de code épinglées.
- **100 paires DEV** = 20 instances déjà existantes ×5 graines ; V10 complet contre 32 évaluations aléatoires indépendantes avant V10, à B=1261.
- **100/100 trajectoires V10 identiques** (état, fitness et meilleur score aux 90 étapes), avec au moins 1 cache hit chaque étape.
- Aucun dépassement de B ; maximum physique **1190/1261** après sonde. Baseline V10 max **1158**.
- Le résultat est illustratif : la **preuve** du majorant ne repose pas sur ces échantillons.

## 5. Limites explicites du certificat

Cette preuve n'inclut **pas** : (a) les 2 secondes de CPU, (b) une fonction de fitness changeant dans le temps, (c) un cache désactivé, avec éviction ou invalidation inadéquate, (d) des requêtes cachées en dehors des sources auditées, (e) les autres configurations de V10, (f) une sonde consommant le flux RNG du moteur ou modifiant l'état initial, (g) la régression due au choix de POLYMATH au lieu de V10.

Si la sonde est pure et utilise un RNG séparé, l'admissibilité G-DET garantit seulement que le **repli V10 de 90 pas** peut être exécuté intégralement au même coût physique et, sous couplage correct des graines, avec la **même trajectoire**. Alors, l'incumbent global après sonde ne peut être inférieur à l'incumbent V10 seul **sur cette branche de repli**. Cela n'implique pas H4≤2% pour **toutes** les routes ÉPOCHÉ.

## 6. Reproductibilité et décision

Archive autonome livrée dans la conversation : `EPOCHE_MARGIN_GATE_AUDIT_V10_UCERT_2026_10_24.zip`, SHA256 `183f7b6913e9985b61d5514440a0472150bbe6c42fb1d9ec54c8ba10b3112818`. Contient les sources vendorizées, un script de preuve/test (`audit_v10_margin_gate.py`), neuf tests, un environnement de saturation, le rapport, les 100 comparaisons détaillées, les dépendances et les SHA256 des composants. **L'archive complète n'est pas encore reproduite sous forme de fichiers GitHub** ; ce rapport public et l'empreinte en attestent seulement la version annoncée.

**GO technique pour G-DET avec U_cert=1171 et B=1261,k=32, sous les conditions listées. NO-START G2.** Le choix de delta, la garde CPU, le seuil A_min et la séparation DEV-CAL/DEV-VAL restent à geler ; H1–H4 sont inchangés.
