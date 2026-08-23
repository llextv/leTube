# LeTube

**10 addressable LED tubes for stage lighting, controlled wirelessly via Art-Net and QLC+.**

## Overview

LeTube is a DIY lighting system made of **10 addressable LED tubes** designed for live shows, events and stage lighting.

The tubes use **WS2815 12 V addressable LED strips** and are controlled by a single **ESP32**. Lighting data is sent wirelessly from **QLC+ using the Art-Net protocol**.

The project was designed to provide a relatively inexpensive, reproducible and modular alternative to commercial pixel tubes.

### Features

* 10 addressable LED tubes
* WS2815 12 V addressable RGB LEDs
* Wireless Art-Net control
* Compatible with QLC+
* One ESP32 controls the complete installation
* 12 V centralized power supply
* Power distribution through dedicated busbars
* No individual controller inside each tube
* Designed for live performances and events

---

## Why I made this project

Commercial LED pixel tubes can be expensive, especially when a large number of fixtures is required.

The goal of LeTube was to build a **10-tube lighting system at a much lower cost**, while keeping it compatible with professional lighting software such as QLC+.

Instead of putting a microcontroller in every tube, a single ESP32 controls the complete installation. This reduces the number of electronic components, simplifies the wiring and makes the system easier to maintain.

---

## Pictures

### 3D / Assembly

![LeTube 3D model](images/image-18.png)

![LeTube assembly](images/image-19.png)

### PCB

The project PCB was designed using KiCad.

> Add a PCB screenshot here if available.

### WLED / Software

![WLED configuration](images/image-14.png)

---

## How it works

LeTube is a set of **10 addressable LED tubes** controlled wirelessly from a lighting control software such as QLC+.

The system uses an **ESP32 as the main controller**.

QLC+ generates the lighting data and sends it over the network using **Art-Net**. The ESP32 receives the Art-Net packets over Wi-Fi and converts the received DMX values into LED data for the WS2815 strips.

### Data flow

```text
┌──────────────┐
│    QLC+      │
│              │
│ Lighting     │
│ control      │
└──────┬───────┘
       │
       │ Art-Net over Wi-Fi
       ▼
┌──────────────┐
│    ESP32     │
│              │
│ Art-Net →    │
│ LED data     │
└──────┬───────┘
       │
       │ WS2815 data
       ▼
┌──────────────────────────┐
│       LED System         │
│                          │
│ Tube 1                   │
│ Tube 2                   │
│ Tube 3                   │
│ ...                      │
│ Tube 10                  │
└──────────────────────────┘
```

Each tube contains a **1-meter section of individually addressable WS2815 LEDs**.

This means that the LEDs are not simply switched on or off together. Each pixel can receive its own RGB color and brightness value.

For example, changing a color or intensity in QLC+ results in the corresponding pixels changing on the tubes.

---

## Power system

The LED strips are powered directly from a dedicated **12 V / 600 W power supply**.

Power is distributed through dedicated positive and negative busbars.

```text
                 ┌─────────────────────┐
                 │ 12 V / 600 W PSU    │
                 └──────────┬──────────┘
                            │
                   ┌────────┴────────┐
                   │ Power Busbars   │
                   └───────┬─────────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       Tube 1           Tube 2           Tube 3
          │                │                │
          └────────────────┼────────────────┘
                           │
                         ...
                           │
                           ▼
                        Tube 10
```

The **WS2815 strips operate at 12 V**, while the ESP32 is powered separately using an appropriate low-voltage supply.

The centralized power architecture makes the system easier to wire and avoids having a separate power supply inside every tube.

---

## Control

### QLC+

QLC+ is used as the lighting control software.

It generates the DMX values and sends them to the ESP32 using **Art-Net over Wi-Fi**.

QLC+ can therefore be used to create:

* Static colors
* RGB effects
* Chases
* Pixel effects
* Scenes
* Sequences
* Live lighting control

### WLED

The LED controller is based on the WLED ecosystem.

Official documentation:

https://kno.wled.ge/

---

## Hardware

The main hardware components are:

* ESP32
* WS2815 12 V addressable LED strip
* Aluminium LED profiles
* 12 V / 600 W power supply
* Power distribution busbars
* Wiring and connectors

See the complete [BOM](BOM.csv).

---

## Project Files

### KiCad

The KiCad project files are available in:

* [KiCad PCB](assets/tube.kicad_pcb)
* [KiCad project](assets/tube.kicad_pro)
* [KiCad schematic](assets/tube.kicad_sch)

### 3D Model

* [STEP model](cad/LeTube.step)

### Production files

The current production files are available here:

`assets/TUBEGEN.zip`

> **Note:** These files are not the final production version yet.

---

## Software

### WLED

https://kno.wled.ge/

### QLC+

QLC+ is used to generate and transmit the Art-Net lighting data.

---

## Assembly

1. Cut the aluminium profiles to the required length.
2. Install the WS2815 LED strips inside the profiles.
3. Prepare the wiring for each tube.
4. Connect the LED strips to the 12 V power distribution system.
5. Connect the LED data line to the ESP32.
6. Power the ESP32 using its dedicated low-voltage supply.
7. Configure the ESP32/WLED network connection.
8. Connect the computer running QLC+ to the same network.
9. Configure the Art-Net output in QLC+.
10. Map the DMX channels to the LED pixels.
11. Test each tube individually before using the complete installation.

> **Safety:** The 12 V power supply can deliver high current. Use appropriately sized wiring, proper fusing and secure connections. Do not work on the power system while it is energized.

---

## Known Issues

* The current production files are not final.
* The project currently relies on Wi-Fi for Art-Net communication.
* The ESP32 controls all 10 tubes from a single controller, so a controller failure affects the complete installation.
* Power distribution and cable sizing must be adapted to the final installation.
* The current design has not been optimized for professional commercial production.

---

## Credits

* **WLED** — LED control firmware and ecosystem
* **QLC+** — Lighting control software
* **ESP32** — Espressif microcontroller platform
* **WS2815** — Addressable LED technology

---

## BOM

| Category           | Item                                                      | Quantity | Unit Price (€) |    Total (€) | Notes                                |
| ------------------ | --------------------------------------------------------- | -------: | -------------: | -----------: | ------------------------------------ |
| Power Supply       | 12 V 600 W Switching Power Supply                         |        1 |          35.99 |        35.99 | Main power supply                    |
| Power Distribution | M6 Battery Bus Bar Power Distribution Block (Red + Black) |        2 |           6.50 |        13.00 | One positive and one negative busbar |
| LED Profile        | Aluminium LED Profile, 0.5 m                              |       20 |           2.55 |        51.00 | 10 m total                           |
| LED Strip          | WS2815 Addressable RGB LED Strip                          |     10 m |          6.656 |        66.56 | 12 V addressable RGB LEDs            |
| Wiring             | Electrical Wire                                           |        — |           0.00 |         0.00 | Available at home                    |
| Controller         | ESP-WROOM-32                                              |        1 |           0.00 |         0.00 | Available at home                    |
| **Total**          |                                                           |          |                | **176.55 €** | ≈ **195 USD**                        |

---

## License

Add your project license here.

For example:

```text
MIT License
```

if you want the hardware/software project to be openly reusable.