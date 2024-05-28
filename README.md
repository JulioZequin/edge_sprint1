<h1>Projeto Simulador de Fórmula E</h1>
    <h2>Visão Geral do Projeto</h2>
    <p>Este projeto simula um carro de Fórmula E utilizando um setup com Arduino. A simulação inclui um potenciômetro para controlar a velocidade do carro, um servo motor para representar o velocímetro e um sensor DHT11 para simular a temperatura do motor do carro. Adicionalmente, o projeto possui LEDs para indicar a velocidade e a economia da bateria, um buzzer para alertas de alta temperatura e um sensor LDR para controlar automaticamente os faróis.</p>
    <h2>Componentes Utilizados</h2>
    <ul>
        <li>Arduino Uno</li>
        <li>Potenciômetro</li>
        <li>Servo Motor</li>
        <li>Sensor de Temperatura e Umidade DHT11</li>
        <li>LEDs (Vermelho, Verde, Amarelo, Branco)</li>
        <li>Buzzer</li>
        <li>LDR (Resistor Dependente de Luz)</li>
        <li>Resistores</li>
        <li>Breadboard e Jumpers</li>
    </ul>
    <h2>Funcionalidades</h2>
    <ul>
        <li><strong>Controle de Velocidade:</strong> O potenciômetro é utilizado para simular o acelerador do carro, controlando a velocidade de 0 a 320 km/h.</li>
        <li><strong>Simulação de Temperatura:</strong> A temperatura do motor é simulada com base na velocidade, variando de 0 a 80°C.</li>
        <li><strong>Indicadores de LED:</strong>
            <ul>
                <li><strong>LED Verde:</strong> Indica baixa velocidade (menos de 110 km/h) e boa economia de bateria.</li>
                <li><strong>LED Amarelo:</strong> Indica velocidade média (entre 110 e 230 km/h) e economia moderada de bateria.</li>
                <li><strong>LED Vermelho:</strong> Indica alta velocidade (maior que 230 km/h) e baixa economia de bateria.</li>
            </ul>
        </li>
        <li><strong>Buzzer:</strong> Emite um alarme se a temperatura exceder 70°C, com um intervalo de 2 segundos.</li>
        <li><strong>Faróis Automáticos:</strong> Um LED branco que é acionado automaticamente com base na leitura do sensor LDR (Resistor Dependente de Luz).</li>
    </ul>
    <h2>Código do Projeto</h2>
    <pre>
        <code>
#include "DHT.h"
#include <Servo.h>

// Definições do DHT11
#define DHTPIN 2      // Pino onde o sensor DHT11 está conectado
#define DHTTYPE DHT11 // Tipo de sensor

DHT dht(DHTPIN, DHTTYPE);
Servo motor;

const int redLed = 13;    // Pino do LED vermelho
const int greenLed = 12;  // Pino do LED verde
const int yellowLed = 11; // Pino do LED amarelo
const int orangeLed= 8;   // Pino do LED branco (faróis)
const int ldrPin = A1;    // Pino do LDR
const int potPin = A0;    // Pino do potenciômetro
const int buzzerPin = 10; // Pino do buzzer

// Variáveis
int sensorValue = 0;     // Valor lido do potenciômetro
float speed = 0.0;       // Velocidade simulada do carro
float temperature = 0.0; // Temperatura do motor
int ldrValue = 0;        // Valor lido do LDR

void setup() {
  Serial.begin(9600);         // Inicializa a comunicação serial
  pinMode(redLed, OUTPUT);    // Define o pino do LED vermelho como saída
  pinMode(greenLed, OUTPUT);  // Define o pino do LED verde como saída
  pinMode(yellowLed, OUTPUT); // Define o pino do LED amarelo como saída
  pinMode(orangeLed, OUTPUT);  // Define o pino do LED laranja como saída
  pinMode(buzzerPin, OUTPUT); // Define o pino do buzzer como saída
  motor.attach(9);            // Define o pino do motor como saída
  dht.begin();                // Inicializa o sensor DHT11
}

void loop() {
  // Leitura do potenciômetro
  sensorValue = analogRead(potPin);
  speed = sensorValue * (320.0 / 1023.0); // Converte para uma velocidade simulada (0-320 km/h)
  
  // Simula a temperatura baseada na velocidade
  temperature = speed * (80.0 / 320.0); // Mapeia a velocidade (0-320 km/h) para uma temperatura (0-80 °C)

  // Leitura do LDR
  ldrValue = analogRead(ldrPin);
  Serial.print("LDR Value: ");
  Serial.println(ldrValue);

  // Controle dos faróis baseado na luz ambiente
  if (ldrValue > 650) {  // Se a leitura do LDR for baixa (indicando pouca luz)
    digitalWrite(orangeLed, HIGH); // Liga os faróis
  } else {
    digitalWrite(orangeLed, LOW);  // Desliga os faróis
  }

  // Exibe a velocidade e a temperatura no monitor serial
  Serial.print("Velocidade do carro: ");
  Serial.print(speed);
  Serial.println(" km/h");
  
  Serial.print("Temperatura do motor: ");
  Serial.print(temperature);
  Serial.println(" *C");
  Serial.println("-----------------------------");

  // Controle dos LEDs baseado na velocidade
  if (speed < 110) {  // Se a velocidade for menor que 110 km/h
    digitalWrite(greenLed, HIGH); // Liga o LED verde
    digitalWrite(yellowLed, LOW); // Desliga o LED amarelo
    digitalWrite(redLed, LOW);    // Desliga o LED vermelho
  } else if (speed < 230) { // Se a velocidade for entre 110 e 230 km/h
    digitalWrite(greenLed, LOW);  // Desliga o LED verde
    digitalWrite(yellowLed, HIGH);// Liga o LED amarelo
    digitalWrite(redLed, LOW);    // Desliga o LED vermelho
  } else { // Se a velocidade for maior que 230 km/h
    digitalWrite(greenLed, LOW);  // Desliga o LED verde
    digitalWrite(yellowLed, LOW); // Desliga o LED amarelo
    digitalWrite(redLed, HIGH);   // Liga o LED vermelho
  }

  // Controle do motor baseado na velocidade (simulação)
  motor.write(map(sensorValue, 0, 1023, 0, 180)); // Ajusta a posição do servo motor

  // Controle do buzzer baseado na temperatura
  if (temperature > 70) {
    digitalWrite(buzzerPin, HIGH); // Liga o buzzer
    delay(2000);                   // Espera 2 segundos
    digitalWrite(buzzerPin, LOW);  // Desliga o buzzer
    delay(2000);                   // Espera 2 segundos
  } else {
    digitalWrite(buzzerPin, LOW);  // Desliga o buzzer
  }

  delay(900); // Espera 0,9 segundo entre leituras
}

</code>
    </pre>

<h2>Video no Simulador</h2>

<h2>Grupo</h2>
<ul>
        <li>Isadora</li>
        <li>Julio</li>
        <li>Khadija</li>
        <li>Kaio</li>
        <li>Pedro</li>
</ul>

    
