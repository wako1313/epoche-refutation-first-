# ÉPOCHÉ — G2 / protocole candidat : vue publique

**Date de référence :** 2026-10-24  
**STATUT : NON SCELLÉ, NON EXÉCUTABLE, AUCUN HOLDOUT G2 GÉNÉRÉ.**  
**Référence de l'original complet :** `PROTOCOLE_G2_CANDIDAT_v1.0.md`, SHA256 `bc5b61fd4a848837af4cd32265b23efdcc19c0e983f2bd79b5a8ba6246b03832`.  
**Important :** cette vue synthétique n'est pas le document source de 19 Ko et ne remplace pas ses détails. Seule l'empreinte est publiée ici dans l'attente du dépôt du fichier source intégral et des artefacts figés.

## Question G2

Une politique ÉPOCHÉ-DP, évaluée sur des familles inédites de fonctions à boîte noire, réduit-elle la perte finale totale par rapport à un diagnostic fixe, un diagnostic aléatoire, une politique sans diagnostic et une politique gloutonne, à nombre d'évaluations et CPU contrôlés ?

La porte G2 ne vise pas à démontrer une « percée » ou une supériorité universelle.

## Conception confirmatoire envisagée

- 200 **instances distinctes**, communes aux sept bras, avec cinq graines d'agent par instance. L'unité inférentielle est l'instance et non une graine. Maximum de 7 000 exécutions par bras × instance × graine cumulés.
- Dimension primaire : n = 56.
- Distribution i.i.d. : linéaire 10 %, quadratique clairsemée (densité 2,2 %) 25 %, quadratique dense (12 %) 25 %, cubique avec 1–3 monômes 25 %, hors-modèle/adversarial 15 %. Les proportions sont des probabilités de tirage, pas des effectifs fixes.
- 20 instances pilotes indépendantes, hors G2, exclusivement pour faisabilité et variance. Paramètres et graines devront être fixés avant générer le G2 définitif.
- Budget maximum : 1 261 appels *physiques* de fonction, **incluant** sondes, reconstruction, validation, optimisation et repli ; temps CPU complet mesuré et plafonné par une valeur fixée avant G2.
- Sept bras : A1 ÉPOCHÉ-DP (Bellman, ≤3 sondes), A2 quatre partitions fixes, A3 sondes aléatoires en nombre apparié à A1, A4 V10 seul, A5 MORPHOSIS descriptif, A6 glouton RVoI, A7 oracle informé de la classe (plafond descriptif).
- Les seules décisions terminales calibrables à ce stade sont V10 et POLYMATH ; aucune perte inventée pour MORPHOSIS, EXTERNE ou BOTTOM.

## Score primaire et hypothèses

`L_total = Q_loss + 0.002*(E/1261) + 0.002*(T/T_max)`, avec `Q_loss = S_ref(M)-S_final` et `S_ref(M)`, `T_max` fixés exclusivement à partir des données de développement.

Hypothèses sur les **moyennes de contraste par instance**, vérifiées au moyen de tests unilatéraux et correction Holm sur H1–H3 :

- **H1 :** `mean(0.85*L_A2 - L_A1) > 0` : réduction relative ≥15 % face au diagnostic fixe.
- **H2 :** `mean(L_A4-L_A1-0.002) > 0` : marge absolue ≥0,002 face à V10.
- **H3 :** `mean(L_A6-L_A1-0.0005) > 0` : marge ≥0,0005 face au glouton.
- **H4 :** borne supérieure unilatérale Clopper-Pearson 95 % ≤2 % sur la fraction d'instances où, pour ≥1 des cinq graines, `S_A1 < S_A4 - 0.001`. Avec n=200, une seule violation suffit à dépasser le seuil de borne.

Méthodes : t apparié au niveau instance, bootstrap stratifié par **instance entière** (10 000 rééchantillonnages), IC95, transparence sur régressions, échecs, CPU et AUC. La puissance de H3 avec n=200 n'est pas garantie ; aucune augmentation opportuniste d'effectif.

**GO G3** si et seulement si H1, H2, H3 et H4 passent les règles annoncées. Sinon **NO-GO** ou **inconclusif** selon les tableaux, sans redéfinir les critères.

## Conditions bloquantes avant véritable gel

1. Spécification complète du générateur déterministe, échelles de coefficients, normalisation, graines et versions logicielles ;
2. définition indépendante, vérifiable et non biaisée des cas hors-modèle ;
3. bibliothèque exacte de sondes et modèle de vraisemblance calibré hors holdout ;
4. score de référence `S_ref(M)` pour chaque famille, et pertes V10/POLYMATH calibrées sans double comptage ;
5. limite CPU, `T_max`, cache et règle d'interruption ;
6. code et scripts statistiques figés, auditables, intégrité SHA256 et publication horodatée des sources **avant** la première instance.

**Publication de cette synthèse et de l'empreinte : acte préparatoire seulement.** Elle n'autorise aucune affirmation de pré-enregistrement final ni d'indépendance d'un tiers.
