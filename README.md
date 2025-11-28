# Sistema de Iluminação Automática Inteligente

## Integrantes da Equipe
- Natanael
- Miguel
- Luiz

## Objetivo do Projeto
Desenvolver um sistema embarcado que controla automaticamente a iluminação de um ambiente baseado na presença de pessoas e luminosidade, além de monitorar e alertar sobre condições de temperatura através de LEDs indicadores e buzzer.

O sistema acende a iluminação principal (LED RGB) apenas quando detecta presença em ambiente escuro, economizando energia. Simultaneamente, monitora a temperatura ambiente e alerta o usuário sobre condições críticas (muito frio ou muito quente) através de LEDs coloridos e sinal sonoro.

## Elementos Utilizados

### Microcontrolador
- 1x Arduino Uno

### Sensores
- 1x Sensor DHT11 (temperatura e umidade)
- 1x Sensor Ultrassônico HC-SR04 (detecção de presença)
- 1x LDR (sensor de luminosidade)

### Atuadores
- 1x LED RGB (iluminação principal)
- 1x LED Vermelho (alerta de temperatura crítica)
- 1x LED Amarelo (alerta de temperatura)
- 1x Buzzer (alarme sonoro)

### Componentes Adicionais
- 1x Protoboard
- 3x Resistor 220Ω (para LEDs)
- 1x Resistor 10kΩ (para LDR)
- Jumpers diversos

## Custo Estimado do Projeto

| Componente | Quantidade | Preço Unitário | Subtotal |
|------------|-----------|----------------|----------|
| Arduino Uno | 1 | R$ 70,00 | R$ 70,00 |
| Sensor DHT11 | 1 | R$ 15,00 | R$ 15,00 |
| Sensor Ultrassônico HC-SR04 | 1 | R$ 12,00 | R$ 12,00 |
| LDR | 1 | R$ 2,00 | R$ 2,00 |
| LED RGB | 1 | R$ 3,00 | R$ 3,00 |
| LED Vermelho | 1 | R$ 0,50 | R$ 0,50 |
| LED Amarelo | 1 | R$ 0,50 | R$ 0,50 |
| Buzzer | 1 | R$ 5,00 | R$ 5,00 |
| Resistores (diversos) | 1 kit | R$ 8,00 | R$ 8,00 |
| Protoboard | 1 | R$ 15,00 | R$ 15,00 |
| Jumpers | 1 kit | R$ 10,00 | R$ 10,00 |
| **TOTAL** | | | **R$ 141,00** |

## Esquema de Montagem

### Conexões dos Componentes

**Sensor Ultrassônico (HC-SR04):**
- VCC → 5V
- TRIG → Pino Digital 9
- ECHO → Pino Digital 10
- GND → GND

**Sensor DHT11:**
- VCC → 5V
- DATA → Pino Digital 2
- GND → GND

**LDR (Sensor de Luminosidade):**
- Terminal 1 → 5V
- Terminal 2 → Pino Analógico A0 + Resistor 10kΩ para GND

**LED RGB:**
- Pino R (Vermelho) → Resistor 220Ω → Pino Digital 3
- Pino G (Verde) → Resistor 220Ω → Pino Digital 5
- Pino B (Azul) → Resistor 220Ω → Pino Digital 6
- GND → GND

**LED Vermelho:**
- Anodo (+) → Resistor 220Ω → Pino Digital 7
- Catodo (-) → GND

**LED Amarelo:**
- Anodo (+) → Resistor 220Ω → Pino Digital 8
- Catodo (-) → GND

**Buzzer:**
- Positivo (+) → Pino Digital 11
- Negativo (-) → GND

### Diagrama de Montagem
[Incluir aqui a imagem/diagrama do Tinkercad ou foto do esquema de ligações]

## Funcionamento do Sistema

### Iluminação Automática (LED RGB)
- **Acende**: Quando está escuro E detecta presença (distância < 100cm)
- **Apaga**: Quando está claro OU não há presença por 5 segundos

### Monitoramento de Temperatura

| Faixa de Temperatura | LED Vermelho | LED Amarelo | Buzzer |
|---------------------|--------------|-------------|---------|
| Abaixo de 10°C | ✅ Aceso | ❌ Apagado | ✅ Ativo |
| 10°C - 17.9°C | ❌ Apagado | ✅ Aceso | ❌ Inativo |
| 18°C - 25.9°C | ❌ Apagado | ❌ Apagado | ❌ Inativo |
| 26°C - 29.9°C | ❌ Apagado | ✅ Aceso | ❌ Inativo |
| 30°C ou mais | ✅ Aceso | ❌ Apagado | ✅ Ativo |

### Lógica de Operação
1. O sistema monitora continuamente todos os sensores
2. A iluminação principal funciona independentemente dos alertas de temperatura
3. Os alertas de temperatura funcionam continuamente, independente da luz ou presença
4. O buzzer só é acionado em situações críticas (temperatura < 10°C ou ≥ 30°C)

## Instalação e Uso

### Requisitos
- Arduino IDE 1.8.19 ou superior
- Biblioteca DHT sensor library (by Adafruit)
- Biblioteca Adafruit Unified Sensor

### Instalação das Bibliotecas
1. Abra o Arduino IDE
2. Vá em `Ferramentas` → `Gerenciar Bibliotecas`
3. Procure por "DHT sensor library"
4. Instale a biblioteca by Adafruit
5. Instale também a dependência "Adafruit Unified Sensor"

### Upload do Código
1. Clone este repositório
2. Abra o arquivo `codigo.ino` no Arduino IDE
3. Conecte o Arduino Uno ao computador via USB
4. Selecione a placa: `Ferramentas` → `Placa` → `Arduino Uno`
5. Selecione a porta correta em `Ferramentas` → `Porta`
6. Clique em `Upload` (seta para direita)

### Montagem Física
1. Siga o esquema de montagem apresentado acima
2. Conecte todos os componentes na protoboard conforme indicado
3. Verifique todas as conexões antes de energizar
4. Conecte o Arduino ao computador ou fonte de alimentação

## Fotos do Sistema
[Adicionar fotos ou GIFs do sistema montado e funcionando]

## Vídeo Demonstrativo
[Link do vídeo no YouTube mostrando o sistema em funcionamento]

## Estrutura do Código

```cpp
// Bibliotecas e definições de pinos
// Setup: Inicialização dos componentes
// Loop principal:
//   - Leitura dos sensores
//   - Detecção de presença
//   - Controle de iluminação
//   - Controle de alertas de temperatura
```

### Principais Funções
- `lerDistancia()`: Lê a distância do sensor ultrassônico
- `controlarIluminacao()`: Gerencia o LED RGB baseado em presença e luminosidade
- `controlarAlertas()`: Gerencia LEDs de temperatura e buzzer

## Observações
- O limite de luminosidade (400) pode precisar ser ajustado conforme o ambiente
- A distância máxima de detecção está configurada para 100cm
- O sistema aguarda 5 segundos sem presença antes de apagar a iluminação
- Valores de temperatura e outros dados podem ser monitorados via Serial Monitor (9600 baud)

## Licença
Este projeto foi desenvolvido para fins educacionais como parte da disciplina de Sistemas Embarcados.
