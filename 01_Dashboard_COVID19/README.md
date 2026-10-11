# Dashboard COVID-19 avec Cassandra, FastAPI et Tkinter

Ce projet charge des données COVID-19 dans Cassandra, les expose avec une API FastAPI et les affiche dans une application de bureau Tkinter. Le tableau de bord propose notamment des graphiques, des comparaisons entre pays, des exports et des prédictions simples.

## Prérequis

- Python 3
- Cassandra accessible sur `127.0.0.1:9042`
- Tkinter disponible dans l'installation Python
- Git LFS pour récupérer le fichier de données suivi par le dépôt

Après avoir cloné le dépôt, récupérez les objets Git LFS :

```bash
git lfs pull
cd 01_Dashboard_COVID19
```

## Installation

Créez un environnement virtuel, activez-le, puis installez les dépendances :

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Sous Windows, activez l'environnement avec `.venv\Scripts\activate`.

## Préparation de Cassandra

Créez le keyspace et la table avec `cqlsh` :

```sql
CREATE KEYSPACE IF NOT EXISTS covid
WITH replication = {
    'class': 'SimpleStrategy',
    'replication_factor': 1
};

USE covid;

CREATE TABLE IF NOT EXISTS stats (
    country TEXT,
    date DATE,
    total_cases INT,
    total_deaths INT,
    PRIMARY KEY ((country), date)
);
```

## Import des données

Le script d'import utilise `Preparation Data/owid-covid-data.csv` et doit être exécuté depuis ce dossier afin que le chemin relatif soit résolu correctement :

```bash
cd "Preparation Data"
python Preparation_Data.py
cd ..
```

L'import nettoie les colonnes utiles puis insère les lignes dans la table `covid.stats` par lots.

## Lancement

Depuis `01_Dashboard_COVID19`, lancez l'API et l'interface de bureau ensemble :

```bash
python main.py
```

Le lanceur démarre :

- l'API FastAPI sur `http://127.0.0.1:8000` ;
- l'interface Tkinter depuis `frontend/F_App.py`.

La documentation interactive de l'API est disponible sur `http://127.0.0.1:8000/docs` et son état peut être vérifié sur `http://127.0.0.1:8000/health`.

Pour les lancer séparément, utilisez deux terminaux ouverts dans `01_Dashboard_COVID19` :

```bash
uvicorn backend.B_App:app --host 127.0.0.1 --port 8000 --reload
```

```bash
python frontend/F_App.py
```

## Structure principale

- `backend/B_App.py` : API FastAPI et accès à Cassandra ;
- `frontend/F_App.py` : interface graphique Tkinter ;
- `frontend/ml_analysis.py` : fonctions de prédiction ;
- `Preparation Data/Preparation_Data.py` : nettoyage et import du jeu de données ;
- `main.py` : lanceur du backend et du frontend.
