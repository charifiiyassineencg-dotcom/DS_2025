# -*- coding: utf-8 -*-
"""
Travail Pratique : Classification de la Qualité des Vins
Algorithmes des k plus proches voisins (k-NN)
"""

# Importation des bibliothèques
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
from sklearn.preprocessing import StandardScaler
import warnings
warnings.filterwarnings('ignore')

# Configuration de l'affichage
pd.set_option('display.max_columns', None)
plt.style.use('seaborn-v0_8')

print("=" * 60)
print("CLASSIFICATION DE LA QUALITÉ DES VINS - k-NN")
print("=" * 60)

# =============================================================================
# 1. CHARGEMENT ET EXPLORATION DES DONNÉES
# =============================================================================

print("\n" + "=" * 50)
print("1. CHARGEMENT ET EXPLORATION DES DONNÉES")
print("=" * 50)

# Chargement du dataset
link = "http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv"
df = pd.read_csv(link, header="infer", delimiter=";")

print("\n✅ Dataset chargé avec succès")
print(f"Dimensions : {df.shape[0]} échantillons, {df.shape[1]} caractéristiques")

print("\n============= Premiers échantillons =============")
print(df.head())

print("\n============= Informations du dataset =============")
df.info()

print("\n============= Qualités des vins (original) =============")
qualities_count = df['quality'].value_counts().sort_index()
print(qualities_count)

# Visualisation de la distribution originale
plt.figure(figsize=(10, 6))
qualities_count.plot(kind='bar', color='skyblue')
plt.title('Distribution des Qualités de Vins (Originale)')
plt.xlabel('Score de Qualité')
plt.ylabel('Nombre d\'échantillons')
plt.grid(axis='y', alpha=0.3)
plt.show()

# =============================================================================
# 2. PRÉPARATION DES DONNÉES - CLASSIFICATION BINAIRE
# =============================================================================

print("\n" + "=" * 50)
print("2. PRÉPARATION DES DONNÉES - CLASSIFICATION BINAIRE")
print("=" * 50)

# Conversion en classification binaire
Y_binary = [0 if val <= 5 else 1 for val in df['quality']]
df['quality_binary'] = Y_binary

print("\n============= Distribution des classes binaires =============")
binary_counts = pd.Series(Y_binary).value_counts()
binary_percentages = pd.Series(Y_binary).value_counts(normalize=True) * 100
print(binary_counts)
print(f"\nPourcentages : {binary_percentages[0]:.1f}% mauvaise qualité, {binary_percentages[1]:.1f}% bonne qualité")

# Visualisation de la distribution binaire
plt.figure(figsize=(8, 6))
colors = ['lightcoral', 'lightgreen']
binary_counts.plot(kind='bar', color=colors)
plt.title('Distribution des Vins par Classe de Qualité')
plt.xlabel('Classe de Qualité (0=Mauvaise, 1=Bonne)')
plt.ylabel('Nombre d\'échantillons')
plt.xticks(rotation=0)
for i, v in enumerate(binary_counts):
    plt.text(i, v + 50, f'{v}\n({binary_percentages[i]:.1f}%)', 
             ha='center', va='bottom', fontweight='bold')
plt.grid(axis='y', alpha=0.3)
plt.show()

# =============================================================================
# 3. ANALYSE STATISTIQUE ET VISUALISATION
# =============================================================================

print("\n" + "=" * 50)
print("3. ANALYSE STATISTIQUE ET VISUALISATION")
print("=" * 50)

# Préparation des variables
X = df.drop(["quality", "quality_binary"], axis=1)
Y = Y_binary

print("\n---------- Statistiques descriptives ----------")
print(X.describe())

# Boxplots
print("\n📊 Génération des boxplots...")
plt.figure(figsize=(12, 8))
sns.boxplot(data=X, orient="v", palette="Set2", width=0.7, notch=True)
plt.xticks(rotation=45)
plt.title('Distribution des Caractéristiques Physico-Chimiques des Vins')
plt.tight_layout()
plt.show()

# Matrice de corrélation
print("\n📈 Génération de la heatmap de corrélation...")
plt.figure(figsize=(12, 10))
correlation_matrix = X.corr()
mask = np.triu(np.ones_like(correlation_matrix, dtype=bool))
sns.heatmap(correlation_matrix, mask=mask, annot=True, cmap='coolwarm', 
            center=0, square=True, fmt='.2f', cbar_kws={"shrink": .8})
plt.title('Matrice de Corrélation des Caractéristiques des Vins')
plt.tight_layout()
plt.show()

# =============================================================================
# 4. DIVISION DES DONNÉES
# =============================================================================

print("\n" + "=" * 50)
print("4. DIVISION DES DONNÉES")
print("=" * 50)

# Division des données
X_train_val, X_test, Y_train_val, Y_test = train_test_split(
    X, Y, shuffle=True, test_size=1/3, random_state=42, stratify=Y
)

X_train, X_val, Y_train, Y_val = train_test_split(
    X_train_val, Y_train_val, shuffle=True, test_size=0.5, 
    random_state=42, stratify=Y_train_val
)

print(f"✅ Division des données terminée:")
print(f"   - Ensemble d'entraînement : {X_train.shape}")
print(f"   - Ensemble de validation : {X_val.shape}")
print(f"   - Ensemble de test : {X_test.shape}")

# Vérification des proportions
print("\n✅ Vérification des proportions:")
print(f"   - Proportion classe 1 - Train : {np.mean(Y_train):.3f}")
print(f"   - Proportion classe 1 - Validation : {np.mean(Y_val):.3f}")
print(f"   - Proportion classe 1 - Test : {np.mean(Y_test):.3f}")
print(f"   - Proportion classe 1 - Original : {np.mean(Y):.3f}")

# =============================================================================
# 5. IMPLÉMENTATION DU k-NN SANS NORMALISATION
# =============================================================================

print("\n" + "=" * 50)
print("5. k-NN SANS NORMALISATION")
print("=" * 50)

# Test initial avec k=3
print("\n---------- Test initial avec k=3 ----------")
knn_initial = KNeighborsClassifier(n_neighbors=3)
knn_initial.fit(X_train, Y_train)

Y_val_pred_initial = knn_initial.predict(X_val)
error_rate_initial = 1 - accuracy_score(Y_val, Y_val_pred_initial)
accuracy_initial = accuracy_score(Y_val, Y_val_pred_initial)

print(f"📊 Résultats avec k=3:")
print(f"   - Taux d'erreur validation : {error_rate_initial:.4f}")
print(f"   - Précision validation : {accuracy_initial:.4f}")

# Recherche du k optimal
print("\n---------- Recherche du k optimal ----------")
k_vector = np.arange(1, 41, 2)
print(f"🔍 Valeurs de k testées : {k_vector}")

error_train = np.empty(len(k_vector))
error_val = np.empty(len(k_vector))
accuracy_train = np.empty(len(k_vector))
accuracy_val = np.empty(len(k_vector))

print("\n🔄 Entraînement des modèles...")
for ind, k in enumerate(k_vector):
    clf = KNeighborsClassifier(n_neighbors=k)
    clf.fit(X_train, Y_train)
    
    # Prédictions sur l'ensemble d'entraînement
    Y_train_pred = clf.predict(X_train)
    accuracy_train[ind] = accuracy_score(Y_train, Y_train_pred)
    error_train[ind] = 1 - accuracy_train[ind]
    
    # Prédictions sur l'ensemble de validation
    Y_val_pred = clf.predict(X_val)
    accuracy_val[ind] = accuracy_score(Y_val, Y_val_pred)
    error_val[ind] = 1 - accuracy_val[ind]

# Recherche du k optimal
err_min = error_val.min()
ind_opt = error_val.argmin()
k_star = k_vector[ind_opt]

print(f"\n✅ k optimal trouvé :")
print(f"   - k* = {k_star}")
print(f"   - Erreur validation minimale : {err_min:.4f}")
print(f"   - Précision validation maximale : {accuracy_val[ind_opt]:.4f}")

# Visualisation des performances
plt.figure(figsize=(12, 8))

# Courbe des erreurs
plt.subplot(2, 1, 1)
plt.plot(k_vector, error_train, 'o-', linewidth=2, markersize=6, label='Erreur Train', color='blue')
plt.plot(k_vector, error_val, 's-', linewidth=2, markersize=6, label='Erreur Validation', color='red')
plt.axvline(x=k_star, color='green', linestyle='--', alpha=0.7, label=f'k optimal = {k_star}')
plt.xlabel('Valeur de k')
plt.ylabel('Taux d\'erreur')
plt.title('Évolution des Taux d\'Erreur en Fonction de k (Sans Normalisation)')
plt.legend()
plt.grid(True, alpha=0.3)

# Courbe des précisions
plt.subplot(2, 1, 2)
plt.plot(k_vector, accuracy_train, 'o-', linewidth=2, markersize=6, label='Précision Train', color='lightblue')
plt.plot(k_vector, accuracy_val, 's-', linewidth=2, markersize=6, label='Précision Validation', color='salmon')
plt.axvline(x=k_star, color='green', linestyle='--', alpha=0.7, label=f'k optimal = {k_star}')
plt.xlabel('Valeur de k')
plt.ylabel('Précision')
plt.title('Évolution de la Précision en Fonction de k (Sans Normalisation)')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Évaluation sur l'ensemble de test
final_model = KNeighborsClassifier(n_neighbors=k_star)
final_model.fit(X_train, Y_train)
Y_test_pred = final_model.predict(X_test)
test_error = 1 - accuracy_score(Y_test, Y_test_pred)
test_accuracy = accuracy_score(Y_test, Y_test_pred)

print(f"\n🎯 Performance finale sur le test set (k={k_star}):")
print(f"   - Taux d'erreur : {test_error:.4f}")
print(f"   - Précision : {test_accuracy:.4f}")

# =============================================================================
# 6. k-NN AVEC NORMALISATION
# =============================================================================

print("\n" + "=" * 50)
print("6. k-NN AVEC NORMALISATION")
print("=" * 50)

print("🔄 Normalisation des données...")
scaler = StandardScaler(with_mean=True, with_std=True)
scaler = scaler.fit(X_train)

X_train_norm = scaler.transform(X_train)
X_val_norm = scaler.transform(X_val)
X_test_norm = scaler.transform(X_test)

print("✅ Normalisation terminée")

# k-NN avec données normalisées
error_train_norm = np.empty(len(k_vector))
error_val_norm = np.empty(len(k_vector))
accuracy_train_norm = np.empty(len(k_vector))
accuracy_val_norm = np.empty(len(k_vector))

print("\n🔄 Entraînement des modèles avec données normalisées...")
for ind, k in enumerate(k_vector):
    clf_norm = KNeighborsClassifier(n_neighbors=k)
    clf_norm.fit(X_train_norm, Y_train)
    
    Y_train_pred_norm = clf_norm.predict(X_train_norm)
    accuracy_train_norm[ind] = accuracy_score(Y_train, Y_train_pred_norm)
    error_train_norm[ind] = 1 - accuracy_train_norm[ind]
    
    Y_val_pred_norm = clf_norm.predict(X_val_norm)
    accuracy_val_norm[ind] = accuracy_score(Y_val, Y_val_pred_norm)
    error_val_norm[ind] = 1 - accuracy_val_norm[ind]

# Recherche du k optimal avec normalisation
err_min_norm = error_val_norm.min()
ind_opt_norm = error_val_norm.argmin()
k_star_norm = k_vector[ind_opt_norm]

print(f"\n✅ k optimal avec normalisation :")
print(f"   - k* = {k_star_norm}")
print(f"   - Erreur validation minimale : {err_min_norm:.4f}")
print(f"   - Précision validation maximale : {accuracy_val_norm[ind_opt_norm]:.4f}")

# Évaluation sur l'ensemble de test avec normalisation
final_model_norm = KNeighborsClassifier(n_neighbors=k_star_norm)
final_model_norm.fit(X_train_norm, Y_train)
Y_test_pred_norm = final_model_norm.predict(X_test_norm)
test_error_norm = 1 - accuracy_score(Y_test, Y_test_pred_norm)
test_accuracy_norm = accuracy_score(Y_test, Y_test_pred_norm)

print(f"\n🎯 Performance finale avec normalisation (k={k_star_norm}):")
print(f"   - Taux d'erreur : {test_error_norm:.4f}")
print(f"   - Précision : {test_accuracy_norm:.4f}")

# =============================================================================
# 7. COMPARAISON AVEC/SANS NORMALISATION
# =============================================================================

print("\n" + "=" * 60)
print("7. COMPARAISON AVEC/SANS NORMALISATION")
print("=" * 60)

print("COMPARAISON DES RÉSULTATS:")
print("=" * 70)
print(f"{'Condition':<25} {'k optimal':<12} {'Erreur Test':<12} {'Précision Test':<15}")
print("=" * 70)
print(f"{'Sans normalisation':<25} {k_star:<12} {test_error:.4f} {'':<8} {test_accuracy:.4f}")
print(f"{'Avec normalisation':<25} {k_star_norm:<12} {test_error_norm:.4f} {'':<8} {test_accuracy_norm:.4f}")
print("=" * 70)

# Calcul de l'amélioration
improvement = ((test_error - test_error_norm) / test_error) * 100
print(f"\n📈 Amélioration du taux d'erreur : {improvement:.2f}%")

# Visualisation comparative
plt.figure(figsize=(15, 12))

# Comparaison des erreurs de validation
plt.subplot(2, 2, 1)
plt.plot(k_vector, error_val, 's-', linewidth=2, markersize=4, label='Sans normalisation', color='red')
plt.plot(k_vector, error_val_norm, 'o-', linewidth=2, markersize=4, label='Avec normalisation', color='green')
plt.axvline(x=k_star, color='red', linestyle='--', alpha=0.5, label=f'k* sans norm = {k_star}')
plt.axvline(x=k_star_norm, color='green', linestyle='--', alpha=0.5, label=f'k* avec norm = {k_star_norm}')
plt.xlabel('Valeur de k')
plt.ylabel('Taux d\'erreur (Validation)')
plt.title('Comparaison des Erreurs de Validation')
plt.legend()
plt.grid(True, alpha=0.3)

# Comparaison des précisions de validation
plt.subplot(2, 2, 2)
plt.plot(k_vector, accuracy_val, 's-', linewidth=2, markersize=4, label='Sans normalisation', color='red')
plt.plot(k_vector, accuracy_val_norm, 'o-', linewidth=2, markersize=4, label='Avec normalisation', color='green')
plt.axvline(x=k_star, color='red', linestyle='--', alpha=0.5, label=f'k* sans norm = {k_star}')
plt.axvline(x=k_star_norm, color='green', linestyle='--', alpha=0.5, label=f'k* avec norm = {k_star_norm}')
plt.xlabel('Valeur de k')
plt.ylabel('Précision (Validation)')
plt.title('Comparaison des Précisions de Validation')
plt.legend()
plt.grid(True, alpha=0.3)

# Comparaison des erreurs d'entraînement
plt.subplot(2, 2, 3)
plt.plot(k_vector, error_train, 's-', linewidth=2, markersize=4, label='Sans normalisation', color='blue')
plt.plot(k_vector, error_train_norm, 'o-', linewidth=2, markersize=4, label='Avec normalisation', color='orange')
plt.xlabel('Valeur de k')
plt.ylabel('Taux d\'erreur (Train)')
plt.title('Comparaison des Erreurs d\'Entraînement')
plt.legend()
plt.grid(True, alpha=0.3)

# Bar plot de comparaison finale
plt.subplot(2, 2, 4)
conditions = ['Sans normalisation', 'Avec normalisation']
accuracies = [test_accuracy, test_accuracy_norm]
errors = [test_error, test_error_norm]

x = np.arange(len(conditions))
width = 0.35

bars1 = plt.bar(x - width/2, accuracies, width, label='Précision', color=['lightcoral', 'lightgreen'], alpha=0.7)
bars2 = plt.bar(x + width/2, errors, width, label='Erreur', color=['red', 'green'], alpha=0.7)

plt.xlabel('Condition')
plt.ylabel('Score')
plt.title('Comparaison Finale des Performances sur le Test Set')
plt.xticks(x, conditions)
plt.legend()

# Ajout des valeurs sur les barres
for bar in bars1:
    height = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2., height + 0.01,
             f'{height:.3f}', ha='center', va='bottom', fontweight='bold')

for bar in bars2:
    height = bar.get_height()
    plt.text(bar.get_x() + bar.get_width()/2., height + 0.01,
             f'{height:.3f}', ha='center', va='bottom', fontweight='bold')

plt.grid(True, alpha=0.3, axis='y')
plt.tight_layout()
plt.show()

# =============================================================================
# 8. ANALYSE DÉTAILLÉE DES RÉSULTATS
# =============================================================================

print("\n" + "=" * 70)
print("8. ANALYSE DÉTAILLÉE DES RÉSULTATS")
print("=" * 70)

print(f"\n📋 RAPPORT DE CLASSIFICATION - SANS NORMALISATION (k={k_star}):")
print("-" * 60)
print(classification_report(Y_test, Y_test_pred, target_names=['Mauvaise Qualité', 'Bonne Qualité']))

print(f"\n📋 RAPPORT DE CLASSIFICATION - AVEC NORMALISATION (k={k_star_norm}):")
print("-" * 60)
print(classification_report(Y_test, Y_test_pred_norm, target_names=['Mauvaise Qualité', 'Bonne Qualité']))

# Matrices de confusion comparées
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 5))

# Matrice de confusion sans normalisation
cm1 = confusion_matrix(Y_test, Y_test_pred)
sns.heatmap(cm1, annot=True, fmt='d', cmap='Reds', ax=ax1,
            xticklabels=['Mauvaise Qualité', 'Bonne Qualité'],
            yticklabels=['Mauvaise Qualité', 'Bonne Qualité'])
ax1.set_title(f'Sans Normalisation (k={k_star})\nPrécision: {test_accuracy:.3f}')
ax1.set_ylabel('Vraie étiquette')
ax1.set_xlabel('Étiquette prédite')

# Matrice de confusion avec normalisation
cm2 = confusion_matrix(Y_test, Y_test_pred_norm)
sns.heatmap(cm2, annot=True, fmt='d', cmap='Greens', ax=ax2,
            xticklabels=['Mauvaise Qualité', 'Bonne Qualité'],
            yticklabels=['Mauvaise Qualité', 'Bonne Qualité'])
ax2.set_title(f'Avec Normalisation (k={k_star_norm})\nPrécision: {test_accuracy_norm:.3f}')
ax2.set_ylabel('Vraie étiquette')
ax2.set_xlabel('Étiquette prédite')

plt.tight_layout()
plt.show()

# =============================================================================
# 9. CONCLUSION ET SYNTHÈSE
# =============================================================================

print("\n" + "=" * 70)
print("9. CONCLUSION ET SYNTHÈSE")
print("=" * 70)

print(f"\n📊 RÉCAPITULATIF DU PROJET:")
print(f"   • Dataset : {df.shape[0]} échantillons, {X.shape[1]} features")
print(f"   • Répartition classes : {binary_counts[0]} mauvais vins ({binary_percentages[0]:.1f}%), "
      f"{binary_counts[1]} bons vins ({binary_percentages[1]:.1f}%)")

print(f"\n🎯 PERFORMANCE FINALE:")
print(f"   • Meilleur k sans normalisation : {k_star} → Précision : {test_accuracy:.3f}")
print(f"   • Meilleur k avec normalisation : {k_star_norm} → Précision : {test_accuracy_norm:.3f}")

if test_accuracy_norm > test_accuracy:
    improvement_acc = ((test_accuracy_norm - test_accuracy) / test_accuracy) * 100
    print(f"   ✅ La normalisation a amélioré la précision de {improvement_acc:.1f}%")
else:
    print(f"   ❌ La normalisation n'a pas amélioré la performance")

print(f"\n🔍 OBSERVATIONS CLÉS:")
print("   1. Impact de k : Les petites valeurs de k montrent du surapprentissage")
print("   2. Normalisation : Essentielle pour k-NN dû aux différentes échelles des features")
print("   3. Performance : Le modèle atteint une précision satisfaisante")

print(f"\n💡 RECOMMANDATIONS:")
print("   • Toujours normaliser les données pour les algorithmes basés sur les distances")
print("   • Utiliser la validation pour sélectionner les hyperparamètres")
print("   • Considérer d'autres algorithmes (Random Forest, SVM) pour comparaison")

# Affichage des features les plus corrélées avec la qualité
correlation_with_quality = df.corr()['quality'].drop('quality').abs().sort_values(ascending=False)
print(f"\n📈 FEATURES LES PLUS CORRÉLÉES AVEC LA QUALITÉ:")
for i, (feature, corr) in enumerate(correlation_with_quality.head(5).items()):
    print(f"   {i+1}. {feature:.<20} |r| = {corr:.3f}")

print("\n" + "=" * 70)
print("PROJET TERMINÉ AVEC SUCCÈS! 🎉")
print("=" * 70)
