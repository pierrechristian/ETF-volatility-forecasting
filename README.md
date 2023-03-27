# ETF-volatility-forecasting
Forecasting the volatilities of three ETFs localized in different geographic regions.

# Context:
The volatility of a stock is a measure of how large the stock price changes, defined as the square-root of the variance of the returns. In modern portfolio theory, the volatility is used as a proxy for the risk associated with an investment: the higher the volatility, the higher the risk. 

Periods of large volatilities tend to be followed by more periods of large volatilities (and similarly for periods of small volatilities), giving rise to a phenomenon known as volatility clustering. Owing to this observation, volatilities in financial time series are modelled as having dependencies on their lagged values using the generalized autoregressive conditional heteroskedasticity (GARCH) models. 

In this project, we will use GARCH to model and forecast the volatilities of three Exchange-Traded Funds (ETFs) from three geographic regions, the United States, East Asia, and South-East Asia. Perhaps of note, our time series will include the 2020 stock market crash due to the COVID-19 pandemic.

# Goals: 
-  Model the volatilities of three ETFs localized in three geographic regions 
-  Forecast the next month's volatilities

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

After this initial procedure, the appropriateness of the models were inspected in the following ways:
1) ACF and PACF plots of the squared standarized residuals were inspected for leftover (unmodelled) GARCH effects
2) Ljung-Box (LB) tests were conducted on the squared standarized residuals; a small LB p-value indicates that there might be leftover GARCH effects that are not modelled by our candidate GARCH processes
3) The histogram of the standarized residuals are plotted over the normal distribution to check for deviations from normality; this checks whether our assumption of normally distributed innovations was appropriate

While the models were appropriate based on 1) and 2) (indicating that most of the GARCH effects were succesfully modelled by our chosen GARCH order parameters), the histogram from 3) show deviations from normal distribution:

![residual_normal](https://user-images.githubusercontent.com/5288149/227815255-af7e4598-b98e-4ea2-9864-ebe2445cd15f.png)

This indicates that the original assumption of normally distributed innovations is not appropriate: the residual distributions overshoot the normal distribution near the mean, but are still quite symmetric. Given these observations, we re-modelled the time series, this time assuming Student-t distributed innovations. The same steps from before were used. This time, the histogram of standarized residuals are well fitted with our assumed distribution, the Student-t distribution:

![residual_student-t](https://user-images.githubusercontent.com/5288149/227815771-668f1831-0006-4352-9055-30efbed6be86.png)

# Results

## Final models
Our final models for the three ETFs are (all with Student-t distributed errors):
-  SPY: GARCH(1,1)
-  AIA: GARCH(1,1)
-  ASEA: ARCH(3,0)

## In-sample and rolling volatilities
To see how our models perform in the timeframe where we have data, we generated two kinds of predictions: in-sample and rolling. 

In-sample predictions: the GARCH models that generate these predictions saw all the available data at once during fitting.
Rolling predictions: in a rolling prediction, we assume that we start at day 0 (at the beginning of the dataset) and see each datapoint sequentially one by one. After every new datapoint, the model for each ETF is updated with the true value of the time series and generate a prediction for the next point. This simulates the situation in real life, where we get to see the datapoints one day at a time.

The modelled volatilities (both in-sample and rolling) for each stock can be seen below overplotted over the daily returns:

![vol_pred_roll](https://user-images.githubusercontent.com/5288149/227816533-1d1ab428-ed5c-48ba-a494-2ae73303e915.png)

Both in-sample and rolling predictions show that when the ETF prices rise/fall sharply, the volatilities spike, which is what we expected!

## Forecasts
Finally, we used the GARCH models to produce a forecast of the volatilities for the 21 days (1 month) after the end of the dataset.

![vol_next_month](https://user-images.githubusercontent.com/5288149/227817358-968e190f-4ead-4f5a-8de6-914860bca606.png)

The notches in the box plots mark 95% confidence intervals. Note that in GARCH models, the first step forecasts (day 0 of the next month) is deterministic, meaning that it has no confidence interval!  

# Future improvements and directions
There are some aspects of the project that can be improved or explored further in the future, here are some of them:

#### Modelling asymmetric responses:
The GARCH model assumes that the stock volatilities have symmetric response, meaning that the volatilities change symmetrically irrespective of whether the stock prices increase or decrease. In reality, stock volatilities tend to rise higher when the stock price drops than if it rises by an equal magnitude – the volatility index VIX tend to move up when the S&P 500 falls down. A more complicated model like the Glosten-Jagannathan-Runkle
(GJR)-GARCH can be used to model this effect. 

#### Modelling seasonal effects:
We saw hints of seasonality in the ASEA ETF for what might be seasonal effects with a seasonal lag=3. These kinds of behaviors can be modelled by creating a more complex mean-model.

#### More (or more localized) geographical samples:

#### More complex non-linear models:
XGBoost etc
