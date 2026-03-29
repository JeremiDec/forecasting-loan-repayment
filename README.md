# Forecasting Loan Repayment - Kaggle Competition

Projekt uczenia maszynowego stworzony w ramach konkursu [Forecasting Loan Repayment](https://www.kaggle.com/competitions/forecasting-loan-repayment) na platformie Kaggle. Celem projektu jest zbudowanie modelu predykcyjnego, ktory na podstawie danych historycznych oceni prawdopodobienstwo splaty pozyczki przez klienta.

## Opis projektu

Zdolnosc do przewidywania, czy pozyczkobiorca splaci swoje zobowiazanie, jest kluczowym elementem zarzadzania ryzykiem w instytucjach finansowych. W tym projekcie analizujemy zestaw danych zawierajacy ponad 593 000 rekordow pozyczkowych, aby sklasyfikowac klientow jako wiarygodnych lub zagrozonych niewyplacalnoscia.

### Cel biznesowy
- Minimalizacja strat finansowych wynikajacych z udzielania pozyczek osobom o wysokim ryzyku.
- Automatyzacja procesu oceny zdolnosci kredytowej.

## Podejscie

### Preprocessing
Minimalne kodowanie - ordinal mapping dla `grade_subgrade` i `education_level`, natywne kategorie dla pozostalych cech kategorycznych. Reczny feature engineering (np. `loan_to_income`, `dti_squared`) nie poprawil wynikow w walidacji krzyzowej - modele gradient boosting same odkrywaja interakcje miedzy cechami.

### Modele
Wytrenowano 5 modeli w 5-fold Stratified K-Fold:

| Model | OOF AUC |
|-------|---------|
| LightGBM | 0.92354 |
| XGBoost | 0.92221 |
| CatBoost | 0.92051 |
| Random Forest | 0.91084 |
| Regresja logistyczna | 0.91023 |

Hiperparametry modeli GBM zostaly ztunowane przy uzyciu Optuna (30 triali na model).

### Stacking
Przetestowano 6 wariantow ensemblowania (stacking z meta-modelem LogReg + srednia wazona z optymalizacja wag). Najlepszy wariant: **stacking 3 modeli GBM (LightGBM + XGBoost + CatBoost)** z meta-modelem regresji logistycznej.

| Wariant | OOF AUC |
|---------|---------|
| Stacking 3 GBM | **0.92355** |
| Srednia wazona LGB+XGB | 0.92338 |
| Srednia wazona 5 modeli | 0.92312 |
| Stacking 5 modeli | 0.92251 |
| Stacking LGB+XGB+LogReg | 0.92239 |
| Srednia wazona LGB+XGB+LogReg | 0.92048 |

Wagi meta-modelu: LightGBM 4.945, XGBoost 1.716, CatBoost 0.379 - LightGBM dominuje, CatBoost wnosi marginalna wartosc. Dodanie slabszych modeli (LogReg, RF) pogarsza wynik - meta-model nadaje im ujemne wagi.

## Dane

Zbior treningowy: 593 994 rekordow, 12 cech (5 numerycznych, 5 kategorycznych + 2 pochodne z grade_subgrade).
Zbior testowy: 254 569 rekordow.
Target: `loan_paid_back` (binarna, ~80% klasy pozytywnej).

Brak brakow danych i duplikatow.

## Technologie

Projekt zrealizowany w Pythonie:
- **Pandas, NumPy** - manipulacja danymi
- **Scikit-learn** - preprocessing, walidacja krzyzowa, modele bazowe (LogReg, RF)
- **LightGBM, XGBoost, CatBoost** - modele gradient boosting
- **Optuna** - tuning hiperparametrow
- **SciPy** - optymalizacja wag w glosowaniu
- **Matplotlib, Seaborn** - wizualizacja
- **Jupyter Notebook** - srodowisko eksperymentalne

## Struktura projektu

```
.
├── data/
│   ├── train.csv
│   └── test.csv
├── notebooks/
│   ├── EDA.ipynb                    # analiza eksploracyjna, feature engineering, uzasadnienie decyzji
│   ├── Model.ipynb                  # pelny pipeline: preprocessing, trening, stacking, inference
│   └── Hiperparams_finding.ipynb    # tuning Optuna dla LightGBM, XGBoost, CatBoost
├── submissions/
│   └── submission.csv
├── README.md
└── requirements.txt
```

### Opis notebookow

**EDA.ipynb** - analiza jakosci danych, rozklady cech, analiza korelacji, porownanie podejsc do feature engineeringu (minimalne vs rozszerzone), waznosc cech, uzasadnienie wyboru modeli.

**Model.ipynb** - kompletny pipeline: preprocessing, trening OOF 5 modeli, korelacja predykcji, porownanie 6 wariantow stackingu/glosowania, analiza wynikow, generowanie submission.

**Hiperparams_finding.ipynb** - optymalizacja hiperparametrow Optuna (30 triali, 5-fold CV) dla LightGBM, XGBoost i CatBoost. Stacking z ztunowanymi parametrami i porownanie z defaultowymi.

## Uruchomienie

```bash
git clone https://github.com/JeremiDec/forecasting-loan-repayment.git
cd forecasting-loan-repayment
pip install -r requirements.txt
```
