# gasoil-crack-spread-analysis
Statistical Analysis and ML forecasting of European Low Sulphur Gas Oil crack spread for refinery hedging.

## Data Sourcing
Data was downloaded year by year as available from MarketWatch as CSV files. 
- BR00 (Brent Crude Oil Futures Continuous Contract)
- GS00 (Low Sulphur Gas Oil Futures Continuous Contract)

## Data Processing
The 17 CSV files for each ticker were read in as pandas DFs, concat'ed to form one large dataframe spanning the futures data from 2009 to 2025. 

Data Processing included:
- Date column transformation to datetime
- Sorting Date column chronologically
- Dropping NaN values
- Initial Plot to verify DF

Finally the combined DFs were saved in "data/processed" as "crude_futures.csv" and "gasoil_futures.csv". These are our cleaned dataframes upon which we will perform EDA etc.