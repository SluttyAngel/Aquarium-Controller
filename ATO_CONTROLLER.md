# Wireless Aquarium ATO Controller

A custom ESP32-S3 based Auto Top-Off (ATO) controller for aquarium automation with wireless dashboard integration.

## Project Overview

### Goals
- Automatic water top-off based on dual water level sensors
- Wireless status reporting to Waveshare ESP32-S3 Touch Display
- Manual override control via dashboard
- Reservoir monitoring and error states
- Safety-critical pump control with timeout protection

## System Architecture

```
Waveshare ESP32-S3 Touch Display
(Battery Powered Dashboard)
            |
            | WiFi / ESP-NOW
            |
            v
ATO Controller Node
(Custom ESP32-S3 XIAO PCBA)
            |
            +-- Low Water Sensor (XKC-Y25-NPN)
            +-- High Water Sensor (XKC-Y25-NPN)
            +-- Reservoir Sensor (XKC-Y25-NPN)
            +-- Pump Relay (HF32F-G 005-HS)
```

## Hardware Specifications

### Microcontroller
- **Module**: Seeed Studio XIAO ESP32-S3
- **Features**: WiFi, Bluetooth, Removable module (not soldered)
- **Mounting**: Female socket headers for easy replacement

### Power System
- **Input**: USB-C connector
- **Voltage**: 5V DC
- **Recommended Adapter**: 5V 3A
- **Polyfuse**: MF-R150 (1.5A hold current)
- **Bulk Capacitor**: 470µF 10V Low ESR

### Water Level Sensors
- **Model**: XKC-Y25-NPN (Capacitive, Non-contact)
- **Quantity**: 3 (Low, High, Reservoir)
- **Connector**: JST-XH 4-pin Right Angle

### Pump Control
- **Relay**: HF32F-G 005-HS (5VDC, 10A @ 250VAC/30VDC)
- **Pump**: ZKSJ DC26D-0525S (5V, 5W)
- **Output**: USB-A connector

## Next Steps

1. [ ] Complete KiCad schematic design
2. [ ] Review and validate schematic
3. [ ] Create PCB layout
4. [ ] Generate BOM and Gerber files

## License

MIT License
