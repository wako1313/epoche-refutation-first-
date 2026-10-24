# ÉPOCHÉ — Erratum au brouillon MARGIN_GATE v0.2

**Statut : NON GELÉ / G2 NO_START.** Ce texte documente les corrections du projet de protocole v0.2 sans remplacer le document utilisateur et sans modifier H1–H4.

1. **Redémarrage** : corriger partout « un candidat de redémarrage par exécution » en « **un candidat de redémarrage par itération** » (si `restart=True`). Le nombre d'appels logiques est `1+90×14=1261`, et le nombre physique maximal `1+90×13=1171` avec cache permanent.
2. **Contrôles adversariaux exécutés** : 96 cas construits (plateaux, égalités, valeurs extrêmes, collisions, redémarrages) et 100 cas DEV existants ; 196/196 sous la borne physique. Une expérience avec cache désactivé atteint 1261 évaluations physiques, ce qui falsifie le certificat hors périmètre.
3. **Relecture indépendante de méthode** : une seconde analyse automatisée des AST et expressions de code confirme la décomposition. Elle ne constitue pas une revue externe humaine.
4. **CPU hors preuve** : une fonction déterministe, statique et volontairement coûteuse produit `2,566s` CPU avec `1171` évaluations physiques. Nécessité d'une instrumentation CPU distincte.
5. **Porte statique** : pour B=1261 et k=32, admissible avec δ≤58 (δ non fixé ici). Pour B=1158/1100/1042, le contrôle rétrospectif refuse les 900 exécutions DEV à k=32.
6. **Reste ouvert** : revue humaine externe si requise, paramètres B/k/δ/CPU et machine fixés avant DEV-VAL, séparation CAL/VAL, test d'identité des trajectoires avec une **vraie** sonde, et mesure M7.

[Rapport DEV détaillé](../dev/GDET_v02_SECOND_AUDIT_2026_10_24.md). Les archives intégrales restent dans la conversation et sont identifiées par SHA256.

**Ce n'est pas un pré-enregistrement scellé.**
