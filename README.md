#Integrantes del grupo de desarrollo 

- Juan David Chaucanez Saldaña 

- Brayan Steven Delgado Gomez

- Jhon Sebastian Gomez Garcia 


# Laboratorio Nro. 4 - Control de Temperatura con Arduino y LCD 
![GitHub](https://img.icons8.com/material-outlined/48/000000/github.png)


En este laboratorio se diseñó y simuló un circuito con Arduino UNO que integra varios componentes electrónicos: un sensor de temperatura, un motor DC, un LED indicador, un transistor NPN, un diodo de protección, un potenciómetro y una pantalla LCD 16x2.
El objetivo principal es controlar el encendido del motor y del LED en función de la temperatura medida por el sensor, además de mostrar información en la pantalla LCD.

El funcionamiento del circuito se basa en la lectura analógica de la temperatura.
Cuando el valor de temperatura supera cierto umbral, el Arduino envía una señal de salida que activa el transistor NPN, permitiendo el paso de corriente hacia el motor DC, el cual comienza a girar.
Al mismo tiempo, se enciende un LED rojo que indica visualmente que el sistema está activo.
Cuando la temperatura desciende por debajo del valor establecido, tanto el motor como el LED se apagan.

El sensor de temperatura (por ejemplo, un LM35 o TMP36) está conectado a una entrada analógica del Arduino, que convierte la señal de voltaje en un valor digital para calcular la temperatura.
La pantalla LCD se utiliza para mostrar mensajes como “Hello World” o los valores medidos, aunque en la simulación, la parte que responde correctamente es la de la temperatura, ya que el encendido del LED y el movimiento del motor dependen directamente de ese dato.


## Materiales

<img width="1506" height="412" alt="materiales" src="https://github.com/user-attachments/assets/d9c7b763-21da-448b-8f8d-653740b309ed" />


## Esquema de conexión

<img width="806" height="632" alt="esquema" src="https://github.com/user-attachments/assets/be22ee8d-1ec1-449a-9043-1884c1bf13ea" />


## link del Tinkercard


[https://www.tinkercad.com/things/4dysGIBp4H1-stunning-blad-curcan/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard ](https://www.tinkercad.com/things/4dysGIBp4H1-stunning-blad-curcan)

## foto del circuito

<img width="1142" height="699" alt="image" src="https://github.com/user-attachments/assets/2019763a-0328-4300-93a1-65b4ad320422" />

El circuito incluye un diodo conectado en paralelo con el motor, que actúa como protección ante los picos de corriente generados por la bobina del motor al encenderse y apagarse.
El potenciómetro ajusta el contraste del LCD, y el transistor funciona como un interruptor controlado por la señal PWM enviada desde el Arduino.

En conclusión, el laboratorio demuestra cómo el Arduino puede leer una variable física (temperatura) y, en respuesta, activar o desactivar dispositivos eléctricos como un motor o un LED.
Aunque otros elementos del montaje, como el potenciómetro o la pantalla LCD, forman parte del circuito, el sistema que se encuentra funcionando correctamente es el de control de temperatura, que regula el motor y el LED

## Código principal ![Arduino](https://img.icons8.com/color/48/000000/arduino.png)

#include <LiquidCrystal.h>

int seconds = 0;
LiquidCrystal lcd_1(12, 11, 5, 4, 3, 2);
int const SensorPin = A0; 
int Sensor_value;
float temperature;
int const led = 13;
int const Fan1 = 10; 
void setup()
   

{
  lcd_1.begin(16, 2); 
  Serial.begin(9600);
  pinMode(led, OUTPUT);
   pinMode(Fan1, OUTPUT);
}

void loop()
{
  // set the cursor to column 0, line 1
  // (note: line 1 is the second row, since counting begins with 0)
  lcd_1.setCursor(0, 1);
  // print the number of seconds since reset:
  //lcd_1.print(seconds);
  //delay(1000);  // Wait for 1000 millisecond(s)
  //seconds += 1;
  
  Sensor_value = analogRead(SensorPin);
  float voltage = Sensor_value * (5.0 / 1023.0);   // Convertir a voltios
  temperature = (voltage - 0.5) * 100;             // Fórmula TMP36

 lcd_1.print("Temperatura: ");
  lcd_1.print(temperature);
  lcd_1.println(" C");

  if ( temperature <= 10) {
    digitalWrite(Fan1, LOW);
	digitalWrite(led, HIGH);
    delay(500);
    digitalWrite(led, LOW);
    delay(500);
  }
  
  else if (temperature >= 11 && temperature <= 25) {
	digitalWrite(led, LOW);
        digitalWrite(Fan1, LOW);
  }
  
  else if (temperature >= 26) {

digitalWrite(Fan1, HIGH);
	digitalWrite(led, HIGH);

   
}
}

