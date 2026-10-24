# ÉPOCHÉ — Amendement H2 v1.3 (CANDIDAT NON SCELLÉ)

**Date : 2026-10-24. G2 = NO_START.** Ceci est une proposition explicite d'amendement, **sans changement du seuil H2 confirmatoire actuellement en vigueur**.

## Motif

La marge historique H2 +0,002 a été empruntée à une distribution différente. Le calcul ancien `0,25×0,00222` ne fournit pas une borne sur les nouvelles familles : la calibration exploratoire actuelle donne un écart de qualité sparse ~0,007934 et 10 % de familles linéaires susceptibles d'avantager POLYMATH. Sur 20 instances DEV (4/famille), une étiquette idéale classant « linéaire ou sparse » et facturée 32 évaluations produit +0,001431 de gain total moyen (ou +0,001548 repondéré), et 4/20 instances présentent au moins une violation de non-régression de >0,001.

## Changement H2 envisagé, non adopté

Pour la métrique primaire **préfixée** `L_total = S_ref(i)−S_final + 0,002 E/1261 + 0,002 CPU/T_norm`, remplacer la marge H2 +0,002 par une marge candidate **+0,0003** : `d_i = L_A4,i−L_A1,i−0,0003`. Exiger test t unilatéral du contraste apparié au niveau instance avec Holm sur H1–H3, IC bootstrap 95 % ; H4 reste **inchangé**. Publier la marge historique +0,002 comme contraste secondaire pour comparabilité.

**Une marge ne se choisit pas pour permettre à une méthode de passer un test.** L'effet minimal pratiquement utile et le score de référence doivent être justifiés indépendamment des observations du contrôleur. Si cette justification échoue, maintenir H2 initial et enregistrer NO-GO.

## Puissance / garde-fou

Les 20 instances DEV avec oracle artificiel ont un écart-type inter-instances des gains ~0,00370. À n=200, α conservateur 0,05/3 et puissance indicative 80 %, une différence vraie doit excéder la nouvelle marge de ~0,00078 (gain vrai ≈0,00108) pour être détectable ; aucun chiffre de puissance n'est garanti pour une sonde réelle encore inconnue. Le contrôle artificiel parfait payé 32 évaluations a 4/20 instances en violation H4 (≥1 graine régressant de >0,001). **Ce problème de budget ne disparaît pas avec un meilleur classificateur.**

## Conditions avant scellement

- Pertes V10/POLYMATH **mesurées à plusieurs budgets**, y compris traitement des timeouts CPU ; aucune interpolation arbitraire.
- S_ref(i), échelle, T_norm, machine, seeds, bibliothèques de sondes, vraisemblances, code et analyses figés.
- Définition de l'utilité pratique du seuil 0,0003, étude de puissance sur corpus DEV indépendant, règle de non-régression H4.
- Un seul manifeste du paquet complet, publication SHA256 datée et vérifiable **avant toute instance confirmatoire G2**.

**Statut final : cette page est un AMENDEMENT CANDIDAT, pas une modification scellée de H2.**
