# ÉPOCHÉ — Résultats DEV v2 (24 octobre 2026)

**Statut : DEV UNIQUEMENT / G2 NON SCELLÉ / AUCUNE INSTANCE HOLDOUT G2 GÉNÉRÉE.**

## Origine et périmètre

Nouveau banc exécuté à partir de `synapse.engine.Engine` (V10, `tabu`), `polymath.recover` / `optimize_model` et `epoche_dp.StructuralDP` (G1bis). Données du pilote ancien non retrouvées : résultats ci-dessous provenant **exclusivement d'un nouveau générateur DEV** à n=56 (linéaire, quadratique clairsemée 2,2 %, quadratique dense 12 %, cubique 1–3 interactions, hors modèle ordre 4). Objectifs bornés analytiquement dans [0,1]. Les sources et traces exactes sont incluses dans une archive de conversation séparée ; **pas encore intégrées aux fichiers de ce dépôt GitHub**.

20 instances distinctes, 5 graines par instance : trois bras complètement équilibrés (300 exécutions, 100 par bras) ; ÉPOCHÉ-DP utilise une calibration exploratoire *insuffisante* de trois instances indépendantes par famille, réservées au développement.

| Politique DEV | Exécutions complètes | Score final ↑ | Loss totale ↓ | CPU moyen (s) | Évals physiques moyennes |
|---|---:|---:|---:|---:|---:|
| V10 historique | 100 | 0.911806099 | 0.090065242 | 0.063299 | 1139.97 |
| Quatre sondes fixes suivies de V10 (contrôle de coût, PAS A2 routé) | 100 | 0.911548684 | 0.090313106 | 0.062107 | 1134.70 |
| ÉPOCHÉ-DP, deux terminaux V10/POLYMATH | 100 | 0.911806099 | 0.090066726 | 0.064783 | 1139.97 |

Loss = `1 - final_score + 0.002*evals_physical/1261 + 0.002*cpu_process_s/2.0`. S_ref=1 est un **majorant analytique**, pas l'optimum exact. **DP a choisi V10 100/100 fois, avec zéro sonde payante**, donc **aucun gain final sur V10**. Le léger surcoût de perte DP est seulement le coût CPU du contrôleur.

### POLYMATH autonome (mêmes 20 instances, une seule graine)

| Famille | CPU médian (s) | Interruptions/dépassements / 4 |
|---|---:|---:|
| Linéaire | 0.075 | 0 |
| Quadratique clairsemée | 0.109 | 0 |
| Quadratique dense | 1.891 | 1 |
| Cubique | 1.983 | 2 |
| Hors-modèle degré 4 | 1.735 | 0 |

**3/20** exécutions POLYMATH ont statut `cpu_timeout` ou `cpu_overrun`. Ces scores partiels ne valent pas une solution admissible au plafond CPU strict. Le contrôle CPU par signal est best effort.

### Profilage de densité (incomplet)

90 exécutions, densités 0–8 % par pas de 1 %, 10 instances/densité, temps médians (s) : **0.069, 0.074, 0.090, 0.103, 0.114, 0.123, 0.204, 0.341, 1.553**. Balayage initial demandé de 20 densités ×10 **non terminé**. Les profils cProfile aux densités 2 %, 7 %, 12 % identifient `sklearn.linear_model._coordinate_descent.enet_path` comme goulot principal (à 12 %, ~1.655 s sur 1.674 s CPU instrumenté).

### Provenance des artefacts originaux

- Archive reproductible locale : `epoche_G2_DEV_pilote_reproductible_2026_10_24.zip`.
- Empreinte SHA256 de l'archive exacte : `0acc91e6da6b2d84e6ff3bb5dd230af86bad0eb91cd3fb7622891ebe059a0b72`.
- Contenu : moteur historique vendorizé, compilateur G1bis, code `src/bench.py`, `src/analyze.py`, `src/profile_functions.py`, neuf tests automatisés (9/9), 300 traces équilibrées JSONL, 20 traces POLYMATH, 90 traces de profilage, 200 traces d'une première tentative déséquilibrée clairement exclue, manifestes SHA256.
- Archive **non encore transférée dans GitHub**. Les chiffres ici sont synthétiques/descriptifs ; validation externe non revendiquée.

## Interprétation et verdict

Le diagnostic groupé courant détecte des interactions cubiques mais ne sépare pas les densités quadratiques. Les pertes et likelihoods établies sur trois cas/famille sont insuffisantes pour une politique confirmatoire. Le fait que Bellman ne sonde pas est un résultat **négatif** sur cette implémentation. **NO-GO pour lancer G2** tant que la calibration, la discrimination de parcimonie, la limite CPU isolée et le routeur A2 ne sont pas terminés.

Aucun résultat G2 H1–H4. Aucune revendication de supériorité ou de nouveau paradigme démontré.
