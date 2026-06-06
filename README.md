# Smart Door Lock System - Code Alpha Task 2 🚪🔒

**Author:** Raj Narayan Gaur  
**Domain:** Internet of Things (IoT) & Embedded Systems  
**Company:** Code Alpha  

## 📝 Project Overview
This repository contains the code and documentation for Task 2: a Password-Based Smart Door Lock System built using **Arduino Uno**. This system automates a door-locking mechanism using a 4x4 Matrix Keypad for password entry and a Servo motor as the physical lock actuator, enhancing security through embedded programming.

## 📸 Project Circuit & Setup Images
*(Niche diye gaye link ko apni photo ke link se replace karein)*

<img width="1361" height="916" alt="Image" src="https://github.com/user-attachments/assets/59b93250-f30b-4717-9a45-6aa250652620" />
## 🎥 Working Video
**[👉 Click Here to Watch the Working Video of Smart Door Lock System](YAHAN_VIDEO_KA_LINK_PASTE_KAREIN)**

## 🛠️ Components Used
* **Microcontroller:** Arduino Uno
* **Actuator:** SG90 Servo Motor
* **Input Module:** 4x4 Matrix Keypad
* **Connecting Medium:** Jumper Wires & Breadboard
* **Power Source:** 9V Battery / USB Cable

## 💻 Arduino Uno Program (Keypad + Servo)
*Note: Make sure to install the `Keypad.h` library in your Arduino IDE before uploading this code.*

```cpp
/*
  Project: Password-Based Smart Door Lock System
  Author: Raj Narayan Gaur
  Internship: Code Alpha (Task 2)
  Device: Arduino Uno
  Components: 4x4 Matrix Keypad, SG90 Servo Motor
*/

#include <Keypad.h>
#include <Servo.h>

Servo doorLock;
const int servoPin = 9; // Servo motor signal pin connected to Pin 9

// Setup for 4x4 Matrix Keypad
const byte ROWS = 4; 
const byte COLS = 4; 

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

// Connect Keypad ROW1, ROW2, ROW3 and ROW4 to these Arduino pins
byte rowPins[ROWS] = {8, 7, 6, 5}; 
// Connect Keypad COL1, COL2, COL3 and COL4 to these Arduino pins
byte colPins[COLS] = {4, 3, 2, A0}; 

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

String defaultPassword = "1234"; // Set your door password here
String enteredPassword = "";

void setup() {
  Serial.begin(9600);
  doorLock.attach(servoPin);
  
  doorLock.write(0); // Initially lock the door (0 degrees)
  Serial.println("System Ready.");
  Serial.println("Enter Password to Unlock (Press '#' to submit, '*' to clear):");
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    if (key == '#') { 
      // '#' is used as the ENTER/SUBMIT button
      if (enteredPassword == defaultPassword) {
        Serial.println("\n[SUCCESS] Access Granted! Door Unlocking...");
        doorLock.write(90); // Rotate servo to 90 degrees to unlock
        delay(5000);        // Keep door open for 5 seconds
        
        Serial.println("Auto Locking Door...");
        doorLock.write(0);  // Rotate back to 0 degrees to lock
      } else {
        Serial.println("\n[ERROR] Access Denied! Wrong Password.");
      }
      
      enteredPassword = ""; // Reset password string for next attempt
      Serial.println("\nEnter Password to Unlock:");
      
    } else if (key == '*') { 
      // '*' is used as the CLEAR/BACKSPACE button
      enteredPassword = "";
      Serial.println("\nInput Cleared. Enter Password:");
      
    } else {
      // Add pressed key to the password string
      enteredPassword += key;
      Serial.print("*"); // Print asterisk for password security on Serial Monitor
    }
  }
}
