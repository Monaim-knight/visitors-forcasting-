# Website Visitors Time Series Analysis: Interpretation Report

## 1. Data Preparation & Visualization
The time series plot of daily website visitors shows the overall trend and any repeating weekly patterns. In this dataset, there is a clear weekly seasonality, as well as some variation in daily counts. No major outliers are present, indicating the data is clean and suitable for analysis.

## 2. Dickey-Fuller Test (Stationarity)
**Output:**
```
Augmented Dickey-Fuller Test

data:  ts_data
Dickey-Fuller = -4.1385, Lag order = 7, p-value = 0.01
alternative hypothesis: stationary
```
**Interpretation:**
The ADF test statistic is -4.1385 with a p-value of 0.01. Since the p-value is less than 0.05, we reject the null hypothesis that the time series is non-stationary. This means the website visitors data is stationary, and its statistical properties do not change over time. This is ideal for ARIMA modeling, and no further differencing is required.

## 3. ACF and PACF Plots
The ACF and PACF plots help identify autocorrelation and the appropriate lag values for ARIMA modeling. In this dataset, the ACF shows significant spikes at lag 1 and at multiples of 7, indicating both short-term and weekly seasonality. The PACF plot shows a strong spike at lag 1, suggesting an autoregressive component. These patterns support the use of a seasonal ARIMA model.

## 4. ARIMA Model Fitting
**Output:**
```
Series: ts_data 
ARIMA(1,0,2)(1,0,1)[7] with non-zero mean 

Coefficients:
         ar1      ma1     ma2    sar1     sma1      mean
      0.8007  -0.1461  0.1837  0.9088  -0.8820  1015.000
s.e.  0.0449   0.0661  0.0589  0.1232   0.1361    24.777

sigma^2 = 5548:  log likelihood = -2088.87
AIC=4191.75   AICc=4192.06   BIC=4219.05

Training set error measures:
                     ME     RMSE      MAE        MPE     MAPE      MASE         ACF1
Training set 0.09552032 73.86871 58.58829 -0.5417068 5.871935 0.5131152 -0.001301231
```
**Interpretation:**
- The best model selected is ARIMA(1,0,2)(1,0,1)[7] with non-zero mean, capturing both short-term and weekly seasonal patterns.
- The coefficients are statistically significant, and the error metrics (RMSE, MAE, MAPE) are low, indicating a good fit.
- The mean value (1015) represents the average daily visitors.
- The model is well-suited for forecasting future website traffic.

## 5. Conclusion
This analysis demonstrates the ability to handle real-world time series data, test for stationarity, select and fit an appropriate ARIMA model, and interpret the results. The model provides actionable insights for predicting future website visitors, which can help with planning and decision-making for website management and growth. 

## 6. Troubleshooting: Data Loading Error and Solution

**Error Encountered:**
When attempting to load the CSV file using `read_csv()`, the following error occurred during data preparation:

```
Error in `$<-`:
! Assigned data `as.Date(data$date)` must be compatible with existing data.
✖ Existing data has 365 rows.
✖ Assigned data has 0 rows.
ℹ Only vectors of size 1 are recycled.
Caused by error in `vectbl_recycle_rhs_rows()`:
! Can't recycle input of size 0 to size 365.
```

**Cause:**
This error was due to the CSV file being space-separated rather than comma-separated. The `read_csv()` function (from the `readr` package) expects comma-separated values by default, so it did not correctly parse the columns, resulting in empty or misnamed columns.

**Solution:**
To resolve this, the data was loaded using `read.table()` with the default separator (which handles space-separated files), as shown below:

```r
# Correct way to load the data
 data <- read.table("../data/3_webtraffic.csv", header = TRUE)
```

Alternatively, `read_delim()` from the `readr` package could be used with `delim = " "` to specify a space as the separator:

```r
# Alternative solution
 data <- read_delim("../data/3_webtraffic.csv", delim = " ")
```

**Outcome:**
After making this change, the data loaded correctly and the analysis proceeded without further issues. This demonstrates the importance of checking data formats and adapting code to handle real-world data challenges. 