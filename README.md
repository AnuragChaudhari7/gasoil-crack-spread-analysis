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

This merged dataframe is stored in "data/processed" as "crack_spread.csv". We will be using this to perform EDA and build it up with our future calculations.

## Exploratory Data Analysis (EDA)
This notebook follows loosely a Box-Jenkins methodology to discover seasonality & stationarity within the data. 

Initial findings show the data is non-stationarity and exhibits yearly seasonality (252 trading days). Thus we used a first order differencing method to remove the stationarity. And after plotting the ACF and PACF graphs, we used this to infer the model to be used (in this case an ARIMA model) with non-seasonal parameters $p$ and $q$ determined. Interestingly the differenced data no longer exhibited seasonality despite only first order differencing. The reason for this I am not sure.

## ARIMA/SARIMA Model Fitting
After fitting and analysing the model results compared to naive prediction, our model does not predict well. There are various reasons as uncovered by our EDA and model assumptions. So this was as expected however, this has facilitated a need for better suited models. This has led to a discussion in the arima_model.ipynb file discussing ML models, exogenous data as well as a hybrid model based on a research paper.