# Bill of Materials (BOM) - Aquarium ATO Controller PCBA

## PCBA-Only Component List

### Power Management

| Reference | Value | Package | Description | Qty | Unit Cost | Supplier | Part Number | Notes |
|-----------|-------|---------|-------------|-----|-----------|----------|------------|-------|
| J1 | USB-C | Receptacle_24pin | USB-C Power Input | 1 | $0.50 | Digikey | 12401610E4#2A | Reversible connector |
| F1 | MF-R150 | 1206 | Polyfuse 1.5A | 1 | $0.15 | Digikey | MF-R150-2 | Self-resetting fuse |
| D1 | SMBJ5.0A | SMB | TVS Diode 5V | 1 | $0.20 | Digikey | SMBJ5.0A-E3/52 | Surge protection |
| C1 | 470µF/10V | 1206 | Bulk Capacitor (Low ESR) | 1 | $0.45 | Digikey | 493-17426-1-ND | Pump startup surge |
| C2 | 100nF | 0603 | Decoupling Capacitor | 2 | $0.08 | Digikey | 490-6328-1-ND | MCU power filtering |
| SW1 | SPST | - | Power Switch 5A | 1 | $1.50 | Amazon | Generic SPST | Manual on/off |

### Microcontroller - Socket Headers Only

| Reference | Value | Package | Description | Qty | Unit Cost | Supplier | Part Number | Notes |
|-----------|-------|---------|-------------|-----|-----------|----------|------------|-------|
| - | Female Headers | 2.54mm | Socket for XIAO | 1 | $1.50 | Digikey | 87831-1 | Allows XIAO removal (socket only) |

### Relay & Switching

| Reference | Value | Package | Description | Qty | Unit Cost | Supplier | Part Number | Notes |
|-----------|-------|---------|-------------|-----|-----------|----------|------------|-------|
| RLY1 | HF32F-G 005-HS | DIP5 | 5VDC Relay 10A | 1 | $2.50 | Digikey | HF32F-G-005-HS-ND | Mechanical relay |
| Q1 | 2N2222 | TO-92 | NPN Transistor | 1 | $0.10 | Digikey | 2N2222-ND | Relay driver |
| D2 | 1N4148 | DO-35 | Relay Flyback Diode | 1 | $0.08 | Digikey | 1N4148-E3/54-ND | Coil protection |
| D3 | SS14 | SMB | Pump Flyback Diode | 1 | $0.15 | Digikey | SS14-E3/57T-ND | Motor protection |
| R1 | 10kΩ | 0603 | Base Resistor | 1 | $0.08 | Digikey | RMCF0603FT10K-ND | Transistor base |

### Status Indicators

| Reference | Value | Package | Description | Qty | Unit Cost | Supplier | Part Number | Notes |
|-----------|-------|---------|-------------|-----|-----------|----------|------------|-------|
| LED1 | Green | 5mm | PWR Indicator | 1 | $0.15 | Digikey | 160-1447-ND | Power indicator |
| LED2 | Blue | 5mm | PUMP Indicator | 1 | $0.15 | Digikey | 160-1129-ND | Pump running |
| LED3 | Red | 5mm | ERR Indicator | 1 | $0.15 | Digikey | 160-1165-ND | Error state |
| LED4 | Amber | 5mm | RELAY Indicator | 1 | $0.15 | Digikey | 160-1074-ND | Relay energized |
| R2-R5 | 330Ω | 0603 | LED Current Limit (×4) | 4 | $0.08 | Digikey | RMCF0603FT330R-ND | ~10mA per LED |

### Connectors - Output

| Reference | Value | Package | Description | Qty | Unit Cost | Supplier | Part Number | Notes |
|-----------|-------|---------|-------------|-----|-----------|----------|------------|-------|
| J2 | USB-A | Receptacle | Pump Output | 1 | $0.80 | Digikey | 12401110-321UCF | 3A rated |

### Connectors - Sensors (JST-XH 4-pin)

| Reference | Value | Package | Description | Qty | Unit Cost | Supplier | Part Number | Notes |
|-----------|-------|---------|-------------|-----|-----------|----------|------------|-------|
| J3 | JST-XH 4pin | Right Angle | LOW SENSOR | 1 | $0.35 | Digikey | S2211-04-UL-TR-ND | Water level input |
| J4 | JST-XH 4pin | Right Angle | HIGH SENSOR | 1 | $0.35 | Digikey | S2211-04-UL-TR-ND | Water level input |
| J5 | JST-XH 4pin | Right Angle | EXPANSION | 1 | $0.35 | Digikey | S2211-04-UL-TR-ND | Future use |
| J6 | JST-XH 4pin | Right Angle | RESERVOIR SENSOR | 1 | $0.35 | Digikey | S2211-04-UL-TR-ND | Reservoir check |

## Cost Breakdown - PCBA Components Only

### Electronics Components
```
Power Management:        $2.78
Socket Headers:          $1.50
Relay & Switching:       $2.91
Status Indicators:       $2.28
Connectors Output:       $0.80
Connectors Sensors:      $1.40
─────────────────────────────
PCB Components Total:   $11.67
```

### Manufacturing
```
PCB Fabrication:        $5.00
PCBA Assembly:         $30.00
Shipping:              $15.00
─────────────────────────────
Manufacturing Total:   $50.00
```

### Grand Total (First Unit - PCBA Only)
```
PCB Components:        $11.67
Manufacturing:         $50.00
─────────────────────────────
TOTAL:                 $61.67
```

### Per-Unit Cost (5-unit batch)
```
PCB Components:        $11.67
Manufacturing (shared):$10.00 per unit
─────────────────────────────
Total per unit:        $21.67
```

## Preferred Suppliers

### Electronics Components
- **Digikey** (US/Global): Fast shipping, excellent for prototypes
- **Mouser** (US/Global): Alternative to Digikey, similar pricing
- **McMaster-Carr** (US): Good for mechanical components

### PCB & PCBA Manufacturing
- **JLC PCB** (China): Fast turnaround (3-5 days), competitive pricing
- **PCBWay** (China): Similar to JLC, slightly higher quality
- **OSH Park** (US): Higher quality, slower turnaround, educational focus

## Assembly Notes

### PCBA Assembly (Recommended)
1. Upload Gerber files + BOM to JLC PCB or PCBWay
2. Service handles stencil + solder paste
3. Service runs pick & place for all SMD components
4. Service runs reflow oven
5. Hand soldering of through-hole components:
   - Female socket headers (for XIAO)
   - USB-C connector
   - USB-A connector
   - JST-XH connectors (×4)
   - Power switch
   - LEDs (if not SMD)

### Hand Soldering Alternative (if no PCBA service)
1. Solder SMD resistors and capacitors first (0603 packages)
2. Solder diodes and transistor
3. Solder relay and through-hole connectors
4. Last: solder LEDs (verify polarity)

## Testing Components Before Assembly

| Component | Test Method | Expected Result |
|-----------|------------|-----------------|
| USB-C Connector | Visual inspection | Clean contacts, proper alignment |
| Polyfuse | Resistance measurement | ~0.3Ω at room temp |
| TVS Diode | Diode test (multimeter) | ~0.6V forward, >10MΩ reverse |
| Relay | Ohm test coil | ~70Ω resistance |
| Capacitors | ESR meter or scope | Low ESR, no leaks |
| LEDs | Power on 5V + resistor | Bright, correct color |
| JST-XH Connectors | Continuity test | No corrosion, smooth contacts |

## Cost Optimization Tips

1. **Buy in bulk**: 10-unit batches reduce per-unit PCBA cost to ~$15-17
2. **Use JLC PCB**: Cheapest option for PCBA in China
3. **Standard components**: All parts are common/in-stock
4. **No rare parts**: Easy to source from multiple suppliers
5. **Alternative relay**: SRD-05VDC-SL-C (~$1.20) instead of HF32F-G (~$2.50)

## Notes

- **XIAO ESP32-S3, pump, and sensors NOT included** - User supplies these separately
- **Socket headers only** - XIAO module plugs in, not soldered
- **Fully assembled PCBA** - All SMD and through-hole components included
- **Ready to use** - Connect XIAO, sensors, and pump to connectors

## Next Steps

1. [ ] Finalize component selections with Digikey/Mouser
2. [ ] Create CSV BOM export for PCBA service
3. [ ] Order PCB manufacturing (5-unit batch recommended)
4. [ ] Source individual components for hand soldering
5. [ ] Plan assembly and testing workflow
