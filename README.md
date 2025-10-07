# Laboratorio Nro. 4 - Control de Temperatura con Arduino y LCD

Este laboratorio consiste en el desarrollo de un sistema de control y monitoreo de temperatura utilizando un sensor TMP36, una pantalla LCD 16x2, un LED indicador y un ventilador (motor). El sistema lee la temperatura ambiente, la muestra en la pantalla LCD, y actúa en función de ciertos rangos de temperatura para controlar el LED y el ventilador.

## Materiales

<img width="1506" height="412" alt="materiales" src="https://github.com/user-attachments/assets/d9c7b763-21da-448b-8f8d-653740b309ed" />


## Esquema de conexión

<img width="806" height="632" alt="esquema" src="https://github.com/user-attachments/assets/be22ee8d-1ec1-449a-9043-1884c1bf13ea" />


## link del Tinkercard

https://www.tinkercad.com/things/4dysGIBp4H1-stunning-blad-curcan/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard 

## Código principal
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

