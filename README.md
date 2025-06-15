# arduino-deteccao-proximidade-com-lcd
# Sistema de Detecção de Proximidade com Arduino

Este projeto foi desenvolvido como uma introdução à programação de sistemas embarcados utilizando **Arduino UNO**, com foco em **sensoriamento**, **atuadores** e **interface com usuário** via display LCD.

## 🔧 Componentes usados
- Arduino UNO
- Sensor ultrassônico HC-SR04 (simulado no Tinkercad)
- Servo motor SG90
- Buzzer (emissor de som)
- LED indicador
- Display LCD 16x2
- Jumpers e protoboard

## 💡 Funcionalidades
- Mede a distância de um objeto à frente do sensor
- Mostra a distância no LCD e Serial Monitor
- Gira o servo motor conforme a proximidade
- Ativa LED e buzzer se o objeto estiver muito próximo
- Simulação de leituras aleatórias se usado sem o sensor real

## 🧪 Ambiente de simulação
O projeto pode ser testado no [Tinkercad Circuits](https://www.tinkercad.com/), plataforma online de simulação de circuitos.
---

## 📎 Código
```cpp
#include <Servo.h>
#include <LiquidCrystal.h>

// Pinos
const int sensorPin = 10;     // Sensor ultrassônico
const int servoPin = 9;       // Servo motor
const int buzzerPin = 7;      // Buzzer
const int ledPin = 13;        // LED indicador no pino 13 agora

// LCD: rs, e, d4, d5, d6, d7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
Servo servo;

int currentAngle = 0;

void setup() {
  servo.attach(servoPin);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.print("Iniciando...");
  delay(1000);
  lcd.clear();

  randomSeed(analogRead(0));
}

// Função com média de múltiplas leituras
long getSmoothedDistance() {
  long total = 0;
  int samples = 5;
  for (int i = 0; i < samples; i++) {
    total += readSimulatedDistance(); // Substitua por readRealDistance() se for real
    delay(20);
  }
  return total / samples;
}

// Simulação de leitura de distância
long readSimulatedDistance() {
  return random(0, 51);
}

// Função para mover o servo suavemente
void moveServoSmoothly(int targetAngle) {
  if (targetAngle < currentAngle) {
    for (int angle = currentAngle; angle >= targetAngle; angle--) {
      servo.write(angle);
      delay(5);
    }
  } else {
    for (int angle = currentAngle; angle <= targetAngle; angle++) {
      servo.write(angle);
      delay(5);
    }
  }
  currentAngle = targetAngle;
}

void loop() {
  long distance = getSmoothedDistance();

  Serial.print("Distancia: ");
  Serial.print(distance);
  Serial.println(" cm");

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia:");
  lcd.setCursor(0, 1);
  lcd.print(distance);
  lcd.print(" cm");

  // Mapeia a distância para ângulo (0 a 180)
  int angle = map(distance, 0, 50, 0, 180);
  angle = constrain(angle, 0, 180); // Garante faixa válida
  moveServoSmoothly(angle);

  // Ativa LED e buzzer se objeto estiver perto
  if (distance > 0 && distance < 20) {
    digitalWrite(buzzerPin, HIGH);
    digitalWrite(ledPin, HIGH);
  } else {
    digitalWrite(buzzerPin, LOW);
    digitalWrite(ledPin, LOW);
  }

  delay(1000); // Tempo entre leituras
}

