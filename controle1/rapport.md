

# 📄 **COMPTE RENDU — Dataset Kaggle : *European Soccer Database***

## **1. Introduction**

Le dataset *European Soccer Database* disponible sur Kaggle est une ressource riche et complète regroupant plusieurs saisons de football professionnel dans de nombreux championnats européens.
Il constitue une base solide pour des projets de Data Science portant sur la performance sportive, l’analyse tactique, les prévisions de résultats ou l’évaluation de la valeur des joueurs.

Le dataset inclut notamment :

* des résultats de matchs,
* des caractéristiques d’équipes,
* des attributs de joueurs,
* des données d’événements de match,
* des cotes bookmakers,
* ainsi que des métadonnées sur les stades, ligues, joueurs et pays.

Grâce à cette granularité, il permet de construire aussi bien des modèles descriptifs qu’inférentiels ou prédictifs.

---

## **2. Source du Dataset**

* **Nom Kaggle :** *European Soccer Database*
* **URL :** [Kaggle (référence standard en Open Data sportif)](https://www.kaggle.com/datasets/hugomathien/soccer?utm_source=chatgpt.com)
* **Format principal :** SQLite (.db) ou CSV selon extraction
* **Volume total :** Plusieurs centaines de milliers d’enregistrements cumulés
* **Période couverte :** Environ 2008–2016 (selon version)

### **Fichiers fournis :**

| Fichier             | Description                                                                  |
| ------------------- | ---------------------------------------------------------------------------- |
| `Match`             | Tous les matchs avec scores, statistiques, dates, arbitres, cotes bookmakers |
| `Team`              | Informations sur les équipes                                                 |
| `Team_Attributes`   | Attributs (style de jeu, pressing, défense, agressivité)                     |
| `Player`            | Informations personnelles joueurs (nom, nationalité, date naissance)         |
| `Player_Attributes` | Attributs FIFA-like par joueur (attaque, défense, vitesse, passe, etc.)      |
| `League`            | Nom des ligues                                                               |
| `Country`           | Pays correspondants                                                          |
| `sqlite_sequence`   | Métadonnées                                                                  |

---

## **3. Structure des données**

### **3.1. Table principale : `Match`**

La table *Match* contient l’essentiel des informations nécessaires à l’analyse sportive.
Variables clés :

* `date` : Date du match
* `home_team_api_id` / `away_team_api_id` : Identifiants internes des équipes
* `home_team_goal` / `away_team_goal` : Score final
* `goal`, `shoton`, `shotoff` (parfois en JSON) : événements
* `B365H`, `B365D`, `B365A` : cotes bookmakers Bet365
* `possession`, `corners`, `fouls`, etc. selon disponibilité

**Taille approximative :** ~25 000 matchs.

---

### **3.2. Tables secondaires (Teams, Players, Attributes)**

#### **Team & Team_Attributes**

* Informations club : nom, pays, ID
* Style de jeu :

  * Buildup play speed
  * Chance creation passing
  * Defence pressure
  * Team Aggression, Width, etc.

Ces variables forment un socle idéal pour un modèle explicatif (SHAP) ou pour enrichir un modèle prédictif.

#### **Player & Player_Attributes**

Chaque joueur possède des attributs mesurés régulièrement, analogues à ceux du jeu FIFA :

* `overall_rating`, `finishing`, `ball_control`,
* `strength`, `stamina`,
* `marking`, `interceptions`,
* `acceleration`, `sprint_speed`,
* `vision`, `passing`, etc.

Les attributs peuvent ensuite être **agrégés par équipe** (moyenne, médiane, percentiles) pour modéliser la force de chaque club.

---

## **4. Qualité et Propreté des Données**

### **4.1. Points positifs**

* Richesse exceptionnelle (matchs + équipes + joueurs + attributs).
* Période temporelle suffisante pour analyses temporelles.
* Grande diversité géographique (plusieurs ligues européennes).
* Présence de cotes bookmakers → variables prédictives clés.

### **4.2. Limites à considérer**

* Certaines colonnes sont au format JSON ou texte imbriqué → parsing nécessaire.
* Missing values fréquentes dans `Player_Attributes` pour joueurs peu connus.
* Variabilité des ligues (certaines saisons incomplètes).
* Cotes bookmakers parfois absentes ou médiocres selon match.

---

## **5. Potentiel Analytique**

Ce dataset permet d’aborder **plusieurs niveaux d’analyse** :

### **5.1. Analyse descriptive (EDA)**

* Taux de victoire domicile vs extérieur
* Nombre moyen de buts par ligue
* Corrélation entre attributs joueurs et performance équipe
* Impact des cotes bookmakers sur la probabilité de victoire

### **5.2. Feature Engineering**

Il est possible d’extraire des variables très pertinentes :

* Rolling averages (forme) sur 3/5/10 derniers matchs
* Head-to-head historique entre deux équipes
* Elo rating par saison
* Moyenne des attributs joueurs par équipe
* Différence de force home vs away
* Probabilités implicites des odds bookmakers
* Variables tactiques (team attributes)

### **5.3. Modélisation prédictive**

Tâches possibles :

1. **Prédiction du résultat d’un match (Home/Draw/Away)**
   — Classification multi-classes, métriques : Accuracy, F1-macro.

2. **Prédiction du nombre de buts**
   — Régression (RMSE, MAE).

3. **Modèle d’aide à la décision pour staff**
   — SHAP value pour comprendre ce qui influence la victoire.

4. **Modèle de scouting / performance joueur**
   — Évaluer la progression d’un joueur via ses attributs.

---

## **6. Problèmes potentiels et solutions**

| Problème                                                   | Solution                                     |
| ---------------------------------------------------------- | -------------------------------------------- |
| Formats JSON dans `Match`                                  | Parsing Python avec `json.loads`             |
| Plusieurs attributs par joueur (multi-encodage par saison) | Feature engineering temporel + interpolation |
| Données manquantes sur odds                                | Imputation ou filtrage                       |
| Table large et multi-source                                | Joins SQL ou pandas merge propre             |
| Fuite d’information temporelle                             | Split chronologique strict                   |

---

## **7. Conclusion générale**

Le dataset Kaggle *European Soccer Database* est l’un des ensembles de données les plus riches pour la Data Science sportive.
Il fournit :

* une profondeur temporelle,
* une combinaison unique de niveaux d’analyse (match/équipe/joueur),
* des variables quantitatives et qualitatives variées,
* des possibilités de feature engineering avancé,
* et un potentiel élevé pour les modèles prédictifs.

Il constitue un excellent support pour :

* les projets académiques,
* les études d’analyse de performance,
* les modèles de prédiction,
* la recherche en intelligence sportive.



