# Performance_Attribution

The objective of performance attribution, as stated by Menchero (2000), is to explain portfolio performance relative to a benchmark, identify the sources of excess return, and relate them to active decisions by the portfolio manager. In other words, attribution measures which of the investment decisions about the portfolio’s underlying risks worked and which did not. This information is critical business intelligence for anyone involved in selecting, managing, or marketing investments. 

Currently, the attribution models work well for single periods with static data. No obvious way exists to combine attribution effects over time. To consider a probable solution to this problem, I spent some time working on performance attribution of Fixed Income (FI) and Equity managers using proxy data to decompose the returns and create an optimized portfolio.

Note, the objective of this attribution exercise was to develop a method of performance attribution that retains the simplicity and intuitive appeal at the same time assists in the predictability of future returns.

Step 1: Factor Gathering ( Equity and Fixed Income Factors)

A number of factors have persisted in the Fixed Income and Equity markets that can be categorized under two broader categories

Macro factors help explain risks and returns across asset classes. These are systematic, economy-wide sources of risk such as real rates and inflation.
Style factors help explain risks and returns within asset classes. They are characteristics, such as value and momentum, which explain the outperformance of certain securities relative to other securities in the same asset class.
For FI, the macro factors that I chose were - Bloomberg Barclays US Liquid Investment Grade Index, Merryl Lynch High Yield Index, and Yield Curve by tenor

Similarly for Equity: Quality Fator, Size Factor, Betting Against Beta, Market return and Momentum were selected under the style factor using AQR factors

Note, AQR daily factors were taken from AQR website. However, this data required data wrangling before collating them together to run a regression.

Step 2: Decomposing Fixed Income Factors

To understand how Fixed Income returns are affected by Shift, Flattening and Inversion of the Yield Curve, decomposing the daily yield curve data using Principal Component Analysis (PCA) was done.

Step 3: Factor Analysis

To avoid any bias in the model by random selection, I preferred to choose Time Series Split to create the cross-validation datasets and then set out a 30% test set aside to test my model predictions.

Sometimes, one may encounter an issue with overfitting based on test/train sets, this can be avoided by using Ridge Regularized Regression with cross-validation to find the model with the best lambda.

The next step was to understand the factors contributing to an individual's returns. Also to understand how my model was performing against the actual it was important to visualize actual return vs predicted return. Expectedly, the model result was well aligned with tight upper and lower bounds.

﻿Step 4: Portfolio Construction

I loaded monthly returns data across both asset classes (you can use daily returns as well for more data sets) and created Mean-Variance Optimized portfolio using two techniques:

Monte Carlo Simulation: 25000 simulations were run with all the managers monthly data to find the portfolio with max Sharpe ratio 
Quadratic Programming optimization: 10000 runs of the optimization were used to produce a portfolio with a relatively higher Sharpe ratio.
As the Quadratic Programming gave a much higher Sharpe ratio, I used the same to create proforma returns and model predictions in Step 5

Step 5: Portfolio Forecasting

Following the identification of the best portfolio using quadratic programming optimization, I tried to forecast its return in the future by creating various lagged returns (lag 3 to lag 10) and then fitting various models to forecast future returns based on lagged returns. 

It appeared that the correlation between lag was minimal - indicating Linear Models will be best suited for forecasting. Below were the models used :

Linear Regression: was used to predict the value of a dependent variable (returns) through best fit line. In my case, I found Mean Absolute Percentage Error high.

Ridge Regression: was used for analyzing multiple regression data that suffer from multicollinearity or multidimensionality, in this case. Model performance turned out to be better than Linear Regression but Mean Absolute Percentage still seemed high

Auto-Regressive Integrated Moving Average (ARIMA): was used to capture a suite of different standard temporal structures in time series data. This model performed better than the above two.

ARIMA predictions turned out to be very well aligned for historical returns.

Conclusion

Return attribution is an essential tool for all performance analysts that generate a true understanding of the sources of added and subtracted value in the portfolio, and allow the performance analysts to participate in the investment decision process and thus add value. 

With evolving Machine Learning techniques, a lot can be achieved in this space to benchmark the returns.

