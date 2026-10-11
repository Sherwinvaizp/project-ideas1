# project-ideas1

/*
  Project: Solar-Powered Wireless Charging System
  Platform: Arduino Uno
  Purpose: Monitor solar panel voltage and enable
           the wireless charging transmitter when
           sufficient input voltage is available.

  Hardware:
  - Solar panel
  - Voltage divider
  - Arduino Uno
  - Compatible wireless power transmitter module
  - Driver/enable interface appropriate for the module
*/

const int solarVoltagePin = A0;
const int transmitterEnablePin = 8;

// Example threshold for a 5V Arduino supply.
// Adjust according to your panel and circuit.
const float minimumSolarVoltage = 4.0;

// Voltage divider: R1 from panel+ to A0,
// R2 from A0 to GND.
const float R1 = 10000.0;
const float R2 = 10000.0;
const float referenceVoltage = 5.0;
const float adcResolution = 1023.0;

void setup() {
  Serial.begin(9600);

  pinMode(transmitterEnablePin, OUTPUT);
  digitalWrite(transmitterEnablePin, LOW);

  Serial.println("Solar Wireless Charging System");
  Serial.println("--------------------------------");
}

void loop() {
  int sensorValue = analogRead(solarVoltagePin);

  float adcVoltage =
      sensorValue * referenceVoltage / adcResolution;

  float solarVoltage =
      adcVoltage * (R1 + R2) / R2;

  Serial.print("Solar Panel Voltage: ");
  Serial.print(solarVoltage, 2);
  Serial.println(" V");

  if (solarVoltage >= minimumSolarVoltage) {
    digitalWrite(transmitterEnablePin, HIGH);
    Serial.println("Solar input sufficient.");
    Serial.println("Transmitter enable signal ON.");
  } else {
    digitalWrite(transmitterEnablePin, LOW);
    Serial.println("Solar input insufficient.");
    Serial.println("Transmitter enable signal OFF.");
  }

  Serial.println("--------------------------------");
  delay(1000);
}
