# Accelerometer_Sleep_Predictions

<img src="sleeping_cartoon.jpg" alt="Zzzzzzzz..." style="width: 100%; height=1px">

### Overview
The purpose of this project was to predict, as precisely as possible, the exact moment that a person fell asleep or woke up given certain biometric information. In this project, I analyzed data provided by the [Healthy Brain Network](https://healthybrainnetwork.org/) containing measurements from a wrist-worn accelorometer. This data had tracking measurements of the wearer's activity (enmo) and the angle at which their arm was positioned (anglez). These measurements were updated in 5-second intervals. 

### Data
The data included the accelerometer records from 277 different study participants. Because there were incremental 5 second observations, each day contained 17280 observations. Typically, for all of those observations there would only be 1 incident of "onset" - falling asleep - and 1 incident of "wakeup" - waking from sleep. Not all of the participants wore the accelerometers consistently, and several also had a mismatched number of onsets and wakeups. Ultimately, those people with asymetrical counts of wakeup and onset were excluded from the modeling data, leaving 269 total participants worth of data. 

![This is an example of one user's daily accelerometer data](sleep_record.png)

As you can see in the image above, the measurements from the accelerometer look quite different when the subject is awake or asleep. I engineered several additional rolling window features based on this accelerometer data to provide more signal for models to latch onto. 

### Evaluation
This project's predictions were challenging to generate. The models that were trained generated binary predictions for the states of "Awake" or "Asleep". These predictions were then filtered to find the first instance of Awake/Asleep that came after its counterpart, i.e. the first "Awake" prediction after a prolonged period of "Asleep" predictions. Per the experiment guidelines, this change from Awake/Asleep also needed to be separated by a period of at least a half-hour. These "change-of-state" predictions, if you will, predicted the timestamp of the event, and finally, that prediction was evaluated for accuracy over different windows of time. For example, if the model predicted sleep onset at 10 pm and `TRUE ONSET` was actually 11 pm, then the prediction would be incorrect for the time window of 30 minutes, but it would be correct for the time window of anything >1 hour. The tolerance windows used in evaluation were: 1, 3, 5, 7.5, 10, 12.5, 15, 20, 25, and 30 minutes. 

### Conclusion

