## Navigation
Pour naviguer entre les différentes étapes du projet, veuillez sélectionner les sous-branches nommées dans l’ordre d’exécution.  
*(Méthodologie → Modélisation → Benchmark & synthèse)*
*Insérer capture d’écran de l’arborescence GitHub ici*

## 🏦 Contexte
Ce projet synthétise mon étude empirique réalisée dans le cadre de mon mémoire de Master au sein du cursus INSEEC PGE - Finance. Ces travaux s’inscrivent dans une problématique centrale de gestion d’actifs : la maîtrise de la volatilité de la valeur liquidative (NAV) dans un environnement de marché instable. L’objectif n’est pas de rechercher la performance maximale, mais de comprendre les moteurs du risque, d’identifier les leviers de stabilisation au sein d'un portefeuille mixte et d’évaluer leur efficacité dans une logique réaliste de pilotage (juste valeur, volatilité réalisée, contraintes opérationnelles).

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
