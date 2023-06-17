# Practica encender led con node-red
Este repositorio muestra como podemos programar una ESP32 y como mandar una señal desde un servidor publico.

## Introducción

### Descripción

En esta practica utilizamos la ```Esp32``` y lo conectamos con un relevador, desde un servidor publico mandamos la señal para encender el led del relevador 

## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- relevador
- node-red



## Instrucciones


Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "DavidVargas";
const char* mqttPassword = "4321";
const char* mqttTopic = "Vargas95";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}
```
2. Con las librerias
![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.23.40.png?raw=true)

3. Realizar la conecion del circuito

![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.34.45.png?raw=true)

4. Se realiza el programa en bloques para node red, se coloca un swich para el boton y un mqttout para conectar el servidor publico a wokwi.

![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.37.41.png?raw=true)

5. Configurar el bloque con el puerto mqtt con el ip 44.195.202.69 como se muestra en la imagen

![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.44.29.png?raw=true)


# Resultados

Con el Dashboard podemos mandal la señal de encender al led del ESP32


![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.02.06.png?raw=true))

![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.29.18.png?raw=true))

![](https://github.com/DavidVar95/Practica-encender-led-con-node-red/blob/main/Captura%20de%20pantalla%202023-06-17%2012.29.30.png?raw=true))

# Créditos

Desarrollado por Ing. David Vargas Gonzalez

