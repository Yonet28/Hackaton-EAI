# Documentation Technique : Hackathon IA x RH

Ce document détaille l'architecture logicielle, les choix algorithmiques et les méthodes d'ingénierie de données mis en place pour le projet de prédiction de la rétention des talents.

## 1. Préparation et Ingénierie des Données

Le pipeline de données repose sur un jeu de données RH de référence (`HRDataset_v14.csv`). Avant la modélisation, plusieurs transformations sont appliquées pour garantir la fiabilité du modèle et sa conformité légale.

### 1.1. Nettoyage et Prévention des Fuites de Données (Data Leakage)
Pour éviter que le modèle n'apprenne à partir de variables dévoilant directement la cible (le fait qu'un employé soit déjà parti), les variables suivantes sont exclues de l'apprentissage :
* `DateofTermination`
* `TermReason`
* `EmpStatusID`

### 1.2. Anonymisation et Cybersécurité (Conformité RGPD)
Les informations permettant d'identifier directement les individus sont purgées du jeu de données d'entraînement afin de garantir la protection des données personnelles :
* `Employee_Name`
* `EmpID`
* `ManagerName`

### 1.3. Gestion des Données Sensibles (IA Éthique)
Pour prévenir la création de biais algorithmiques et de discriminations lors de l'apprentissage, les variables dites "sensibles" sont mises à l'écart du processus d'entraînement :
* `Sex` (Sexe)
* `RaceDesc` (Origine/Ethnie)
* `MaritalDesc` (Statut marital)

Ces variables sont toutefois conservées de manière isolée pour permettre d'éventuels audits d'équité.

## 2. Modélisation (Machine Learning)

### 2.1. Algorithme
Le modèle prédictif repose sur un algorithme de **Random Forest Classifier** implémenté via `scikit-learn`. Ce choix offre un excellent compromis entre performance prédictive et capacité d'explicabilité par rapport à des réseaux de neurones profonds.

### 2.2. Cible (Target)
La variable cible est `Termd`, une variable binaire où :
* `0` = Employé en poste
* `1` = Employé ayant quitté l'entreprise

### 2.3. Évaluation
La validation du modèle s'effectue sur un jeu de test séparé. Les performances sont mesurées via :
* L'exactitude (Accuracy)
* La matrice de confusion (pour évaluer les faux positifs et faux négatifs)
* Le rapport de classification (Précision, Rappel, F1-Score)
* L'aire sous la courbe ROC (ROC-AUC)

## 3. Implémentation des Piliers de l'IA de Confiance

### 3.1. IA Frugale (Mesure de l'Empreinte Carbone)
L'impact environnemental du développement est quantifié à l'aide de la bibliothèque **CodeCarbon**.
* Le module `EmissionsTracker` est instancié avant l'entraînement du modèle.
* Il mesure la consommation énergétique de la machine (CPU, RAM) et l'estime en équivalent CO2 émis.
* Les résultats permettent d'auditer la sobriété du code.

### 3.2. IA Explicable (XAI)
Le modèle Random Forest n'étant pas nativement transparent à 100%, la bibliothèque **SHAP** (SHapley Additive exPlanations) est intégrée.
* Un `TreeExplainer` calcule la contribution marginale (valeur SHAP) de chaque caractéristique pour chaque prédiction.
* Cela permet de passer d'un simple score de risque à une explication précise (ex: "Le risque de 85% est principalement dû à un salaire bas et une faible satisfaction").

## 4. Interface Utilisateur et Logique Métier

L'application de restitution est développée avec le framework **Gradio**.

### 4.1. Composants
* **Filtres interactifs** : Tri des employés par département et par seuil de risque.
* **Tableau de bord global** : Affichage des indicateurs de performance (KPIs) tels que le risque moyen et le nombre de profils à risque.
* **Panneau de détail** : Vue individuelle par employé sélectionné.

### 4.2. Fonctions de Traduction Métier
Le code intègre des fonctions spécifiques pour rendre les résultats de l'IA actionnables par les RH :
* `get_reasons` : Extrait et classe les variables ayant le plus fort impact (SHAP) pour les traduire en motifs textuels clairs.
* `build_actions` : Moteur de règles qui associe les motifs extraits par SHAP à des recommandations d'actions RH concrètes (ex: "Programmer une révision salariale", "Proposer une formation").