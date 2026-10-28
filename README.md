# Ranking Circular Economy Performance across the EU27

A Python framework that ranks the 27 EU member states on circular economy performance using 16 official Eurostat indicators (2010-2024), tests how stable the ranking is, and profiles unusual country-years.

## Approach

1. **Data:** 16 indicators from the Eurostat Circular Economy Indicators (CEI) database, covering production and consumption, waste management, secondary raw materials, and competitiveness and innovation.
2. **Preparation:** missing values imputed (Iterative Imputer vs KNN compared), skewed indicators log-transformed, min-max normalisation that respects each indicator's direction (higher or lower is better).
3. **Weighting:** two objective schemes, Entropy and CRITIC.
4. **Ranking:** TOPSIS (primary) and VIKOR (cross-check).
5. **Robustness:** Monte Carlo simulation, 5,000 trials with randomly drawn indicator weights (Dirichlet), reporting each country's average rank and volatility.
6. **Clustering and outliers:** K-Means at country level; DBSCAN on a 370-observation country-year panel, with PCA used to profile and cross-check outliers.

## Selected findings

- Entropy- and CRITIC-weighted rankings differ substantially for several countries (e.g. Finland ranks 4th vs 20th).
- Under random weights, Slovenia, Slovakia, Czechia, the Netherlands and Croatia are the most consistently high-ranked; Luxembourg and Finland, top-4 under Entropy weights, fall to roughly 10th-20th on average, i.e. their lead depends on the weighting.
- DBSCAN flags 45 of 370 country-years (12.2%) as noise, mostly Malta, Luxembourg and Estonia; Estonia, Finland and Ireland form small, distinct clusters.
- The first principal component explains only about a third of the variance, so performance is multidimensional.

## Limitations

- Secondary data; some indicators needed imputation.
- Entropy weighting puts about 24% of the weight on one indicator (recycling patents per million inhabitants), which drives much of the ranking's sensitivity.
- DBSCAN results depend on `eps` and `min_samples`. The PCA-distance outlier check agrees with DBSCAN only modestly beyond chance (see the agreement cell in the notebook).
- Rankings depend on the 16 chosen indicators.

## How to run

1. Download the 20 Eurostat CEI datasets as CSV (`estat_cei_*.csv`) from the [Eurostat database](https://ec.europa.eu/eurostat/web/circular-economy/database) and place them in the same folder as the notebook.
2. `pip install -r requirements.txt`
3. Open `eu27_circular_economy_mcda.ipynb` and run all cells. Outputs go to `data/` and `outputs/`.

## Data source

Eurostat, Circular Economy Indicators. Reused with attribution under Eurostat's reuse policy.
