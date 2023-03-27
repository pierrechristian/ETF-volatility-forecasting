# ETF-volatility-forecasting
Forecasting the volatilities of three ETFs localized in different geographic regions.

# Context:
The volatility of a stock is a measure of how large the stock price changes, defined as the square-root of the variance of the returns. In modern portfolio theory, the volatility is used as a proxy for the risk associated with an investment: the higher the volatility, the higher the risk. 

Periods of large volatilities tend to be followed by more periods of large volatilities (and similarly for periods of small volatilities), giving rise to a phenomenon known as volatility clustering. Owing to this observation, volatilities in financial time series are modelled as having dependencies on their lagged values using the generalized autoregressive conditional heteroskedasticity (GARCH) models. 

In this project, we will use GARCH to model and forecast the volatilities of three Exchange-Traded Funds (ETFs) from three geographic regions, the United States, East Asia, and South-East Asia. Perhaps of note, our time series will include the 2020 stock market crash due to the COVID-19 pandemic.

# GOALS: 
- ) Model the volatilities of three ETFs localized in three geographic regions 
- ) Forecast the next month's volatilities

# Data Source:
Market data from Yahoo! finance loaded using the yfinance library

# Description of Data:
Daily time series of prices from 01/01/2018 to 02/20/2023 for the following three ETFs:
-  SPY (SPDR S&P 500 ETF Trust): tracks the S&P500 (500 largest companies listed on stock exchanges in the United States)
-  AIA (iShares Asia 50 ETF): tracks 50 of the largest Asian stocks -- heavily weighted towards East Asian companies
-  ASEA (Global X FTSE Southeast Asia ETF): tracks 40 of the largest South East Asian stocks

The daily returns of these ETFs are plotted below:
![daily_returns](https://user-images.githubusercontent.com/5288149/227814390-adec6c12-35c7-4993-bb5c-0035b945dd89.png)
Notice the spike in volatility in 2020, which is brought upon by COVID-19 pandemic.

# Methodology:
The time series were modelled with the GARCH model. Instead of manually guessing for the best GARCH order parameters, we performed the following procedure:
1) A grid search was performed over the p, q order parameters, the model with the lowest AIC is taken as candidate
2) The t-statistics of the candidate model parameters are inspected to ensure that they are significant 

