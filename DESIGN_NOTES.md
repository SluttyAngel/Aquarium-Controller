# Design Notes - Aquarium ATO Controller

## Component Selection Rationale

### Microcontroller: ESP32-S3 XIAO

**Selection Reasoning**:
- Compact form factor (21.5mm × 17.8mm) fits custom PCB design
- Built-in WiFi and Bluetooth for wireless dashboard integration
- GPIO count sufficient for 3 sensors + relay + 4 LEDs
- Removable module design allows easy replacement
- Strong community support and documentation

### Water Level Sensors: XKC-Y25-NPN

**Selection Reasoning**:
- Non-contact capacitive sensing (no moving parts)
- Supports thicker glass up to 10mm
- Cleaner appearance than winged float switches
- NPN output compatible with ESP32 GPIO inputs
- More reliable in long-term aquarium use

### Pump: ZKSJ DC26D-0525S

**Specifications**:
- Voltage: 5V DC
- Power: 5W
- Estimated Current: ~1A nominal, 1.5-2.5A startup

### Relay: HF32F-G 005-HS

**Decision**: Keep Mechanical Relay
**Reasons**:
- Physical electrical isolation between control circuit and pump
- Better safety for aquarium application
- Easier troubleshooting with visual coil indicator
- Better fit for safety-critical ATO system

### Connectors: JST-XH 4-pin

**Reasons for selection**:
- Easier sourcing than JST-PH
- Stronger retention
- Easier crimping
- More common connector family

## Protection Circuit Design

### TVS Diode: SMBJ5.0A
- Input surge protection
- Placement: Between USB-C input and main power rail

### Bulk Capacitor: 470µF 10V Low ESR
- Pump startup surge suppression
- Placement: Near relay and USB-A pump output

### Relay Coil Flyback Diode: 1N4148
- Protects relay driver transistor from inductive kickback
- Placement: Across relay coil

### Pump Flyback Diode: SS14 or 1N5819
- Suppress motor kickback when relay opens
- Placement: Across pump motor (at relay output)

## Safety Considerations

### Primary Safety Features
1. **Redundant Sensors**: Two water level sensors for fail-safe operation
2. **Reservoir Check**: Prevents running on empty
3. **Timeout Protection**: 45-second pump timeout prevents endless running
4. **Electrical Isolation**: Relay isolation between control and pump circuits
5. **Surge Protection**: TVS diode and polyfuse on input
