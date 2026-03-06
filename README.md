// --- PIN DEFINITIONS ---

// Flame Sensors
const int flameLeft = A3;
const int flameCenter = A4;
const int flameRight = A5;

// Motor Driver
const int ENA = 3;
const int IN1 = 5;
const int IN2 = 6;

const int ENB = 11;
const int IN3 = 7;
const int IN4 = 8;

// Relay
const int pumpRelay = 10;

const int moveSpeed = 60;
const int stopSpeed = 0;

void setup() {

  pinMode(flameLeft, INPUT);
  pinMode(flameCenter, INPUT);
  pinMode(flameRight, INPUT);

  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);

  pinMode(ENB, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  pinMode(pumpRelay, OUTPUT);

  digitalWrite(pumpRelay, HIGH); // Pump OFF
}

void loop() {

  bool fireLeft = digitalRead(flameLeft) == LOW;
  bool fireCenter = digitalRead(flameCenter) == LOW;
  bool fireRight = digitalRead(flameRight) == LOW;

  // If fire detected → STOP + PUMP ON
  if (fireLeft || fireCenter || fireRight) {

    stopRobot();
    digitalWrite(pumpRelay, LOW);   // Pump ON
    return; // prevents robot from moving again in this loop
  }

  // No fire → keep moving forward
  moveForward();
  digitalWrite(pumpRelay, HIGH);  // Pump OFF
}

// --- FUNCTIONS ---

void moveForward() {

  analogWrite(ENA, moveSpeed);
  analogWrite(ENB, moveSpeed);

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);

  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stopRobot() {

  analogWrite(ENA, stopSpeed);
  analogWrite(ENB, stopSpeed);

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);

  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
