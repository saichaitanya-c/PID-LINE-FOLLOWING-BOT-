# PID-LINE-FOLLOWING-BOT


**Line-Following Robot**:

A robot that uses sensors to detect a line on the ground and follows it by adjusting its movement. The line is usually a dark strip on a lighter surface or vice versa.


**PID Controller**:

A control system mechanism used to correct errors in a system by calculating three components: Proportional, Integral, and Derivative. This helps in reducing the difference between a desired setpoint (in this case, following the line) and the actual position of the robot.


This code is designed to control a line-following robot using a PID (Proportional-Integral-Derivative) controller. The robot is equipped with two infrared (IR) sensors that detect the presence of a line on the ground, and two servo motors that control the movement of the robot.



https://github.com/user-attachments/assets/1d4ab062-9dff-41eb-ac13-071c1bcc9d93


### Theory of Operation

1. **Sensor Inputs**:
   - The robot has two IR sensors placed on either side of the line it needs to follow. The sensors detect whether they are over the line (`1`) or off the line (`0`).
   - **SENSOR1_PIN** and **SENSOR4_PIN** are the pins connected to these IR sensors. The sensors' readings are used to calculate the robot's error, which indicates how far off the robot is from the center of the line.

2. **Error Calculation**:
   - The error is calculated as the difference between the readings of the two sensors: `error = sensor4 - sensor1`.
   - If the error is zero, it means both sensors are either over the line (indicating the robot is centered) or both are off the line (which typically shouldn't happen, as it indicates the robot is completely off course).
   - A positive error means the robot is veering to one side, and a negative error means it's veering to the other side.

3. **PID Components**:
   - **Proportional (P)**: This component directly corrects the error. It is the primary factor that adjusts the robot's speed based on the current error. If the error is large, the correction is large, and vice versa.
   - **Integral (I)**: This component considers the accumulation of past errors. It helps to eliminate residual steady-state error that the proportional component alone cannot correct. The integral term is accumulated over time but is limited by the `integralMax` and `integralMin` values to prevent "integral windup."
   - **Derivative (D)**: This component predicts the future trend of the error by considering the rate of change of the error. It helps to smooth out the control by dampening the response to rapid changes.

4. **PID Calculation**:
   - The PID value is calculated using the formula: 
     \[
     \text{pidValue} = (Kp \times \text{error}) + (Ki \times \text{integral}) + (Kd \times \text{derivative})
     \]
   - This value determines how much correction needs to be applied to the robot's speed to bring it back on track.

5. **Motor Speed Adjustment**:
   - The robot's movement is controlled by two servo motors. The speed of these motors is adjusted based on the PID value.
   - The left motor speed is increased by the PID value, and the right motor speed is decreased by the PID value. This differential steering allows the robot to turn in the direction of the line when it detects an error.

6. **Movement Control**:
   - The `moveForward()` function adjusts the servo angles based on the calculated speeds. The servos are controlled by writing an angle to them, where `90` is the neutral position. The robot turns by adjusting the angles away from `90`.

7. **Continuous Feedback Loop**:
   - The `loop()` function continuously reads the sensor values, calculates the error, updates the PID components, and adjusts the motor speeds. This loop allows the robot to react to changes in the line's position and keep following it.

### Summary

In summary, this code makes the robot follow a line by using sensor data to calculate how far off the robot is from the line (error) and then adjusting the motor speeds using a PID controller. The PID controller helps the robot make smooth corrections and maintain its path on the line. The proportional, integral, and derivative components each play a specific role in ensuring the robot follows the line accurately and efficiently.
