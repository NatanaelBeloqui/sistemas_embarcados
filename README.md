Sistema de Iluminação Automática Inteligente
Integrantes da Equipe

[Nome Completo do Integrante 1]
[Nome Completo do Integrante 2]
[Nome Completo do Integrante 3]

Objetivo do Projeto
Desenvolver um sistema embarcado que controla automaticamente a iluminação de um ambiente baseado na presença de pessoas e luminosidade, além de monitorar e alertar sobre condições de temperatura através de LEDs indicadores e buzzer.
O sistema acende a iluminação principal (LED RGB) apenas quando detecta presença em ambiente escuro, economizando energia. Simultaneamente, monitora a temperatura ambiente e alerta o usuário sobre condições críticas (muito frio ou muito quente) através de LEDs coloridos e sinal sonoro.
Elementos Utilizados
Microcontrolador

1x Arduino Uno

Sensores

1x Sensor DHT11 (temperatura e umidade)
1x Sensor Ultrassônico HC-SR04 (detecção de presença)
1x LDR (sensor de luminosidade)

Atuadores

1x LED RGB (iluminação principal)
1x LED Vermelho (alerta de temperatura crítica)
1x LED Amarelo (alerta de temperatura)
1x Buzzer (alarme sonoro)

Componentes Adicionais

1x Protoboard
3x Resistor 220Ω (para LEDs)
1x Resistor 10kΩ (para LDR)
Jumpers diversos

Custo Estimado do Projeto
ComponenteQuantidadePreço UnitárioSubtotalArduino Uno1R$ 70,00R$ 70,00Sensor DHT111R$ 15,00R$ 15,00Sensor Ultrassônico HC-SR041R$ 12,00R$ 12,00LDR1R$ 2,00R$ 2,00LED RGB1R$ 3,00R$ 3,00LED Vermelho1R$ 0,50R$ 0,50LED Amarelo1R$ 0,50R$ 0,50Buzzer1R$ 5,00R$ 5,00Resistores (diversos)1 kitR$ 8,00R$ 8,00Protoboard1R$ 15,00R$ 15,00Jumpers1 kitR$ 10,00R$ 10,00TOTALR$ 141,00
Esquema de Montagem
Conexões dos Componentes
Sensor Ultrassônico (HC-SR04):

VCC → 5V
TRIG → Pino Digital 9
ECHO → Pino Digital 10
GND → GND

Sensor DHT11:

VCC → 5V
DATA → Pino Digital 2
GND → GND

LDR (Sensor de Luminosidade):

Terminal 1 → 5V
Terminal 2 → Pino Analógico A0 + Resistor 10kΩ para GND

LED RGB:

Pino R (Vermelho) → Resistor 220Ω → Pino Digital 3
Pino G (Verde) → Resistor 220Ω → Pino Digital 5
Pino B (Azul) → Resistor 220Ω → Pino Digital 6
GND → GND

LED Vermelho:

Anodo (+) → Resistor 220Ω → Pino Digital 7
Catodo (-) → GND

LED Amarelo:

Anodo (+) → Resistor 220Ω → Pino Digital 8
Catodo (-) → GND

Buzzer:

Positivo (+) → Pino Digital 11
Negativo (-) → GND

Diagrama de Montagem
[Incluir aqui a imagem/diagrama do Tinkercad ou foto do esquema de ligações]
Funcionamento do Sistema
Iluminação Automática (LED RGB)

Acende: Quando está escuro E detecta presença (distância < 100cm)
Apaga: Quando está claro OU não há presença por 5 segundos

Monitoramento de Temperatura
Faixa de TemperaturaLED VermelhoLED AmareloBuzzerAbaixo de 10°C✅ Aceso❌ Apagado✅ Ativo10°C - 17.9°C❌ Apagado✅ Aceso❌ Inativo18°C - 25.9°C❌ Apagado❌ Apagado❌ Inativo26°C - 29.9°C❌ Apagado✅ Aceso❌ Inativo30°C ou mais✅ Aceso❌ Apagado✅ Ativo
Lógica de Operação

O sistema monitora continuamente todos os sensores
A iluminação principal funciona independentemente dos alertas de temperatura
Os alertas de temperatura funcionam continuamente, independente da luz ou presença
O buzzer só é acionado em situações críticas (temperatura < 10°C ou ≥ 30°C)

Instalação e Uso
Requisitos

Arduino IDE 1.8.19 ou superior
Biblioteca DHT sensor library (by Adafruit)
Biblioteca Adafruit Unified Sensor

Instalação das Bibliotecas

Abra o Arduino IDE
Vá em Ferramentas → Gerenciar Bibliotecas
Procure por "DHT sensor library"
Instale a biblioteca by Adafruit
Instale também a dependência "Adafruit Unified Sensor"

Upload do Código

Clone este repositório
Abra o arquivo codigo.ino no Arduino IDE
Conecte o Arduino Uno ao computador via USB
Selecione a placa: Ferramentas → Placa → Arduino Uno
Selecione a porta correta em Ferramentas → Porta
Clique em Upload (seta para direita)

Montagem Física

Siga o esquema de montagem apresentado acima
Conecte todos os componentes na protoboard conforme indicado
Verifique todas as conexões antes de energizar
Conecte o Arduino ao computador ou fonte de alimentação

Fotos do Sistema
[Adicionar fotos ou GIFs do sistema montado e funcionando]
Vídeo Demonstrativo
[Link do vídeo no YouTube mostrando o sistema em funcionamento]
Estrutura do Código
cpp// Bibliotecas e definições de pinos
// Setup: Inicialização dos componentes
// Loop principal:
//   - Leitura dos sensores
//   - Detecção de presença
//   - Controle de iluminação
//   - Controle de alertas de temperatura
Principais Funções

lerDistancia(): Lê a distância do sensor ultrassônico
controlarIluminacao(): Gerencia o LED RGB baseado em presença e luminosidade
controlarAlertas(): Gerencia LEDs de temperatura e buzzer

Observações

O limite de luminosidade (400) pode precisar ser ajustado conforme o ambiente
A distância máxima de detecção está configurada para 100cm
O sistema aguarda 5 segundos sem presença antes de apagar a iluminação
Valores de temperatura e outros dados podem ser monitorados via Serial Monitor (9600 baud)