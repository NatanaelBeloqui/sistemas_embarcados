# Código


// Bibliotecas
#include <DHT.h>

// Definição dos pinos
// Sensores
#define DHTPIN 2
#define DHTTYPE DHT11
#define TRIG_PIN 9
#define ECHO_PIN 10
#define LDR_PIN A0

// Atuadores
#define LED_RGB_R 3
#define LED_RGB_G 5
#define LED_RGB_B 6
#define LED_VERMELHO 7
#define LED_AMARELO 8
#define BUZZER 11

// Variáveis de controle
DHT dht(DHTPIN, DHTTYPE);
unsigned long tempoSemPresenca = 0;
bool presencaDetectada = false;
const int TEMPO_APAGAR = 5000; // 5 segundos em milissegundos
const int LIMITE_ESCURO = 400;
const float DISTANCIA_MAXIMA = 100.0; // cm

void setup() {
  Serial.begin(9600);
  
  // Inicializa DHT
  dht.begin();
  
  // Configuração dos pinos
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LDR_PIN, INPUT);
  
  pinMode(LED_RGB_R, OUTPUT);
  pinMode(LED_RGB_G, OUTPUT);
  pinMode(LED_RGB_B, OUTPUT);
  pinMode(LED_VERMELHO, OUTPUT);
  pinMode(LED_AMARELO, OUTPUT);
  pinMode(BUZZER, OUTPUT);
  
  // Desliga tudo no início
  digitalWrite(LED_RGB_R, LOW);
  digitalWrite(LED_RGB_G, LOW);
  digitalWrite(LED_RGB_B, LOW);
  digitalWrite(LED_VERMELHO, LOW);
  digitalWrite(LED_AMARELO, LOW);
  digitalWrite(BUZZER, LOW);
}

void loop() {
  // Leitura dos sensores
  float temperatura = dht.readTemperature();
  float umidade = dht.readHumidity();
  int luminosidade = analogRead(LDR_PIN);
  float distancia = lerDistancia();
  
  // Verifica se a leitura do DHT11 falhou
  if (isnan(temperatura) || isnan(umidade)) {
    Serial.println("Erro ao ler DHT11!");
    delay(2000);
    return;
  }
  
  // Debug no Serial Monitor
  Serial.print("Temp: ");
  Serial.print(temperatura);
  Serial.print("°C | Umidade: ");
  Serial.print(umidade);
  Serial.print("% | Luz: ");
  Serial.print(luminosidade);
  Serial.print(" | Distancia: ");
  Serial.print(distancia);
  Serial.println(" cm");
  
  // Verifica presença
  if (distancia > 0 && distancia < 100) {
    presencaDetectada = true;
    tempoSemPresenca = millis();
    Serial.println("PRESENCA DETECTADA!");
  } else {
    if (millis() - tempoSemPresenca > TEMPO_APAGAR) {
      presencaDetectada = false;
      Serial.println("SEM PRESENCA");
    }
  }
  
  // Sistema de Iluminação (LED RGB)
  controlarIluminacao(luminosidade, presencaDetectada);
  
  // Sistema de Temperatura (LEDs e Buzzer)
  controlarAlertas(temperatura);
  
  delay(500); // Delay entre leituras
}

// Função para ler distância do sensor ultrassônico
float lerDistancia() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  long duracao = pulseIn(ECHO_PIN, HIGH);
  float distancia = duracao * 0.034 / 2;
  
  return distancia;
}

// Função para controlar LED RGB (Iluminação)
void controlarIluminacao(int luz, bool presenca) {
  bool estaEscuro = (luz < LIMITE_ESCURO);
  
  if (estaEscuro && presenca) {
    // Acende LED RGB em branco
    analogWrite(LED_RGB_R, 255);
    analogWrite(LED_RGB_G, 255);
    analogWrite(LED_RGB_B, 255);
  } else {
    // Apaga LED RGB
    analogWrite(LED_RGB_R, 0);
    analogWrite(LED_RGB_G, 0);
    analogWrite(LED_RGB_B, 0);
  }
}

// Função para controlar alertas de temperatura
void controlarAlertas(float temp) {
  // Desliga tudo primeiro
  digitalWrite(LED_VERMELHO, LOW);
  digitalWrite(LED_AMARELO, LOW);
  digitalWrite(BUZZER, LOW);
  
  if (temp < 10.0) {
    // Temperatura crítica FRIA
    digitalWrite(LED_VERMELHO, HIGH);
    digitalWrite(BUZZER, HIGH);
  } 
  else if (temp >= 10.0 && temp < 18.0) {
    // Temperatura fria
    digitalWrite(LED_AMARELO, HIGH);
  }
  else if (temp >= 18.0 && temp < 26.0) {
    // Temperatura confortável - nada aceso
  }
  else if (temp >= 26.0 && temp < 30.0) {
    // Temperatura quente
    digitalWrite(LED_AMARELO, HIGH);
  }
  else if (temp >= 30.0) {
    // Temperatura crítica QUENTE
    digitalWrite(LED_VERMELHO, HIGH);
    digitalWrite(BUZZER, HIGH);
  }
}
