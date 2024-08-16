#include <Servo.h>

#define SENSOR1_PIN 11
#define SENSOR4_PIN 10

#define LEFT_SERVO_PIN 12
#define RIGHT_SERVO_PIN 13

#define BASE_SPEED 6 // Base motor speed 5

Servo leftServo;
Servo rightServo;

// PID constants
const float Kp = 3.1;  // Proportional gain 3.1
const float Ki = 0.7;  // Integral gain     0.7
const float Kd = 1.6;  // Derivative gain   1.6

float error = 0;
float previousError = 0;
float integral = 0;
float derivative = 0;

// Integral windup limits
const float integralMax = 40;
const float integralMin = - 40;

void setup() {
  leftServo.attach(LEFT_SERVO_PIN);
  rightServo.attach(RIGHT_SERVO_PIN);

  Serial.begin(9600);

  pinMode(SENSOR1_PIN, INPUT);
  pinMode(SENSOR4_PIN, INPUT);

  moveForward(BASE_SPEED, BASE_SPEED);
}

void loop() {
  int sensor1 = digitalRead(SENSOR1_PIN);  // Read digital sensor 1
  int sensor4 = digitalRead(SENSOR4_PIN);  // Read digital sensor 4

  Serial.print("Sensors: ");
  Serial.print(sensor1); Serial.print(" ");
  Serial.println(sensor4);

  // Calculate error: difference between sensor readings
  error = sensor4 - sensor1;

  // Calculate integral
  integral += error;
  // Apply integral windup guard
  if (integral > integralMax) {
    integral = integralMax;
  } else if (integral < integralMin) {
    integral = integralMin;
  }

  // Calculate derivative
  derivative = error - previousError;

  // Print PID components to Serial Monitor
  Serial.print("Error: ");
  Serial.print(error);
  Serial.print(", Integral: ");
  Serial.print(integral);
  Serial.print(", Derivative: ");
  Serial.println(derivative);

  // Calculate PID value
  float pidValue = (Kp * error) + (Ki * integral) + (Kd * derivative);

  // Adjust motor speeds based on PID value
  float leftSpeed = BASE_SPEED + pidValue;
  float rightSpeed = BASE_SPEED - pidValue;

  moveForward(leftSpeed, rightSpeed);

  // Save the current error as previous error for the next loop
  previousError = error;

  delay(50);
}

void moveForward(float leftSpeed, float rightSpeed) {
  leftServo.write(90 - leftSpeed);
  rightServo.write(90 + rightSpeed);
}

void stopMotors() {
  leftServo.write(90);
  rightServo.write(90);
}
