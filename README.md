# STM32 HC-05 Bluetooth Auto Configuration (AT Mode)

## 📌 Project Overview
This project demonstrates **automatic configuration of an HC-05 Bluetooth module** using an **STM32 microcontroller** (tested on Nucleo-F411RE) in **AT command mode**.

The firmware sends a predefined sequence of AT commands to:
- Verify communication with the HC-05
- Set a custom Bluetooth device name
- Set a pairing PIN
- Indicate successful configuration using an LED

The design uses **UART interrupt-based reception** and a **deterministic finite state machine (FSM)** to ensure reliable and non-blocking operation.

---

## 🎯 Project Objective
Manually configuring HC-05 modules via serial terminals is repetitive and error-prone.  
This project automates the entire process so that **each power-up configures the module consistently without user interaction**.

---

## 🧠 System Architecture
+------------------+ UART1 (38400 baud) +-------------+
| STM32 Nucleo | <----------------------------> | HC-05 Module|
| (USART1) | | (AT Mode) |
+------------------+ +-------------+
|
| UART2 (Debug Output)
v
+------------------+
| PC / PuTTY |
+------------------+

---

## ⚙️ Key Features
- ✅ UART interrupt-based RX (non-blocking)
- ✅ Finite State Machine (FSM) for AT command sequencing
- ✅ Real-time forwarding of HC-05 responses to PC terminal
- ✅ Automatic Bluetooth name & PIN configuration
- ✅ LED indication on successful configuration
- ✅ HAL-based, CubeMX-compatible code structure

---

## 🔁 Finite State Machine (FSM)

| State        | Action Performed |
|-------------|------------------|
| `SEND_AT`   | Send `AT` and wait for `OK` |
| `SEND_NAME`| Send `AT+NAME=MyNucleo` |
| `SEND_PIN` | Send `AT+PSWD="0000"` |
| `CONF_DONE`| Configuration complete, LED ON |

The FSM only advances **after receiving a valid `OK` response**, ensuring robustness.

---

## 🔧 Hardware Requirements
- STM32 Nucleo-F411RE (or compatible STM32 board)
- HC-05 Bluetooth Module
- USB cable
- External 3.3V supply (recommended for HC-05)
- LED (on-board or external)

---

## 🔌 Pin Connections

| STM32 Pin | HC-05 Pin |
|----------|----------|
| USART1_TX | RX |
| USART1_RX | TX |
| 3.3V | EN / KEY (AT mode enable) |
| GND | GND |

> ⚠️ Ensure **HC-05 is powered in AT mode** (KEY/EN high before power-up).

---

## 🖥️ UART Configuration

| Interface | Purpose | Baud Rate |
|---------|--------|-----------|
| USART1 | HC-05 Communication | 38400 |
| USART2 | Debug Output (PC) | 38400 |

---

## 🧩 Software Design Highlights

### UART Interrupt Reception
- Single-byte interrupt reception
- Bytes accumulated into a response buffer
- Command considered complete on newline (`\n`)
- Interrupt automatically re-armed

### Response Handling
- Responses forwarded live to PC terminal
- `OK` keyword detection triggers FSM transition

### LED Feedback
- LED turns ON after successful configuration

---

## 🚀 How to Use

1. Put HC-05 into **AT mode**
2. Flash the firmware to STM32
3. Open PuTTY / TeraTerm on USART2
4. Power the board
5. Observe automatic configuration messages
6. LED turns ON when configuration completes

---

## 🔮 Possible Improvements
- Timeout handling for missing responses
- EEPROM storage for dynamic name/PIN
- Support for AT command retries
- Migration to DMA-based UART RX
- Menu-based configuration via PC terminal

---

## 🧪 Tested With
- STM32 Nucleo-F411RE
- HC-05 Bluetooth Module (AT Mode @ 38400 baud)
- STM32CubeIDE
- PuTTY terminal

⭐ If this project helped you, consider giving it a star on GitHub!

