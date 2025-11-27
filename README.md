# Soil Moisture Auto-Watering System (Arduino)

This project is a simple automated watering system that turns a pump **ON** when the soil is dry and **OFF** when the soil is wet. It uses a **capacitive soil-moisture sensor (digital output)** and a **5V relay module** to control a small DC water pump.
---
## How It Works

The Arduino reads the moisture sensor on **digital pin 6**:

* `LOW` → Soil is dry → Pump turns **ON**
* `HIGH` → Soil is wet → Pump turns **OFF**

The pump is driven through a **1-channel 5V relay module** controlled by **digital pin 3**.

The system checks soil moisture every **400 milliseconds**.

---

## Hardware Required

* **Arduino Uno R3 microcontroller board**
* **USB-A to USB-B cable** (for power and uploading code)
* **Capacitive soil-moisture sensor module** (using the digital OUT pin)
* **1-channel 5V relay module** (recommended for pump control)
* **Small DC water pump**
* **Vinyl tubing (2 × 1 m)** for water delivery
* **Dupont jumper wires** (male–male, male–female, female–female)

---

## Circuit Overview

* Capacitive moisture sensor **OUT** → `D6`

* Capacitive moisture sensor **VCC** → `5V`

* Capacitive moisture sensor **GND** → `GND`

* Relay **IN** → `D3`

* Relay **VCC** → `5V`

* Relay **GND** → `GND`

* Pump **+ (positive)** → Relay **NO** (Normally Open) terminal

* Pump **– (negative)** → Power **GND**

* Relay **COM** → Pump supply **+** (5V or external supply, depending on pump)

The relay acts as a switch: when `D3` is driven HIGH by the Arduino, the relay closes and powers the pump.

---

## Arduino Code

int water;

void setup() {
  pinMode(3, OUTPUT);    // Pump or relay output
  pinMode(6, INPUT);     // Moisture sensor digital output
  digitalWrite(3, LOW);  // Ensure pump is OFF at startup
}

void loop() {
  water = digitalRead(6);   // Read moisture level (digital)

  if (water == LOW) {
    // Soil dry -> turn pump ON
    digitalWrite(3, HIGH);
  } else {
    // Soil wet -> turn pump OFF
    digitalWrite(3, LOW);
  }

  delay(400);  // Short delay for stability
}
---

## How the Code Works (Simple Explanation)

### `setup()`

* Sets **pin 3** as an `OUTPUT` → used to control the relay that drives the pump.
* Sets **pin 6** as an `INPUT` → reads the digital signal from the capacitive soil-moisture sensor.
* Ensures the pump starts **OFF** for safety.

### `loop()`

1. Reads the digital moisture signal from pin `6` using `digitalRead(6)`.
2. If the sensor returns `LOW`, this is interpreted as **dry soil**, so the pump is switched **ON** via `digitalWrite(3, HIGH)`.
3. If the sensor returns `HIGH`, this is interpreted as **wet soil**, so the pump is switched **OFF** via `digitalWrite(3, LOW)`.
4. A short `delay(400)` prevents rapid switching and unnecessary relay chatter.

---

## Notes

* A **capacitive soil-moisture sensor** is more durable than resistive probes and less prone to corrosion. This project uses its **digital output (DO)**, which internally compares moisture to an adjustable threshold.
* The **relay module is mandatory** when driving a real DC water pump; never connect the pump directly to the Arduino pins, as they cannot supply enough current and you risk damaging the board.
* For more precise control, you can:

  * Use the **analog output (AO)** of the capacitive sensor with `analogRead(A0)` and implement a custom threshold.
  * Add longer pump run-times or more advanced timing logic instead of simply turning the pump on as long as the sensor reads dry.

---

If you want, I can also give you an updated `.ino` file using the **analog output** of the capacitive sensor (A0) and a threshold-based control instead of the digital pin 6 logic.

