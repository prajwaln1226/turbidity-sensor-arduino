# turbidity-sensor-arduino
Arduino project for measuring water turbidity using an analog sensor, LCD display, and LEDs for status indication.
# Turbidity Sensor with Arduino

This Arduino project measures water turbidity using an analog sensor and displays the results on a 16x2 LCD via I2C. It also uses LEDs to indicate the water clarity level: clear, cloudy, or dirty.

## Components Used
- Arduino UNO (or compatible)
- Turbidity Sensor (analog output)
- 16x2 LCD with I2C interface
- 3 LEDs (Green, Yellow, Red)
- Resistors (220Ω for LEDs)
- Jumper wires and breadboard

## Working Principle
- The turbidity sensor outputs an analog voltage proportional to water clarity.
- The Arduino reads the value, maps it to a percentage, and displays it on the LCD.
- LEDs indicate:
  - **Green** → Clear water
  - **Yellow** → Cloudy water
  - **Red** → Dirty water

## Circuit Connections
| Arduino Pin | Component            |
|-------------|----------------------|
| A0          | Turbidity Sensor OUT |
| 7           | Green LED            |
| 8           | Yellow LED           |
| 9           | Red LED               |
| I2C (SDA,SCL)| LCD Display          |

## Code
```cpp
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x3F, 16, 2); // LCD at I2C address 0x3F
int sensorPin = A0;

void setup() {
  lcd.begin(16, 2);
  lcd.backlight();
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
}

void loop() {
  int sensorValue = analogRead(sensorPin);
  int turbidity = map(sensorValue, 0, 640, 100, 0);
  
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Turbidity: ");
  lcd.print(turbidity);
  
  if (turbidity < 20) {
    digitalWrite(7, HIGH);
    digitalWrite(8, LOW);
    digitalWrite(9, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Its CLEAR   ");
  }
  else if (turbidity < 50) {
    digitalWrite(7, LOW);
    digitalWrite(8, HIGH);
    digitalWrite(9, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Its CLOUDY  ");
  }
  else {
    digitalWrite(7, LOW);
    digitalWrite(8, LOW);
    digitalWrite(9, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Its DIRTY   ");
  }
  
  delay(500);
}
