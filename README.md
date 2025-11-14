# AI-theft-detection-system
/*
  ===============================================
  THE FINAL TINKERCAD PROTOTYPE
  ===============================================
  
  This code combines all our sensors to build a complete alarm system.

  - SENSORS -
  PIR Motion Sensor: Pin 2
  Ultrasonic Echo:     Pin 3
  Ultrasonic Trig:     Pin 4
  Tilt/Vibration:      Pin 6 (Tilt Switch)

  - OUTPUTS (Alarms) -
  LED:     Pin 13
  Buzzer:  Pin 8
  
  - LOGIC -
  If motion is detected, OR a package is too close, OR
  the system is tampered with (tilted), then
  turn ON the LED and sound the Buzzer.
*/

// --- Constants (naming our pins) ---
const int ledPin = 13;      // The pin for the visual alarm (LED)
const int pirPin = 2;       // The pin for the PIR Motion Sensor (INPUT)
const int echoPin = 3;      // The "listen" pin for the Ultrasonic Sensor (INPUT)
const int trigPin = 4;      // The "send" pin for the Ultrasonic Sensor (OUTPUT)
const int vibePin = 6;      // The pin for the Tilt/Vibration Sensor (INPUT)
const int buzzerPin = 8;    // The pin for the audio alarm (Buzzer)


// --- Global Variables (to store sensor states) ---
int pirState = LOW;         // Stores if motion is detected (HIGH or LOW)
int vibeState = LOW;        // Stores if vibration is detected (HIGH or LOW)
long duration;              // Stores the echo time from the ultrasonic sensor
int distance;               // Stores the calculated distance in cm


// The setup() function runs once at the start of the simulation
void setup() {
  // --- Set Output Pins ---
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(trigPin, OUTPUT);   // Ultrasonic "trig" pin is an output
  
  // --- Set Input Pins ---
  pinMode(pirPin, INPUT);
  pinMode(echoPin, INPUT);    // Ultrasonic "echo" pin is an input
  pinMode(vibePin, INPUT);
  
  // Start the Serial Monitor (for debugging)
  // You can click the "Serial Monitor" button in Tinkercad to see the distance!
  Serial.begin(9600);
}


// The loop() function runs over and over, forever
void loop() {
  
  // === 1. READ THE "SIMPLE" SENSORS ===
  
  // Read the state of the PIR motion sensor
  pirState = digitalRead(pirPin);
  
  // Read the state of the Tilt (vibration) sensor
  vibeState = digitalRead(vibePin);


  // === 2. READ THE "COMPLEX" SENSOR (ULTRASONIC) ===
  
  // This 4-line sequence is the standard way to get a reading
  // 1. Set the trigger pin low for a clean start
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  // 2. Send a 10-microsecond sound pulse
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // 3. Listen for the echo and measure the time it took
  duration = pulseIn(echoPin, HIGH);
  
  // 4. Calculate the distance based on the speed of sound
  // (duration * 0.034 / 2) = distance in centimeters
  distance = duration * 0.034 / 2;

  
  // --- Optional: Print the distance to the Serial Monitor ---
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");


  // === 3. MAKE THE FINAL DECISION ===
  
  // This is the "brain" of our project.
  // We use "||" which means "OR".
  // IF the PIR sees motion...
  // OR the distance is less than 50cm...
  // OR the tilt sensor is triggered...
  
  if (pirState == HIGH || distance < 50 || vibeState == HIGH) {
    
    // --- ALARM ON ---
    // If ANY of those things are true, turn on the alarms.
    digitalWrite(ledPin, HIGH);   // Turn on the LED
    tone(buzzerPin, 1000);        // Play a 1000Hz sound on the buzzer
  
  } else {
    
    // --- ALARM OFF ---
    // If all sensors are normal, turn off the alarms.
    digitalWrite(ledPin, LOW);    // Turn off the LED
    noTone(buzzerPin);            // Stop the buzzer sound
  
  }
  
  // Wait for 100 milliseconds before taking the next sensor reading
  // This gives the sensors time to settle.
  delay(100);
}
