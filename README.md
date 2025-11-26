# Soil Moisture Auto-Watering System (Arduino)

This project is a simple automated watering system that turns a pump ON when the soil is dry and OFF when the soil is wet. It uses a digital soil-moisture sensor and a relay or output pin to control a water pump.

## **How It Works**

The Arduino reads the moisture sensor on **digital pin 6**:

* **LOW** → Soil is dry → Pump turns **ON**
* **HIGH** → Soil is wet → Pump turns **OFF**

The pump or relay is controlled through **digital pin 3**.

The system checks soil moisture every 400 milliseconds.

---

## **Hardware Required**

* Arduino Uno/Nano
* Digital soil-moisture sensor
* Relay module (recommended for pump control)
* Water pump (5V or external power + relay)
* Jumper wires

---

## **Circuit Overview**

* Moisture sensor OUT → **Pin 6**
* Relay IN (or output device) → **Pin 3**
* Sensor VCC → 5V
* Sensor GND → GND
* Relay VCC/GND → 5V/GND
* Pump → Relay output terminals

---

## **Arduino Code**

int water;

void setup() {
  pinMode(3, OUTPUT);   // Pump or relay output
  pinMode(6, INPUT);    // Moisture sensor input
  digitalWrite(3, LOW); // Ensure pump is OFF at startup
}

void loop() {
  water = digitalRead(6);   // Read moisture level

  if (water == LOW) {
    digitalWrite(3, HIGH);  // Soil dry → turn pump ON
  } else {
    digitalWrite(3, LOW);   // Soil wet → turn pump OFF
  }

  delay(400);               // Short delay for stability
}

## **How the Code Works (Simple Explanation)**

1. **setup()**

   * Pin 3 is set as an OUTPUT → used to control the pump/relay.
   * Pin 6 is set as an INPUT → receives digital moisture readings.
   * The pump is turned off at startup for safety.

2. **loop()**

   * Reads the moisture level.
   * If the sensor returns **LOW**, the soil is dry, so the pump is switched ON.
   * If the sensor returns **HIGH**, the soil has enough moisture, so the pump is switched OFF.
   * A short delay prevents unnecessary rapid switching.

---

## **Notes**

* Many soil-moisture sensors output noisy signals; using a relay is strongly recommended to protect the Arduino.
* For longer pump run-times or stability, you can add timing logic or analog calibration.
