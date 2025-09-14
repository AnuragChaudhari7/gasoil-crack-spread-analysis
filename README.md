# gasoil-crack-spread-analysis
Statistical Analysis and ML forecasting of European Low Sulphur Gas Oil crack spread for refinery hedging.

## Data Sourcing
Data was downloaded year by year as available from MarketWatch as CSV files. 
- BR00 (Brent Crude Oil Futures Continuous Contract)
- GS00 (Low Sulphur Gas Oil Futures Continuous Contract)

## Data Cleaning
The 17 CSV files for each ticker were read in as pandas DFs, concat'ed to form one large dataframe spanning the futures data from 2009 to 2025. 

Data Processing included:
- Date column transformation to datetime
- Sorting Date column chronologically
- Dropping NaN values
- Initial Plot to verify DF
- Dropping duplicate dates

Finally the combined DFs were saved in "data/processed" as "crude_futures.csv" and "gasoil_futures.csv". These are our cleaned dataframes.

## Data Processing
The CSV files of the cleaned Crude and Gas Oil data were imported before being merged. The merging was done by intersection of dates. 

The data have different units for the prices. The Brent Crude futures is in dollars/bbl whilst the LSGO futures is in dollars/metric ton. Thus approximating the conversion factors from energy companies, I divided LSGO prices by 7.46 approximating the number of barrels per metric ton.

After which a crack_spread calculation was performed by the differences between the date by date close prices. 

This merged dataframe is stored in "data/processed" as "crack_spread.csv". I will be using this to perform EDA and build it up with our future calculations.

## Exploratory Data Analysis (EDA)
This notebook follows loosely a Box-Jenkins methodology to discover seasonality & stationarity within the data. 



# Gasoil Crack Spread Analysis

## Project Overview
This repository performs a **statistical and machine learning–based forecasting analysis** of the **European Low Sulphur Gas Oil (LSGO) crack spread** for potential refinery hedging strategies.  
The crack spread is defined as the **difference between refined product (LSGO) and crude oil (Brent) futures prices**.  

I have implemented a structured data pipeline:
1. **Data Cleaning** → Consolidation of futures data from multiple CSVs.  
2. **Data Processing** → Unit normalization, merging, crack spread calculation.  
3. **Exploratory Data Analysis (EDA)** → Stationarity & seasonality checks (Box–Jenkins methodology).  
4. **Modeling** → ARIMA/SARIMA forecasting and comparison with naive models.  
5. **Discussion** → Limitations and directions for ML and hybrid approaches.


## Repository Structure
```
.
├── data/
│   ├── raw/                  # Raw futures CSVs (BR00 & GS00 from MarketWatch)
│   └── processed/            # Cleaned & processed datasets (crude, gasoil, crack_spread)
├── notebooks/
│   ├── data_cleaning.ipynb   # Data ingestion & cleaning pipeline
│   ├── data_processing.ipynb # Unit conversion, merging & crack spread calculation
│   ├── eda.ipynb             # Stationarity, ACF/PACF, differencing
│   └── arima_model.ipynb     # ARIMA modeling & performance evaluation
├── README.md                 # Project documentation
└── requirements.txt          # Python dependencies
```


## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/<your-username>/gasoil-crack-spread-analysis.git
cd gasoil-crack-spread-analysis
```

### 2. Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

Dependencies include:
- `pandas` (data handling)
- `numpy` (numerical analysis)
- `matplotlib`, `seaborn` (visualizations)
- `statsmodels` (time series modeling)
- `scikit-learn` (metrics, future ML models)
- `jupyter` (interactive analysis)


## Data Pipeline

### Data Cleaning (`data_cleaning.ipynb`)
- Reads **17 yearly CSVs** for Brent (`BR00`) and Gas Oil (`GS00`) futures (2009–2025).
- Concatenates into a single time series per ticker.
- Operations:
  - Date parsing → `datetime`
  - Sorting chronologically
  - Handling missing values
  - Removing duplicates
- Exports → `data/processed/crude_futures.csv`, `gasoil_futures.csv`.

### Data Processing (`data_processing.ipynb`)
- Brent futures prices are in **USD/barrel**.
- Gasoil futures prices are in **USD/metric ton**.
- Conversion → `1 metric ton ≈ 7.46 barrels`.
- Crack spread computed as:
\[
	ext{Crack Spread}_t = P^{LSGO}_t / 7.46 - P^{Brent}_t
\]
- Merged dataframe saved as → `data/processed/crack_spread.csv`.

### Exploratory Data Analysis (`eda.ipynb`)
- **Objective**: Identify statistical properties (stationarity, autocorrelation, seasonality).
- Tests applied:
  - Rolling mean/variance plots
  - Augmented Dickey–Fuller (ADF) test
  - Autocorrelation (ACF) & Partial Autocorrelation (PACF)
- Key Findings:
  - Raw series → **non-stationary**, annual seasonality (~252 trading days).
  - After **first-order differencing**, series becomes stationary and seasonality largely disappears.

### ARIMA Modeling (`arima_model.ipynb`)
- Follows Box–Jenkins methodology.
- Candidate models fit:
  - ARIMA(\(p,1,q\)) with differenced series.
  - Seasonal SARIMA models tested but seasonality diminished post-differencing.
- Evaluation:
  - Compared against naive forecast (last value carried forward).
  - Metrics: RMSE, MAE, MAPE.
- Results:
  - ARIMA performed **marginally better** than naive models, but predictive power remains weak.
- Discussion:
  - Structural market factors not captured by univariate ARIMA.
  - Need for **exogenous regressors (ARIMAX)** or **ML-based approaches**.

## Results & Insights
- Crack spread exhibits **non-stationarity** and **weak predictability** using ARIMA.
- Market is influenced by exogenous drivers (refinery demand, shipping, weather, regulations).
- Purely statistical forecasting is insufficient → motivates **hybrid econometric + ML models**.

## Future Work
- **Exogenous features**: Add refinery margins, demand data, shipping indices.
- **Machine Learning Models**:
  - Random Forests, Gradient Boosting
  - LSTMs for time series
  - Hybrid ARIMA–ML models
- **Backtesting** crack-spread hedging strategies.
- **Visualization dashboards** for monitoring spreads.

## References
1. Box, G.E.P., Jenkins, G.M., & Reinsel, G.C. (2015). *Time Series Analysis: Forecasting and Control*.
2. MarketWatch Futures Data — Brent & Gas Oil.
3. Research on hybrid ARIMA–ML crack spread forecasting models.

## Author
Developed by Anurag Chaudhari as part of refinery risk management and quant finance research.



Initial findings show the data is non-stationarity and exhibits yearly seasonality (252 trading days). Thus I used a first order differencing method to remove the stationarity. And after plotting the ACF and PACF graphs, I used this to infer the model to be used (in this case an ARIMA model) with non-seasonal parameters $p$ and $q$ determined. Interestingly the differenced data no longer exhibited seasonality despite only first order differencing. The reason for this I am not sure.

## ARIMA/SARIMA Model Fitting
After fitting and analysing the model results compared to naive prediction, our model does not predict well. There are various reasons as uncovered by our EDA and model assumptions. So this was as expected however, this has facilitated a need for better suited models. This has led to a discussion in the arima_model.ipynb file discussing ML models, exogenous data as well as a hybrid model based on a research paper.
