## Analyse des Performances du Modèle

### Dataset
- **Source:** Housing_Price_Data.json depuis S3
- **Taille:** 545 observations, 13 features originales (14 après one-hot encoding)
- **Target:** PRICE
- **Split:** 80% train / 20% test (random_state=42)

### Comparaison des 5 Modèles

| Rang | Modèle | MAE | RMSE | R² |
|------|--------|-----|------|-----|
| 1 | **XGBoost** | **12,917** | **33,312** | **0.8725** |
| 2 | Random Forest | 20,373 | 34,945 | 0.8597 |
| 3 | Decision Tree | 17,529 | 36,861 | 0.8438 |
| 4 | Gradient Boosting | 22,101 | 38,960 | 0.8256 |
| 5 | Linear Regression | 39,584 | 53,620 | 0.6696 |

### Pourquoi XGBoost ?

XGBoost domine sur les 3 métriques simultanément (MAE, RMSE, R²). Par rapport au 2ème modèle (Random Forest), XGBoost réduit l'erreur moyenne de 36% (12,917 vs 20,373). Son apprentissage séquentiel par boosting corrige progressivement les erreurs, tandis que sa régularisation L1/L2 intégrée évite l'overfitting malgré un dataset de seulement 545 lignes.

Linear Regression est nettement en retrait (R²=0.67), confirmant que les relations prix/features sont fortement non-linéaires.

### Hyperparamètres Optimisés (GridSearchCV, 3-fold CV)

| Paramètre | Valeur | Rôle |
|-----------|--------|------|
| `learning_rate` | 0.2 | Convergence rapide, adapté au petit dataset |
| `max_depth` | 15 | Arbres profonds capturant les interactions complexes |
| `min_child_weight` | 5 | Régularisation contre l'overfitting |
| `n_estimators` | 100 | Nombre d'arbres optimal pour ce learning rate |

### Importance des Features

Résultats cohérents avec les fondamentaux de l'immobilier :

1. **AREA** — Facteur n°1, le prix/m² est le référentiel universel
2. **BATHROOMS** — Indicateur de standing, passer de 1 à 2 augmente significativement la valeur
3. **STORIES** — Plus d'étages = plus de surface habitable sur un même terrain
4. **AIRCONDITIONING** — Marqueur de confort moderne, quasi indispensable
5. **PREFAREA** — "Location, location, location" — l'emplacement est roi
6. **PARKING** — Luxe en zone urbaine, critère éliminatoire pour beaucoup d'acheteurs
7. **BEDROOMS** — Importance modérée car corrélé avec AREA
8. **MAINROAD** — Impact ambivalent (accessibilité vs nuisances)
9. **BASEMENT/GUESTROOM/HOTWATERHEATING** — Impact marginal
10. **FURNISHINGSTATUS** — Faible impact, les meubles sont temporaires

### Métriques Finales du Modèle Déployé

- **MAE:** 12,917 — En moyenne, l'estimation est à ±13K du prix réel
- **RMSE:** 33,312 — Les erreurs importantes restent contenues
- **R²:** 0.8725 — Le modèle explique 87% de la variance des prix

### Limites

- Dataset de 545 lignes — risque de variance sur de nouvelles données
- Pas de feature géographique (GPS, ville, code postal)
- Pas de données temporelles (année construction, date de vente)
- Gradient Boosting sous-performant avec paramètres par défaut — un tuning dédié l'améliorerait
