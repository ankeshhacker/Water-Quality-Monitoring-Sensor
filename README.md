# Water-Quality-Monitoring-Sensor

An IoT-based system for monitoring pH, turbidity, and temperature of water in real time using Arduino Uno.

# Features
-> Measures pH, turbidity, and temperature in real-time.
-> Displays readings on 16x2 LCD and Serial Monitor.
-> Easily calibratable for different water quality standards.
-> (Optional) Can be upgraded to IoT-enabled using ESP32 to send data to a web dashboard.

# Hardware Components
1. Arduino Uno
2. pH Sensor (e.g., Gravity Analog pH Sensor)
3. Turbidity Sensor (SEN0189)
4. Temperature Sensor (DS18B20 or LM35)
5. 16x2 LCD (I2C module recommended)
6. Breadboard, Jumper Wires
7. Power Source (USB/Battery)

# Arduino Code
cpp
Copy
Edit
#include <LiquidCrystal.h>
#include <OneWire.h>
#include <DallasTemperature.h>

// LCD pins: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Temperature sensor
#define ONE_WIRE_BUS 6
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

// Sensor pins
const int pHSensorPin = A0;
const int turbidityPin = A1;

void setup() {
  lcd.begin(16, 2);
  Serial.begin(9600);
  sensors.begin();
  lcd.print("Water Quality");
  delay(2000);
}

void loop() {
  // Read pH
  int pHValue = analogRead(pHSensorPin);
  float voltage = pHValue * (5.0 / 1023.0);
  float pH = 3.5 * voltage;  // Approximation (calibration needed)

  // Read Turbidity<img width="1580" height="1180" alt="ck" src="https://github.com/user-attachments/assets/5ed12a37-fad1-4f72-98e4-43a29be16324" />

  int turbidityValue = analogRead(turbidityPin);
  float turbidityNTU = map(turbidityValue, 0, 1023, 0, 1000);  // Rough NTU mapping

  // Read Temperature
  sensors.requestTemperatures();
  float temperatureC = sensors.getTempCByIndex(0);

  // Display on Serial Monitor
  Serial.print("pH: "); Serial.print(pH);
  Serial.print(" | Turbidity: "); Serial.print(turbidityNTU); Serial.print(" NTU");
  Serial.print(" | Temp: "); Serial.println(temperatureC);

  // Display on LCD
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("pH: "); lcd.print(pH,1);
  lcd.setCursor(0,1);
  lcd.print("Turb: "); lcd.print(turbidityNTU); lcd.print("NTU");
  delay(2000);
}


# Manufacturing Steps
1. Assemble the circuit: Connect sensors and LCD to Arduino as per the diagram.
2. Calibrate the sensors: Use buffer solutions for pH and clean/muddy samples for turbidity.
3. Test the readings using Serial Monitor.
4. Enclosure: Place components in a waterproof box with probes extended.
5. (Optional) Upgrade to ESP32 for online monitoring via web dashboard.

# Future Improvements
1. ESP32 integration for cloud-based monitoring.
2. Data logging on SD card or Firebase database.
3. Custom PCB design for compact manufacturing.
