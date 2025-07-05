# Automatisierung für Bambu Lab Drucker mit Home Assistant

Diese Home Assistant Automationen steuern zwei Funktionen für Bambu Lab Drucker (X1C und P1S):

1. **Automatisches Abschalten der Drucker** (per Tasmota-Steckdose), wenn sie im Leerlauf sind und die Düse abgekühlt ist.
2. **Automatisches Einschalten der USB-Beleuchtung**, sobald einer der Drucker läuft, und Ausschalten, wenn keiner mehr läuft.

---

### ✨ Funktionen

#### 🔌 Drucker-Automation:
- Erkennt Druckstatus (`idle`, `finish`, `failed`)
- Prüft, ob Düsentemperatur ≤ 50 °C ist
- Schaltet die Tasmota-Steckdose automatisch ab
- Protokolliert Aktionen im System-Log

#### 💡 USB-Licht-Automation:
- Schaltet `switch.drucker_usb_licht` **ein**, wenn `drucker_x1c` **oder** `drucker_p1s` an sind
- Schaltet es **aus**, wenn **beide** Drucker aus sind

---

### 📦 Voraussetzungen

- [Bambu Lab Integration (via HACS)](https://github.com/nickw444/home-assistant-bambulab)
- Eine über MQTT angebundene [**Tasmota-Steckdose**] (https://www.amazon.de/dp/B0054PSH9C?ref=ppx_yo2ov_dt_b_fed_asin_title&th=1)
- Home Assistant mit Automationen in YAML oder UI

---

### ⚙️ Logik – Druckerabschaltung

Ein Drucker wird **automatisch ausgeschaltet**, wenn:

- Er seit **mindestens 10 Minuten** in einem dieser Zustände ist:
  - `idle` *(Leerlauf)*
  - `finish` *(Druck beendet)*
  - `failed` *(Fehlgeschlagen)*
- Die Düsentemperatur ist **≤ 50 °C**
- Die Steckdose ist noch **eingeschaltet**

---

### ⚙️ Logik – USB-Beleuchtung

- Sobald **einer der Drucker eingeschaltet ist**, wird das USB-Licht **aktiviert**
- Sobald **beide Drucker ausgeschaltet** sind, wird es **deaktiviert**

---

### 🧩 Beispielhafte Entitäten

```yaml
switch.drucker_x1c
switch.drucker_p1s
switch.drucker_usb_licht
sensor.x1c_druckstatus
sensor.p1s_druckstatus
sensor.x1c_temperatur_der_duse
sensor.p1s_temperatur_der_duse

----
# Home Assistant Automations for Bambu Lab Printers

These Home Assistant automations manage two behaviors for Bambu Lab printers (X1C and P1S):

1. **Automatic shutdown** via smart plugs when the printer is idle and the nozzle is cool.
2. **Automatic USB light control**: turns on when any printer is on, and off when both are off.

---

## ✨ Features

### 🔌 Printer Shutdown Automation
- Monitors print states: `idle`, `finish`, `failed`
- Checks if nozzle temperature is ≤ 50 °C
- Turns off the smart plug automatically
- Logs shutdown events to Home Assistant system log

### 💡 USB Light Automation
- Turns **on** `switch.drucker_usb_licht` when **either** `drucker_x1c` or `drucker_p1s` is on
- Turns **off** the light when **both** printers are off

---

## 📦 Requirements

- [Bambu Lab Integration (via HACS)](https://github.com/nickw444/home-assistant-bambulab)
  - Provides printer state and nozzle temperature as sensors
- One **Tasmota-based smart plug** per printer (via MQTT)
- Home Assistant with YAML or UI-based automation support

---

## ⚙️ Shutdown Logic

A printer will automatically shut down if:

- It has been in one of the following states for **at least 10 minutes**:
  - `idle`
  - `finish`
  - `failed`
- The nozzle temperature is **≤ 50 °C**
- The smart plug (`switch.drucker_x1c` or `switch.drucker_p1s`) is **still on**

---

## ⚙️ USB Light Logic

- If **either** printer is **on**, the USB light is **turned on**
- If **both** printers are **off**, the USB light is **turned off**

---

## 🧩 Example Entities

```yaml
switch.drucker_x1c
switch.drucker_p1s
switch.drucker_usb_licht
sensor.x1c_druckstatus
sensor.p1s_druckstatus
sensor.x1c_temperatur_der_duse
sensor.p1s_temperatur_der_duse
