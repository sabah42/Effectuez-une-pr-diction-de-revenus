# Effectuez-une-pr-diction-de-revenus

## Table des matières

- [Description du projet](#description-du-projet)
- [Structure du projet](#structure-du-projet)
- [Jeu de données](#jeu-de-données)
- [Outils et bibliothèques](#outils-et-bibliothèques)
- [Préparation des données](#préparation-des-données)
- [Analyse exploratoire](#analyse-exploratoire)
- [Création du nouvel échantillon](#création-du-nouvel-échantillon)
- [Modélisation](#modélisation)
- [Résultats clés](#résultats-clés)
- [Recommandations](#recommandations)

## Description du projet
Ce projet a été réalisé dans le cadre du parcours Data Analyst chez OpenClassrooms.
Il vise à construire un modèle statistique capable d'estimer le revenu potentiel d’un individu, à partir de trois variables :
- Le revenu moyen du pays ;
- L’indice de Gini du pays (mesurant les inégalités) ;
- La classe de revenu des parents.

Il s’agit ici **d’une modélisation exploratoire**, sans passer à la prédiction finale.  
Le but est de **comprendre l’impact des origines sociales et géographiques** sur les revenus futurs, à l’échelle mondiale.
## Structure du projet

## Jeu de données

- **Base de données 1** : World Income Distribution (2008)
- **Base de données 2** : Population mondiale par pays
- **Base de données 3** : Indices de Gini (Banque mondiale)
- **Base de données 4** : Coefficients d’élasticité (mobilité intergénérationnelle)
- Toutes les données ont été harmonisées pour couvrir une année par pays.

## Outils et bibliothèques

- Python 3
- Pandas / NumPy
- Matplotlib / Seaborn
- Statsmodels
- Scikit-learn
- SciPy

## Préparation des données

- Suppression des doublons et harmonisation des années (2004–2011)
- Traitement des valeurs manquantes dans les colonnes : `gdp_ppp`, `centile`, `population`, `indice_gini`
- Calculs ou estimations des valeurs manquantes (Gini, population)
- Agrégation des données par pays

## Analyse exploratoire

- Visualisation des **distributions de revenus** pour 5 pays représentatifs (Islande, Vietnam, Paraguay, Chili, France)
- Création des **courbes de Lorenz** pour illustrer les inégalités
- Analyse des **indices de Gini** sur plusieurs années
- Classement des pays par indice de Gini (France située parmi les pays les plus égalitaires)

## Création du nouvel échantillon

- Simulation de la **classe de revenu des parents** à partir du coefficient d’élasticité `ρ`
- Génération de **distributions conditionnelles** `P(c_parent | c_enfant, ρ)` pour chaque pays
- Création d’un nouvel échantillon 500 fois plus grand que l’original
- Attribution des classes `c_parent` par échantillonnage aléatoire conditionné

## Modélisation

### ANOVA
- Modèle : `revenu ~ pays`
- Variance expliquée : **49.4%**
- En logarithme : **72.9%**

### Régression linéaire 1
- Modèle : `revenu ~ revenu_moyen + indice_gini`
- Variance expliquée : **49.4%**
- En logarithme : **72.7%**
- Le revenu moyen est hautement significatif.

### Régression linéaire 2 (avec c_parent)
- Modèle : `revenu ~ revenu_moyen + indice_gini + classe_revenu_parent`
- Variance expliquée : **52%**
- En logarithme : **78.3%**
- Le rôle du pays reste dominant.

### Régression finale
- `ln(revenu) ~ ln(revenu_moyen) + indice_gini + classe_revenu_parent`
- Variance expliquée : **85.1%**
- Toutes les variables sont significatives.

## Résultats clés

- Le **revenu moyen du pays** explique à lui seul **plus de 60%** de la variance du revenu individuel.
- L’**indice de Gini** a un impact plus limité.
- La **classe sociale des parents** joue un rôle important mais secondaire.
- Le **modèle logarithmique** est systématiquement le plus performant.
- **Les résidus ne suivent pas une loi normale**, ce qui suggère des pistes d’amélioration.

## Recommandations

- Utiliser le modèle log-transformé pour toute étude de revenu.
- Mieux prendre en compte d'autres facteurs (éducation, santé, genre, etc.).
- Approfondir l’étude dans les pays à forte mobilité sociale (faible `ρ`).
- Mener une collecte de données complémentaire sur les individus (niveau d’études, secteur d’activité…).



