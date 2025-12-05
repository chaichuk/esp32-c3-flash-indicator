# 🏠 ESP32-C3 Garage Door Visual Monitor (ESPHome)

A compact, mains-powered visual indicator for garage door status, integrated with **Home Assistant**. 

Designed to solve the problem of monitoring a garage door status where opening sensors are present but a physical status indicator is needed inside the house and door isn't in direct vision. It uses the onboard LED of an **ESP32-C3 SuperMini** to provide real-time feedback.

## 🌟 Features

* **Real-time Status:** Monitors an existing door sensor via Home Assistant API.
* **Visual Feedback:**
    * 🚪 **Door Closed:** LED Off.
    * ⚠️ **Door Opening/Open:** Slow blinking (Strobe Warning).
    * ⬇️ **Door Closing:** Fast blinking (Fast Strobe) for 30 seconds after the "Close" button is pressed.
* **ESP32-C3 SuperMini Fixes:** Includes specific configuration to fix USB logging and WiFi stability issues common to this board.

## 🛠 Hardware

* **Board:** ESP32-C3 SuperMini (compact form factor).
* **Power:** Any USB-A/C charger.
* **Connectivity:** WiFi (2.4GHz).

## ⚙️ How it works

The device does not have physical sensors attached. Instead, it acts as a remote display for Home Assistant entities:
1.  It listens to the state of a **Door Sensor** (`binary_sensor`).
2.  It listens to the activation of a **Close Switch** (`switch`).
3.  Based on these states, it controls the onboard LED.

## 📝 Configuration (YAML)

This project uses [ESPHome](https://esphome.io/).

### Prerequisites
* Home Assistant with ESPHome Add-on installed.
* A working Door Sensor in HA.
* A working Door Control Switch in HA.

### Important Note on ESP32-C3 SuperMini
This board has a ceramic antenna and uses USB-CDC for logging. The following configuration parts are **critical**:

1.  **Fix for "Hanging Logs" / No Serial Output:**
    ```yaml
    platformio_options:
      build_flags:
        - "-DARDUINO_USB_CDC_ON_BOOT=1"
        - "-DARDUINO_USB_MODE=1"
    logger:
      hardware_uart: USB_SERIAL_JTAG
    ```

2.  **Fix for Weak WiFi / Disconnections:**
    ```yaml
    wifi:
      power_save_mode: none
      output_power: 20dB
    ```

### Installation
1.  Copy the code from `garage-monitor.yaml`.
2.  Update `ssid`, `password`, and `encryption: key`.
3.  **Crucial:** Update `entity_id` fields under `binary_sensor` to match your actual Home Assistant entities:
    * `entity_id: binary_sensor.your_door_contact`
    * `entity_id: switch.your_garage_close_switch`
4.  Flash the device via USB cable first.

## 🚥 LED States

| LED Behavior | Meaning |
| :--- | :--- |
| **OFF** | Door is Closed |
| **Slow Blink** (0.5s) | Door is Opening/Open |
| **Fast Blink** (0.1s) | Closing command received (lasts 30s) |

## 🤝 Contributing
Feel free to fork this project and add support for RGB LEDs (Neopixel) or OLED screens!
