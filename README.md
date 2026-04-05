# MBAESG_2026__EVALUATION_DATAENGINEER_MLOPS

Analyse de l'Importance des Features et Interprétation Métier
Importance des Features (XGBoost)
Les features les plus influentes dans la prédiction du prix, classées par importance :

1. AREA (Surface) — Importance Très Élevée
La surface habitable est le facteur n°1 du prix immobilier partout dans le monde. C'est logique : plus une maison est grande, plus elle coûte cher. La relation n'est pas parfaitement linéaire — les très grandes surfaces ont un prix au m² souvent plus élevé (effet prestige), ce que XGBoost capture mieux qu'une régression linéaire.

Dans la vie réelle : C'est le premier critère que regarde un acheteur. Les annonces immobilières mettent toujours la surface en avant. Un acheteur compare le prix/m² entre différents biens.

2. BATHROOMS (Salles de bain) — Importance Élevée
Le nombre de salles de bain est un indicateur indirect du standing de la maison. Une maison avec 3 salles de bain est généralement plus luxueuse qu'une avec 1 seule, même à surface égale. C'est aussi un marqueur de confort familial.

Dans la vie réelle : Ajouter une salle de bain est l'une des rénovations qui augmente le plus la valeur d'un bien. Les familles nombreuses et les acheteurs haut de gamme recherchent plusieurs salles de bain.

3. STORIES (Étages) — Importance Élevée
Plus d'étages signifie généralement plus de surface habitable et une meilleure organisation de l'espace (séparation jour/nuit). Les maisons à étages sont aussi perçues comme plus "imposantes".

Dans la vie réelle : Une maison de plain-pied vs. R+2 sur le même terrain n'a pas du tout la même valeur. Les étages permettent aussi de maximiser la construction sur un terrain limité en zone urbaine.

4. AIRCONDITIONING (Climatisation) — Importance Moyenne-Élevée
La climatisation est un marqueur de confort moderne. Son impact sur le prix dépend du contexte — elle a plus de valeur dans les régions chaudes. C'est aussi un indicateur que la maison a été bien entretenue/modernisée.

Dans la vie réelle : Dans les marchés où les étés sont chauds, une maison sans clim se vend significativement moins cher. C'est devenu quasi indispensable dans de nombreuses régions, et son absence est un point de négociation pour l'acheteur.

5. PREFAREA (Zone Privilégiée) — Importance Moyenne-Élevée
L'emplacement est roi en immobilier. Être dans une zone privilégiée (bon quartier, proximité des services, sécurité) est un multiplicateur de prix. Deux maisons identiques dans des quartiers différents peuvent avoir des prix très différents.

Dans la vie réelle : "Location, location, location" — le mantra de l'immobilier. Les zones privilégiées offrent de meilleures écoles, moins de criminalité, plus de commerces, et une meilleure valorisation à long terme.

6. PARKING (Places de parking) — Importance Moyenne
Le nombre de places de parking reflète la praticité de la maison. En zone urbaine, le parking est un luxe qui se monnaye cher. Il indique aussi la taille du terrain.

Dans la vie réelle : Dans les villes denses, une place de parking peut valoir 20-50K€. Les familles avec 2 voitures cherchent absolument un double garage. C'est aussi un critère éliminatoire pour beaucoup d'acheteurs.

7. BEDROOMS (Chambres) — Importance Moyenne
Le nombre de chambres définit la capacité d'accueil de la famille. Cependant, son importance est modérée car elle est corrélée avec la surface (une grande maison a naturellement plus de chambres).

Dans la vie réelle : Le passage de 2 à 3 chambres est le seuil le plus impactant — c'est la différence entre un couple et une famille. Au-delà de 4 chambres, l'impact marginal diminue.

8. MAINROAD (Route Principale) — Importance Moyenne-Faible
Être sur une route principale offre un meilleur accès mais aussi plus de bruit et de trafic. L'impact est ambivalent.

Dans la vie réelle : C'est un compromis accessibilité vs. tranquillité. Les maisons sur route principale se vendent plus vite (visibilité) mais parfois moins cher (nuisances). XGBoost capture cette nuance mieux qu'un modèle linéaire.

9. BASEMENT (Sous-sol) — Importance Faible-Moyenne
Un sous-sol ajoute de l'espace utilisable (stockage, buanderie, salle de jeux) sans augmenter l'emprise au sol.

Dans la vie réelle : Un sous-sol aménagé peut ajouter 10-20% à la valeur. Mais un sous-sol humide ou inondable est un point négatif. Le dataset ne distingue pas ces cas.

10. FURNISHINGSTATUS (Ameublement) — Importance Faible
L'état d'ameublement a un impact limité car les meubles sont temporaires et subjectifs. Une maison meublée se vend légèrement plus cher, mais c'est surtout un facilitateur de vente rapide.

Dans la vie réelle : Le mobilier est rarement inclus dans le prix de vente en France, mais dans d'autres marchés (location, résidence secondaire), c'est un plus. L'impact principal est sur le délai de vente, pas le prix.

11. GUESTROOM & HOTWATERHEATING — Importance Faible
Ce sont des features secondaires qui n'influencent que marginalement le prix. Le chauffage eau chaude est quasi standard, et la chambre d'amis est un bonus mais pas un critère décisif.

Pourquoi ces Résultats sont Cohérents
L'ordre d'importance correspond exactement aux critères utilisés par les professionnels de l'immobilier pour évaluer un bien :

Surface + Emplacement = 60-70% du prix
Standing (bathrooms, clim, étages) = 20-25% du prix
Extras (parking, sous-sol, ameublement) = 5-15% du prix
XGBoost a appris ces relations implicitement à partir des données, ce qui valide la qualité du modèle et du dataset.
