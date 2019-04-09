# Use OpenCV and dlib to alert sleepy driver
## https://medium.com/@patrickhk/use-opencv-and-dlib-to-alert-sleepy-driver-8872375772e

To build a drowsiness detector to alert sleepy driver, we need nothing fancy but OpenCV and dlib.

### Lets brainstorm what should be done to build the detector
1. How to run it in real time?
2. How to define sleepy or drowsiness?
3. How to capture sleepy/drowsiness feature and respond to it?
We can obtain streaming frame from our webcam/IP cam/camera sources.

### How to define and capture sleepy or drowsiness
Imagine what would a sleepy driver do? When people are sleepy, they tend to slowly closing their eyes and head down. If we can capture the change of “size” of their eyes, we maybe able to tell if the driver is sleepy.

### Eye Aspect ratio(EAR)
We can use Eye Aspect ratio(EAR) to capture the “size” I mentioned above.
![ear](https://cdn-images-1.medium.com/max/800/1*dm1whASgAis4hcumBRN_bg.png)
EAR is roughly equal eh/ew. When we are falling to sleep, our eyes will close gradually and eh will decrease while ew wont change much. As a result the EAR value will be lowered. Therefore by setting a threshold value for the Eye Aspect ratio magnitude, we can tell if the driver is sleepy. However normally will blink their eyes occasionally, we need another threshold value as to distinguish if the driver is just blink his eyes or really falling to sleep.

Lets say threshold Eye Aspect ratio magnitude is 0.25 and threshold counter value is 35. If the captured Eye Aspect ratio is lower than 0.25, it will trigger the counter to increase one. If the Eye Aspect ratio is lower than 0.25 for 35 times continuously, will trigger the final alarm.

### How to capture the Eye Aspect ratio through camera
We can use the facial landmark detector from dlib to locate and capture the all the landmark information of the driver. Then we convert these information into x,y coordinate and extract particularly the left and right eyes. Then we can calculate the Eye Aspect ratio of each eyes and take average.

### What to do next if the driver is sleepy?
Play a loud music / alarm ringtone to scare him!

### Result
I don’t have a car so I just pretend I am a sleepy driver. Normal blinking eyes wont trigger the alert. The alert will be triggered only if I start falling sleep. Can adjust the threshold value to adjust the sensitivity of the detector
https://youtu.be/ovw8XbMeDPM

### Discussion
1. The FPS of my detector is maintained around 19–25fps, depending on hardware. For me this fps is smooth enough. But imagine if a driver is driving super fast like at 100km/h, this fps maybe not enough.
2. What happen if the driver wear a sun glasses? The detector will be an epic fail because we can’t capture the Eye Aspect ratio. We should develop another metrics, such as monitoring the head movement of the driver. But in practical, how many drivers will fall asleep under bright sunlight?
3. What happen if the driver have small eyes. Honestly this will really affect the result (I try to stimulate this situation). The solution is to fine tune the threshold value of the Eye Aspect ratio.
4. What happen if the driver wearing colored contact lens? It wont affect as long as dlib landmark detector can detect the eyes.
5. To improve the detector, we can assign more threshold Eye Aspect ratio to classify the drowsiness of the driver. More several drowsiness comes with louder alarm or integrate with the car speed control system.
