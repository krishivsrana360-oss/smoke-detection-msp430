# Smoke Detection System — MSP430FRxxxx

Bare metal firmware implementation of a dual-wavelength optical smoke 
detection system built during industrial training at Teq Diligent, Ahmedabad.

## System Overview

A three-layer architecture for industrial-grade smoke detection:

- **Sensing Layer** — Optical smoke sensor with dual LED (470nm Blue + 850nm IR),
  2 photodetectors, 14-bit ADC in a 3.8×5mm package
- **Processing Layer** — MSP430FRxxxx MCU handling I²C sensor read, signal 
  processing, frame assembly, and UART TX
- **Host Layer** — Python script for live real-time PTR graph via matplotlib

## Signal Flow
Smoke Particles → Optical Sensor —[I²C @ 100kHz]→ MSP430FRxxxx
—[UART/RS485 @ 9600 baud]→ Python Script → Real-Time PTR Graph

## Key Features

- Dual-wavelength detection — distinguishes true smoke from dust/fog/steam
- Zero false alarms in all testing sessions
- Custom 21-byte frame protocol with hardware CRC16 validation
- I²C fault detection with 9-clock bus recovery
- eFUSE gain calibration for per-unit sensor accuracy
- All firmware variables stored in FRAM — no Flash write overhead
- Foreground/background firmware model (ISR + main loop)

## Tech Stack

- **MCU** — MSP430FRxxxx (16-bit RISC, FRAM, ultra-low-power)
- **Sensor** — Dual-wavelength optical smoke sensor (I²C, address 0x64)
- **Protocols** — I²C (100kHz), UART 8N1 (9600 baud), RS485 half-duplex
- **Tools** — Code Composer Studio (CCS), Python + pyserial + matplotlib
- **Language** — C (bare metal, no RTOS)

## Signal Processing Pipeline

1. Raw 32-bit ADC read from Blue (470nm) and IR (850nm) channels
2. 8-sample moving average filter (circular buffer in FRAM)
3. LED peak current calculation
4. Photodetector current (IPD) computation
5. PTR calculation with eFUSE gain calibration
6. Baseline tracking with exponential moving average
7. Obscuration calculation → alarm trigger at β > 5.0 %/ft

## Frame Protocol

**Request (4 bytes):** SOF 0xAA | CMD 0x01 | Frame Count | CRC16

**Response (21 bytes):** SOF | Frame Count | Raw ADC Blue+IR | 
PTR Blue+IR | Obscuration Blue+IR | Alarm Status | CRC16

## Test Results

| Test | Result |
|------|--------|
| I²C register read-back | PASS |
| UART echo-back at 9600 baud | PASS |
| CRC16 corruption detection | PASS |
| Smoke detection (Blue PTR spike) | PASS |
| Alarm trigger and auto-reset | PASS |
| False alarm under dust/ambient | ZERO |

## Hardware

- MSP-EXP430FR5969 LaunchPad development board
- Optical smoke sensor (Analog Devices)
- RS485 transceiver module
- 4.7kΩ pull-up resistors on I²C bus (1.8V logic)

## Built At

Industrial Training — Teq Diligent, Ahmedabad  
