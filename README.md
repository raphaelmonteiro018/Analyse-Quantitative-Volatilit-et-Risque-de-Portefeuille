# 📈 Méthodologie & Analyse Descriptive

## 🎯 Objectif de l'étape
- Poser un cadre analytique clair avant toute modélisation.
- Récupérer et fiabiliser les données.
- Comprendre les moteurs de la volatilité de la NAV.
- Vérifier empiriquement l’existence de leviers de stabilisation.
- Eviter toute interprétation biaisée avant la phase économétrique.

## 🔗 Sources de données & pipeline de récupération
Les données utilisées proviennent de sources publiques reconnues pour leur fiabilité et leur usage professionnel :

- Données des marchés traditionnels : indices actions, obligations souveraines et actifs de référence (via Yahoo Finance).
- Données macro-financières : taux d’intérêt, politique monétaire et indicateurs économiques (via FRED).
- Données des marchés numériques : métriques de marché et de liquidité agrégées à partir de fournisseurs spécialisés (DeFi Llama, Dune Analytics).

L’ensemble des séries est récupéré, nettoyé et harmonisé via Python à l’aide d’un pipeline automatisé :
- Téléchargement et mise à jour des données via requetage API.
- Alignement calendaire.
- Calcul des métriques glissantes pour toute la période étudiée (volatilités, spreads, ratios).

> Le code associé est documenté et accessible dans le dépôt afin de permettre une revue complète de la méthodologie.

## 🗓️ Récupération & Harmonisation temporelle des données
Les séries étudiées combinent :
- des marchés traditionnels cotés **5j/7**,
- des instruments numériques cotés **7j/7**.

Afin d’assurer une comparabilité parfaite :
- Toutes les séries sont alignées sur un calendrier quotidien (365j).
- Application du procédé LOCF (Last Observation Carried Forward) pour les jours non cotés.
- Annualisation homogène des volatilités.

## 📊 Validation empirique par les statistiques descriptives

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

### 💡 Lecture :
- La volatilité moyenne du panier SAS est **≈ 8 fois inférieure** à celle des obligations souveraines à 5 ans.
- Même dans ses **épisodes de stress extrême**, le risque maximal du SAS reste équivalent au **niveau moyen** des T-Bonds.
- Le **Percentile 90 du SAS (1,11%)** reste très inférieur au **Percentile 10 des T-Bonds (2,97%)**.

En termes de **conservation de la valeur**, le panier SAS se comporte comme un instrument monétaire à faible variance, y compris en période de tension.

## 📐 Interprétation du coefficient de variation
Le CV élevé du SAS (**115,58%**) est purement mécanique :
- Moyenne proche de zéro.
- Rares pics de volatilité.

L’écart-type (0,81%) confirme que la **variabilité absolue reste marginale**. Cette asymétrie justifie le recours ultérieur à des outils conditionnels (quantile regression) plutôt qu’une analyse moyenne.

## 📏 Standardisation des variables (Z-score)

Les variables évoluant sur des échelles et unités de mesure très différentes, toutes les variables explicatives sont standardisées :

- Transformation en unités d’écart-type.
- Interprétation directe des coefficients en sensibilité relative.
- Comparabilité directe entre les poches du portefeuille.

## 🔍 Analyse de corrélation pré-modélisation

### Bloc 1 — Actions (proxy risque marché)

| Variable | vix_level | vix_var_daily | SP500_vol_30j |
|-------|-----------|---------------|---------------|
| vix_level | 1 | 0,195 | 0,679 |
| vix_var_daily | 0,195 | 1 | -0,063 |
| SP500_vol_30j | 0,679 | -0,063 | 1 |

Décision :  
- Conservation de la variable SP500_vol_30j (cohérence temporelle avec Y).
- VIX conservé uniquement à titre informatif.

### Bloc 2 — Obligations

| Variable | Yield | Vol 30j | Spread | Fed Rate |
|--------|-------|---------|--------|----------|
| Yield | 1 | 0,486 | -0,611 | 0,919 |
| Vol 30j | 0,486 | 1 | -0,174 | 0,385 |
| Spread | -0,611 | -0,174 | 1 | -0,874 |
| Fed Rate | 0,919 | 0,385 | -0,874 | 1 |

Décision :
- Exclusion des taux bruts (colinéarité extrême).
- Conservation des variables Yield_Spread_5Y_Fed et T-Bond_vol_30j.

### Bloc 3 — Actifs numériques stables

| Variable | SAS_vol | Mcap | Volume | Dominance | Liquidity |
|--------|--------|------|--------|-----------|-----------|
| SAS_vol | 1 | -0,28 | 0,00 | 0,26 | 0,01 |
| Volume | 0,00 | 0,00 | 1 | -0,04 | **0,9997** |

Décision :
- Exclusion du volume brut (colinéarité).
- Conservation des variables SAS_vol_30j, SAS_Liquidity_Ratio et USDT_Dominance.

### Bloc 4 — Variable de contrôle (test de non-contamination du portefeuille)

| Variable | btc_vol_30j | btc_price |
|--------|-------------|-----------|
| btc_vol_30j | 1 | -0,28 |
| btc_price | -0,28 | 1 |

Décision :
- Conservation de btc_vol_30j pour la cohérence temporelle avec Y.

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

## 🧠 Enseignements clés
- Le risque est **massivement concentré sur la poche actions**.
- Certaines variables ont une **contribution négative nette** à la volatilité globale.
- Les effets ne sont **ni linéaires ni symétriques**, surtout en régime de stress.
- La structure du modèle est **statistiquement saine et auditable**.

## ➡️ Prochaine étape
👉 **Modélisation économétrique**
- OLS robuste (HC3).
- Quantification des contributions marginales.
- Analyse du risque de queue via régression des quantiles.
