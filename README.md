# Practica 5 DHT22, Ultrasonico y LCD.
## Introducción
En esta práctica lograremos con la ayuda de una ESP32, ULTRASÓNICO, el sensor DHT11 y LCD,  mostrar datos de temperatura, distancia y créditos.
## Materiales a utlizar
+ WOKWI
+ Tarjeta ESP32
+ Sensor DHT11
+ Sensor ULTRASONICO
+ LCD 16x2 (I2C)
## Instrucciones
### Requisitos previos
Para poder hacer uso de este repositorio se requiere entrar a la plataforma de WOKWI.
### Preparación del entorno
Abrir la terminal de programación y colocar el siguiente código:
```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int DHT_PIN = 15;
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 2;   //Pin digital 3 para el Echo del sensor

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  lcd.init();
  lcd.backlight();

}

void loop() {

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  delay(2000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Distancia: " + String(d) + "cm");
  delay(2000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Ing. Abraham");
  lcd.setCursor(0,1);
  lcd.print("Ing. Mecanica");
  delay(2000);
}
```
Instalar la libreria de LiquidCrystal I2C y DHT sensor library for ESPx como se muestra en la siguiente imagen.
![]()
![]()
## Instrucciones de operación
1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. Visualizar los datos en la pantalla LCD.
4. Colocar la distancia dando doble click al sensor ULTRASONICO.
5. Colocar los valores de temperatura y humedad dando doble click al sensor DHT11.
## Conexiones
Hacer las siguientes conexiones
![]()
## Resultados
Se mostrarás los siguientes datos una vez ejecutado.
## Créditos
Desarrollado por Ing. Alejandro Barrera
