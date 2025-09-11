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

The data have different units for the prices. The Brent Crude futures is in \$/bbl whilst the LSGO futures is in \$/metric ton. Thus approximating the conversion factors from energy companies, I divided LSGO prices by 7.46 approximating the number of barrels per metric ton.

After which a crack_spread calculation was performed by the differences between the date by date close prices. 

This merged dataframe is stored in "data/processed" as "crack_spread.csv". We will be using this to perform EDA and build it up with our future calculations.