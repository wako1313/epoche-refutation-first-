# ÉPOCHÉ — Protocole G2, version candidate consolidée v1.1

**Rédaction :** 24 octobre 2026. **Statut : CANDIDAT / NON SCELLÉ / NO_START.**
**Remplace pour la préparation, mais n'efface pas :** v1.0, SHA256 `bc5b61fd4a848837af4cd32265b23efdcc19c0e983f2bd79b5a8ba6246b03832`.  
**Objet :** triage structural à budget contraint ; **aucun holdout G2 ne doit être généré avant le scellement du paquet complet**.

Ce document consolide la proposition détaillée transmise le 24/10/2026 et l'audit de la v1.0. Il conserve H1–H4 mais refuse les calibrations inventées, les références calculées sur les résultats et les adversaires biaisés contre un comparateur.

## 1. Question, périmètre et critères

Question : sur des problèmes pseudo-booléens inédits à 56 variables, ÉPOCHÉ-DP réduit-il la perte totale (qualité + appels réels + CPU) face à un diagnostic fixe, un diagnostic aléatoire, un glouton et V10 à budget égal ?

**Ne constitue pas un résultat G2 :** un gain sur les données de développement, l'exactitude des tests G1bis, ou un nouveau « paradigme ».

200 instances **indépendantes**, partagées entre tous les bras, cinq graines d'agent appariées par instance et par bras ; unité d'analyse : instance (moyenne des cinq graines). Les proportions de familles sont des probabilités de tirage i.i.d., et non un nombre forcé par famille. Six bras concurrentiels représentent **6 000 exécutions** ; A7 oracle ajoute jusqu'à **1 000 exécutions descriptives**, donc 7 000 lignes attendues si chaque oracle est exécuté cinq fois.

Le pilote/calibration est **strictement disjoint** des 200 instances G2 et n'entre dans aucun test confirmatoire. La totalité des règles de génération et des graines doit être scellée publiquement avant la première génération G2. Les données de développement peuvent être produites avant le scellement final, mais doivent être étiquetées « DEV / NON G2 ».

## 2. Générateur et distribution (cible de scellement)

| Famille | Probabilité | Paramètres transmis | État |
|---|---:|---|---|
| M_lin | 0,10 | n=56, a_i ~ N(0,1), base ±1, graine maître 1000 | Intercept et normalisation à fixer |
| M_quad_s | 0,25 | densité des paires 0,022, b_ij ~ N(0,0.5), graine 2000 | Distribution des termes linéaires/intercept à fixer |
| M_quad_d | 0,25 | densité 0,12, b_ij ~ N(0,0.5), graine 3000 | Idem |
| M_cub | 0,25 | fond quadratique + 1–3 monômes cubiques de supports uniformes, graine 4000 | Amplitudes, fond et doublons à fixer |
| M_outside | 0,15 | famille non quadratique hors-modèle, graine 5000 | Générateur indépendant des bras, construit et validé hors G2 |

**Interdiction :** ne pas construire la strate M_outside uniquement autour des quatre supports du plan A2 ; sinon A1 serait favorisé artificiellement. Les cas *ciblés* contre A2 relèvent exclusivement de G2-adv, rapporté séparément.

Les nombres 1000–5000 désignent des graines maîtresses de familles, **pas une fonction de dérivation complètement spécifiée**. Le paquet final doit figer le PRNG, la version des dépendances, le schéma instance→graine, la normalisation commune des objectifs, les distributions/échelles manquantes et le code du générateur. Les graines du pilote (6000–6020) ne doivent pas chevaucher le corpus G2.

**IMPORTANT :** « générée avant scellement » est rejeté. On peut produire le corpus de développement, mais les **200 instances d'audit doivent être générées uniquement après le scellement du code + configuration + graines**.

Pas de bruit dans G2 primaire. n=100, bruit gaussien et changements de régime exigent un protocole secondaire distinct, scellé avant sa propre analyse ; leurs résultats ne modifient pas H1–H4.

## 3. Bras et sondes

| Bras | Politique | Statut |
|---|---|---|
| A1 | ÉPOCHÉ-DP, croyance sur cinq classes, Bellman exact à horizon ≤3 sondes, terminal V10 ou POLYMATH | Confirmatoire |
| A2 | Diagnostic fixe : **quatre triplets de coordonnées** (0,1,2), (3,4,5), (6,7,8), (9,10,11), 8 valeurs par triplet avant cache | Confirmatoire |
| A3 | Diagnostic aléatoire : même bibliothèque et nombre de sondes que A1, enregistré comme contrôle apparié par profondeur | Descriptif, ne doit pas guider A1 |
| A4 | V10 tabu authentique non modifié, budget complet | Confirmatoire principal |
| A5 | POLYMATH seul, reconstruction sans routage ni gardien | Descriptif |
| A6 | Glouton RVoI, même bibliothèque/vraisemblances/budgets que A1 | Confirmatoire |
| A7 | Oracle informé de la **classe**, pas des coefficients, routeur parmi décisions réellement calibrées | Plafond descriptif |

Une sonde à trois *groupes de variables* (« grouped/block difference ») est **distincte** d'un triplet de coordonnées. Les partitions couvrantes G1bis n'entrent dans le G2 primaire que si leur liste, leurs coûts, leurs ancres et les tests de couverture sont figés avant génération. Le dossier final doit fixer exactement les ≤3 sondes admissibles à la DP. A2 ne devient pas automatiquement un test groupé.

A3, qui reçoit une longueur égale au nombre de sondes réellement faites par A1, est un contrôle apparié **conditionnel** et doit être exécuté sans accès aux valeurs/objectifs vus par A1. Ajouter une analyse exploratoire de contrôle indépendant préfixé de longueur fixe si utile.

Réserve de budget : la contrainte `reserve = coût(V10 à budget plein)` **ne peut pas coexister avec une sonde payante sous B=1261**. La DP doit calibrer et utiliser le coût d'un V10 **réduit** ; elle ne peut pas garantir `score(A1) >= score(A4)-ε` sur chaque exécution. Mesurer la fréquence de violation H4. Le même budget et le même mécanisme de cache sont appliqués à tous les bras.

## 4. Plafonds, score et risque

`B_max=1261` appels **physiques** à f par exécution, incluant sondes/validation/reconstruction/solveur et éventuel repli. Mémoïsation commune : coût physique facturé une fois, chaque hit journalisé. `C_max=2.0 sec` CPU processus proposé par le porteur, à confirmer sur matériel et instrumentation de développement ; une fois confirmé, plafond dur pour tous les bras.

`L_total(i,g,a) = (S_ref(i)-S_final(i,g,a)) + 0.002*E(i,g,a)/1261 + 0.002*T(i,g,a)/T_norm`.

**S_ref(i) et T_norm sont pré-définis avant G2**. Interdiction d'utiliser « meilleur score obtenu parmi les bras de G2 » comme S_ref : cela serait défini *par les données testées*. La référence doit être une borne analytique par instance déterminée exclusivement par le générateur ou une valeur de référence verrouillée hors G2, avec transformation de score/échelle cohérente et non négative pour rendre H1 relatif interprétable. En cas d'impossibilité de définir une telle référence, **NO_START et amendement avant scellement**. `T_norm=2.0 sec` est candidat cohérent avec `C_max=2.0 sec` (la valeur historique 1,82 s n'est plus imposée).

Attention : le seuil **relatif** H1 de 15 % est sensible au choix de la référence additive S_ref. Publier la règle exacte et une analyse de sensibilité sur données de développement ; ne pas choisir S_ref en fonction des résultats G2.

Pour Bellman, les pertes terminales sont des **pertes de qualité futures conditionnelles au budget restant** ; la DP facture séparément les appels et le CPU des sondes et des solveurs. **Ne jamais** ajouter à une table de `L_total` déjà facturée les mêmes ressources une seconde fois.

Aucun `L(d,M)` fictif pour M_lin/M_outside ni pour MORPHOSIS, EXTERNE ou BOTTOM. Les décisions réellement admissibles à A1 sont V10 et POLYMATH **après** calibration indépendante de leurs pertes sur les cinq familles. Si cette calibration manque, statut NO_START ; des proxies sont admissibles seulement en simulation de développement clairement marquée.

## 5. Statistique, hypothèses et GO/NO-GO

Cinq graines résumées par une valeur moyenne par instance et par bras `L[i,a]`. Contrastes par instance :

- H1 : `C1[i]=0.85*L[i,A2]-L[i,A1]` ; réussite si moyenne > 0 au test unilatéral corrigé : réduction relative ≥15 %.
- H2 : `C2[i]=L[i,A4]-L[i,A1]-0.002` ; seuil absolu face à V10.
- H3 : `C3[i]=L[i,A6]-L[i,A1]-0.0005` ; supériorité pratique face au glouton.
- H4 : `V[i]=1` si **une ou plusieurs** des cinq graines présente `S_final(A1)<S_final(A4)-0.001` sur la même instance/graine. La **borne supérieure Clopper–Pearson unilatérale 95 %** de la proportion de V[i]=1 doit être ≤0,02.

H1–H3 : tests t appariés unilatéraux des contrastes contre zéro, niveau familial 0,05 et correction Holm ; 10 000 bootstrap **par instance entière** pour intervalles descriptifs 95 %, graine d'analyse scellée. Avec n=200, H4 requiert effectivement 0 violation pour passer la borne. Effets inférieurs aux seuils pratiques : NO-GO, même si une comparaison contre zéro serait significative.

Les limites de puissance sont explicites : avec σ des contrastes appariés ~0,003 et alpha effectif conservateur 0,05/3, n~124 pour détecter 0,0008 mais n~318 pour 0,0005. Le protocole garde **n=200**, sauf amendement scellé pré-G2 motivé sur le pilote de développement ; aucune augmentation après inspection du holdout.

**G2 GO si et seulement si H1, H2, H3 ET H4 passent**, sinon NO-GO promotion G3, avec descriptif « non concluant » lorsque puissance insuffisante. Aucun arrêt/consultation séquentielle fondé sur les scores, y compris H4, avant l'analyse finale.

Rapports secondaires : P95 de perte de qualité, score final, AUC incumbent, CPU médian, appels médians, taux de repli, croyance finale, fréquence d'erreurs de routage et distribution par famille ; aucune de ces métriques ne remplace H1–H4.

## 6. Pilote et calibration obligatoires (avant scellement)

L'expérience pilote **n'est pas** G2. Le plan de la version concurrente propose 20 instances en tout : c'est acceptable comme test d'intégration, mais **4 cas par famille sont insuffisants pour calibrer de manière crédible 5 likelihoods et 10 pertes V10/POLYMATH**. Séparer :
1. *Smoke test* disjoint : 20 instances stratifiées (4 de chaque famille), 5 graines, A1/A2/A4, vérification du compteur d'évaluations, CPU, statistiques, fiabilité, aucune décision confirmatoire.
2. *Calibration développement* : volume **à décider après analyse de puissance et de stabilité**, données distinctes et hachées avant G2 ; vraisemblances empiriques par famille et par sonde, pertes V10/POLYMATH conditionnelles au budget, normalisation de score, calibration du routeur A2, gestion des cas rares. Cette étape peut être plus grande que 20.
3. Verrouillage de `S_ref`, `T_norm`, maximum CPU, sondes, générateur, likelihoods, pertes et code. Puis création d'un **seul manifeste de scellement** incluant les SHA256 de tous les composants, publié par un commit et idéalement une archive à horodatage externe.

Ne pas substituer les estimations hypothétiques des familles M_lin ou M_outside à des données mesurées. Les archives V10/POLYMATH sont disponibles pour développer le banc, mais **aucun wrapper G2 ni calibration complète ne sont encore attestés**.

## 7. Traçabilité / reproductibilité / règles d'incident

Les 7 000 exécutions potentielles consignent `instance_id, family, instance_seed, agent_seed, arm, parent_protocol_sha, generator_sha, solver_version, probe_library_sha, likelihood_sha, loss_calib_sha, evals_real, evals_cache_hit, cpu_process_s, final_score, auc, probes, decision, posterior, fallback, budget_overrun, exception`.

Les instances G2 ne peuvent pas être inspectées, régénérées avec modification du générateur ou utilisées pour calibration ; **une relecture identique à partir du paquet scellé est autorisée pour reproduction** (l'interdiction vise la sélection opportuniste, pas la reproductibilité). Toute erreur ou sortie manquante suit une règle d'imputation/échec définie **avant** G2 et apparaît dans le tableau complet.

GitHub suffit à dater publiquement le commit d'un candidat ; ce n'est ni une garantie d'indépendance scientifique ni un horodatage cryptographique immuable. Sceller aussi l'arbre de code et les configurations avec SHA256, et, si possible, déposer l'archive source par tiers indépendant.

## 8. Gouvernance des versions

La v1.0 et son SHA256 historique demeurent intacts. La v1.1 est une **amélioration candidate, non une réécriture silencieuse**. Après le pilote, toute retouche exige un amendement explicite suivi d'un nouveau gel du paquet complet **avant la première instance G2**. Une fois G2 lancé, les seuils, bras, tailles et familles ne peuvent plus changer.

**VERDICT AU 24/10/2026 : NO_START G2 — calibration/générateur/routage à finaliser.**
