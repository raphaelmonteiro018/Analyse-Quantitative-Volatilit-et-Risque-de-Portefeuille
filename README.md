# 📈 Modélisation Économétrique

## 🎯 Objectifs
- Quantifier l’impact moyen et extrême des leviers de risque sur la volatilité réalisée de la NAV (Y).  
- Identifier les actifs stabilisateurs dans les régimes de marché "croisière" vs "crise".  
- Fournir une lecture opérationnelle de la résilience de la poche SAS (Stable Aggregated Stablecoins).  

## 🔹 Régression Linéaire Multiple (OLS)

**Modèle :** OLS robuste (HC3) sur variables standardisées (Z-score)  
**Variable cible :** `Y_vol_port_30j` — Volatilité réalisée du portefeuille à 30 jours  

### Résultats principaux

| Variable | β (standardisé) | p-value | Interprétation |
|----------|----------------|---------|----------------|
| S&P 500 Vol (30j) | 0,8363 | 0,0000 | Dominance du risque actions |
| T-Bond Vol (30j) | 0,0043 | 0,7989 | Risque obligataire absorbé |
| Yield Spread (5Y) | 0,0066 | 0,6931 | Cycle monétaire peu significatif |
| SAS Volatility (30j) | -0,0325 | 0,0000 | Stabilisateur : effet amortisseur SAS |
| SAS Liquidity Ratio | -0,0104 | 0,4464 | Liquidity ratio peu significatif |
| USDT Dominance | -0,0725 | 0,0011 | Flight-to-quality vers stablecoins liquides |
| Bitcoin Vol (30j) | 0,0823 | 0,0000 | Variable de contrôle, impact positif |

**Diagnostics :**  
- R² = 0,833 → 83,3 % de la variance est expliquée.
- VIF < 3,1 → Pas de multicolinéarité.
- Hétéroscédasticité confirmée → justification HC3.
- Non-normalité des résidus et présence de queues épaisses → indication que la relation moyenne masque des comportements différenciés selon les régimes de volatilité, justifiant l’usage d’une régression des quantiles.

**Enseignements :**  
- Le risque actions domine le portefeuille.  
- La poche SAS agit comme stabilisateur actif.  
- USDT Dominance reflète un mécanisme de flight-to-quality.  
- Bitcoin capte partiellement le risque crypto lié aux actions tech.

> ⚠️ Limite OLS : le modèle OLS estime la moyenne conditionnelle de la volatilité (E[Y|X]). Il ne permet pas de capturer l’hétérogénéité des effets selon le niveau de la distribution, ce qui justifie le recours à une régression des quantiles.

## 🔹 Régression des Quantiles

**Objectif :** Isoler le risque de queue et comparer régimes médian (P50) vs stress (P90).  

### Comparaison des scénarios

| Variable | β (P50) | Signif. | β (P90) | Signif. | Evolution |
|----------|-----------|---------|-----------|---------|-----------|
| S&P 500 Vol (30j) | 0,9822 | *** | 1,0530 | *** | +7,2 % |
| T-Bond Vol (30j) | -0,0154 | n.s. | -0,0746 | *** | -383,8 % |
| Yield Spread (5Y) | 0,0172 | n.s. | 0,0338 | ** | +96,3 % |
| SAS Volatility (30j) | -0,0234 | *** | -0,0359 | *** | -53,0 % |
| SAS Liquidity Ratio | -0,0193 | *** | -0,0179 | n.s. | +6,9 % |
| USDT Dominance | -0,0105 | n.s. | -0,0651 | *** | -519,3 % |
| Bitcoin Vol (30j) | 0,0320 | *** | -0,0305 | *** | -195,1 % |

**Lecture clé :**  
- Le SAS renforce sa protection en période de stress (-53 %).  
- L’USDT devient un pilier de stabilité lors des krachs.  
- La volatilité du T-Bond s’active comme ancre défensive lors des stress.  
- Le S&P 500 reste le moteur principal du risque mais est en partie neutralisé par les actifs refuges.  

> Le portefeuille 60-35-5 ne subit pas passivement le marché : la combinaison SAS + T-Bonds agit comme un **frein qui s'accentue** en période de crise.

## 🧠 Enseignements principaux
- La poche SAS agit comme stabilisateur **asymétrique**, augmentant son efficacité lorsque la pression du marché augmente.  
- L’analyse par quantiles permet de séparer le bruit de marché du risque de queue et de confirmer le rôle anti-fragile du SAS.  
- L’approche HC3 garantit des résultats robustes malgré hétéroscédasticité et queues épaisses.  
- La combinaison actifs classiques (T-Bonds) + numériques (SAS) neutralise mécaniquement l’accélération de la volatilité des actions.  

## ➡️ Prochaine étape
- Intégration des visualisations conditionnelles de stress.  
- Tests complémentaires sur les dynamiques temporelles extrêmes.
- Intégration d'un rendement DeFi sur la poche SAS (uniquement à titre informationnel en raison de l'incertitude règlementaire sur ces sujets).
- Réalisation du benchmark.
