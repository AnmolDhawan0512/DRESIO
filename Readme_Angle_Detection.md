## DRESIO

***Calculating Joint Angles***
For calculation of angle of a joint, we need 3 points, firstpoint, midpoint and lastpoint. Also, three axeses are needed, namely x,y,z for certain cases like shoulder abduction and shoulder flexion where the points are same but the angle changes based on axes. 

The **angle** is calculated by using:

#### CASE 1: When taking X and Y into consideration
```java
static double getAngle(PoseLandmark firstPoint, PoseLandmark midPoint, PoseLandmark lastPoint) {
     double result =
        Math.toDegrees(
            atan2(lastPoint.getPosition().y - midPoint.getPosition().y,
                      lastPoint.getPosition().x - midPoint.getPosition().x)
                - atan2(firstPoint.getPosition().y - midPoint.getPosition().y,
                      firstPoint.getPosition().x - midPoint.getPosition().x));
                      result = Math.abs(result); // Angle should never be negative
                      if (result > 180) {
      result = (360.0 - result); // Always get the acute representation of the angle
     }
      return result;
    }
```

#### CASE 2: When Y and Z are taken into consideration
```java
   static double getAngle(PoseLandmark firstPoint, PoseLandmark midPoint, PoseLandmark lastPoint) {
    double result =
            Math.toDegrees(
                    atan2(lastPoint.getPosition().z - midPoint.getPosition().z,
                            lastPoint.getPosition().y - midPoint.getPosition().y)
                            - atan2(firstPoint.getPosition().z - midPoint.getPosition().z,
                            firstPoint.getPosition().y - midPoint.getPosition().y));
    result = Math.abs(result); // Angle should never be negative
    if (result > 180) {
      result = (360.0 - result); // Always get the acute representation of the angle
    }
    return result;
  }
```

#### CASE 3: When taking X and Z into consideration
```java
static double getAngle(PoseLandmark firstPoint, PoseLandmark midPoint, PoseLandmark lastPoint) {
  double result =
        Math.toDegrees(
            atan2(lastPoint.getPosition().z - midPoint.getPosition().z,
                      lastPoint.getPosition().x - midPoint.getPosition().x)
                - atan2(firstPoint.getPosition().z - midPoint.getPosition().z,
                      firstPoint.getPosition().x - midPoint.getPosition().x));
  result = Math.abs(result); // Angle should never be negative
  if (result > 180) {
      result = (360.0 - result); // Always get the acute representation of the angle
  }
  return result;
}
``` 

#### CASE 4: When taking X, Y, Z into consideration
```java
static double getAngle(PoseLandmark firstPoint, PoseLandmark midPoint, PoseLandmark lastPoint) {
  double result =
        Math.toDegrees(
            atan2(lastPoint.getPosition().z - midPoint.getPosition().z,
                      lastPoint.getPosition().y - midPoint.getPosition().y,
                      lastPoint.getPosition().x - midPoint.getPosition().x)
                - atan2(firstPoint.getPosition().z - midPoint.getPosition().z,
                      firstPoint.getPosition().y - midPoint.getPosition().y,
                      firstPoint.getPosition().x - midPoint.getPosition().x));
  result = Math.abs(result); // Angle should never be negative
  if (result > 180) {
      result = (360.0 - result); // Always get the acute representation of the angle
  }
  return result;
}
```

Then we get the angles for all the 33 landmarks as in a human body which goes like this.
***Note:*** 
This is the angle for 6 joints namely: Elbow, Shoulder Flexion, Shoulder Abduction, Hip, Knees, Ankle
The computing goes as follows:

#### ANGLE 1: Hips
```java
double rightHipAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE)); 
 ```               
                `//Similar for leftHipAngle`
                
#### ANGLE 2: Elbows
```java
double rightElbowAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_WRIST),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER));
```  
             `   //Similar for leftElbowAngle`
                
#### ANGLE 3: Shoulder Flexions
```java
double rightShoulderFlexionAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
```       
             `   //Similar for leftShoulderFlexionAngle
                //Note, when using Shoulder Flexion it means that the X-AXIS should always be Null or ZERO. Any value added to it will be Shoulder Abduction.`
                
#### ANGLE 4: Shoulder Abductions
```java
double rightShoulderAbductionAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
```            
                //Similar for leftShoulderAbductionAngle
                //Note, when using Shoulder Abduction it means that the Z-AXIS should always be Null or ZERO. Any value added to it will be Shoulder Abduction.
                //Refer to the Angle Cases as mentioned above

#### ANGLE 5: Knees
```java
double rightKneeAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ANKLE),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
```            
                //Similar for leftKneeAngle
                
#### ANGLE 6: Ankles
```java
double rightAnkleAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_FOOTINDEX),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ANKLE),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE));
```            
                `//Similar for leftKneeAngle`

Now we know how to:
1. ***Calculate the angle*** (the axes into consideration over various exercises and/or movements)
2. Get the ***angles for the joints*** that are needed for us to design 3-5 exercises for Energize App.

We can therefore move to various exercises and their requirements.

***Note:***
1. Wherever "=" has been mentioned, it means that if the person passes that angle during exercise, we can increment the "rep counter".
2. For all the exercises there has to be 2 methods, One for LEFT and one for RIGHT.
3. **Case 1: Detection of Exercise that the person is going to perform: Take SHORT into consideration.**
4. **Case 2: Calculating angle to increment rep counter or inform person to add more angle to Exercise: Take LONG into consideration.**

| EXERCISES  | ANGLES USED | SHORT | LONG |
| ------------- | ------------- |------------- |------------- |
| Push-Up  | Shoulder Abduction<br/>Shoulder Flexion<br/>Hips<br/>Knees<br/>Elbow  | Shoulder Abduction = 10 <br/> Shoulder Flexion = 50 <br/> Hip >= 165<br/>Knees >= 175 <br/> Elbow = 175           |Shoulder Abduction = 45 <br/> Shoulder Flexion = 10 <br/> Hip >= 65<br/>Knees >= 175 <br/> Elbow = 80           |
| Squats  | Hip<br/>Knee<br/>Ankle  | Hip = 165 <br/> Knee = 175 <br/> Ankle = 85 | Hip = 70 <br/> Knee = 70 <br/> Ankle = 80 |
| Bicep Curl  | Shoulder Flexion <br/> Elbow  | Shoulder Flexion <= 30 <br/> Elbow = 30 | Shoulder Flexion = 0 <br/> Elbow = 160 |
| Leg Raise  | Knees <br/> Hip  | Knees >= 160 <br/> Hips = 100 | Knees >= 160 <br/> Hips = 160 |
| Lateral Raise  | Elbow <br/> Shoulder Abduction  | Elbow >= 170 <br/> Shoulder Abduction = 80 | Elbow >= 170 <br/> Shoulder Abduction = 5 |

With the information above we have enough data about:
1. How to ***calculate angle.***
2. What are ***various angles*** needed for an exercises.
3. What are the ***placemarks needed*** for those angles to be calculated.
4. What are the angles needed to ***detect a posture*** for exercise.
5. What are the ***threshold to increase the repetition counter or motivate user to do more.***

We can simply create methods for various exercises. 

# In Java - PseudoCode

### For calculating the threshold to increase the repetition counter or motivate user to do more.
```java
public void performSquatRight(){
 if(rightShoulderAbductionAngle==45 && rightShoulderFlexionAngle==10 && rightHipAngle>=165 && rightKneeAngle>=175 && rightElbowAngle==80){
     System.out.println("Good Job"); 
          //Various marks can be added for excellent or good job or try more etc.
          //Use Switch Case for the same.
     repcounter++;
     }
 else
     System.out.println("Put your " + placemark + "to an angle of 165");
}
```
          `//Note: This was for performance from Right Joints, Same for left is needed.
           //When performSquatRight()&&performSquatLeft==True --> repcounter++`

### For detecting the exercise that the user is going to perform.
```java
public void detectSquatRight(){
 if(rightShoulderAbductionAngle==10 && rightShoulderFlexionAngle==50 && rightHipAngle>=165 && rightKneeAngle>=175 && rightElbowAngle==175){
     System out.println("Okay! Squats!");
     }
```
          `//Note: This was for detection from Right Joints, Same for left is needed.
           //When detectSquatRight()&&detectSquatLeft==True --> Detection Confirmed`
