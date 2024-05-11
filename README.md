#include <Servo.h>

#define TRIG_PIN 14
#define ECHO_PIN 12
#define SERVO_PIN 13

Servo servo;

void setup() {
  Serial.begin(9600);
  servo.attach(SERVO_PIN);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  long duration, distance;
  
  // Ultrasonik sensörden mesafe okuma
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = (duration / 2) / 29.1;  // Hesaplanmış mesafe cm cinsinden
  
  Serial.print("Mesafe: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // Mesafe 5 cm'den kısa ise servo motoru 90 dereceye döndür
  if (distance < 5) {
    servo.write(90);
    delay(2000);
    servo.write(0);
  }
  
  delay(500); // Sensörün aşırı sık okunmasını engellemek için kısa bir gecikme
}
