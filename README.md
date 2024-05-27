<h1>Projeto: Simulador de Velocidade, Temperatura e Economia de Bateria para Formula E com Arduino</h1>
 <h2>Descrição do Projeto</h2>
    <p>Este projeto visa criar um sistema de simulação de velocidade, temperatura e economia de bateria para um carro de Formula E utilizando o Arduino. O sistema inclui um potenciômetro para simular o acelerador, LEDs para indicar o status da velocidade e da economia de bateria, um servo motor para representar o velocímetro, um sensor DHT11 para medir a temperatura e um buzzer para alertar quando a temperatura estiver muito alta.</p>
<h2>Componentes Utilizados</h2>
    <ul>
        <li>Arduino Uno</li>
        <li>Potenciômetro</li>
        <li>LEDs (vermelho, verde e amarelo)</li>
        <li>Servo Motor</li>
        <li>Sensor de Temperatura e Umidade DHT11</li>
        <li>Buzzer</li>
        <li>Jumpers</li>
        <li>Protoboard</li>
    </ul>
 <h2>Configuração do Circuito</h2>
    <ul>
        <li><strong>Potenciômetro</strong>: Conectado ao pino A0.</li>
        <li><strong>LEDs</strong>:
            <ul>
                <li>LED Verde: Conectado ao pino 12.</li>
                <li>LED Amarelo: Conectado ao pino 11.</li>
                <li>LED Vermelho: Conectado ao pino 13.</li>
            </ul>
        </li>
        <li><strong>Servo Motor</strong>: Conectado ao pino 9.</li>
        <li><strong>Sensor DHT11</strong>: Conectado ao pino 2.</li>
        <li><strong>Buzzer</strong>: Conectado ao pino 10.</li>
    </ul>
  <h2>Funcionalidades</h2>
    <ul>
        <li><strong>Simulação de Velocidade</strong>: O potenciômetro simula o acelerador do carro, ajustando a velocidade entre 0 e 320 km/h.</li>
        <li><strong>Indicação de Status e Economia de Bateria</strong>:
            <ul>
                <li>LED Verde acende para velocidades abaixo de 110 km/h (indicando economia de bateria).</li>
                <li>LED Amarelo acende para velocidades entre 110 km/h e 230 km/h (indicando estado de alerta).</li>
                <li>LED Vermelho acende para velocidades acima de 230 km/h (indicando alto consumo de bateria).</li>
            </ul>
        </li>
        <li><strong>Servo Motor</strong>: Representa o velocímetro, ajustando sua posição de acordo com a velocidade.</li>
        <li><strong>Monitoramento de Temperatura</strong>: O sensor DHT11 mede a temperatura que aumenta conforme a velocidade do carro.</li>
        <li><strong>Alerta de Temperatura</strong>: O buzzer soa intermitentemente quando a temperatura ultrapassa 70°C.</li>
    </ul>
    <h2>Código</h2>
    <pre>
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
const int potPin = A0;    // Pino do potenciômetro
const int buzzerPin = 10; // Pino do buzzer

// Variáveis
int sensorValue = 0;     // Valor lido do potenciômetro
float speed = 0.0;       // Velocidade simulada do carro
float temperature = 0.0; // Temperatura do motor

void setup() {
  Serial.begin(9600);         // Inicializa a comunicação serial
  pinMode(redLed, OUTPUT);    // Define o pino do LED vermelho como saída
  pinMode(greenLed, OUTPUT);  // Define o pino do LED verde como saída
  pinMode(yellowLed, OUTPUT); // Define o pino do LED amarelo como saída
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
    </pre>
    <h2>Video de apresentação</h2>
