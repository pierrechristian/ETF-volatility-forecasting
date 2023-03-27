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

![daily_returns](https://user-images.githubusercontent.com/5288149/227814758-d17fda74-f86c-4ff0-8b56-54c187b4d880.png)

Notice the spike in volatility in 2020, which is brought upon by COVID-19 pandemic.

# Methodology:
The time series were initially modelled with the GARCH model with normally distributed innovations. Instead of manually guessing for the best GARCH order parameters, we performed the following procedure:
1) A grid search was performed over the p, q order parameters, the model with the lowest AIC is taken as candidate
2) The t-statistics of the candidate model parameters were inspected to ensure their statistical significance

After this initial procedure, the model is validated in the following way:
1) ACF and PACF plots of the squared standarized residuals were inspected for leftover (unmodelled) GARCH effects
2) Ljung-Box (LB) tests were conducted on the squared standarized residuals; a small LB p-value indicates that there might be leftover GARCH effects that are not modelled by our candidate GARCH processes
3) The histogram of the standarized residuals are plotted over the normal distribution to check for deviations from normality 

In step 3), we are checking that our assumption of normally distributed innovation is appropriate. While the 

[residual_normal.pdf](https://github.com/pierrechristian/ETF-volatility-forecasting/files/11073541/residual_normal.pdf)
