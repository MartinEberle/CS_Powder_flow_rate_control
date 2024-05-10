# CS_Powder_flow_rate_control 
## User interface overview
The dashboard is divided into two main windows. In the top window the user can select the filter settings, and the powder feeders in use. The data recording can be started and stopped using the respective buttons (Start and Stop). 
Previous data recording can be deleted using the “Reset” button. Raw data can be saved clicking the “Save” button. In the bottom window the powder flow rate is displayed live in the form of a scatterplot, where the y-axis (vertical) represents the powder flow rate in kg/hr and the x-axis (horizontal) represents the time represented in seconds.

# Technical Specifications
## Data acquisition pathway
In the present version the program uses simulated scale weight data that represent the weight of powder in two powder feeders used for cold spray additive manufacturing. 
The start weight for powder feeder 1 is set to 3 kg and the start weight for powder feeder 2 is set to 4 kg. 
The program adjusts the respective starting weight iteratively by subtracting a random number within the range (-0.0001 kg to 0.0003 kg), creating variations in the generated values. 
The negative value of -0.0001 kg is selected to simulate erroneous scale weight readings. For each reading a time stamp is taken by the software. 

## Data processing
Incoming data consists of the two scale weight readings for powder feeder 1 and 2 and the time stamp taken for each reading. 
The timestamp values are converted into a formatted string representing the time in the "hour:minute:second" format (e.g., "14:30:45"). Each data point is saved in an array. 

### Raw data filter
**First raw data filter**
Data array is filtered out if:
* The difference between previous reading and current reading for a selected scale weight (1, 2 or 1+2) is smaller than 0.
* The current reading for a selected scale weight is smaller than 0.
* The time stamps of the previous reading and current reading are the same.

**Second raw data filter**
Based on a user-selected Window size NR the average and standard deviation of the last NR recorded scale weight values are calculated. The calculated standard deviation is weighted with a user-defined Standard deviation factor. 
If the difference between the calculated average of the last NR scale weight readings and the current weight scale reading exceeds the weighted standard deviation, the current scale weight reading is discarded.

### Moving average calculation
Base on the filtered raw data a moving average is calculated as follows:
Based on a user-selected Window size NM the average of the last NM scale weight values is calculated. 
If the difference between consecutively calculated moving averages exceeds a user defined Max allowed change, the most recent (current) calculated moving average is filtered out.

### Powder feeder flow rate calculation
The powder flow rate in kg/hr is determined based on the user specified window size NPF for the calculated moving averages using the following formula:
Powder flow rate = (〖MA〗_i-〖MA〗_(i+Npf))/(T_i- T_(i+Npf) )  ∙3600 s/hr
Where: 
* MAi: moving average calculated at position i in kg
* MAi+NPF: moving average calculated at position i+NPF in kg
* Ti: time stamp at position i in seconds
* Ti+NPF: time stamp at position i + NPF in seconds




