# 🚦 Semáforo Inteligente com Sensor de Pedestre (Arduino)

## 🧭 Introdução
Este projeto simula um **semáforo inteligente** controlado por **Arduino Uno**, que detecta pedestres por meio de um **sensor ultrassônico HC-SR04**.  
Quando o sensor identifica um pedestre a menos de **10 cm**, o sistema **interrompe o ciclo normal** e acende a luz **vermelha** para permitir a travessia.  
O código utiliza **ponteiros em C++** para controlar os LEDs, tornando o sistema mais flexível e didático.

---

## 🧩 Materiais Utilizados

| Componente | Quantidade | Função |
|-------------|-------------|--------|
| Arduino Uno | 1 | Microcontrolador |
| LED Vermelho | 1 | Indica “Pare” |
| LED Amarelo | 1 | Indica “Atenção” |
| LED Verde | 1 | Indica “Siga” |
| Resistores 220 Ω | 3 | Proteção dos LEDs |
| Sensor Ultrassônico HC-SR04 | 1 | Detecta pedestres |
| Protoboard e Jumpers | 1 | Conexões elétricas |

---

## 💻 Código do Projeto

```cpp
// ==========================
// Pinos dos LEDs
// ==========================
int vermelho = 13;
int amarelo = 12;
int verde = 11;

// Ponteiros para os LEDs
int *pVermelho = &vermelho;
int *pAmarelo = &amarelo;
int *pVerde = &verde;

// ==========================
// Sensor ultrassônico HC-SR04
// ==========================
const int trigPin = 9;
const int echoPin = 8;
long duracao;
int distancia;

// ==========================
// Função: medirDistancia()
// ==========================
int medirDistancia() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duracao = pulseIn(echoPin, HIGH, 20000);
  distancia = duracao * 0.034 / 2;
  return distancia;
}

// ==========================
// Função: acendeLed()
// ==========================
void acendeLed(int *pino, int tempo) {
  digitalWrite(*pino, HIGH);
  delay(tempo);
  digitalWrite(*pino, LOW);
}

// ==========================
// Setup
// ==========================
void setup() {
  pinMode(*pVermelho, OUTPUT);
  pinMode(*pAmarelo, OUTPUT);
  pinMode(*pVerde, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

// ==========================
// Loop principal
// ==========================
void loop() {
  int d = medirDistancia();
  Serial.print("Distancia: ");
  Serial.print(d);
  Serial.println(" cm");

  if (d > 0 && d <= 10) {
    // Pedestre detectado → vermelho
    digitalWrite(*pVermelho, HIGH);
    digitalWrite(*pAmarelo, LOW);
    digitalWrite(*pVerde, LOW);
    delay(3000);
  } else {
    // Ciclo normal
    acendeLed(pVermelho, 6000);
    acendeLed(pVerde, 4000);
    acendeLed(pAmarelo, 2000);
  }

  delay(500);
}

```

## Avaliação dos Pares

| Avaliador | Montagem física (4) | Pontos | Temporização (3) | Pontos | Código & estrutura (3) | Pontos | Observações gerais                                                         | Total (10) |
|-----------|----------------------|--------|-------------------|--------|------------------------|--------|----------------------------------------------------------------------------|------------|
| Kaian     | Contempla parcialmente | 3.9    | contempla          | 3    | Contempla               | 3    | Boa implementação do sistema de proximidade, só pecou na organização dos fios | 9.9        |
| Lívia     | Contempla parcialmente | 3.8    | contempla          | 3   | Contempla               | 3    | Código com estrutura avançada, estrutura física um pouco confusa | 9.8        |

