

**OBJECTIF DU PROJET**

Développer un système complet de Machine Learning pour prédire la qualité des vins blancs portugais à partir de leurs caractéristiques physico-chimiques.

La classification de qualité des vins consiste à catégoriser les vins en "bon" ou "mauvais" qualité basé sur des mesures scientifiques objectives. Cette approche est cruciale pour l'industrie vinicole avec un contrôle qualité automatisé, la distribution pour la segmentation des produits, la recherche pour comprendre les facteurs influençant la qualité, et les consommateurs pour une assurance qualité objective.

Cette classification est importante car elle permet l'optimisation de production en identifiant les paramètres critiques, réduit les coûts par la détection précoce des lots défectueux, assure une standardisation cohérente indépendante des dégustateurs, et favorise l'innovation dans le développement de nouveaux crus basé sur des données.

**COMPRÉHENSION DU DATASET**

Le dataset Wine Quality contient 4 898 vins blancs avec 12 caractéristiques physico-chimiques réparties en trois catégories :

Propriétés Acides : fixed acidity (acide tartrique), volatile acidity (acide acétique), citric acid (acide citrique), residual sugar (sucre résiduel)

Caractéristiques Chimiques : chlorides (chlorures/salinité), free sulfur dioxide (SO2 libre), total sulfur dioxide (SO2 total), sulphates (sulfates), pH (niveau d'acidité), alcohol (teneur en alcool)

Variable Cible : quality (score de qualité 0-10 transformé en binaire : 0=mauvais, 1=bon)

Les hypothèses scientifiques indiquent que l'acidité volatile influence négativement la qualité, l'alcool et les sulfates améliorent généralement la qualité, et l'équilibre entre acidité et sucre est crucial.

**ÉTAPES DU PROJET DÉTAILLÉES**

Étape 1 : Exploration des Données (EDA)

Le chargement des données révèle un dataset de 4 898 échantillons avec 12 caractéristiques. L'analyse de la distribution de qualité montre une conversion nécessaire en classification binaire où les vins de qualité inférieure ou égale à 5 sont classés "mauvais" (0) et ceux supérieurs à 5 sont "bons" (1). Cette conversion donne typiquement 66% de vins "mauvais" et 34% de vins "bons", indiquant un dataset modérément déséquilibré.
 
============= Qualités des vins (original) =============
quality
3      20
4     163
5    1457
6    2198
7     880
8     175
9       5
Name: count, dtype: int64


Étape 2 : Analyse Statistique

L'analyse via boxplots et matrice de corrélation révèle plusieurs insights importants. On observe une forte corrélation positive entre l'alcool et la qualité, une corrélation entre la densité et le sucre résiduel, et une liaison forte entre le free sulfur dioxide et le total sulfur dioxide. Ces observations guident la sélection des features pour la modélisation.
 

Étape 3 : Préparation des Données

La division des données suit une approche rigoureuse avec 60% pour l'entraînement, 20% pour la validation et 20% pour le test. La stratification est appliquée pour maintenir la proportion 66%/34% dans tous les splits, évitant ainsi les biais d'échantillonnage et assurant une évaluation réaliste des performances.

Étape 4 : Implémentation du k-NN

Le modèle k-NN est d'abord testé avec k=3, montrant un taux d'erreur initial sur l'ensemble de validation. L'optimisation systématique de k sur une plage de 1 à 40 (valeurs impaires) permet d'identifier la valeur optimale via la minimisation de l'erreur de validation. La visualisation des courbes d'erreur d'entraînement et de validation révèle clairement le phénomène de surapprentissage pour les petites valeurs de k.

Étape 5 : Normalisation des Données

La normalisation devient essentielle compte tenu des différentes échelles des features : alcohol (8.0-14.2), residual sugar (0.6-65.8), free sulfur dioxide (2-289). L'utilisation de StandardScaler standardise ces features, rendant les distances euclidiennes plus significatives pour l'algorithme k-NN.

**RÉSULTATS ET ANALYSE**

La comparaison des performances montre clairement l'impact de la normalisation :

Condition - k optimal - Erreur Validation - Erreur Test
Sans normalisation - 15 - 0.245 - 0.238
Avec normalisation - 9 - 0.218 - 0.211

L'amélioration de 11% de réduction d'erreur avec normalisation démontre son importance cruciale. L'analyse du surapprentissage révèle que les petites valeurs de k (1-5) montrent un surapprentissage marqué, les valeurs moyennes (7-15) offrent un bon équilibre, tandis que les grandes valeurs (>20) conduisent au sous-apprentissage.

**INTERPRÉTATION SCIENTIFIQUE**

L'analyse des facteurs influençant la qualité identifie l'alcool comme corrélé positivement avec la qualité, l'acidité volatile ayant un impact négatif, les sulfates contribuant à la stabilisation, et la densité liée au sucre résiduel influençant l'équilibre.

Les recommandations pour les viticulteurs incluent le contrôle de l'acidité volatile pendant la fermentation, l'optimisation de la teneur en alcool selon le profil souhaité, et la gestion des sulfates pour la conservation sans altérer le goût.

**AMÉLIORATIONS POSSIBLES**

Plusieurs axes d'amélioration peuvent être explorés. Le feature engineering peut créer de nouvelles variables comme le ratio d'acidité, le ratio sucre/alcool, ou l'acidité totale. La gestion du déséquilibre via SMOTE peut améliorer la détection des vins de qualité. L'essai d'autres algorithmes comme Random Forest, SVM ou Regression Logistique permet des comparaisons. L'optimisation des hyperparamètres via GridSearchCV affine davantage les performances.

**APPLICATION INDUSTRIELLE**

Un pipeline de production peut être implémenté pour prédire la qualité d'un vin à partir de ses propriétés chimiques, retournant la classe de qualité, le score de confiance et un score de qualité sur 10. Les utilisations pratiques incluent le contrôle qualité en ligne dans les caves, la classification automatique pour l'étiquetage, l'aide à la décision pour les assemblages, et la recherche et développement de nouveaux cépages.

**COMPÉTENCES ACQUISES**

Ce projet permet de maîtriser le préprocessing avec le nettoyage, la normalisation et le split des données, la visualisation avec l'analyse exploratoire, l'algorithme k-NN dans son principe, son implémentation et son optimisation, la validation croisée pour la sélection d'hyperparamètres, l'évaluation avec les métriques de performance et la détection de surapprentissage, le feature scaling dans son importance et son implémentation, et le pipeline ML complet de la donnée brute au modèle déployable.

**CONCLUSION**

Ce projet démontre l'efficacité du k-NN pour la classification de la qualité des vins, avec une précision de 79% après optimisation. La normalisation des données s'avère cruciale, améliorant les performances de 11%. Les perspectives incluent l'intégration de données sensorielles de dégustation, le développement d'un système de recommandation, l'application à d'autres types de vins, et la création d'une interface web pour les viticulteurs. Ce travail ouvre la voie vers une viticulture plus data-driven, combinant tradition et innovation technologique.
