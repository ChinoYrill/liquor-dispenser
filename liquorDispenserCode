#include <Wire.h> 
#include <Adafruit_LiquidCrystal.h>

// Pin assignments
const int trigPin = 9;
const int echoPin = 8;
const int relayPin = 7;
const int button1Pin = 2;
const int button2Pin = 3;
const int button3Pin = 4;

// Variables
int motorDuration = 0; // Default 1 second
int durationOptions[] = {4000, 1000, 2000, 3000}; // 1s, 2s, 3s, 4s
int currentDurationIndex = 0;
bool objectDetected = false;
unsigned long lastButtonPress1 = 0;
unsigned long lastButtonPress2 = 0;
unsigned long lastButtonPress3 = 0;

// LCD display setup
Adafruit_LiquidCrystal lcd(0);

void setup() {
  // Set up pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(relayPin, OUTPUT);
  pinMode(button1Pin, INPUT_PULLUP);
  pinMode(button2Pin, INPUT_PULLUP);
  pinMode(button3Pin, INPUT_PULLUP);

  // Initial state for relay
  digitalWrite(relayPin, LOW); // Relay starts off (NC)
  
  // Initialize Serial Monitor
  Serial.begin(9600);

  // Initialize LCD
  lcd.begin(16, 2); // Set up 16x2 LCD
  showWelcomeScreen();
}

void loop() {
  long distance = getDistance();

  // Object detection logic with threshold buffer
  if (distance <= 20) {  // Object detected
    if (!objectDetected) {  // Object was not detected previously
      objectDetected = true;
      showObjectDetected();  // Display message when object is detected
    }

    // Handle button presses (normal operations)
    handleButtonPresses();
  } else {  // No object detected, add buffer threshold
    if (objectDetected) {  // Object was previously detected, now it's out of range
      objectDetected = false;
      showNoObjectDetected();  // Show "No Object Detected"
    }
    
    // If no object is detected, show "No Object Detected" when any button is pressed
    if (digitalRead(button1Pin) == LOW || digitalRead(button2Pin) == LOW || digitalRead(button3Pin) == LOW) {
      showNoObjectDetected();  // Display message if no object is present and any button is pressed
    }
  }
  
  delay(100); // Loop delay to avoid excessive checking
}

// Handle button presses with debouncing for each button
void handleButtonPresses() {
  unsigned long currentMillis = millis();
  
  // Button 1: Cycle motor durations
  if (digitalRead(button1Pin) == LOW && currentMillis - lastButtonPress1 > 300) {
    lastButtonPress1 = currentMillis;
    currentDurationIndex = (currentDurationIndex + 1) % 4; // Cycle through 4 options
    motorDuration = durationOptions[currentDurationIndex];
    showVolumeSelection(motorDuration);
  }

  // Button 2: Start motor for selected duration
  if (digitalRead(button2Pin) == LOW && currentMillis - lastButtonPress2 > 300) {
    lastButtonPress2 = currentMillis;
    if (motorDuration == 0) {
      showVolumeReminder(); // Reminder to choose a volume first
    } else {
      showDispensingStatus();
      activateRelayForDuration(motorDuration);
      showDispenseComplete();
    }
  }

  // Button 3: Start motor while holding button
  if (digitalRead(button3Pin) == LOW && currentMillis - lastButtonPress3 > 300) {
    lastButtonPress3 = currentMillis;
    showHoldToDispense();
    activateRelayWhilePressed();
    showDispenseComplete();
  }
}

// Function to get distance from ultrasonic sensor
long getDistance() {
  long duration, distance;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration * 0.034) / 2; // Convert to cm
  return distance;
}

// Function to activate relay for a set duration with object detection check
void activateRelayForDuration(int duration) {
  digitalWrite(relayPin, HIGH); // Activate relay (switch from NC to NO)
  unsigned long relayStartTime = millis();

  while (millis() - relayStartTime < duration) {
    // Check if object is removed during dispensing, stop motor immediately
    long distance = getDistance();
    if (distance > 25) {  // Object removed, stop motor
      digitalWrite(relayPin, LOW);
      showNoObjectDetected();  // Show "No Object Detected"
      return; // Exit the function early to stop the motor
    }
    delay(10);  // Non-blocking delay to allow checking the distance periodically
  }
  
  digitalWrite(relayPin, LOW);  // Deactivate relay (switch back to NC)
}

// Function to activate relay while button is pressed with object detection check
void activateRelayWhilePressed() {
  digitalWrite(relayPin, HIGH); // Activate relay
  while (digitalRead(button3Pin) == LOW) {
    // Check if object is removed during dispensing, stop motor immediately
    long distance = getDistance();
    if (distance > 25) {  // Object removed, stop motor
      digitalWrite(relayPin, LOW);
      showNoObjectDetected();  // Show "No Object Detected"
      return; // Exit the function early to stop the motor
    }
    delay(10);  // Non-blocking delay to allow checking the distance periodically
  }
  digitalWrite(relayPin, LOW);  // Deactivate relay when button is released
}

// LCD Functions

void showWelcomeScreen() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("NASAAN ANG");
  lcd.setCursor(0, 1);
  lcd.print("PULUTAN?");
}

void showVolumeSelection(int motorDuration) {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Gano katagal?");
  lcd.setCursor(0, 1);
  lcd.print(motorDuration / 1000); // Display the selected volume in seconds
  lcd.print(" sec lang?");
}

void showVolumeReminder() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Pili ka muna");
  lcd.setCursor(0, 1);
  lcd.print("Kupal!");
}

void showDispensingStatus() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Wag mong kunin");
  lcd.setCursor(0, 1);
  lcd.print("di pa tapos!");
}

void showHoldToDispense() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Diin mo lang");
  lcd.setCursor(0, 1);
  lcd.print("tas bitaw!");
}

void showDispenseComplete() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("TAGAYIN MO NA!");
  
  // Display the current volume selected on the second row
  lcd.setCursor(0, 1);
  lcd.print(motorDuration / 1000); // Display the selected volume in seconds
  lcd.print(" sec muna!");
}


void showNoObjectDetected() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("WALA NAMAN");
  lcd.setCursor(0, 1);
  lcd.print("BASO EH!!!");
}

void showObjectDetected() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("G NA YAN!");
  lcd.setCursor(0, 1);
  lcd.print(motorDuration / 1000); // Display the selected volume in seconds
  lcd.print(" sec muna!");
}

// Error message display
void showErrorMessage() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Error");
  lcd.setCursor(0, 1);
  lcd.print("Try again");
}

// Out of Service display
void showOutOfService() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Out of Service");
  lcd.setCursor(0, 1);
  lcd.print("Contact support");
}
