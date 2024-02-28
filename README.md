# Challenge Description:

## Overview

This repo is based on our team's submission for the groundwater time series modeling challenge. The goal of this challenge is to predict the groundwater level over a period of time using meteorological data. The constraint is that we are not allowed to use groundwater levels from previous time points to make predictions. The project was submitted in 2023, but I recently upraded our original code to include an LSTM model (still in progress), and also made our code more modular. 

There are three main notebooks:
- [Exploratory Data Analysis](Germany_EDA.ipynb)
- [Original ML model](Germany_prediction.ipynb)
- [LSTM model](Germany_prediction_LSTM.ipynb)


## Author(s)

- Xinyue (Selina) Wang (Brown University DSI)
- Yang Zheng (Brown University DSI)


## Location description

The well is situated in Bavaria, in the Upper Jurassic Malm Karst aquifer. It is a deep, confined aquifer (partly artisian), which is overlain by a local alluviual aquifer in a small river valley. Surface elevation is about 375 masl, depth to groundwater 0.9 m on average.





## Model workflow

### Overview:
- Using the input data, we created addional features, which include the mean value of each feature over the past 15, 20 and 90 days, as well as a 'day' feature that specifies the day of the month
- We used the MSE as our evaluation metric, and tried three different ML algorithms: Lasso regression, SVR, and random forest regressor
- We split the data into training and test sets, where observations in the test set always occur later in time than observations in the training set. We trained the model on the training set, predicted on the test set, and calculate an MSE score on the test set. We repeated this procedure for 5 iterations, each interation consists of a test set that is later in time than the previous test set, and a training set that includes all the datapoints that are earlier in time than the test set. This gives 5 different models and 5 different MSE test scores for each ML algorithm. We do these iterations to account for uncertainties in our model results, since some of the models we trained are deterministic, and there is also no randomness in a regular time-series split.
- In both locations (Germany and Netherlands), the SVR algorithm performed the best, in terms of having a low mean MSE score, low variance in the MSE scores as well as a relatively shorter run time
- We then use these 5 models to produce 5 predictions on the test data used for submission, and take the average of the 5 predictions as our final prediction. We take the square root of the mean of the 5 MSE scores as the RMSE of our prediction. The 95% prediction interval is calculated by adding and subtracting 1.96*RMSE to our final prediction.


### Python and package versions:
Python version 3.10.5\
numpy version 1.22.4\
matplotlib version 3.5.2\
sklearn version 1.1.1\
pandas version 1.4.2


## Supplementary information

### Input data description

The following input data are provided to model the head time series. This data were collected from the E-OBS dataset 
v25.0e at 0.1deg grid size.

- Daily Precipitation (RR) in mm/d.
- Daily mean temperature (TG) in degree Celsius.
- Daily minimum temperature (TM) in degree Celsius.
- Daily maximum temperature (TX) in degree Celsius.
- Daily averaged sea level pressure (PP) in hPa.
- Daily averaged relative humidity (HU) in %.
- Daily mean wind speed (FG) in m/s.
- Daily mean global radiation (QQ) in W/m2.
- Potential evaporation (ET) computed with Makkink in mm/d.

### Calibration and testing data

The head data are split into a training and testing period. Data are provided for the training/calibration period. Participants have to provide a simulated time 
series for the entire period:

- **Training period:** 2002-05-01 to 2016-12-31
- **Testing period:** 2017-01-01 to 2021-12-31

