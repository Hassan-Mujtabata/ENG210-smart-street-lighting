# 💡 ENG210-smart-street-lighting

> Arduino-based smart street lighting system that automatically controls LEDs using ambient light sensing and motion detection — powered by solar energy. Built for SDG 7: Affordable and Clean Energy.

---

## 📌 Overview

This project was built for **ENG 210 – Computer Architecture** at Canadian University Dubai. It simulates an intelligent solar-powered street lighting system that conserves energy by activating lights only when needed — at night and only when motion is detected nearby.

The system uses a **photoresistor (LDR)** to detect day/night conditions and an **ultrasonic sensor** for motion detection. An **Arduino Uno** processes the sensor inputs and controls three LEDs and an LCD display accordingly. The circuit is designed to run on solar power with a 9V battery backup.

🔗 **Tinkercad Simulation:** [View Circuit on Tinkercad](https://www.tinkercad.com/things/54fViBD3Xyl-copy-of-engineering-final-/editel)

---

## 🗂️ Project Structure

```
ENG210-smart-street-lighting/
│
├── smart_streetlight.ino     # Arduino source code
├── report/
│   └── ENG210_Project_Report.pdf   # Full report with circuit diagrams & test cases
└── README.md
```

---

## 🔧 Hardware Components

| Component | Purpose | Pin(s) |
|---|---|---|
| Arduino Uno | Central microcontroller — processes all inputs/outputs | — |
| Photoresistor (LDR) | Detects ambient light intensity (day vs. night) | A0 |
| Ultrasonic Sensor (HC-SR04) | Measures distance for motion detection | TRIG→D3, ECHO→D2 |
| LED × 3 | Street lights — illuminate based on conditions | D11, D12, D13 |
| Resistors | Current limiting for LEDs | GND |
| LCD Display (I2C) | Displays light level, distance, and system status | SDA→A4, SCL→A5 |
| Solar Cell | Charges battery during daytime | — |
| 9V Battery | Powers the system at night / backup power | — |

---

## ⚙️ Software & Libraries

| Library | Purpose |
|---|---|
| `Adafruit LiquidCrystal` | LCD communication — cursor, text display, backlight control |
| `Arduino IDE` | Code compilation and upload |

---

## 🧠 System Logic

### Day / Night Detection
- LDR reads analog value from pin A0
- `sensorval > 700` → **Daytime** — all LEDs off, system conserves energy
- `sensorval ≤ 700` → **Nighttime** — system activates, motion detection enabled

### Motion Detection (Night only)
| Distance from Ultrasonic Sensor | LED Behaviour |
|---|---|
| 0 – 100 cm | All 3 LEDs ON — close motion detected |
| 100 – 200 cm | 1 LED ON — distant motion detected |
| > 200 cm | All LEDs OFF — no motion |

### LCD Output
- Startup: displays `"Smart Streetlight"`
- Runtime: shows live light intensity value and detected distance

---

## 📐 System Design

```
[Solar Cell] → [Battery 9V] → [Arduino Uno]
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
             [LDR Sensor]  [Ultrasonic Sensor]  [LCD Display]
              (A0)          (D2, D3)             (A4, A5)
                                    │
                          [LED 1] [LED 2] [LED 3]
                          (D11)   (D12)   (D13)
```

---

## 🔁 Code Flow

**`setup()`**
1. Initialize LCD (16×2), turn on backlight
2. Display startup message `"Smart Streetlight"`
3. Configure pin modes — ECHO (D2) as INPUT, TRIG (D3) as OUTPUT
4. Begin Serial Monitor

**`loop()`**
1. Read LDR value from A0
2. Send ultrasonic pulse, calculate distance from echo time
3. Display light intensity and distance on LCD + Serial Monitor
4. If daytime (`sensorval > 700`) → turn off all LEDs
5. If nighttime → check distance and activate LEDs accordingly

---

## ✅ Test Cases

| Test | Condition | Expected Result | Status |
|---|---|---|---|
| 1 | Full simulation in Tinkercad | All components functional | ✅ Pass |
| 2 | Nighttime, object far (>200cm) | All LEDs off | ✅ Pass |
| 3 | Nighttime, object close (<100cm) | All 3 LEDs on | ✅ Pass |
| 4 | Daytime, object close | All LEDs off — LDR overrides motion | ✅ Pass |
| 5 | Daytime, object far | All LEDs off | ✅ Pass |

---

## 🌱 SDG Alignment

This project directly supports **SDG 7: Affordable and Clean Energy** by:
- Using solar power as the primary energy source
- Eliminating unnecessary energy use during daytime
- Activating lights only on motion detection at night
- Using low-cost, scalable components suitable for underdeveloped areas

---

## 🔮 Future Work

- Add infrared sensor for more accurate motion detection (removes false triggers from wind)
- Integrate traffic monitoring to adjust light timing dynamically
- Add energy consumption logging to track and optimize usage over time
- Explore additional renewable energy sources (wind, kinetic)

---

## 👥 Team

| Student | ID |
|---|---|
| **Hassan Mujtaba** | 20220002085 |
| **Maryam Alyaseen** | 20220002536 |
| **Fasmin Nizar** | 20230003378 |
| **Devanarayan KS** | 20230003282 |

---

## 🏫 Course Information

**ENG 210 – Computer Architecture**  
School of Engineering, Applied Sciences, and Technology  
Canadian University Dubai — Spring 2024–25  
Instructor: Dr. Kuljeet Kaur

---

## ⚠️ Notes

- Circuit simulation is available on Tinkercad — no physical hardware required to test
- The Adafruit LiquidCrystal library must be installed in Arduino IDE before compiling
- LDR threshold values (700) may need calibration depending on ambient lighting conditions in your environment
