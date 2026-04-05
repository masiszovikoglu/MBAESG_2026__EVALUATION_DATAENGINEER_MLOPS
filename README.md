# MBAESG_2026__EVALUATION_DATAENGINEER_MLOPS

Analyse des Performances du Modèle
Dataset
Source: Housing_Price_Data.json depuis S3 (s3://logbrain-datalake/datasets/house_price/)
Taille: 545 observations, 13 features originales (14 après one-hot encoding)
Target: PRICE
Split: 80% train / 20% test (random_state=42)
Comparaison des 5 Modèles
Rang	Modèle	MAE	RMSE	R²
1	XGBoost	12,917	33,312	0.8725
2	Random Forest	20,373	34,945	0.8597
3	Decision Tree	17,529	36,861	0.8438
4	Gradient Boosting	22,101	38,960	0.8256
5	Linear Regression	39,584	53,620	0.6696
Analyse des Résultats
XGBoost domine sur les 3 métriques — c'est le seul modèle à obtenir simultanément le meilleur MAE, le meilleur RMSE et le meilleur R². C'est un résultat clair et sans ambiguïté.

Écart MAE entre modèles :

XGBoost vs Random Forest : -36% d'erreur moyenne (12,917 vs 20,373)
XGBoost vs Linear Regression : -67% d'erreur moyenne (12,917 vs 39,584)
Observations clés :

Linear Regression est nettement en retrait (R²=0.67) — les relations prix/features sont fortement non-linéaires, ce qu'un modèle linéaire ne peut pas capturer
Decision Tree a un MAE correct (17,529) mais un RMSE élevé (36,861), signe d'erreurs ponctuelles très grandes (overfitting sur certaines observations)
Gradient Boosting est étonnamment en 4ème position — avec les paramètres par défaut, il sous-performe par rapport à XGBoost et Random Forest
Random Forest est solide (R²=0.86) mais son MAE est 58% plus élevé que XGBoost, indiquant des prédictions individuelles moins précises
Modèle Retenu : XGBoost Optimisé
Hyperparamètres (GridSearchCV, 3-fold CV) :

Paramètre	Valeur	Rôle
learning_rate	0.2	Convergence rapide, adapté au petit dataset
max_depth	15	Arbres profonds capturant les interactions complexes
min_child_weight	5	Régularisation contre l'overfitting
n_estimators	100	Nombre d'arbres optimal pour ce learning rate
Métriques finales :

MAE: 12,917 — En moyenne, l'estimation est à ±13K du prix réel
RMSE: 33,312 — Les erreurs importantes restent contenues
R²: 0.8725 — Le modèle explique 87% de la variance des prix
Pourquoi XGBoost ?
Gradient Boosting séquentiel — Chaque arbre corrige les erreurs du précédent, contrairement à Random Forest qui agrège des arbres indépendants
Régularisation L1/L2 intégrée — Évite l'overfitting sans sacrifier la performance (contrairement à Decision Tree)
Capture des interactions — AREA × AIRCONDITIONING × PREFAREA ont un effet multiplicatif que seul un modèle non-linéaire peut apprendre
Robuste aux petits datasets — 545 lignes suffisent grâce au boosting progressif
Importance des Features
Les features les plus influentes, cohérentes avec les fondamentaux de l'immobilier :

AREA (surface) — Facteur n°1 du prix, le prix/m² est le référentiel universel
BATHROOMS — Indicateur de standing (passer de 1 à 2 SdB augmente significativement la valeur)
STORIES — Plus d'étages = plus de surface habitable sur un même terrain
AIRCONDITIONING — Marqueur de confort moderne, quasi indispensable
PREFAREA — "Location, location, location" — l'emplacement est roi en immobilier
PARKING — Luxe en zone urbaine, critère éliminatoire pour beaucoup d'acheteurs
BEDROOMS — Importance modérée car corrélé avec AREA
MAINROAD — Impact ambivalent (accessibilité vs nuisances)
BASEMENT/GUESTROOM/HOTWATERHEATING — Impact marginal
FURNISHINGSTATUS — Faible impact, les meubles sont temporaires
Limites et Axes d'Amélioration
Dataset limité (545 lignes) — risque de variance sur de nouvelles données
Pas de features géographiques (GPS, ville, code postal) — facteur n°1 manquant
Pas de données temporelles (année construction, date de vente)
Gradient Boosting sous-performant — les paramètres par défaut ne sont pas optimaux, un tuning dédié pourrait améliorer ses résultats
Pistes : stacking XGBoost + Random Forest, ajout de LightGBM/CatBoost, feature engineering avancé
