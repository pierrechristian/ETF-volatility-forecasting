# ETF-volatility-forecasting
Forecasting the volatilities of three ETFs localized in different geographic regions.

# Context:

The volatility of a stock is a measure of how large the stock price changes, defined as the square-root of the variance of the returns. In modern portfolio theory, the volatility is used as a proxy for the risk associated with an investment: the higher the volatility, the higher the risk. 

Periods of large volatilities tend to be followed by more periods of large volatilities (and similarly for periods of small volatilities), giving rise to a phenomenon known as volatility clustering. Owing to this observation, volatilities in financial time series are modelled as having dependencies on their lagged values using the generalized autoregressive conditional heteroskedasticity (GARCH) models. 

In this project, we will use GARCH to model and forecast the volatilities of three Exchange-Traded Funds (ETFs) from three geographic regions, the United States, East Asia, and South-East Asia. Perhaps of note, our time series will include the 2020 stock market crash due to the COVID-19 pandemic.

# GOALS: 

- ) Model the volatilities of three ETFs localized in three geographic regions 
- ) Forecast the next month's volatilities

# Description of Data:
Daily time series of ETF prices from 01/01/2018 to 02/20/2023

# Data Source:
Market data from Yahoo! finance loaded using the yfinance library

# 
