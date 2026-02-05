# 🧪 Méthodologie & Analyse Descriptive

## 🎯 Objectif de cette étape
Cette étape vise à poser un cadre analytique robuste avant toute modélisation, dans une logique proche du **FP&A / Risk Management** :

- fiabiliser les données,
- comprendre les **moteurs de la volatilité de la NAV**,
- vérifier empiriquement l’existence de **leviers de stabilisation**,
- éviter toute interprétation biaisée avant la phase économétrique.

## 🗓️ Harmonisation temporelle des données

Les séries étudiées combinent :
- des marchés traditionnels cotés **5j/7**,
- des instruments assimilables à des actifs monétaires cotés **7j/7**.

Afin d’assurer une comparabilité parfaite :
- toutes les séries sont alignées sur un **calendrier quotidien (365j)**,
- application du procédé **LOCF (Last Observation Carried Forward)** pour les jours non cotés,
- annualisation homogène des volatilités.

> 🎯 Résultat : des séries de même longueur, exploitables pour une analyse quotidienne cohérente de la volatilité réalisée.

## 📊 Validation empirique par statistiques descriptives

### Comparaison des volatilités réalisées (30 jours)
Période étudiée : **2021 – 2025**

| Métrique | Panier SAS (30j) | T-Bonds 5Y (30j) |
|--------|------------------|-----------------|
| **Moyenne** | **0,70 %** | **5,57 %** |
| **Écart-type** | 0,81 % | 2,01 % |
| **Coefficient de variation** | 115,58 % | 36,14 % |
| **Minimum** | 0,09 % | 1,53 % |
| **Percentile 10** | 0,23 % | 2,97 % |
| **Médiane** | 0,52 % | 5,39 % |
| **Percentile 90** | 1,11 % | 8,10 % |
| **Maximum** | 5,63 % | 11,81 % |

### 💡 Lecture business
- La volatilité moyenne du panier SAS est **≈ 8 fois inférieure** à celle des obligations souveraines à 5 ans.
- Même dans ses **épisodes de stress extrême**, le risque maximal du SAS reste équivalent au **niveau moyen** des T-Bonds.
- Le **Percentile 90 du SAS (1,11%)** reste très inférieur au **Percentile 10 des T-Bonds (2,97%)**.

👉 En termes de **conservation de la valeur**, le panier SAS se comporte comme un instrument monétaire à faible variance, y compris en période de tension.

## 📐 Interprétation du coefficient de variation
Le CV élevé du SAS (**115,58%**) est purement mécanique :
- moyenne proche de zéro,
- rares pics de volatilité.

L’écart-type (0,81%) confirme que la **variabilité absolue reste marginale**.  
Cette asymétrie (moyenne < médiane < rares extrêmes) justifie le recours ultérieur à des **outils conditionnels (quantile regression)** plutôt qu’une analyse moyenne.

## 📏 Standardisation des variables (Z-score)

Les variables évoluant sur des échelles très différentes (facteur 1 à 8), toutes les variables explicatives sont **standardisées** :

- transformation en unités d’écart-type,
- interprétation directe des coefficients en **sensibilité relative**,
- comparabilité directe entre poches du portefeuille.

> 🎯 Lecture FP&A : mesure de l’impact sur la NAV pour un choc équivalent (1σ).

## 🔍 Analyse de corrélation pré-modélisation

### Bloc 1 — Actions (proxy risque marché)

| Variable | vix_level | vix_var_daily | SP500_vol_30j |
|-------|-----------|---------------|---------------|
| vix_level | 1 | 0,195 | 0,679 |
| vix_var_daily | 0,195 | 1 | -0,063 |
| SP500_vol_30j | 0,679 | -0,063 | 1 |

**Décision** :  
- conservation de **SP500_vol_30j** (cohérence temporelle avec Y),
- VIX conservé uniquement à titre informatif.

### Bloc 2 — Obligations

| Variable | Yield | Vol 30j | Spread | Fed Rate |
|--------|-------|---------|--------|----------|
| Yield | 1 | 0,486 | -0,611 | 0,919 |
| Vol 30j | 0,486 | 1 | -0,174 | 0,385 |
| Spread | -0,611 | -0,174 | 1 | -0,874 |
| Fed Rate | 0,919 | 0,385 | -0,874 | 1 |

**Décision** :
- exclusion des taux bruts (colinéarité extrême),
- conservation :
  - **Yield_Spread_5Y_Fed**
  - **T-Bond_vol_30j**

### Bloc 3 — Facteurs assimilables à la liquidité

| Variable | SAS_vol | Mcap | Volume | Dominance | Liquidity |
|--------|--------|------|--------|-----------|-----------|
| SAS_vol | 1 | -0,28 | 0,00 | 0,26 | 0,01 |
| Volume | 0,00 | 0,00 | 1 | -0,04 | **0,9997** |

**Décision** :
- exclusion du volume brut (colinéarité),
- conservation :
  - **SAS_vol_30j**
  - **SAS_Liquidity_Ratio**
  - **USDT_Dominance**

### Bloc 4 — Variable de contrôle (Crypto)

| Variable | btc_vol_30j | btc_price |
|--------|-------------|-----------|
| btc_vol_30j | 1 | -0,28 |
| btc_price | -0,28 | 1 |

**Décision** : conservation de **btc_vol_30j** pour cohérence avec Y.

## 🔗 Matrice inter-blocs (architecture finale)

| Variable | Corrélation avec Y |
|--------|--------------------|
| SP500_vol_30j | **0,91** |
| T-Bond_vol_30j | 0,40 |
| Yield_Spread_5Y_Fed | 0,41 |
| SAS_vol_30j | -0,15 |
| SAS_Liquidity_Ratio | ~0 |
| USDT_Dominance | **-0,59** |
| btc_vol_30j | 0,27 |

## 🧠 Enseignements clés (FP&A / Risk)
- Le risque est **massivement concentré sur la poche actions**.
- Certaines variables ont une **contribution négative nette** à la volatilité globale.
- Les effets ne sont **ni linéaires ni symétriques**, surtout en régime de stress.
- La structure du modèle est **statistiquement saine et auditable**.

## ➡️ Prochaine étape
👉 **Étape 2 – Modélisation économétrique**
- OLS robuste (HC3),
- quantification des contributions marginales,
- analyse du risque de queue via régression des quantiles.
