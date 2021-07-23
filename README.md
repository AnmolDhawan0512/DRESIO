# DRESIO
Calculating Joint Angles 
For calculation of angle of a joint, we need 3 points, firstpoint, midpoint and lastpoint. Also, three axeses are needed, namely x,y,z for certain cases like shoulder abduction 
and shoulder flexion where the points are same but the angle changes based on axes. 

The angle is calculated by using:
CASE 1: When taking x and y into consideration
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
CASE 2: When Y and Z are taken into consideration
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
CASE 3: When taking x and z into consideration
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

CASE 4: When taking x,y,z into consideration
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
Note: This is the angle for 6 joints namely: Elbow, Shoulder Flexion, Shoulder Abduction, Hip, Knees, Ankle
The computing goes as follows:

ANGLE 1: Hips
```java
double rightHipAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE)); 
 ```               
                `//Similar for leftHipAngle`
                
ANGLE 2: Elbow
```java
double rightElbowAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_WRIST),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER));
```  
             `   //Similar for leftElbowAngle`
                
Angle 3: Shoulder Flexions
```java
double rightShoulderFlexionAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
```       
             `   //Similar for leftShoulderFlexionAngle
                //Note, when using Shoulder Flexion it means that the X-AXIS should always be Null or ZERO. Any value added to it will be Shoulder Abduction.`
                
Angle 4: Shoulder Abduction
```java
double rightShoulderAbductionAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
```            
                //Similar for leftShoulderAbductionAngle
                //Note, when using Shoulder Abduction it means that the Z-AXIS should always be Null or ZERO. Any value added to it will be Shoulder Abduction.
                //Refer to the Angle Cases as mentioned above

Angle 5: Knees
```java
double rightKneeAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ANKLE),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
```            
                //Similar for leftKneeAngle
                
Angle 6: Ankle
```java
double rightAnkleAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_FOOTINDEX),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ANKLE),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE));
```            
                `//Similar for leftKneeAngle`

Now we know how to:
1. Calculate the angle (the axes into consideration over various exercises and/or movements)
2. Get the angles for the joints that are needed for us to design 3-5 exercises for Energize App.

We can therefore move to various exercises and their requirements.

Note:
1. Wherever "=" has been mentioned, it means that if the person passes that angle during exercise, we can increment the "rep counter".
2. For all the exercises there has to be 2 methods, One for LEFT and one for RIGHT.
3. CASE 1: DETECTION OF EXERCISE THAT THE PERSON IS GOING TO PERFORM: Take SHORT into consideration. 
4. CASE 2: CALCULATING ANGLE TO INCREMENT REP COUNTER or INFORM PERSON TO ADD MORE ANGLE TO EXERCISE: Take LONG into consideration.

| Exercise  | Angles Used | Short | Long |

| ------------- | ------------- |------------- |------------- |

| Push-Up  | Shoulder Abduction<br/>Shoulder Flexion<br/>Hips<br/>Knees<br/>Elbow  | <br/> Shoulder Abduction = 10 <br/> Shoulder Flexion = 50 <br/> Hip >= 165<br/>Knees >= 175 <br/> ELbow = 175           |<br/> Shoulder Abduction = 45 <br/> Shoulder Flexion = 10 <br/> Hip >= 65<br/>Knees >= 175 <br/> ELbow = 80           |

| Content Cell  | Content Cell  |             |             |
