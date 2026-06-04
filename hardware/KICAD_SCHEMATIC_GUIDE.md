# KiCad Schematic Design Guide - ATO Controller

## Project Structure

```
hardware/kicad/
├── ATO_Controller.kicad_sch          (Main schematic file)
├── ATO_Controller.kicad_pcb          (PCB layout file)
├── ATO_Controller-cache.lib          (Component library cache)
├── sym-lib/                          (Custom symbols)
└── fp-lib/                           (Custom footprints)
```

## Schematic Sections

### 1. Power Input Section

**Components**:
- USB-C Connector (USB_C_Receptacle_24pin)
- TVS Diode (SMBJ5.0A) for surge protection
- Polyfuse (MF-R150, 1.5A hold current)
- Main 5V Power Rail (+5V)
- Ground Rail (GND)

**Design Notes**:
- TVS diode placed immediately after USB-C connector
- Polyfuse between TVS diode and main rail
- Main power rail distributes to all subsystems

### 2. MCU Section - ESP32-S3 XIAO

**Components**:
- XIAO ESP32-S3 Module (in socket headers)
- Decoupling capacitors (100nF near power pins)
- USB-C for firmware flashing (accessible on top edge)

**GPIO Assignments**:
```
GPIO1  ← LOW SENSOR input
GPIO2  ← HIGH SENSOR input
GPIO3  ← RESERVOIR SENSOR input
GPIO4  → RELAY CONTROL
GPIO5  → PUMP LED
GPIO6  → ERROR LED
GND    → Ground reference
+5V    → Power supply
```

**Notes**:
- Module plugs into female socket headers (not soldered)
- USB-C port must remain accessible
- Keep WiFi antenna clear of nearby components

### 3. Relay Control Section

**Components**:
- Relay (HF32F-G 005-HS)
- NPN Transistor (2N2222 or equivalent) for relay driver
- Relay Coil Flyback Diode (1N4148)
- Base Resistor (10kΩ) for transistor
- Collector Resistor (1kΩ) optional

**Circuit Flow**:
```
GPIO4 (XIAO) → 1kΩ Base Resistor → 2N2222 Base
2N2222 Collector → Relay Coil
Relay Coil → 1N4148 Diode (flyback protection) → GND
Relay Ground → Main GND Rail
```

**Design Notes**:
- Flyback diode protects transistor from inductive spike
- Relay coil switches pump power path
- Relay contacts rated 10A @ 5VDC (well over 3A pump requirement)

### 4. Pump Power Output Section

**Components**:
- USB-A Connector (pump output)
- Pump Flyback Diode (SS14 or 1N5819)
- Bulk Capacitor (470µF 10V Low ESR)
- Protection resistor if needed

**Circuit Flow**:
```
Main +5V → Relay Contacts → Pump Flyback Diode → USB-A +5V
470µF Bulk Cap (across +5V and GND near USB-A)
Pump Flyback Diode returns to Main GND
```

**Design Notes**:
- Bulk capacitor suppresses pump startup surge (1.5-2.5A)
- Pump flyback diode protects relay contacts from motor kickback
- USB-A connector rated for 3A continuous (matches PCB trace rating)
- Wide traces (30mil) from relay to USB-A for 3A current

### 5. Sensor Input Section

**Components**:
- 4× JST-XH 4-pin connectors (bottom edge, right-angle)
  - LOW SENSOR connector
  - HIGH SENSOR connector
  - EXPANSION connector
  - RESERVOIR SENSOR connector

**Connector Pinout** (all identical):
```
Pin 1: GND (Black wire)
Pin 2: +5V (Red wire)
Pin 3: SIGNAL (Yellow wire to GPIO)
Pin 4: NC (not connected)
```

**GPIO Connections**:
```
LOW SENSOR Signal → GPIO1 (with internal pull-up enabled in firmware)
HIGH SENSOR Signal → GPIO2 (with internal pull-up enabled in firmware)
RESERVOIR SENSOR Signal → GPIO3 (with internal pull-up enabled in firmware)
EXPANSION → Reserved for future use
```

**Design Notes**:
- All sensors use capacitive detection (XKC-Y25-NPN)
- Sensors output NPN logic (active low to GND when triggered)
- Internal GPIO pull-ups (20-50kΩ) enabled via firmware
- Connectors on bottom edge for clean wiring into enclosure

### 6. Status LED Section

**LED Indicators**:
- LED1 (Green): PWR indicator - powered directly from +5V rail
- LED2 (Blue): PUMP indicator - GPIO5 controlled
- LED3 (Red): ERR indicator - GPIO6 controlled
- LED4 (Amber): RELAY indicator - powered from relay coil

**LED Circuits**:
```
LED1 (PWR): +5V → 330Ω Resistor → Green LED → GND
LED2 (PUMP): GPIO5 → 330Ω Resistor → Blue LED → GND
LED3 (ERR): GPIO6 → 330Ω Resistor → Red LED → GND
LED4 (RELAY): Relay Coil Power → 330Ω Resistor → Amber LED → GND
```

**Design Notes**:
- 330Ω current limiting resistor for each LED
- LEDs mounted on left internal area in vertical column
- Visible through enclosure window or indicator holes
- PWR and RELAY LEDs independent of firmware

### 7. Switch Section

**Component**:
- SPST Power Switch (5A rated)

**Circuit**:
```
USB-C +5V → Power Switch → Main +5V Rail
```

**Placement**:
- Left edge of PCB, side-accessible through enclosure
- Manual on/off independent of firmware

## Schematic Creation Workflow

### Step 1: Create Project in KiCad
1. Open KiCad Project Manager
2. Create new project: `ATO_Controller`
3. Create new schematic: `ATO_Controller.kicad_sch`

### Step 2: Set Up Schematic Sheet
1. Configure page size (A4 or letter)
2. Add title block with project info
3. Add sheet number and date

### Step 3: Add Components (in order)
1. Add power input section
2. Add MCU section (XIAO socket)
3. Add relay control section
4. Add pump output section
5. Add sensor input connectors
6. Add LED indicators
7. Add power switch

### Step 4: Draw Connections
1. Connect power rails (bus lines)
2. Connect ground rails (bus lines)
3. Connect signal lines between sections
4. Add labels to all signal nets

### Step 5: Add Annotations
1. Add component reference designators
2. Add value annotations
3. Add net labels for clarity

### Step 6: Electrical Rules Check (ERC)
1. Run ERC to verify no conflicts
2. Fix any warnings or errors
3. Verify all required connections exist

### Step 7: Generate Netlist
1. Tools → Generate Netlist
2. Save as `ATO_Controller.net`
3. Review for correctness

## Component Values Summary

| Reference | Value | Package | Part Number |
|-----------|-------|---------|-------------|
| U1 | XIAO ESP32-S3 | - | Seeed Studio Module |
| D1 | SMBJ5.0A | SMB | TVS Diode |
| F1 | MF-R150 | 1206 | Polyfuse 1.5A |
| C1 | 100nF | 0603 | Decoupling Cap |
| C2 | 470µF/10V | 1206 | Bulk Cap (Low ESR) |
| Q1 | 2N2222 | TO-92 | NPN Transistor |
| RLY1 | HF32F-G 005-HS | DIP | 5V Relay |
| D2 | 1N4148 | DO-35 | Relay Flyback Diode |
| D3 | SS14 | SMB | Pump Flyback Diode |
| R1 | 10kΩ | 0603 | Relay Base Resistor |
| R2-R5 | 330Ω | 0603 | LED Current Limit (×4) |
| LED1 | Green | 5mm | PWR Indicator |
| LED2 | Blue | 5mm | PUMP Indicator |
| LED3 | Red | 5mm | ERR Indicator |
| LED4 | Amber | 5mm | RELAY Indicator |
| SW1 | SPST | - | Power Switch |
| J1 | USB-C | Receptacle | Power Input |
| J2 | USB-A | Receptacle | Pump Output |
| J3-J6 | JST-XH 4-pin | Right Angle | Sensor Connectors |

## Design Tips

1. **Ground plane**: Consider 2-layer PCB with ground plane on bottom
2. **Power distribution**: Use wide traces (30mil) for 3A pump path
3. **Signal integrity**: Keep sensor signal lines away from power lines
4. **Relay protection**: Flyback diodes are critical for reliability
5. **Testing**: Add test points for voltage rails during development

## Next Steps

After schematic is complete:
1. Peer review schematic for errors
2. Create PCB layout from schematic
3. Route traces for power and signals
4. Generate Gerber files for manufacturing
5. Create BOM for component sourcing
