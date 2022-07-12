This project calculates of a given dataset the spikes. The spike calculation is based on linear regression filter sliding over the dataset. 
The windowsize and accuracy (read sensitivity) can be set, even as the begin and end array, which needs to be ignored.
Since the anamolies are most of the time facts from the past and seldom only at the end of the dataset, the spike is calculated as the midpoint of the window. 
This makes the linear regression more precise. Bigger spikes can be noticed by setting P1. 
This P1 defines a spike in case suddenly the standard deviation increases a factor P10 of the data in the analyzed window

The output of the Pyhton script contains a lot of paramters of the dataset and also whether the given datapoint y on x is identified as a spike. 
Whether the algorithm identifies a point on x as an anomaly depends on the windowsize and the accuracy. 
A bigger windowsize creates a polynoom which deviates more from the real dataset, and makes the result less sentitive to spikes. 
This can be mitigated by reducing the accuracy. So a small windowsize and a big number on accuracy makes the script insensitive for spikes.
This means that the algorithm must be tuned with these hyperparameters.

Next info and code will follow soon
# -*- coding: utf-8 -*-
"""
Created on Tue Jul 12 10:19:20 2022

@author: Fred Coerver

This module takes a pandas frame as input, together with several hyperparameters. 
The pandas data frame must have a straight forward x and y column. Not morem Not less.
The output are two pandas dataframes.
1) table with statistal information of the input and whether a value x is identified as a spike or dip in the
dataset, based on the input parameters. The algorithm makes a window of datapoints from x - windowsizeleft to  x + windowsizeright
Linear regression is performed on this x-range with the give y-values. The regression value : y = slope*x+intercept
This window is iterated over the pandas frame and the regression values are put into the output table, along with the other statistical values
Input dataframe structure :
    1) pandas index as int
    2) x or DateTime value format %Y-%m-%d %H:%M:%S {working on an update that also accepts floats as x}. 
    Columnname must be "DateTime" and must be next/right to the index
    3) y {float or int}. Columnn name is free. 

The dataframe can have more columns, but the script only analyzes 1 columns at a time.
So in in case you want to analyze more columns, you need to call the script for each column and slicing 
the dataframe in a way that the input frame complies to 1), 2) and 3)

Hyperparameters
ignore_startsamples = 5 (in case you want to omit starting rows from your calculation of the dataset. 
                         This parameter does not delete the rows! = default is 5. 
                         In case you increase the windowsizeleft, you might need to increase this value as well)
ignore_endsamples = 3 (in case you want to omit starting rows from your calculation of the dataset. 
                         This parameter does not delete the rows! 
                         In case you increase the windowsizeright, you might need to increase this value as well)

defaults:
P1inc=10 ()                  ---> In case the standard deviation of a regression window is P1inc-times higher as the previous std, 
                                  then this x value is marked as a spike
accuracy = 4                 ---> If a y-value exceeds the regressed value +- accuracy * std then this x-value is marked as spike
window_sizeleft = 3          ---> the regresssionwindow is #windowsizeleft x-values from the analyzed x-point and #windowsizeright x-values
                                  from the analyzed x-point. So suppose the algorythm calculates whether x[j] is a spike, 
                                  then the window x[j-windowsizeleft] until x[j+windowsizeright] is considerd as the window of x-points.
                                  From these x- an y-point the regression is calculated, and the regression value is compared with the actuel y-value
                                  
window_sizeright = 3         ---> amount of datapoint right from the x-value, which is subject for analyzing whether it is a spike/dip or not
ignoreendsamples = 3         ---> the Endresult output dataframe is capped with [ignorestartsamples:-ignoreendsamples] before returning to main


"""
