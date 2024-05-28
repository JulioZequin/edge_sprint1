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

#define DHTPIN 2
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);
Servo motor;

const int redLed = 13;
const int greenLed = 12;
const int yellowLed = 11;
const int potPin = A0;
const int buzzerPin = 10;
const int ldrPin = A1;
const int headlightPin = 8;

int sensorValue = 0;
float speed = 0.0;
float temperature = 0.0;
int ldrValue = 0;

void setup() {
    Serial.begin(9600);
    pinMode(redLed, OUTPUT);
    pinMode(greenLed, OUTPUT);
    pinMode(yellowLed, OUTPUT);
    pinMode(buzzerPin, OUTPUT);
    pinMode(headlightPin, OUTPUT);
    motor.attach(9);
    dht.begin();
}

void loop() {
    sensorValue = analogRead(potPin);
    speed = sensorValue * (320.0 / 1023.0);
    temperature = speed * (80.0 / 320.0);
    
   Serial.print("Velocidade do carro: ");
   Serial.print(speed);
   Serial.println(" km/h");
   Serial.print("Temperatura do motor: ");
   Serial.print(temperature);
   Serial.println(" *C");
   Serial.println("-----------------------------");
    
   if (speed < 110) {
        digitalWrite(greenLed, HIGH);
        digitalWrite(yellowLed, LOW);
        digitalWrite(redLed, LOW);
    } else if (speed < 230) {
        digitalWrite(greenLed, LOW);
        digitalWrite(yellowLed, HIGH);
        digitalWrite(redLed, LOW);
    } else {
        digitalWrite(greenLed, LOW);
        digitalWrite(yellowLed, LOW);
        digitalWrite(redLed, HIGH);
    }

   motor.write(map(sensorValue, 0, 1023, 0, 180));

   if (temperature > 70) {
        digitalWrite(buzzerPin, HIGH);
        delay(2000);
        digitalWrite(buzzerPin, LOW);
        delay(2000);
    } else {
        digitalWrite(buzzerPin, LOW);
    }

   ldrValue = analogRead(ldrPin);
    if (ldrValue < 500) {
        digitalWrite(headlightPin, HIGH);
    } else {
        digitalWrite(headlightPin, LOW);
    }

   delay(900);
}
        </code>
    </pre>

<h2>Video no Simulador</h2>

    
