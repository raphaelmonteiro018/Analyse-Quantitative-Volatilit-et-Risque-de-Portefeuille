# 📈 Méthodologie & Analyse Descriptive

## 🎯 Objectifs
- Poser un cadre analytique clair avant toute modélisation.
- Récupérer et fiabiliser les données.
- Comprendre les moteurs de la volatilité de la NAV.
- Vérifier empiriquement l’existence de leviers de stabilisation.
- Eviter toute interprétation biaisée avant la phase économétrique.

## 🔗 Sources des données
Les données utilisées proviennent de sources publiques reconnues pour leur fiabilité et leur usage professionnel :

- Données des marchés traditionnels : prix de cloture des indices actions, obligations souveraines et actifs de référence (via Yahoo Finance).
- Données macro-financières : taux d’intérêt, politique monétaire et indicateurs économiques (via FRED).
- Données des marchés numériques : métriques de marché et de liquidité agrégées à partir de fournisseurs spécialisés (DeFi Llama, Dune Analytics).

L’ensemble des séries est récupéré, nettoyé et harmonisé via Python à l’aide d’un pipeline automatisé :
- Téléchargement et mise à jour des données via requetage API.
- Alignement calendaire.
- Calcul des métriques glissantes pour toute la période étudiée (volatilités, spreads, ratios).
- Génération des fichiers source au format Excel.

> Le code associé est documenté et accessible dans le dépôt afin de permettre une revue complète de la méthodologie.

## 🗓️ Harmonisation temporelle des données
Les séries étudiées combinent :
- des marchés traditionnels cotés **5j/7**,
- des instruments numériques cotés **7j/7**.

Afin d’assurer une comparabilité parfaite :
- Toutes les séries sont alignées sur un calendrier quotidien (365j).
- Application du procédé LOCF (Last Observation Carried Forward) pour les jours non cotés.
- Annualisation homogène des volatilités.

## ⚙️Ingénierie des données & Variable cible (Y)
- Construction du rendement composite : Calcul quotidien d'un rendement pondéré (60/35/5) intégrant les variations du S&P 500, des T-Bonds à 5 ans et du panier SAS (sans intégration de rendement DeFi pour le moment).
- Gestion du biais d'initialisation : Ingestion des données dès novembre 2020 pour garantir une variable cible (Y) calculée et stable dès le premier jour de la période d'étude (01/01/2021).
- Annualisation statistique : Application d'un facteur d'annualisation ($\sqrt{365}$) pour normaliser les volatilités réalisées sur 30 jours glissants.

## 🏗️ Architecture SAS (Stable Aggregated Stablecoins)
Inspirée de travaux académiques sur la diversification des actifs numériques, la poche SAS est conçue comme un équivalent-cash numérique "auto-nettoyant".

- Composition stratégique : Sélection de l'USDT (Tether) et de l'USDC (Circle). Ces deux actifs représentent ~85% de la capitalisation totale du secteur, constituant ainsi le benchmark de liquidité idéal.
- Pondération dynamique (Market Cap Weighting) : Au lieu d'une répartition fixe (50/50), la poche est rééquilibrée quotidiennement selon la dominance relative de chaque émetteur :    $$Poids_{i,t} = \frac{Capitalisation_{i,t}}{\sum Capitalisation_{SAS,t}}$$
- Gestion active du risque émetteur : Si le marché perd confiance en un émetteur (baisse de sa capitalisation), le modèle réduit mécaniquement son exposition au profit de l'autre.
> Note : Ce mécanisme reproduit le comportement des Money Market Funds en intégrant un réflexe de Flight-to-Safety en temps réel.

## 📊 Statistiques descriptives

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

Les variables évoluant sur des échelles et unités de mesure très différentes ces dernières sont standardisées :
- Transformation en unités d’écart-type.
- Interprétation directe des coefficients en sensibilité relative.
- Comparabilité directe entre les poches du portefeuille.

## 🔍 Analyse de corrélation

Cette étape permet de :
- Valider l’absence de colinéarité excessive entre classes d’actifs.
- Observer les relations préliminaires avec la volatilité de la NAV.
- Confirmer la pertinence économique des facteurs retenus.

### Variables retenues
- **Y_vol_port_30j** : Volatilité réalisée de la NAV du portefeuille (variable cible).
- **SP500_vol_30j** : Proxy du risque actions.
- **T-Bond_vol_30j** : Volatilité de la poche obligataire (T-Bonds à 5 ans).
- **Yield_Spread_5Y_Fed** : Baromètre du cycle monétaire.
- **SAS_Volatility_30j** : Risque spécifique du panier de stablecoins (variations liées au peg).
- **SAS_Liquidity_Ratio** : Profondeur et capacité d’absorption du marché des stablecoins.
- **USDT_Dominance** : Indicateur de concentration du risque sur le marché des stablecoins et sur la poche SAS (pondérée dynamiquement entre USDT/USDC selon cette dominance).
- **btc_vol_30j** : Variable de contrôle (écosystème crypto global).

### 📊 Matrice de corrélation

| Variables | Y_vol_port_30j | SP500_vol_30j | T-Bond_vol_30j | Yield_Spread_5Y_Fed | SAS_Volatility_30j | SAS_Liquidity_Ratio | USDT_Dominance | btc_vol_30j |
|---------|----------------|---------------|----------------|---------------------|--------------------|---------------------|----------------|-------------|
| **Y_vol_port_30j** | 1 | 0,9071 | 0,4038 | 0,4127 | -0,1548 | -0,0059 | -0,5910 | 0,2709 |
| **SP500_vol_30j** | 0,9071 | 1 | 0,4624 | 0,3914 | -0,1504 | 0,0010 | -0,5926 | 0,2238 |
| **T-Bond_vol_30j** | 0,4038 | 0,4624 | 1 | -0,1739 | -0,1359 | -0,0388 | -0,3630 | -0,2086 |
| **Yield_Spread_5Y_Fed** | 0,4127 | 0,3914 | -0,1739 | 1 | 0,0633 | 0,0583 | -0,6106 | 0,4614 |
| **SAS_Volatility_30j** | -0,1548 | -0,1504 | -0,1359 | 0,0633 | 1 | 0,0099 | 0,2571 | 0,2728 |
| **SAS_Liquidity_Ratio** | -0,0059 | 0,0010 | -0,0388 | 0,0583 | 0,0099 | 1 | -0,0361 | 0,0134 |
| **USDT_Dominance** | -0,5910 | -0,5926 | -0,3630 | -0,6106 | 0,2571 | -0,0361 | 1 | -0,1135 |
| **btc_vol_30j** | 0,2709 | 0,2238 | -0,2086 | 0,4614 | 0,2728 | 0,0134 | -0,1135 | 1 |

### 🧠 Enseignements
- La volatilité du portefeuille est massivement expliquée par le risque actions (corrélation > 0,90).
- Le risque obligataire et le cycle monétaire jouent un rôle secondaire mais significatif.
- Les variables stablecoins (SAS) assimilables à des instruments monétaires présentent :
  - une **corrélation négative** avec la volatilité de la NAV.
  - une **faible dépendance** aux marchés traditionnels.
- La concentration du risque (USDT_Dominance) agit comme un facteur de stabilisation conditionnelle.
- L’absence de colinéarité excessive valide une modélisation multivariée robuste.

## ➡️ Prochaine étape
👉 **Modélisation économétrique**
- OLS robuste (HC3).
- Quantification des contributions marginales.
- Analyse du risque de queue via régression des quantiles.
