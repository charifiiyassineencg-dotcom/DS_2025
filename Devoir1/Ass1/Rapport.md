# 📚 Analyse Complète du Dataset "Student Performance"

**Source :** UCI Machine Learning Repository  
**URL :** https://archive.ics.uci.edu/dataset/320/student+performance  
**Créateur :** Paulo Cortez (Université de Minho, Portugal)  
**Date de publication :** 26 novembre 2014  
**DOI :** 10.24432/C5TG7T  
**Licence :** Creative Commons Attribution 4.0 International (CC BY 4.0)

---

## 📋 Table des Matières

1. [Vue d'Ensemble](#vue-densemble)
2. [Structure du Dataset](#structure-du-dataset)
3. [Description des 33 Variables](#description-des-33-variables)
4. [Cas d'Usage et Applications](#cas-dusage-et-applications)
5. [Analyses Possibles](#analyses-possibles)
6. [Insights et Hypothèses](#insights-et-hypothèses)
7. [Guide Pratique d'Utilisation](#guide-pratique-dutilisation)
8. [Analyses Exploratoires](#analyses-exploratoires)
9. [Contexte Académique](#contexte-académique)
10. [Considérations Techniques](#considérations-techniques)

---

## 📋 Vue d'Ensemble

### Description Générale

Ce dataset porte sur la **performance des étudiants dans l'enseignement secondaire** (lycée) de deux écoles portugaises. Il vise à prédire les résultats académiques des élèves en fonction de facteurs démographiques, sociaux et scolaires.

### Caractéristiques Clés

| Caractéristique | Valeur |
|-----------------|--------|
| **Type de dataset** | Multivarié |
| **Domaine** | Sciences Sociales |
| **Tâches associées** | Classification, Régression |
| **Type de features** | Entier, Catégoriel, Binaire |
| **Nombre d'instances** | 649 étudiants |
| **Nombre de features** | 33 attributs (30 features + 3 notes) |
| **Valeurs manquantes** | Non (0%) |
| **Taille du fichier** | 20 KB (student.zip) |

### Origine des Données

Les données ont été collectées via :
- **Rapports scolaires officiels** (bulletins de notes)
- **Questionnaires** remplis par les étudiants

Les écoles concernées :
- **GP** - Gabriel Pereira
- **MS** - Mousinho da Silveira

---

## 📊 Structure du Dataset

### Deux Datasets Distincts

Le dataset est divisé en **deux fichiers séparés** :

1. **student-mat.csv** - Performance en **Mathématiques**
2. **student-por.csv** - Performance en **Portugais** (langue)

Chaque fichier contient les mêmes 33 attributs mais pour des matières différentes.

### Variables Cibles (Target)

Les variables à prédire sont les **notes finales** :

- **G1** - Note de la 1ère période (0-20)
- **G2** - Note de la 2ème période (0-20)
- **G3** - Note finale de l'année (0-20) ⭐ **CIBLE PRINCIPALE**

**⚠️ Note importante :** G3 a une forte corrélation avec G1 et G2 (puisque G3 est la note finale incluant les périodes précédentes). Il est plus difficile mais beaucoup plus utile de prédire G3 sans utiliser G1 et G2.

---

## 🔍 Description des 33 Variables

### 1️⃣ Informations Scolaires (2 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **school** | Catégoriel | École de l'étudiant | 'GP' (Gabriel Pereira) ou 'MS' (Mousinho da Silveira) |
| **reason** | Catégoriel | Raison du choix de l'école | 'home' (proximité), 'reputation', 'course' (programme), 'other' |

### 2️⃣ Informations Démographiques (4 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **sex** | Binaire | Sexe de l'étudiant | 'F' (féminin) ou 'M' (masculin) |
| **age** | Numérique | Âge de l'étudiant | 15 à 22 ans |
| **address** | Binaire | Type d'adresse | 'U' (urbain) ou 'R' (rural) |
| **famsize** | Binaire | Taille de la famille | 'LE3' (≤3 personnes) ou 'GT3' (>3 personnes) |

### 3️⃣ Contexte Familial (6 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **Pstatus** | Binaire | Statut de cohabitation des parents | 'T' (ensemble) ou 'A' (séparés) |
| **Medu** | Numérique | Éducation de la mère | 0 (aucune) à 4 (supérieure) |
| **Fedu** | Numérique | Éducation du père | 0 (aucune) à 4 (supérieure) |
| **Mjob** | Catégoriel | Emploi de la mère | 'teacher', 'health', 'services', 'at_home', 'other' |
| **Fjob** | Catégoriel | Emploi du père | 'teacher', 'health', 'services', 'at_home', 'other' |
| **guardian** | Catégoriel | Tuteur de l'étudiant | 'mother', 'father', 'other' |

**Échelle d'éducation parentale :**
- 0 = Aucune éducation
- 1 = Éducation primaire (4th grade)
- 2 = 5ème à 9ème année
- 3 = Éducation secondaire
- 4 = Éducation supérieure (université)

### 4️⃣ Temps et Déplacements (2 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **traveltime** | Numérique | Temps de trajet domicile-école | 1 (<15 min), 2 (15-30 min), 3 (30-60 min), 4 (>60 min) |
| **studytime** | Numérique | Temps d'étude hebdomadaire | 1 (<2h), 2 (2-5h), 3 (5-10h), 4 (>10h) |

### 5️⃣ Support Éducatif (4 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **failures** | Numérique | Nombre d'échecs scolaires passés | 0, 1, 2, 3, ou 4 (≥3 échecs) |
| **schoolsup** | Binaire | Support éducatif supplémentaire de l'école | 'yes' ou 'no' |
| **famsup** | Binaire | Support éducatif familial | 'yes' ou 'no' |
| **paid** | Binaire | Cours payants supplémentaires (maths/portugais) | 'yes' ou 'no' |

### 6️⃣ Activités et Ambitions (3 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **activities** | Binaire | Activités extra-scolaires | 'yes' ou 'no' |
| **nursery** | Binaire | A fréquenté une crèche/maternelle | 'yes' ou 'no' |
| **higher** | Binaire | Veut poursuivre des études supérieures | 'yes' ou 'no' |

### 7️⃣ Facteurs Technologiques et Sociaux (2 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **internet** | Binaire | Accès internet à domicile | 'yes' ou 'no' |
| **romantic** | Binaire | En couple (relation romantique) | 'yes' ou 'no' |

### 8️⃣ Bien-être et Relations (4 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **famrel** | Numérique | Qualité des relations familiales | 1 (très mauvaise) à 5 (excellente) |
| **freetime** | Numérique | Temps libre après l'école | 1 (très faible) à 5 (très élevé) |
| **goout** | Numérique | Fréquence des sorties avec amis | 1 (très faible) à 5 (très élevée) |
| **health** | Numérique | État de santé actuel | 1 (très mauvais) à 5 (très bon) |

### 9️⃣ Comportements à Risque (2 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **Dalc** | Numérique | Consommation d'alcool en semaine | 1 (très faible) à 5 (très élevée) |
| **Walc** | Numérique | Consommation d'alcool le week-end | 1 (très faible) à 5 (très élevée) |

### 🔟 Assiduité (1 variable)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **absences** | Numérique | Nombre d'absences scolaires | 0 à 93 |

### 1️⃣1️⃣ Variables Cibles - Notes (3 variables)

| Variable | Type | Description | Valeurs possibles |
|----------|------|-------------|-------------------|
| **G1** | Numérique | Note de la 1ère période | 0 à 20 |
| **G2** | Numérique | Note de la 2ème période | 0 à 20 |
| **G3** | Numérique | **NOTE FINALE** (cible principale) | 0 à 20 |

---

## 🎯 Cas d'Usage et Applications

### Tâches de Machine Learning

#### 1. Régression
**Objectif :** Prédire la note finale (G3) sur une échelle continue (0-20)

**Applications :**
- Prédiction précise des résultats finaux
- Identification des étudiants nécessitant un soutien
- Estimation de l'impact de chaque facteur sur la performance

**Métriques d'évaluation :**
- RMSE (Root Mean Squared Error)
- MAE (Mean Absolute Error)
- R² Score

#### 2. Classification Binaire
**Objectif :** Prédire si un étudiant va réussir (G3 ≥ 10) ou échouer (G3 < 10)

**Applications :**
- Système d'alerte précoce pour échecs scolaires
- Allocation des ressources de soutien
- Intervention ciblée

**Métriques d'évaluation :**
- Accuracy
- Precision/Recall
- F1-Score
- ROC-AUC

#### 3. Classification Multi-classes
**Objectif :** Classer les étudiants en 5 niveaux de performance

**Niveaux suggérés :**
- Échec : 0-9
- Passable : 10-11
- Moyen : 12-13
- Bien : 14-16
- Excellent : 17-20

**Applications :**
- Segmentation des étudiants
- Programmes d'enseignement différencié
- Évaluation comparative

**Métriques d'évaluation :**
- Accuracy
- Confusion Matrix
- Classification Report

---

## 📈 Analyses Possibles

### Analyses Descriptives

1. **Distribution des notes**
   - Histogrammes G1, G2, G3
   - Statistiques (moyenne, médiane, écart-type)
   - Identification de patterns (bimodalité, asymétrie)

2. **Analyse démographique**
   - Performance par sexe
   - Impact de l'âge
   - Différences urbain vs rural

3. **Facteurs socio-économiques**
   - Corrélation éducation parentale / performance
   - Impact du type d'emploi parental
   - Effet du statut familial (parents ensemble/séparés)

4. **Comportements et habitudes**
   - Temps d'étude vs résultats
   - Impact des activités extra-scolaires
   - Effet de la consommation d'alcool
   - Relations amoureuses et performance

### Analyses Prédictives

1. **Feature Importance**
   - Quels facteurs prédisent le mieux G3 ?
   - Poids relatifs des variables
   - Interactions entre features

2. **Modèles de Machine Learning**
   - Régression linéaire / Ridge / Lasso
   - Decision Trees / Random Forest
   - Gradient Boosting (XGBoost, LightGBM)
   - Support Vector Machines
   - Neural Networks

3. **Scénarios de prédiction**
   - **Scénario 1 :** Prédire G3 avec G1 et G2 (facile, corrélation forte)
   - **Scénario 2 :** Prédire G3 sans G1 et G2 (difficile, plus utile pratiquement)
   - **Scénario 3 :** Prédire G1 en début d'année (intervention précoce)

### Analyses Causales

1. **Impact des interventions**
   - Effet du support scolaire (schoolsup)
   - Impact des cours payants (paid)
   - Rôle du soutien familial (famsup)

2. **Facteurs modifiables vs non-modifiables**
   - Non-modifiables : sexe, âge, éducation parentale
   - Modifiables : temps d'étude, absences, consommation alcool
   - Recommandations actionnables pour améliorer performance

---

## 💡 Insights et Hypothèses

### Hypothèses à Tester

1. **Éducation parentale**
   - **H1 :** Plus l'éducation des parents est élevée, meilleure est la performance de l'étudiant
   - **Justification :** Capital culturel, valorisation de l'éducation

2. **Temps d'étude**
   - **H2 :** Plus de temps d'étude = meilleures notes (relation non-linéaire possible)
   - **Nuance :** Au-delà d'un seuil, rendements décroissants

3. **Échecs passés**
   - **H3 :** Les échecs passés sont le meilleur prédicteur d'échecs futurs
   - **Implication :** Besoin d'interventions intensives pour ces étudiants

4. **Consommation d'alcool**
   - **H4 :** Corrélation négative entre consommation alcool et performance
   - **Distinction :** Alcool en semaine (Dalc) plus impactant que week-end (Walc)

5. **Relations amoureuses**
   - **H5 :** Être en couple peut affecter négativement la performance (distraction)
   - **Alternative :** Peut avoir effet positif (bien-être émotionnel)

6. **Accès internet**
   - **H6 :** Accès internet = meilleures notes (ressources éducatives)
   - **Nuance :** Peut aussi être source de distraction

7. **Support scolaire**
   - **H7 :** Paradoxe possible - étudiants avec support scolaire ont notes plus basses
   - **Explication :** Support donné à ceux en difficulté (biais de sélection)

### Patterns Attendus

1. **Effet cumulatif**
   - Les difficultés s'accumulent : échecs → baisse motivation → plus d'absences → plus d'échecs

2. **Facteurs de résilience**
   - Famille supportive + ambitions supérieures = facteur protecteur contre échecs

3. **Multiplicateurs**
   - Éducation parentale élevée + accès internet + support familial = effets synergiques

---

## 🛠️ Guide Pratique d'Utilisation

### Installation et Chargement

#### Méthode 1 : Via ucimlrepo (Recommandé)

```python
# Installation
pip install ucimlrepo

# Chargement
from ucimlrepo import fetch_ucirepo

# Récupération du dataset
student_performance = fetch_ucirepo(id=320)

# Accès aux données
X = student_performance.data.features  # Features
y = student_performance.data.targets   # Target (G3)

# Métadonnées
print(student_performance.metadata)

# Information sur les variables
print(student_performance.variables)
```

#### Méthode 2 : Téléchargement Manuel

```python
import pandas as pd
import zipfile
import requests

# Télécharger le fichier
url = "https://archive.ics.uci.edu/static/public/320/student+performance.zip"
response = requests.get(url)

# Sauvegarder et décompresser
with open("student_performance.zip", "wb") as f:
    f.write(response.content)

with zipfile.ZipFile("student_performance.zip", "r") as zip_ref:
    zip_ref.extractall("student_data")

# Charger les datasets
df_mat = pd.read_csv("student_data/student-mat.csv", sep=";")
df_por = pd.read_csv("student_data/student-por.csv", sep=";")

print(f"Mathématiques : {df_mat.shape}")
print(f"Portugais : {df_por.shape}")
```

### Préparation des Données

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder, StandardScaler
from sklearn.model_selection import train_test_split

# Charger les données (exemple avec Maths)
df = pd.read_csv("student-mat.csv", sep=";")

# 1. Encoder les variables catégorielles
label_encoders = {}
categorical_columns = ['school', 'sex', 'address', 'famsize', 'Pstatus', 
                       'Mjob', 'Fjob', 'reason', 'guardian', 
                       'schoolsup', 'famsup', 'paid', 'activities', 
                       'nursery', 'higher', 'internet', 'romantic']

for col in categorical_columns:
    le = LabelEncoder()
    df[col] = le.fit_transform(df[col])
    label_encoders[col] = le

# 2. Séparer features et target
# Scénario 1 : Avec G1 et G2
X_with_grades = df.drop(['G3'], axis=1)
y = df['G3']

# Scénario 2 : Sans G1 et G2 (plus réaliste)
X_without_grades = df.drop(['G1', 'G2', 'G3'], axis=1)

# 3. Split train/test
X_train, X_test, y_train, y_test = train_test_split(
    X_without_grades, y, test_size=0.2, random_state=42
)

# 4. Normalisation (optionnel mais recommandé)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

print(f"Train set : {X_train.shape}")
print(f"Test set : {X_test.shape}")
```

### Exemple de Modélisation Complète

```python
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_error
import matplotlib.pyplot as plt

# 1. Plusieurs modèles à comparer
models = {
    'Ridge Regression': Ridge(alpha=1.0),
    'Random Forest': RandomForestRegressor(n_estimators=100, random_state=42),
    'Gradient Boosting': GradientBoostingRegressor(n_estimators=100, random_state=42)
}

# 2. Entraînement et évaluation
results = {}

for name, model in models.items():
    # Entraînement
    model.fit(X_train_scaled, y_train)
    
    # Prédictions
    y_pred_train = model.predict(X_train_scaled)
    y_pred_test = model.predict(X_test_scaled)
    
    # Métriques
    results[name] = {
        'train_r2': r2_score(y_train, y_pred_train),
        'test_r2': r2_score(y_test, y_pred_test),
        'test_rmse': np.sqrt(mean_squared_error(y_test, y_pred_test)),
        'test_mae': mean_absolute_error(y_test, y_pred_test)
    }
    
    print(f"\n{name}:")
    print(f"  Train R²: {results[name]['train_r2']:.4f}")
    print(f"  Test R²: {results[name]['test_r2']:.4f}")
    print(f"  Test RMSE: {results[name]['test_rmse']:.4f}")
    print(f"  Test MAE: {results[name]['test_mae']:.4f}")

# 3. Feature Importance (Random Forest)
rf_model = models['Random Forest']
feature_importance = pd.DataFrame({
    'feature': X_train.columns,
    'importance': rf_model.feature_importances_
}).sort_values('importance', ascending=False)

print("\nTop 10 Features les plus importantes:")
print(feature_importance.head(10))

# 4. Visualisation
plt.figure(figsize=(12, 6))
plt.barh(feature_importance.head(10)['feature'], 
         feature_importance.head(10)['importance'])
plt.xlabel('Importance')
plt.title('Top 10 Features - Random Forest')
plt.tight_layout()
plt.savefig('feature_importance.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## 📊 Analyses Exploratoires

### 1. Analyse Univariée

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Distribution des notes finales
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

axes[0].hist(df['G1'], bins=20, color='skyblue', edgecolor='black', alpha=0.7)
axes[0].set_title('Distribution G1 (1ère période)', fontweight='bold')
axes[0].set_xlabel('Note')
axes[0].set_ylabel('Fréquence')

axes[1].hist(df['G2'], bins=20, color='lightgreen', edgecolor='black', alpha=0.7)
axes[1].set_title('Distribution G2 (2ème période)', fontweight='bold')
axes[1].set_xlabel('Note')

axes[2].hist(df['G3'], bins=20, color='salmon', edgecolor='black', alpha=0.7)
axes[2].set_title('Distribution G3 (Finale)', fontweight='bold')
axes[2].set_xlabel('Note')

plt.tight_layout()
plt.savefig('distribution_notes.png', dpi=300, bbox_inches='tight')
plt.show()

# Statistiques descriptives
print("\n=== STATISTIQUES DESCRIPTIVES DES NOTES ===")
print(df[['G1', 'G2', 'G3']].describe())

# Taux de réussite (G3 >= 10)
success_rate = (df['G3'] >= 10).mean() * 100
print(f"\nTaux de réussite global: {success_rate:.2f}%")
```

### 2. Analyse Bivariée

```python
# Corrélations entre variables numériques
numerical_cols = df.select_dtypes(include=[np.number]).columns
correlation_matrix = df[numerical_cols].corr()

plt.figure(figsize=(16, 14))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', center=0, 
            fmt='.2f', square=True, linewidths=0.5)
plt.title('Matrice de Corrélation - Toutes Variables Numériques', 
          fontsize=16, fontweight='bold', pad=20)
plt.tight_layout()
plt.savefig('correlation_matrix.png', dpi=300, bbox_inches='tight')
plt.show()

# Focus sur G3
g3_correlations = correlation_matrix['G3'].sort_values(ascending=False)
print("\n=== CORRÉLATIONS AVEC G3 (Note Finale) ===")
print(g3_correlations)

# Visualisation des top corrélations
plt.figure(figsize=(10, 8))
g3_correlations[1:11].plot(kind='barh', color='steelblue')
plt.title('Top 10 Corrélations avec G3', fontweight='bold', fontsize=14)
plt.xlabel('Coefficient de Corrélation')
plt.axvline(x=0, color='red', linestyle='--', alpha=0.5)
plt.tight_layout()
plt.savefig('top_correlations_g3.png', dpi=300, bbox_inches='tight')
plt.show()
```

### 3. Analyses par Groupes

```python
# Performance par sexe
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# Sexe
df.boxplot(column='G3', by='sex', ax=axes[0, 0])
axes[0, 0].set_title('Performance (G3) par Sexe')
axes[0, 0].set_xlabel('Sexe (F=Female, M=Male)')
axes[0, 0].set_ylabel('Note Finale (G3)')
plt.sca(axes[0, 0])
plt.xticks([1, 2], ['Female', 'Male'])

# Éducation de la mère
df.boxplot(column='G3', by='Medu', ax=axes[0, 1])
axes[0, 1].set_title('Performance selon Éducation Mère')
axes[0, 1].set_xlabel('Niveau Éducation Mère (0-4)')
axes[0, 1].set_ylabel('Note Finale (G3)')

# Temps d'étude
df.boxplot(column='G3', by='studytime', ax=axes[1, 0])
axes[1, 0].set_title('Performance selon Temps d\'Étude')
axes[1, 0].set_xlabel('Temps Étude (1:<2h, 2:2-5h, 3:5-10h, 4:>10h)')
axes[1, 0].set_ylabel('Note Finale (G3)')

# Échecs passés
df.boxplot(column='G3', by='failures', ax=axes[1, 1])
axes[1, 1].set_title('Performance selon Échecs Passés')
axes[1, 1].set_xlabel('Nombre d\'Échecs Passés')
axes[1, 1].set_ylabel('Note Finale (G3)')

plt.suptitle('')
plt.tight_layout()
plt.savefig('performance_by_groups.png', dpi=300, bbox_inches='tight')
plt.show()
```

### 4. Analyse Multivariée

```python
# Scatter plot matrix pour variables clés
key_vars = ['G3', 'studytime', 'failures', 'absences', 'Medu', 'Fedu', 'age']
sns.pairplot(df[key_vars], diag_kind='kde', plot_kws={'alpha': 0.6})
plt.suptitle('Relations entre Variables Clés', y=1.02, fontsize=16, fontweight='bold')
plt.savefig('pairplot_key_vars.png', dpi=300, bbox_inches='tight')
plt.show()

# Impact combiné de plusieurs facteurs
fig, axes = plt.subplots(2, 2, figsize=(15, 12))

# Éducation parents vs G3
axes[0, 0].scatter(df['Medu'], df['G3'], alpha=0.5, s=50, c='blue', label='Mère', edgecolors='black', linewidth=0.5)
axes[0, 0].scatter(df['Fedu'], df['G3'], alpha=0.5, s=50, c='red', label='Père', edgecolors='black', linewidth=0.5)
axes[0, 0].set_xlabel('Niveau Éducation Parentale (0-4)', fontweight='bold')
axes[0, 0].set_ylabel('Note Finale (G3)', fontweight='bold')
axes[0, 0].set_title('Éducation Parentale vs Performance', fontweight='bold', fontsize=12)
axes[0, 0].legend()
axes[0, 0].grid(alpha=0.3)

# Échecs passés vs G3
axes[0, 1].scatter(df['failures'], df['G3'], alpha=0.5, s=50, color='green', edgecolors='black', linewidth=0.5)
axes[0, 1].set_xlabel('Nombre d\'Échecs Passés', fontweight='bold')
axes[0, 1].set_ylabel('Note Finale (G3)', fontweight='bold')
axes[0, 1].set_title('Échecs Passés vs Performance', fontweight='bold', fontsize=12)
axes[0, 1].grid(alpha=0.3)

# Absences vs G3
axes[1, 0].scatter(df['absences'], df['G3'], alpha=0.5, s=50, color='orange', edgecolors='black', linewidth=0.5)
axes[1, 0].set_xlabel('Nombre d\'Absences', fontweight='bold')
axes[1, 0].set_ylabel('Note Finale (G3)', fontweight='bold')
axes[1
[student-performance-analysis.md](https://github.com/user-attachments/files/23503178/student-performance-analysis.md)
