# Hackathon IA x RH - Rétention des Talents

## 1. Objectifs du Projet
Ce projet propose une solution basée sur l'intelligence artificielle pour aider les départements des Ressources Humaines à comprendre les causes profondes du turnover et à prédire les risques de démission des employés. 

Il a été conçu en respectant scrupuleusement les quatre piliers d'une IA de confiance :
* Cybersécurité et conformité RGPD (anonymisation des données).
* Éthique (prévention des biais discriminatoires liés au genre, à l'origine, etc.).
* Frugalité (mesure et limitation de l'empreinte carbone du modèle).
* Explicabilité (compréhension claire des décisions de l'algorithme).

## 2. Périmètre (Scope)
L'application repose sur un jeu de données synthétiques (environ 400 employés). Le périmètre technique du projet couvre les étapes suivantes :
* Nettoyage, préparation et anonymisation rigoureuse des données personnelles.
* Analyse exploratoire des principaux motifs de départ.
* Entraînement d'un modèle d'apprentissage automatique (Random Forest) pour la prédiction des démissions.
* Évaluation continue de l'impact énergétique via CodeCarbon.
* Création d'un tableau de bord interactif pour la restitution des résultats.

## 3. Personas
* **Direction RH / Managers (Utilisateurs finaux)** : Ils utilisent le tableau de bord pour identifier rapidement les talents à risque de départ, comprendre les raisons sous-jacentes grâce à l'IA explicable, et mettre en place des plans d'action ciblés (entretiens, revalorisations salariales, etc.).
* **Équipe Technique / Data Scientists (Concepteurs)** : Ils assurent le développement, la maintenance et l'audit du modèle, en veillant à la sécurité, l'équité et la sobriété du code.

## 4. Instructions d'Installation et d'Utilisation

### Prérequis
Assurez-vous d'avoir Python installé sur votre environnement. Les bibliothèques nécessaires sont les suivantes :
* pandas
* scikit-learn
* shap
* gradio
* codecarbon
* matplotlib
* seaborn

### Déploiement
1. Clonez ce dépôt ou téléchargez les fichiers du projet.
2. Assurez-vous que le fichier de données source nommé `HRDataset_v14.csv` est placé dans le même répertoire que le notebook.
3. Ouvrez le fichier `Hackaton.ipynb` (par exemple via Jupyter Notebook ou Google Colab).
4. Exécutez l'ensemble des cellules du notebook de manière séquentielle.
5. La dernière cellule lancera l'interface utilisateur Gradio. Cliquez sur le lien local (ou public) généré dans la console pour accéder au tableau de bord interactif.

## 5. Architecture Technique
Le pipeline de données suit cette structure :
1. Ingestion du fichier CSV.
2. Module RGPD : Suppression des identifiants directs. Mise à l'écart des données sensibles pour audit ultérieur.
3. Entraînement : Random Forest Classifier couplé à CodeCarbon pour le suivi CO2.
4. Explicabilité : Utilisation de SHAP pour déterminer l'importance de chaque variable dans le score de risque.
5. Interface : Application Gradio pour la visualisation et la recommandation d'actions RH.