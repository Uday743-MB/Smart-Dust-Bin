# Smart-Dust-Bin
Multi-Control Smart Dustbin with Integrated Floor Cleaning and Sterilization

Objective:
The objective of this project is to design and develop a smart, contactless dustbin system that enhances hygiene and user convenience through automation and intelligent control. This system integrates gesture sensing, voice recognition via Bluetooth, and mobile app-based control to enable hands-free operation of the dustbin lid. Additionally, the project incorporates an inbuilt floor cleaning mechanism using moisture brushes and a sterilization unit powered by either UV light or disinfectant spray, aiming to maintain a clean and sanitized environment around the bin. Powered by an Arduino UNO and supported by sensors, servo motors, and a Bluetooth module (HC-05), this smart dustbin not only minimizes the need for manual contact, thereby reducing the risk of infection, but also demonstrates an innovative approach to modern waste management using embedded systems and automation technologies.
Features to Include:
Voice Control (via mobile app or voice module)
Hand Gesture Control (using gesture sensor)
Proximity Auto-Open (for nearby presence)
Inbuilt Floor Cleaning (using brushes & disinfectant)
Mobile App Control (open/close via phone)
Sterilization (via UV or disinfectant spray)
Required Components (You have):
Arduino UNO
Gesture Sensor (like APDS-9960 or KY-033)
Servo Motors (for bin lid & cleaning mechanism)
Proximity Sensor (like HC-SR04 or IR)
Moisture Brushes (motor-driven for sweeping)
Disinfectant Sprayer or UV light
Battery (as Power Supply)
Bluetooth module (like HC-05 for mobile app control)
Step-by-Step Workflow:
1. Gesture Control for Lid:
Use the gesture sensor to detect a hand wave.
If gesture detected → rotate servo to open lid.
After delay (e.g., 5 seconds) → close lid.
2. Proximity Sensor Function:
If distance < 20 cm → auto-open lid using servo.
Add delay → auto-close.
3. Mobile App Control (via Bluetooth):
Use an HC-05 Bluetooth module.
Create an Android app using MIT App Inventor.
Use buttons like "Open Lid", "Start Cleaning".
Send 'O' to open, 'C' to close, 'S' to start cleaning, etc.
4. Voice Control:
Add voice commands using mobile’s Google Assistant + Bluetooth.
Or integrate voice recognition in the MIT App itself.
Command like "Open Dustbin" triggers Arduino via Bluetooth.
5. Floor Cleaning Mechanism:
Attach rotating moisture brushes under the bin.
Drive them using a DC motor connected to Arduino.
Use a servo/small pump to spray disinfectant or power UV LEDs for 10 seconds after cleaning.
Arduino Pin Configuration (Example):
Hardware Connections Table
Component
Arduino Pin
Notes
Gesture Sensor
D2
Digital OUT (e.g., KY-033)
HC-SR04 (Proximity)
TRIG: D3
ECHO: D4
5V Power Required
Servo for Lid
D5
PWM pin, powered from 5V
Servo for Brushes
D6
PWM pin
disinfectant Pump/UV LED
D7
Digital Output to control relay/LED
HC-05 Bluetooth Module
TX to D10
RX to D11
Use voltage divider on HC-05 RX
Battery Power
Vin or Barrel Jack
9–12V recommended
Common Ground
Gnd 
All components share common GND


(Use SoftwareSerial if needed to free Serial Monitor)
Arduino Code
Assumptions:
Gesture sensor gives HIGH on gesture detection (e.g., KY-033)
Proximity sensor is HC-SR04
Two Servo Motors: one for lid, one for rotating brushes
Pin D7 controls UV LED or disinfectant sprayer
HC-05 uses SoftwareSerial on pins 10 and 11

Complete Arduino Code:
#include <Servo.h>
#include <SoftwareSerial.h>
// Bluetooth
SoftwareSerial BT(10, 11); // RX, TX
// Components
Servo lidServo;
Servo brushServo;
#define GESTURE_PIN 2
#define TRIG_PIN 3
#define ECHO_PIN 4
#define UV_PIN 7
long duration;
int distance;
void setup() {
  Serial.begin(9600);
  BT.begin(9600);
  pinMode(GESTURE_PIN, INPUT);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(UV_PIN, OUTPUT);
  lidServo.attach(5);
  brushServo.attach(6);
  closeLid();
}
void loop() {
  // Check Bluetooth Commands
  if (BT.available()) {
    char command = BT.read();
    if (command == 'O') openLid();
    else if (command == 'C') closeLid();
    else if (command == 'S') startCleaning();
    else if (command == 'D') disinfect();
  }
  // Check Gesture
  if (digitalRead(GESTURE_PIN) == HIGH) {
    openLid();
    delay(5000);
    closeLid();
  }
  // Proximity Detection
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = duration * 0.034 / 2;

  if (distance < 20) {
    openLid();
    delay(5000);
    closeLid();
  }
}
void openLid() {
  lidServo.write(90); // Open position
}
void closeLid() {
  lidServo.write(0); // Closed position
}
void startCleaning() {
  brushServo.write(90); // Start brush rotation
  delay(5000);
  brushServo.write(0);  // Stop brush
}
void disinfect() {
  digitalWrite(UV_PIN, HIGH);
  delay(5000);
  digitalWrite(UV_PIN, LOW);
}

📱 Creating and Exporting the Mobile App (.aia File)
Access MIT App Inventor:
Navigate to MIT App Inventor and sign in with your Google account.
Start a New Project:
Click on "Projects" > "Start New Project" and name it appropriately (e.g., SmartDustbinApp).
Design the User Interface:
Add the necessary components such as Buttons for "Open Lid", "Start Cleaning", and "Disinfect".
Include a SpeechRecognizer component for voice commands.
Add a BluetoothClient component to handle Bluetooth communication with the HC-05 module.
Program the Blocks:
Use the Blocks editor to define the behavior of each component. For instance, when the "Open Lid" button is clicked, send a specific command via Bluetooth to the Arduino.
Implement logic to handle voice commands using the SpeechRecognizer.
Export the Project (.aia File):
Once your app is complete, go to "Projects" > "Export selected project (.aia) to my computer".
This will download the .aia file, which contains your app's source code and can be shared or imported later.MIT CML

🔄 Importing an .aia File into MIT App Inventor
If you have an existing .aia file and wish to import it:
Open MIT App Inventor:
Visit MIT App Inventor and sign in.
Import the Project:
Click on "Projects" > "Import project (.aia) from my computer".
Browse and select the .aia file you wish to import.MIT CMLYouTube
Edit and Customize:
Once imported, you can modify the app as needed within the MIT App Inventor environment.

📂 Sharing the .aia File
You can share the .aia file with others by sending it via email or uploading it to a cloud storage service like Google Drive or Dropbox.
Recipients can then import the .aia file into their own MIT App Inventor accounts to view or modify the app.
🔗 How to Connect Mobile App to HC-05 Bluetooth Module
🧠 What You Need:
Arduino UNO
HC-05 Bluetooth Module
MIT App Inventor App (installed on your Android phone)
Arduino code (to receive and act on commands)
App with BluetoothClient component

⚙️ 1. Wiring the HC-05 to Arduino UNO

HC-05 Pin
Connect to Arduino UNO
VCC
5V
GND
GND
TXD
RX (Pin 0)
RXD
TX (Pin 1) via a voltage divider (5V → 3.3V)

🛑 Important: Use a voltage divider (two resistors like 1kΩ and 2kΩ) to protect the HC-05 RX pin from 5V.
📲 2. Configure MIT App Inventor App
Add Components:
BluetoothClient (non-visible)
ListPicker (to select HC-05)
Buttons (e.g., Open Lid, Clean, Disinfect)
Optionally, SpeechRecognizer for voice commands
Blocks Setup:
A. List Available Devices
when ListPicker1.BeforePicking
    set ListPicker1.Elements to BluetoothClient1.AddressAndNames
B. Connect to HC-05
when ListPicker1.AfterPicking
    call BluetoothClient1.Connect to ListPicker1.Selection
C. Send Command to Arduino
when Button1.Click
    call BluetoothClient1.SendText("open", false)
when Button2.Click
    call BluetoothClient1.SendText("clean", false)

 3. Arduino Code Snippet
#include <Servo.h>
Servo servo;
String command;
void setup() {
  Serial.begin(9600);
  servo.attach(9);
}
void loop() {
  if (Serial.available()) {
    command = Serial.readStringUntil('\n');

    if (command == "open") {
      servo.write(90); // open lid
      delay(3000);
      servo.write(0);  // close lid
    } else if (command == "clean") {
      // turn on motor or cleaning brush
    }
    // Add more conditions like "disinfect"
  }
}
✅ Final Checklist:
Make sure Bluetooth is ON in your phone
Pair the phone with HC-05 (default password is 1234 or 0000)
Select HC-05 in the app’s ListPicker
Press control buttons in the app to send commands




Mobile App using MIT App Inventor
Steps:
Go to: MIT App Inventor
Create New Project → Name: SmartDustbinApp
Design Screen:
Add 3 Buttons:
btnOpen → Text: "Open Lid"
btnClean → Text: "Start Cleaning"
btnDisinfect → Text: "Disinfect"
Add Voice Button:
Add SpeechRecognizer from Media section
Add BluetoothClient component
