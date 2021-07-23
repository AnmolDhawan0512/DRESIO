# DRESIO
Calculating Joint Angles 
For calculation of angle of a joint, we need 3 points, firstpoint, midpoint and lastpoint. Also, three axeses are needed, namely x,y,z for certain cases like shoulder abduction 
and shoulder flexion where the points are same but the angle changes based on axes. 

The angle is calculated by using:
CASE 1: When taking x and y into consideration

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

CASE 2: When taking y and z into consideration
`static double getAngle(PoseLandmark firstPoint, PoseLandmark midPoint, PoseLandmark lastPoint) {
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
}`

CASE 3: When taking x and z into consideration
`
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
`
CASE 4: When taking x,y,z into consideration
`static double getAngle(PoseLandmark firstPoint, PoseLandmark midPoint, PoseLandmark lastPoint) {
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
`
Then we get the angles for all the 33 landmarks as in a human body which goes like this.
Note: This is the angle for 6 joints namely: Elbow, Shoulder Flexion, Shoulder Abduction, Hip, Knees, Ankle
The computing goes as follows:

ANGLE 1: Hips
`double rightHipAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE)); 
 `               
                `//Similar for leftHipAngle`
                
ANGLE 2: Elbow
`double rightElbowAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_WRIST),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER));
            `
             `   //Similar for leftElbowAngle`
                
Angle 3: Shoulder Flexions
`double rightShoulderFlexionAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));
            `
             `   //Similar for leftShoulderFlexionAngle
                //Note, when using Shoulder Flexion it means that the X-AXIS should always be Null or ZERO. Any value added to it will be Shoulder Abduction.`
                
Angle 4: Shoulder Abduction
`double rightShoulderAbductionAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ELBOW),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_SHOULDER),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));`
            
                //Similar for leftShoulderAbductionAngle
                //Note, when using Shoulder Abduction it means that the Z-AXIS should always be Null or ZERO. Any value added to it will be Shoulder Abduction.

Angle 5: Knees
`double rightKneeAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ANKLE),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_HIP));`
            
                //Similar for leftKneeAngle
                
Angle 6: Ankle
`double rightAnkleAngle = getAngle(
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_FOOTINDEX),   
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_ANKLE),
                pose.getPoseLandmark(PoseLandmark.Type.RIGHT_KNEE));`
            
                //Similar for leftKneeAngle
