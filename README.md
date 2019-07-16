# NYC Taxi Trip Data Science Project

![picture alt](http://i.huffpost.com/gen/3118218/images/o-NEW-YORK-TAXI-facebook.jpg)


# Introduction
The New York City yellow taxi cabs are one of the most distinguishing symbols of the city that never sleeps. While there are numerous ways to get around the city, perhaps the most convenient way is via the taxi cabs. It gives people a chance to take a breather, sit back, relax, and use their phone for recreational or professional purposes. 

# Motivation
In a city that is always cluttered with millions of pedestrians, cars, public transit, and others, it would be beneficial to understand just how long you can expect your trip from point A to point B to take. Given various data about geolocation, trip duration, and the weather, it would be beneficial for both the taxi driver and the passengers to know when and where the taxi demand would be high, and also to know how long a trip can be expected to take. 

# Overview

The initial data cleaning/wrangling work can be found in the notebooks folder. The initial findings from the data analysis can also be found in the notebooks folder. 

The models folder contains several jupyter notebooks: 

1. Clustering notebook, which uses KMeans Clustering (with 15 clusters) to group the trips by their pickup location coordinates. These coordinate labels will later be used in other models as predictor variables. 
2. The ride duration prediction notebook, which uses both simple linear regression and XGBoost models. The scoring metric here will be judged by the Root Mean Log Squared Error, meaning that the predictions will be made on the log(trip_duration), and the scale will be in log-seconds.
3. Time Series Analysis notebook, which uses ARIMA and Seasonal ARIMA models to forecast taxi ride demand in NYC. 
4. Neural Network notebook, which contains both simple sequential neural networks and Long Short Term Memory (LSTM) network to forecast taxi ride demand in NYC.

# Findings

**Predicting Taxi Trip Duration (in log-seconds)**

| Model | Root Mean Log Squared Error | Hyperparameters |
| :---         |     :---:      |          ---: |
| Linear Regression  | 0.518     | n/a    |
| XGBoost : reg-linear     | 0.384       | xgb_pars (in documentation)     |

- The most important features in determining taxi trip durations are: distance, direction, day of week (Sunday) and pickup cluster (14 -midtown area).

**Taxi Ride Demand - Time Series Forecasting (in # of rides)**

| Model | Forecasting RMSE | Hyperparameters |
| :---         |     :---:      |          ---: |
| SARIMAX   |  490    | p,d,q,s = (1,0,1,7), weekly detrended    |
| Basic Neural Network     | 448       | 2 layers, 50 neurons, ReLu activation, 20 epochs    |
| LSTM    | 527       |  50 neurons, 40 epochs, batch size = 1  |

- The basic neural network had an out of sample forecasting root mean squared error of 448 over the course of 15 days. The basic neural network also properly modelled the weekly seasonal trend of the time series. This was all done without detrending the original dataset before feeding it in as an input. The LSTM seems to need more data observations to be an effective forecasting tool. It may also just be that the number of taxi trips taken in a day aren't very dependent on the values of taxi rides of previous days (making the use of LSTM unnecessary).

<p align="center">
  <img src="https://user-images.githubusercontent.com/49466466/61259711-5694f580-a7b6-11e9-83ad-0da60ae5ece9.png" width="600" height="300">
</p>

# Future Work
Other notable data can be wrangled to add more meaning or increase accuracy to the ride duration prediction models. An example might be finding data on if there were major events held in the city at some time (i.e at MSG or some other venue). This may very well cause traffic delays and jams. We could also collect more historical data about the number of taxi rides in previous years, so that our time series models can be detrended on an annual level (it may help with weather related seasonality changes as well). 

