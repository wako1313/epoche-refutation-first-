# ÉPOCHÉ — D₂ parcimonie : spécification seulement, PAS de développement de sonde

On propose 32 différences finies mixtes de paires (4 évaluations chaque, au maximum 128 appels physiques hors cache) pour distinguer `M_quad_s` (p≈0,022) et `M_quad_d` (p≈0,12) à n=56.

Sous le modèle **idéal** de paires indépendantes et d'une mesure exacte de nullité, seuil « dense si au moins deux paires non nulles » :
- faux dense sachant sparse ≈15,602 % ;
- faux sparse sachant dense ≈8,972 % ;
- erreur équilibrée ≈12,287 %.

Ces calculs contredisent une précision promise >95 % avec 32 paires. Les vrais syndromes D₂ peuvent également être non nuls à cause d'interactions cubiques, et le bruit exige une tolérance calibrée. Enfin 1261−128=1133, inférieur au coût V10 terminal *supposé* de 1150 évaluations : il faut des modèles de pertes/qualité à budget réduit.

**Conclusion : aucune sonde D₂ n'est implémentée dans ce cycle ; tester sa valeur uniquement après correction des coûts et de la gouvernance H2/H4.**
