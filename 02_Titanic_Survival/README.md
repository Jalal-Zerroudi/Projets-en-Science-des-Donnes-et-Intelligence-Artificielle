
# Titanic Survival Prediction API

Ce projet propose une API web construite avec **FastAPI** pour prédire la survie de passagers du Titanic, à partir de caractéristiques comme leur âge, classe, sexe, etc. Il inclut également une visualisation des performances du modèle et une analyse statistique des données de test.

---

## Fonctionnalités

- **API REST** avec FastAPI pour la prédiction de survie (`/predict`)
- **Page HTML `/metrics`** affichant :
  - Performances du modèle (Accuracy, Precision, Recall, F1 Score)
  - Aperçu des données de test
  - Analyse statistique des données (âge moyen, répartition par sexe et classe)
- Modèle de machine learning entraîné avec `LogisticRegression` sur le dataset Titanic
- Backend proprement structuré (FastAPI + Jinja2)

---

## Structure du projet

```
Titanic_Survival/
├── CreateModels/
│   ├── TitanicDataset.csv
│   ├── main.ipynb           # Préparation, entraînement et sauvegarde du modèle
│
├── backend/
│   ├── main.py              # API FastAPI
│   ├── model_handler.py     # Classe de gestion du modèle
│   ├── model.pkl            # Modèle ML + X_test/y_test
│   └── templates/
│       └── metrics.html     # Page HTML avec analyse + performances
```

---

## Lancer l'application

### 1. Prérequis

- Python 3.8+
- Bibliothèques Python :
  ```
  pip install fastapi uvicorn scikit-learn pandas jinja2
  ```

### 2. Entraîner et sauvegarder le modèle

Dans `CreateModels/main.ipynb` :

```python
# Extrait final
with open("../backend/model.pkl", "wb") as f:
    pickle.dump({
        "model": model,
        "X_test": X_test,
        "y_test": y_test
    }, f)
```

### 3. 🚀 Lancer FastAPI

Dans le dossier `backend` :

```bash
uvicorn main:app --reload
```

- Interface Swagger UI : [http://localhost:8000/docs](http://localhost:8000/docs)
- Analyse et performance : [http://localhost:8000/metrics](http://localhost:8000/metrics)

---

## 📌 Exemple d'appel API `/predict`

Les champs sont obligatoires. L'ordre ci-dessous correspond aux caractéristiques utilisées dans `CreateModels/main.ipynb` et reconstruites par le backend :

| Champ | Type API | Signification / encodage d'entraînement |
| --- | --- | --- |
| `Pclass` | entier | Classe : 1, 2 ou 3 |
| `Sex` | entier | 0 = female, 1 = male |
| `Age` | nombre | Âge du passager |
| `SibSp` | entier | Nombre de frères/sœurs et conjoints à bord |
| `Parch` | entier | Nombre de parents et enfants à bord |
| `Fare` | nombre | Tarif du billet |
| `Embarked` | entier | 0 = S, 1 = C, 2 = Q |

Ces codes décrivent les données d'entraînement ; le modèle Pydantic actuel ne restreint pas les entiers aux catégories indiquées et ne définit pas de bornes numériques métier.

Depuis un terminal compatible avec les commandes shell POSIX, une fois le serveur lancé :

```bash
curl -X POST http://localhost:8000/predict \
  -H 'Content-Type: application/json' \
  -d '{"Pclass":3,"Sex":1,"Age":22.0,"SibSp":1,"Parch":0,"Fare":7.25,"Embarked":0}'
```

La réponse contient une clé `survived` de type booléen : `true` pour la classe prédite « survie », `false` sinon. La valeur dépend du modèle chargé dans `backend/model.pkl` ; l'exemple de passager ne garantit donc pas une prédiction particulière.

Le serveur charge le fichier pickle dès son démarrage. N'utilisez qu'un artefact de confiance : le chargement d'un pickle peut exécuter du code. Les métriques de `/metrics` portent sur `X_test` et `y_test` enregistrés dans ce même fichier.

---

## 🧠 Modèle utilisé

- `LogisticRegression` (Scikit-learn)
- Données issues du dataset Titanic (`TitanicDataset.csv`)
