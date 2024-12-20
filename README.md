# DHT22-CON-ULTRASONICO-CON-LCD

**Objetivo**

El objetivo de esta práctica es integrar el microcontrolador ESP32 con varios sensores, como el DHT22 para medir temperatura y humedad, el HC-SR04 para medir distancia, y una pantalla LCD para visualizar los datos en tiempo real. Esta actividad permite explorar la interacción de múltiples sensores con la placa ESP32, creando un sistema funcional y práctico.
___________________________

**Material**

•	Placa ESP32: Microcontrolador con capacidades Wi-Fi y Bluetooth, ideal para aplicaciones IoT.

•	Sensor DHT22: Sensor digital para la medición precisa de temperatura y humedad.

•	Sensor HC-SR04: Sensor ultrasónico que permite medir distancias en centímetros.

•	Pantalla LCD 20x4 con I2C: Pantalla de 20x4 caracteres con interfaz I2C para mostrar los datos obtenidos de los sensores.

•	Plataforma Wokwi: Herramienta de simulación electrónica en línea que facilita el diseño y prueba de circuitos sin necesidad de hardware físico.


________________________________
**Diagrama de Conexión**

•	Sensor DHT22: El pin de señal del sensor está conectado al GPIO 15 del ESP32.

•	Sensor HC-SR04: El pin Trigger está conectado al GPIO 4 y el pin Echo al GPIO 2 del ESP32.

•	Pantalla LCD 20x4: Se conecta a la ESP32 utilizando el protocolo I2C con la dirección I2C configurada a 0x27.

![](https://github.com/marcorea97/DHT22-CON-ULTRASONICO-CON-LCD/blob/main/LCDMODCONEXION.png)
_____________________________

**Descripción del Código**

El código tiene como objetivo leer los datos de los sensores DHT22 y HC-SR04 y mostrar la información tanto en el monitor serie como en la pantalla LCD. El flujo de ejecución se describe a continuación:

1.	Configuración del sensor DHT22: El sensor se inicializa en el pin GPIO 15.
   
2.	Configuración del sensor HC-SR04: Los pines Trigger y Echo se configuran para medir la distancia mediante ultrasonido.
	
3.	Pantalla LCD: La pantalla LCD muestra la temperatura, la humedad y la distancia, alternando entre los valores cada 2 segundos.
	
4.	Monitor Serie: Los valores de temperatura, humedad y distancia también se envían al monitor serie para su visualización en la computadora.

___________________________________________-


**Código**

const int Trigger = 4; // Pin Trigger del sensor
const int Echo = 2; // Pin Echo del sensor

#include <LiquidCrystal_I2C.h> // Librería para LCD
#define I2C_ADDR 0x27
#define LCD_COLUMNS 20
#define LCD_LINES 4
#include "DHTesp.h" // Librería para DHT

const int DHT_PIN = 15; // Pin del sensor de temperatura y humedad

DHTesp dhtSensor;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();
  pinMode(Trigger, OUTPUT); // Pin Trigger como salida
  pinMode(Echo, INPUT); // Pin Echo como entrada
  digitalWrite(Trigger, LOW); // Inicializa el pin Trigger en LOW
}

void loop() {
  long t; // Tiempo de eco
  long d; // Distancia en centímetros

  digitalWrite(Trigger, HIGH); 
  delayMicroseconds(10); // Envío de pulso ultrasónico
  digitalWrite(Trigger, LOW);

  t = pulseIn(Echo, HIGH); // Medición del tiempo de eco
  d = t / 59; // Cálculo de la distancia en cm

  // Lectura de temperatura y humedad
  TempAndHumidity data = dhtSensor.getTempAndHumidity();
  Serial.println("Temperatura: " + String(data.temperature, 1) + "°C");
  Serial.println("Humedad: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  Serial.print("Distancia: ");
  Serial.print(d); // Imprime la distancia en serie
  Serial.println(" cm");
  Serial.println("---");
  
  delay(2000); // Espera de 2 segundos

  // Pantalla LCD
  lcd.clear();
  lcd.setCursor(4, 0);
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
  delay(2000);

  lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Marco Rea");
  lcd.setCursor(6, 1);
  lcd.print("Ing. Mecanica");
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Temperatura: " + String(data.temperature, 1) + "\xDF" + "C ");
  lcd.setCursor(0, 1);
  lcd.print("Humedad: " + String(data.humidity, 1) + "% ");
  delay(2000);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distancia: " + String(d) + " cm");
  delay(2000);
}

___________________________

**Explicación del Código**
•	Sensor HC-SR04: El sensor ultrasónico HC-SR04 utiliza el pin Trigger para emitir una señal y el pin Echo para recibir el rebote. El tiempo que tarda el eco en regresar se utiliza para calcular la distancia.

•	Sensor DHT22: El sensor DHT22 se configura para leer la temperatura y la humedad, y los valores obtenidos se imprimen en el monitor serie y en la pantalla LCD.

•	Pantalla LCD: La pantalla se actualiza cada 2 segundos para mostrar alternativamente la temperatura, la humedad y la distancia. Se utilizan las funciones lcd.setCursor() y lcd.print() para posicionar y mostrar los datos.

•	Monitor Serie: Los datos también se envían al monitor serie para visualizarlos en la computadora.

____________________________

**Resultados Esperados**


![](
https://github.com/marcorea97/DHT22-CON-ULTRASONICO-CON-LCD/blob/main/3.png
)

![](https://github.com/marcorea97/DHT22-CON-ULTRASONICO-CON-LCD/blob/main/4.png)

![](https://github.com/marcorea97/DHT22-CON-ULTRASONICO-CON-LCD/blob/main/5%20TEMP.png)

![](https://github.com/marcorea97/DHT22-CON-ULTRASONICO-CON-LCD/blob/main/6%20DIST.png)


Al ejecutar el código, los resultados esperados son los siguientes:

•	Monitor Serie: Se visualizarán los valores de temperatura, humedad y distancia en la consola del monitor serie.

•	Pantalla LCD: La información en la pantalla LCD se actualizará cada 2 segundos, alternando entre la temperatura, humedad y distancia.

______________________________________

**Conclusión**

Esta práctica demuestra cómo integrar diversos sensores con la placa ESP32 y utilizar una pantalla LCD para visualizar los datos en tiempo real. La combinación del sensor DHT22 para medir temperatura y humedad, junto con el HC-SR04 para medir distancias, ofrece una solución efectiva para aplicaciones de monitoreo ambiental y sistemas IoT. 
