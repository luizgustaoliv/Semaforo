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
// Controle de tempo
// ==========================
unsigned long tempoAnterior = 0;
int estado = 1; // começa no verde
unsigned long intervalos[] = {6000, 4000, 2000}; // vermelho, verde, amarelo

bool objetoDetectado = false;

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
// Setup
// ==========================
void setup() {
  pinMode(*pVermelho, OUTPUT);
  pinMode(*pAmarelo, OUTPUT);
  pinMode(*pVerde, OUTPUT);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

// ==========================
// Loop principal
// ==========================
void loop() {
  int d = medirDistancia();
  unsigned long tempoAtual = millis();

  // --- Verifica detecção ---
  if (d > 0 && d <= 10) {
    // Objeto detectado → força vermelho
    objetoDetectado = true;
    digitalWrite(*pVermelho, HIGH);
    digitalWrite(*pAmarelo, LOW);
    digitalWrite(*pVerde, LOW);
    return;
  } 
  else {
    // Objeto saiu → reinicia ciclo no verde
    if (objetoDetectado) {
      objetoDetectado = false;
      estado = 1; // volta para verde
      tempoAnterior = tempoAtual; // reinicia contagem de tempo
    }
  }

  // --- Ciclo normal ---
  if (tempoAtual - tempoAnterior >= intervalos[estado]) {
    tempoAnterior = tempoAtual;
    estado = (estado + 1) % 3; // 0=vermelho, 1=verde, 2=amarelo
  }

  // Define LED conforme o estado
  digitalWrite(*pVermelho, estado == 0 ? HIGH : LOW);
  digitalWrite(*pVerde, estado == 1 ? HIGH : LOW);
  digitalWrite(*pAmarelo, estado == 2 ? HIGH : LOW);
}

```

## Avaliação dos Pares

| Avaliador | Montagem física (4) | Pontos | Temporização (3) | Pontos | Código & estrutura (3) | Pontos | Observações gerais                                                         | Total (10) |
|-----------|----------------------|--------|-------------------|--------|------------------------|--------|----------------------------------------------------------------------------|------------|
| Kaian     | Contempla parcialmente | 3.9    | contempla          | 3    | Contempla               | 3    | Boa implementação do sistema de proximidade, só pecou na organização dos fios | 9.9        |
| Lívia     | Contempla parcialmente | 3.8    | contempla          | 3   | Contempla               | 3    | Código com estrutura avançada, estrutura física um pouco confusa | 9.8        |

