# Prédiction de la gravité des collisions de véhicules à Seattle

## Présentation
Ce projet fait partie du projet final du **Certificat professionnel IBM Data Science**. Il vise à construire un modèle de classification capable d’estimer la gravité d’une collision routière dans la région de Seattle, à partir de variables liées aux conditions météorologiques, à l’état de la chaussée, au type de localisation, au secteur géographique et au jour de la semaine.

L’objectif principal est de déterminer si une collision correspond à un accident avec dommages matériels seulement ou à un accident avec blessés. Ce type d’analyse peut soutenir la prise de décision en matière de sécurité routière, notamment pour prioriser certaines interventions, améliorer la signalisation, cibler des zones à risque ou mieux comprendre les conditions associées aux collisions.

## Objectifs
Le projet est structuré en plusieurs étapes complémentaires, de la compréhension du problème métier jusqu’à l’évaluation des modèles prédictifs. Chaque étape contribue à transformer les données brutes en informations exploitables pour l’analyse et la modélisation.

### 1. Compréhension du problème métier
- **Contexte** : Les collisions routières représentent un enjeu important pour les autorités publiques, les gestionnaires d’infrastructures et les usagers de la route.
- **Question analytique** : Étant donné les conditions de route, la météo, le jour de la semaine et la zone géographique, quelle est la gravité probable d’un accident?
- **Valeur attendue** : Fournir un appui analytique pour orienter les mesures de prévention, de signalisation et d’aménagement routier.

### 2. Collecte et chargement des données
- **Source de données** : Le projet utilise le jeu de données public des collisions routières de la ville de Seattle, fourni dans le cadre du cours IBM.
- **Chargement des données** : Les données sont importées depuis un fichier CSV et chargées dans un DataFrame Pandas.
- **Volume initial** : Le jeu de données contient 194 673 enregistrements et 38 variables avant nettoyage et sélection.

### 3. Nettoyage et préparation des données
- **Analyse des valeurs manquantes** : Les colonnes et observations comportant des données manquantes sont inspectées afin d’évaluer leur impact sur l’analyse.
- **Suppression des colonnes peu exploitables** : Les variables comportant un taux trop élevé de valeurs nulles sont exclues du traitement.
- **Filtrage des collisions** : Certaines catégories de collision, comme les véhicules stationnés ou les cas trop génériques, sont retirées afin de conserver un périmètre plus cohérent pour la modélisation.
- **Conversion des types de données** : La date de l’incident est convertie en format datetime afin de permettre l’extraction du jour de la semaine.

### 4. Ingénierie des variables
- **Variable cible** : La variable `SEVERITYCODE` est recodée pour distinguer les collisions avec dommages matériels des collisions avec blessés.
- **Variables explicatives** : Le modèle utilise principalement `DISTRICT`, `WEATHER`, `ROADCOND`, `ADDRTYPE` et `WEEKDAY`.
- **Création de la variable `WEEKDAY`** : Le jour de la semaine est extrait à partir de la date de l’incident.
- **Création de la variable `DISTRICT`** : Les coordonnées géographiques sont regroupées en quatre secteurs afin de représenter des zones de collision dans Seattle.
- **Encodage des catégories** : Les variables catégorielles sont transformées en valeurs numériques afin d’être utilisées par les algorithmes de classification.

### 5. Analyse exploratoire des données
- **Analyse des distributions** : Les collisions sont comparées selon la météo, l’état de la route, le jour de la semaine, le type d’adresse et le secteur géographique.
- **Matrice de corrélation** : Une matrice de corrélation est utilisée pour observer les relations entre les variables numériques.
- **Tableaux croisés** : Des tableaux de contingence permettent de comparer la gravité des accidents selon les différentes conditions observées.
- **Constats principaux** : Dans l’échantillon analysé, le volume de collisions est particulièrement élevé le vendredi, sur chaussée sèche, par temps clair et dans les secteurs 1 et 2.

### 6. Visualisation géospatiale
- **Carte de chaleur** : Folium est utilisé pour générer une carte de chaleur des collisions à partir des coordonnées géographiques.
- **Regroupement des incidents** : Un cluster de marqueurs permet de visualiser la concentration des accidents dans la ville.
- **Objectif analytique** : Cette étape facilite l’identification visuelle des zones où les collisions sont les plus fréquentes.

### 7. Modélisation prédictive
- **Préparation des jeux d’entraînement et de test** : Les données sont divisées en un jeu d’entraînement et un jeu de test, avec une proportion de 80 % pour l’entraînement et 20 % pour le test.
- **Standardisation** : Les variables explicatives sont standardisées afin d’améliorer la stabilité de certains algorithmes.
- **Algorithmes comparés** : Quatre approches de classification sont évaluées : K-Nearest Neighbors, Decision Tree, Support Vector Machine et Logistic Regression.
- **Mesures d’évaluation** : Les modèles sont comparés à l’aide du Jaccard score, du F1-score et du Log Loss lorsque applicable.

## Résultats
Les résultats obtenus sur le jeu de test sont les suivants :

| Algorithme | Jaccard | F1-score | Log Loss |
|---|---:|---:|---:|
| KNN | 0.549240 | 0.539595 | N/A |
| Decision Tree | 0.596014 | 0.473522 | N/A |
| SVM | 0.599992 | 0.450109 | N/A |
| Logistic Regression | 0.599660 | 0.455767 | 0.666213 |

La régression logistique est retenue comme modèle de référence, notamment parce qu’elle fournit une estimation probabiliste et permet l’utilisation du Log Loss. Toutefois, les résultats doivent être interprétés avec prudence : la matrice de confusion montre que le modèle prédit correctement une grande partie des collisions avec dommages matériels, mais qu’il détecte beaucoup moins bien les collisions avec blessés.

Cette limite est importante dans un contexte de sécurité routière, car la classe des accidents avec blessés représente un enjeu plus critique. Pour une utilisation opérationnelle, il serait pertinent d’améliorer le traitement du déséquilibre entre classes, d’ajouter de nouvelles variables explicatives et de tester des stratégies de rééchantillonnage ou de pondération des classes.

## Conclusion
Ce projet démontre une démarche complète de science des données appliquée à un problème de sécurité routière. Il couvre la compréhension du problème, la préparation des données, l’analyse exploratoire, la visualisation géospatiale, la modélisation supervisée et l’évaluation des résultats.

L’analyse permet de mettre en évidence certains contextes associés à un plus grand volume de collisions dans l’échantillon étudié, notamment certains jours de la semaine, certaines conditions de route et certains secteurs géographiques. Les modèles testés fournissent une première base prédictive, mais les performances obtenues montrent aussi les limites d’un modèle construit à partir d’un nombre restreint de variables.

Dans une perspective professionnelle, ce projet constitue une base solide pour illustrer un flux de travail de science des données : formulation d’une question métier, préparation des données, création de variables, visualisation, comparaison de modèles et interprétation critique des résultats.

## Technologies utilisées
- **Python** : Langage principal utilisé pour l’analyse et la modélisation.
- **Pandas et NumPy** : Manipulation, nettoyage et transformation des données.
- **Matplotlib et Seaborn** : Visualisation statistique et exploration des données.
- **Folium** : Visualisation géographique et carte de chaleur des collisions.
- **Scikit-learn** : Préparation des données, séparation entraînement/test, standardisation, classification et évaluation des modèles.
- **Jupyter Notebook** : Environnement de travail utilisé pour documenter l’ensemble de l’analyse.

## Structure du dépôt
- **Predicting-vehicle-collisions-Seattle.ipynb** : Notebook principal contenant l’analyse complète, les visualisations et les modèles de classification.
- **README.md** : Présentation structurée du projet, des objectifs, de la méthodologie, des résultats et des limites.
- **data/** : Dossier optionnel pouvant contenir le jeu de données si celui-ci est conservé localement dans le dépôt.
- **images/** : Dossier optionnel pour ajouter des captures de graphiques, de cartes ou de matrices de confusion dans la documentation GitHub.

## Pistes d’amélioration
- **Rééquilibrage des classes** : Tester des méthodes comme `class_weight`, le sous-échantillonnage ou le suréchantillonnage afin d’améliorer la détection des accidents avec blessés.
- **Variables supplémentaires** : Ajouter des informations temporelles, de circulation, de luminosité ou d’environnement routier pour enrichir le modèle.
- **Optimisation des hyperparamètres** : Utiliser une recherche par grille ou une validation croisée pour comparer plus rigoureusement les modèles.
- **Évaluation plus ciblée** : Mettre davantage l’accent sur le rappel de la classe `Injury`, car cette classe est plus sensible pour la prévention routière.
- **Modernisation du code** : Remplacer `jaccard_similarity_score`, déprécié dans les versions récentes de Scikit-learn, par `jaccard_score`.


Ce dépôt présente une étude de cas concrète en science des données, centrée sur la prédiction de la gravité des collisions routières et l’interprétation des résultats dans un contexte de décision publique.
