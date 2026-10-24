# ÉPOCHÉ — amendement candidat v1.2 (DEV, NON SCELLÉ)

**24 octobre 2026 — NO_START G2.**

Cette proposition actualise la préparation du protocole v1.1 sans toucher à son statut pré-confirmatoire.

## CPU et faisabilité

Profilage partiel POLYMATH à n=56 : dix instances par densité 0–8 % ; médianes CPU (s) : 0.069, 0.074, 0.090, 0.103, 0.114, 0.123, 0.204, 0.341, 1.553. cProfile attribue le principal coût dense à `Lasso.enet_path`. La couverture 9/20 densités n'autorise PAS à inventer `c_id(densité)` complet.

**À mesurer sur DEV isolé :** coût CPU médian et queue par densité, budget physique, timeout *dur* en processus séparé, politique des sorties incomplètes. Les nombres 0.0111, 0.0152, 0.0160 proposés comme pertes POLYMATH après « ratio CPU 1.4 » **ne sont pas validés**.

## Référence et calibration

Un générateur DEV construit des fonctions ±1 avec une borne analytique supérieure `S_ref(i)=1`. Cette référence ne dépend d'aucun bras G2. Sa normalisation et l'effet de son offset sur la marge H1 relative doivent être figés avant G2.

La calibration exploratoire publiée dans le rapport DEV utilise 3 instances/famille : insuffisante. La campagne de calibration plus large (graines candidates 7000–7100) **n'a pas été exécutée**. Le générateur G2 et les exactes graines de validation restent à sceller.

ÉPOCHÉ-DP actuellement ne sonde pas : 100 décisions sur 100 routent vers V10. Ajouter une sonde discriminante pour les interactions clairsemées vs denses, **sans tuning sur le futur G2**, avant de prétendre tester la valeur du routage.

## Blocs à figer avant premier holdout

```yaml
status: NO_START_G2
generator_sha256: TO_FILL
solver_V10_sha256: TO_FILL
solver_POLYMATH_sha256: TO_FILL
dp_and_probes_sha256: TO_FILL
calibration_dataset_sha256: TO_FILL
observation_likelihoods_sha256: TO_FILL
terminal_losses_by_budget_sha256: TO_FILL
S_ref_rule: analytic_upper_bound_candidate_1
cpu_hard_limit_seconds: TO_VALIDATE
cpu_normalization_seconds: TO_VALIDATE
noise_policy: primary_none
instance_seeds_holdout: TO_SEAL
analysis_script_sha256: TO_FILL
```

Après complétion et vérification de tous les champs, déposer manifeste SHA256 intégral et éventuel horodatage tiers **avant** création du jeu confirmatoire. Aucun des artefacts DEV existants ne constitue ce scellement.

Le dossier technique de référence de cette étape est [le rapport DEV](../dev/DEV_PILOT_2026_10_24_RESULTS.md).
