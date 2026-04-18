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
