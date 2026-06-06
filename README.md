# Smart Door Lock System - Code Alpha Task 2 🚪🔒

**Author:** Raj Narayan Gaur  
**Domain:** Internet of Things (IoT)  
**Company:** Code Alpha  

## 📝 Project Overview
This repository contains the code and documentation for my Task 2 project: a Smart Door Lock System. (Yahan 2-3 line mein batao ki lock kaise kaam karta hai, jaise RFID, keypad, ya fingerprint sensor ka use karke).

## 🛠️ Components Used
*  Arduino UNO (Jo bhi use kiya ho, wahan likho)
* Servo Motor / Solenoid Lock
* Jumper Wires & Breadboard
* (Aur koi component ho toh add karo)

## 📸 Project Images
*(Apni upload ki hui photo par click karke uska URL copy karo aur yahan paste karo is format mein: `![Project Photo](image_url)`)*

## 🎥 Working Video
**[Click here to watch the working video of the Smart Door Lock System](Yahan_Apne_YouTube_Ya_Drive_Video_Ka_Link_Daalo)**

## 💻 How It Works
(Yahan steps mein batao ki circuit kaise connect kiya aur lock kaise open/close hota hai).
Step 4: Finalize aur Share
README file set karne ke baad page ke bottom par jao aur "Commit changes" par click kardo.

Ab tumhari GitHub repository puri tarah se ready hai!

Tum is repository ka URL copy karke apni LinkedIn post mein add kar sakte ho, jisse recruiters aur connections directly tumhara code, photos, aur working video dekh saken.

/*
  Project: Smart Door Lock System
  Author: Raj Narayan Gaur
  Internship: Code Alpha (Task 2)
  Device: Arduino Uno
*/

#include <Servo.h>

Servo doorLock;
const int servoPin = 9; // Servo ka signal pin Arduino ke Pin 9 par hai

void setup() {
  Serial.begin(9600);
  doorLock.attach(servoPin);
  
  doorLock.write(0); // Shuruat mein door locked rahega (0 degree)
  Serial.println("System Ready. Send 'O' to Open, 'C' to Close.");
}

void loop() {
  if (Serial.available() > 0) {
    char command = Serial.read();

    if (command == 'O' || command == 'o') {
      Serial.println("Unlocking Door...");
      doorLock.write(90); // 90 degree par door open
      delay(5000);        // 5 seconds tak open rahega
      
      Serial.println("Auto Locking Door...");
      doorLock.write(0);  // Wapas lock ho jayega
    } 
    else if (command == 'C' || command == 'c') {
      Serial.println("Locking Door...");
      doorLock.write(0);  // Door close karne ke liye
    }
  }
}
