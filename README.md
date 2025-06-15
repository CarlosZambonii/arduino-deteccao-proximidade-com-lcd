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

## 📷 Imagem do circuito

![Circuito](./circuito.png)

---

## 📎 Código
```cpp
// Exemplo de um trecho
long readSimulatedDistance() {
  return random(0, 51);
}
