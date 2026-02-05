## Navigation
Pour naviguer entre les différentes étapes du projet, veuillez sélectionner les sous-branches nommées dans l’ordre d’exécution.  
*(Méthodologie → Modélisation → Benchmark & synthèse)*
*Insérer capture d’écran de l’arborescence GitHub ici*

## 🏦 Contexte
Ce projet synthétise l'étude empirique réalisée dans le cadre de mon mémoire de Master (INSEEC PGE - Finance). La problématique centrale repose sur la maîtrise de la volatilité de la valeur liquidative (NAV) dans un environnement de marché instable. Contrairement aux approches spéculatives classiques, l'intéret premier de ce travail consiste à traiter les actifs numériques non pas comme des vecteurs de performance, mais comme des **équivalents-cash numériques**. Cette étude explore la substitution d'une fraction de la poche obligataire par une poche de stablecoins à pondération dynamique afin de déterminer le potentiel de ces nouveaux actifs pour les fonds.

## 💎 Pourquoi les Stablecoins ?
L'innovation de ce modèle repose sur la création d'une poche **SAS (Stable Aggregated Stablecoins)**. 
- Alternative aux T-Bonds : Les obligations, bien que sûres à maturité, subissent une volatilité de prix (juste valeur) impactant la NAV quotidienne. Mon travail démontre que le panier SAS présente une volatilité moyenne **8 fois inférieure** à celle des T-Bonds à 5 ans sur la période 2021-2025.
- Pondération Dynamique : Le panier n'est pas statique, il utilise une pondération basée sur la dominance relative (Market Cap) de l'USDT et de l'USDC. Cette approche offre des propriétés "auto-nettoyantes" face aux crises de confiance (phénomènes de flight-to-safety).
- Ancre de Liquidité : L'objectif est d'isoler le passage d'un risque de taux vers un risque de parité (depeg), tout en maintenant une exposition constante au risque de marché via la poche actions.

## 🎯 Objectifs
- Identifier les déterminants de la volatilité du portefeuille et mesurer l’impact relatif de chaque poche sur la volatilité réalisée à 30 jours de la NAV.
- Tester la décorrélation des sources de risque entre les différentes classes d’actifs.
- Comparer les comportements selon les régimes de marché en distinguant un régime normal d’un régime de stress.
- Adopter une logique proche de l’asset management réel, orientée gestion du risque plutôt que rendement théorique.
- Fournir une méthodologie claire, auditable et reproductible, basée sur un pipeline de données automatisé.

## 🚀 Résultats
- Dominance du risque action confirmée : la volatilité du marché actions demeure le principal moteur de la variabilité de la NAV.
- Existence de leviers stabilisateurs : certaines poches présentent une contribution négative et statistiquement significative à la volatilité globale.
- Décorrélation en période de tension : les sources de risque ne réagissent pas de manière homogène lors des chocs de marché.
- Renforcement des mécanismes de protection en régime de stress, mis en évidence par l’analyse conditionnelle.
- Lecture du risque robuste grâce à l’isolation des dynamiques de queue.

## 🔁 Workflow
1. Construction du portefeuille étudié et définition des pondérations.
2. Collecte et harmonisation des données macro-financières et de marché sur un calendrier quotidien.
3. Analyse par les statistiques descriptives : distributions, percentiles et comparaison des niveaux de volatilité.
4. Modélisation économétrique :
   - Régression linéaire multiple (OLS) avec erreurs robustes (traitement de l'hétéroscédasticité des résidus via le protocole HC3).
   - Diagnostics statistiques (hétéroscédasticité, non-normalité).
   - Régression des quantiles pour isoler le risque de queue.
5. Interprétation orientée gestion du risque et synthèse des enseignements.

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
- Étape 1 : Méthodologie, construction du dataset et analyses descriptives.
- Étape 2 : Modélisation économétrique et interprétation des leviers de risque.
- Étape 3 : Benchmark de portefeuilles et synthèse risk–return.
