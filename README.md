# Robotic Arm Control with Blynk

This project allows you to control a robotic arm using an ESP32, stepper motors, a servo motor, and an electromagnet, all integrated with the Blynk platform.

## Features
- Control two stepper motors for robotic arm movements.
- Adjust the servo motor's angle using a slider.
- Activate or deactivate the electromagnet using a button.
- Real-time control via the Blynk mobile app.

---

## Hardware Requirements
- ESP32 microcontroller
- 2 Stepper motors
- Servo motor
- Electromagnet
- Supporting components (e.g., motor drivers, jumper wires)

---

## Software Requirements
- Arduino IDE
- Blynk library
- ESP32Servo library

---

## Wiring Diagram

### Stepper Motors
| Pin | Function            | ESP32 Pin |
|-----|---------------------|-----------|
| IN1 | Stepper Motor 1     | D12       |
| IN2 | Stepper Motor 1     | D13       |
| IN3 | Stepper Motor 1     | D14       |
| IN4 | Stepper Motor 1     | D15       |
| IN1 | Stepper Motor 2     | D4        |
| IN2 | Stepper Motor 2     | D5        |
| IN3 | Stepper Motor 2     | D6        |
| IN4 | Stepper Motor 2     | D7        |

### Servo Motor
| Pin       | ESP32 Pin |
|-----------|-----------|
| Servo Pin | D0        |

### Electromagnet
| Pin             | ESP32 Pin |
|-----------------|-----------|
| Electromagnet   | D1        |

---

## Installation
1. Install the required libraries:
   - Blynk library
   - ESP32Servo library

2. Set up the Blynk mobile app:
   - Create a new project.
   - Add the following widgets:
     - **Button** for Stepper Motor 1 (V1).
     - **Button** for Stepper Motor 2 (V2).
     - **Slider** for Servo Motor (V3).
     - **Button** for Electromagnet (V4).

3. Obtain the Blynk Authentication Token from the app.

4. Update the following details in the code:
   ```cpp
   #define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
   #define BLYNK_TEMPLATE_NAME "YOUR_TEMPLATE_NAME"
   #define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"

   char ssid[] = "YOUR_WIFI_SSID";
   char pass[] = "YOUR_WIFI_PASSWORD";
   ```

5. Connect your hardware as per the wiring diagram.

6. Upload the code to the ESP32 using the Arduino IDE.

---
## Images of the Project
![WhatsApp Image 2025-01-12 at 17 49 42_f45e9170](https://github.com/user-attachments/assets/754a054d-2a92-436c-87da-0ed87df24cde)
![WhatsApp Image 2025-01-12 at 17 28 43_639b023d](https://github.com/user-attachments/assets/ceb7210a-be6f-4bd6-8d41-e786c81a0d7a)
![WhatsApp Image 2025-01-12 at 17 49 42_c8b4a143](https://github.com/user-attachments/assets/1986eaea-c05b-4d2d-a15e-d235c3e2c678)


---

## Code Explanation

### Libraries Used
```cpp
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <ESP32Servo.h>
```
- `WiFi.h`: Enables Wi-Fi functionality.
- `BlynkSimpleEsp32.h`: Integrates ESP32 with the Blynk platform.
- `ESP32Servo.h`: Allows control of the servo motor.

### Pin Definitions
```cpp
#define IN1_PIN D12
#define IN2_PIN D13
#define IN3_PIN D14
#define IN4_PIN D15
#define SERVO_PIN D0

#define IN1_PIN_2 D4
#define IN2_PIN_2 D5
#define IN3_PIN_2 D6
#define IN4_PIN_2 D7

#define ELECTROMAGNET_PIN D1
```
- Pins for stepper motors, servo motor, and electromagnet are defined for easy reference.

### Motor Control
Stepper motor control is achieved using the following sequence:
```cpp
const int steps[8][4] = {
  {1, 0, 0, 0}, {1, 1, 0, 0}, {0, 1, 0, 0}, {0, 1, 1, 0},
  {0, 0, 1, 0}, {0, 0, 1, 1}, {0, 0, 0, 1}, {1, 0, 0, 1}
};
```
The `moveStepMotor` function calculates the required steps and direction to move the motor.

### Blynk Control Functions
1. **Stepper Motor 1:**
   ```cpp
   BLYNK_WRITE(V1) {
     float targetAngle = param.asFloat();
     moveStepMotor(targetAngle, currentAngle, currentStep, true);
   }
   ```
2. **Stepper Motor 2:**
   ```cpp
   BLYNK_WRITE(V2) {
     float targetAngle = param.asFloat();
     moveStepMotor(targetAngle, currentAngle2, currentStep2, false);
   }
   ```
3. **Servo Motor:**
   ```cpp
   BLYNK_WRITE(V3) {
     int angle = param.asInt();
     myServo.write(constrain(angle, 0, 180));
   }
   ```
4. **Electromagnet:**
   ```cpp
   BLYNK_WRITE(V4) {
     int state = param.asInt();
     digitalWrite(ELECTROMAGNET_PIN, state == 1 ? HIGH : LOW);
   }
   ```

---

## Usage
- Open the Blynk app and connect to your ESP32.
- Use the buttons and slider to control the robotic arm.
- Monitor the serial output for feedback.

---

## Notes
- Ensure your Wi-Fi credentials and Blynk token are correctly configured.
- Adjust the `stepDelay` value if needed to fine-tune stepper motor performance.

---

## License
This project is open-source and licensed under the MIT License.

---
