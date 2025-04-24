#define EYE_BLINK_SENSOR_PIN 2
#define BUZZER_PIN 8

int eyeState = 0;
unsigned long eyeClosedTime = 0;
unsigned long maxClosedDuration = 2500; // 2.5 seconds

void setup() {
  pinMode(EYE_BLINK_SENSOR_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  eyeState = digitalRead(EYE_BLINK_SENSOR_PIN);

  if (eyeState == LOW) { // eyes closed
    if (eyeClosedTime == 0) {
      eyeClosedTime = millis(); // start timer
    } else if (millis() - eyeClosedTime >= maxClosedDuration) {
      digitalWrite(BUZZER_PIN, HIGH); // eyes closed too long
    }
  } else {
    eyeClosedTime = 0; // reset if eyes open
    digitalWrite(BUZZER_PIN, LOW);
  }

  delay(100); // debounce delay
}
