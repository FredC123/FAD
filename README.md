INSTALLATION:
copy fad_v02.py to your anaconda3 ./Anaconda3/Lib directory
copy Test_FAD_v01.py to your working environment directory and the same for the example excel files.
!!!!!!!!!!!!!!!!!! Open Test_FAD_v01.py and read the description !!!!!!!!!!!!!!

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

More information is included in the attached Worddocument
