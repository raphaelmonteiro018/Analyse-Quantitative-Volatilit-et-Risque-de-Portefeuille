## 🏦 Contexte
Ce projet synthétise l'étude empirique réalisée dans le cadre de mon mémoire de Master (INSEEC PGE - Finance). La problématique centrale repose sur la maîtrise de la volatilité de la valeur liquidative (NAV) dans un environnement de marché instable. Contrairement aux approches spéculatives classiques, l'intéret premier de ce travail consiste à traiter les actifs numériques non pas comme des vecteurs de performance, mais comme des **équivalents-cash numériques**. Cette étude explore la substitution d'une fraction de la poche obligataire par une poche de stablecoins à pondération dynamique afin de déterminer leur prétendue stabilité pour les portefeuilles.

## 💎 Pourquoi les Stablecoins ?
L'innovation de ce modèle repose sur la création d'une poche **SAS (Stable Aggregated Stablecoins)**. 
- Alternative aux T-Bonds : Les obligations, bien que sûres à maturité, subissent une volatilité de prix (juste valeur) impactant la NAV quotidienne. Mon travail démontre que le panier SAS présente une volatilité moyenne **8 fois inférieure** à celle des T-Bonds à 5 ans sur la période 2021-2025.
- Pondération dynamique : Le panier n'est pas statique, il utilise une pondération basée sur la dominance relative (Market Cap) de l'USDT et de l'USDC. Cette approche offre des propriétés "auto-nettoyantes" face aux crises de confiance (phénomènes de flight-to-safety).
- Ancre de liquidité : L'objectif est d'isoler le passage d'un risque de taux vers un risque de parité (depeg), tout en maintenant une exposition constante au risque de marché via la poche actions.

## 🎯 Objectifs
- Identifier les déterminants de la volatilité du portefeuille et mesurer l’impact relatif de chaque poche sur la volatilité réalisée à 30 jours de la NAV.
- Tester la décorrélation des sources de risque entre les différentes classes d’actifs.
- Comparer les comportements selon les régimes de marché en distinguant un régime normal d’un régime de stress.
- Adopter une logique proche de l’asset management réel, orientée gestion du risque plutôt que rendement théorique.
- Fournir une méthodologie claire, auditable et reproductible, basée sur un pipeline de données automatisé.

## 🚀 Résultats
*L'étude démontre que la structure 60-35-5 ne subit pas le marché, elle active des mécanismes de défense endogènes.*

- Le SAS comme "frein d’urgence" : Validation statistique d'une contribution négative à la volatilité globale (beta = -0.0325). Le panier de stablecoins n'est pas juste décorrélé, il agit comme un amortisseur de volatilité actif.
- Mutation du SAS en régime de stress : L'analyse par régression des quantiles (P50 et P90) révèle une montée en puissance de 53% de l'efficacité stabilisatrice du SAS lors des krachs. Plus la pression augmente, plus le SAS protège la NAV.
- Neutralisation de l'accélération du risque : Tandis que le bêta du S&P 500 s'emballe en période de crise (passant de 0.98 à 1.05), la combinaison T-Bonds + SAS neutralise mécaniquement la contagion de la variable cible (Y = volatilité réalisée à 30j de la NAV).
- Isolation des dynamiques de queue : Grâce à la régression des quantiles, mon modèle sépare le bruit de marché du risque de queue, prouvant que le risque de dépeg (perte de parité) est statistiquement dominé par le bénéfice qu'apporte la poche SAS en terme de stabilité.
- Robustesse HC3 : Confirmation des résultats sous conditions d'hétéroscédasticité sévère, garantissant une lecture du risque non biaisée par la variance des résidus.

## 🔁 Workflow
1. Construction du portefeuille étudié et définition des pondérations.
2. Collecte et harmonisation des données macro-financières et de marché sur un calendrier quotidien.
3. Analyse par les statistiques descriptives : distributions, percentiles et comparaison des niveaux de volatilité.
4. Modélisation économétrique :
   - Régression linéaire multiple (OLS) avec erreurs robustes (traitement de l'hétéroscédasticité des résidus via le protocole HC3).
   - Diagnostics statistiques (hétéroscédasticité, non-normalité).
   - Régression des quantiles pour isoler le risque de queue.
5. Interprétation et synthèse des enseignements.

## 🏗️ Outils utilisés
- Python : Pandas, NumPy, Statsmodels
- SQL : création de queries dans le cadre de requetes API
- Sources de données :
  - FRED
  - Yahoo Finance
  - Agrégateurs de données de marché
- Méthodes statistiques :
  - Statistiques descriptives avancées
  - OLS robuste
  - Régression des quantiles

## 📁 Contenu du projet
- Étape 1 : Méthodologie et analyses descriptives.
- Étape 2 : Modélisation et identification du risque.
- Étape 3 : Benchmark et synthèse opérationnelle.

## Navigation
Pour naviguer entre les différentes étapes du projet, veuillez sélectionner les sous-branches nommées dans l’ordre d’exécution.  
*(Méthodologie → Modélisation → Benchmark & synthèse)*
*Insérer capture d’écran de l’arborescence GitHub ici*
