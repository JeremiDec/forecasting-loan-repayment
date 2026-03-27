# Forecasting Loan Repayment - Kaggle Competition

Projekt uczenia maszynowego stworzony w ramach konkursu [Forecasting Loan Repayment](https://www.kaggle.com/competitions/forecasting-loan-repayment) na platformie Kaggle. Celem projektu jest zbudowanie modelu predykcyjnego, który na podstawie danych historycznych oceni prawdopodobieństwo spłaty pożyczki przez klienta.

## 📌 Opis projektu

Zdolność do przewidywania, czy pożyczkobiorca spłaci swoje zobowiązanie, jest kluczowym elementem zarządzania ryzykiem w instytucjach finansowych. W tym projekcie analizujemy zestaw danych zawierający informacje o ponad 111 000 rekordów pożyczkowych, aby sklasyfikować klientów jako wiarygodnych lub zagrożonych niewypłacalnością (default).

### Cel biznesowy:
- Minimalizacja strat finansowych wynikających z udzielania pożyczek osobom o wysokim ryzyku.
- Automatyzacja procesu oceny zdolności kredytowej.

## 📊 Dane



## 🛠️ Technologie

Projekt został zrealizowany przy użyciu języka **Python** oraz następujących bibliotek:
- **Pandas & NumPy** – manipulacja i analiza danych.
- **Scikit-learn** – budowa modeli klasyfikacyjnych i preprocessing.
- **XGBoost / LightGBM / CatBoost** – zaawansowane algorytmy boostingowe.
- **Matplotlib & Seaborn** – wizualizacja danych i wyników EDA.
- **Jupyter Notebook** – środowisko eksperymentalne.

## 🚀 Struktura projektu

```text
.
├── data/
├── notebooks/
│   ├── EDA_final.ipynb
│   └── submission_pipeline.ipynb
├── submissions/
├── README.md
└── requirements.txt


## Uruchomienie projektu

1. git clone [https://github.com/JeremiDec/forecasting-loan-repayment.git](https://github.com/JeremiDec/forecasting-loan-repayment.git)
cd forecasting-loan-repayment
2. pip install -r requirements.txt
