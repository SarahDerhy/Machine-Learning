# Project in Machine Learning — Movie Rating Prediction

## Authors

| Name        | ID        |
| ----------- | --------- |
| Shirel Amar | 207065103 |
| Sarah Derhy | 340889435 |

## Project overview

This project was completed as part of a Machine Learning course.

The goal is to predict the IMDb average rating of a movie using only information that can be available before the movie release. The target variable is:

```text
averageRating
```

The initial dataset contains 133,884 movies and 13 columns:

```text
tconst
primaryTitle
startYear
genres
lead_actors_ids
runtimeMinutes
averageRating
Language
Country
numVotes
budget
BoxOffice
plot
```

To avoid data leakage, the following columns are excluded from the model features:

```text
averageRating
numVotes
BoxOffice
```

The dataset is enriched with IMDb crew data in order to add information about movie directors.

---

## Main files

```text
Final Project - Machine Learning.ipynb
```

Main notebook containing data loading, preprocessing, feature engineering, model training, evaluation and analysis.

```text
report.pdf
```

Final report describing the methodology, results and conclusions.

```text
trained_model.pkl
```

Serialized trained pipeline containing the preprocessing steps and the selected final model.

```text
requirements.txt
```

List of required Python libraries.

```text
dataset.7z
```

Compressed initial dataset used in the project.

---

## Data preparation

The function:

```python
prepare_data(df)
```

is designed to work independently on raw input data, including an external test dataset or a single movie row.

It performs the following operations:

* loads IMDb crew data;
* adds director information;
* counts the number of directors per movie;
* cleans genre values;
* creates binary columns for the most frequent genres;
* creates a binary `genre_Other` feature for less frequent genres;
* cleans country values and normalizes country variants;
* groups countries into broader categories;
* handles invalid release years;
* creates engineered features;
* removes unused columns and leakage features.

Missing-value imputation, scaling and categorical encoding are performed inside the pipelines in order to avoid data leakage during cross-validation.

---

## Engineered features

The main engineered features are:

```text
num_directors
genre_<name>
genre_Other
country_group
country_known
decade
num_genres
num_lead_actors
directors_x_genres
age_x_country_known
```

---

## Models

Two regression models are compared:

### Elastic Net

Elastic Net combines Lasso and Ridge regularization.

The tuned hyperparameters are:

```text
alpha = 0.01
l1_ratio = 0.1
```

### Random Forest

Random Forest uses multiple regression trees and can capture more complex relationships.

The tuned hyperparameters are:

```text
n_estimators = 200
max_depth = 20
min_samples_leaf = 10
```

---

## Model evaluation

The models are evaluated using 10-fold cross-validation.

| Model         |              RMSE |               MAE |                R² |
| ------------- | ----------------: | ----------------: | ----------------: |
| Elastic Net   |     1.137 ± 0.047 |     0.873 ± 0.036 |     0.222 ± 0.021 |
| Random Forest | **1.114 ± 0.052** | **0.849 ± 0.040** | **0.254 ± 0.027** |

Random Forest achieves the lowest RMSE and is selected as the final model.

---

## Additional analyses

The notebook also includes:

* extraction of the largest overpredictions and underpredictions;
* qualitative analysis of model errors;
* comparison between Elastic Net and Random Forest outliers;
* fairness analysis by genre and release decade;
* feature importance analysis;
* suggested features for future improvements.

---

## How to run the project

1. Clone or download the repository.

2. Open the main notebook:

```text
Final Project - Machine Learning.ipynb
```

3. Install the required libraries:

```bash
pip install -r requirements.txt
```

4. Run all notebook cells from top to bottom.

The first cells import the required libraries and load the necessary data. IMDb crew data is also loaded automatically from IMDb.

At the end of the notebook, the trained model is saved as:

```text
trained_model.pkl
```

---

## Requirements

Install the required libraries with:

```bash
pip install -r requirements.txt
```

The notebook uses, among others:

```text
pandas
numpy
scikit-learn
matplotlib
requests
py7zr
pycountry
joblib
```
