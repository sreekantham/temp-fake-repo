# Interface Specifications - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document defines all external interfaces for the PM-5000 PMIC, including electrical, digital communication, and mechanical interfaces.

## 2. Pin Configuration

### 2.1 Pin Assignment Table

| Pin # | Name | Type | Description |
|-------|------|------|-------------|
| 1-2 | VIN | Power | Input power supply |
| 3 | EN | Digital Input | Master enable (active high) |
| 4 | ADDR0 | Digital Input | I2C address bit 0 |
| 5 | ADDR1 | Digital Input | I2C address bit 1 |
| 6 | NC | - | No connect |
| 7-8 | SW1 | Power | Channel 1 switch node |
| 9-11 | VOUT1 | Power | Channel 1 output |
| 12 | GND | Ground | Ground |
| 13 | SDA | Digital I/O | I2C data (open-drain) |
| 14 | SCL | Digital Input | I2C clock |
| 15 | INT# | Digital Output | Interrupt output (open-drain, active low) |
| 16-17 | NC | - | No connect |
| 18-24 | GND | Ground | Ground |
| 25-31 | GND | Ground | Ground |
| 32-33 | SW2 | Power | Channel 2 switch node |
| 34-36 | VOUT2 | Power | Channel 2 output |
| 37-38 | SW3 | Power | Channel 3 switch node |
| 39 | NC | - | No connect |
| 40-42 | VOUT3 | Power | Channel 3 output |
| 43-45 | VOUT4 | Power | Channel 4 output |
| 46-48 | GND | Ground | Ground |
| PAD | EPAD | Thermal | Exposed thermal pad (must connect to GND) |

## 3. Power Interface Specifications

### 3.1 Input Power (VIN)

**Description:** Main input power supply for all internal circuits and output stages.

**Electrical Characteristics:**
| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Input voltage (operating) | 5.0 | 12.0 | 12.0 | V | Normal operation |
| Input voltage (no damage) | -0.3 | - | 16.0 | V | No damage range |
| Quiescent current | - | 15 | 25 | mA | All channels enabled, no load |
| Shutdown current | - | 20 | 50 | µA | All channels disabled |
| Input capacitance (required) | 10 | 22 | 100 | µF | Ceramic, X7R/X5R |

**Input Filter Requirements:**
- Minimum bulk capacitance: 10µF ceramic + 47µF electrolytic (optional)
- ESR: <100mΩ
- Voltage rating: >16V
- Place within 5mm of VIN pins

### 3.2 Output Power (VOUT1-4)

**Description:** Four independent programmable voltage outputs.

**Electrical Characteristics:**
| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Output voltage range | 0.5 | - | 5.0 | V | Programmable |
| Output voltage accuracy | - | ±0.05 | ±0.1 | % | At 25°C |
| Output current (continuous) | 0 | - | 3.0 | A | Per channel |
| Output current (peak, 100ms) | - | - | 3.5 | A | Per channel |
| Output capacitance (required) | 22 | 47 | 220 | µF | Ceramic, X7R |
| Output capacitance (ESR) | - | - | 50 | mΩ | Maximum ESR |

**Output Filter Requirements:**
- Minimum output capacitance: 22µF per channel
- Recommended: 47µF ceramic (22µF × 2 in parallel)
- Voltage rating: >10V
- Dielectric: X7R or X5R (not Y5V)
- Place within 5mm of VOUT pins

### 3.3 Switch Nodes (SW1-4)

**Description:** Switching nodes for buck converter stages. **CAUTION:** High dV/dt signals.

**Electrical Characteristics:**
| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Switch node voltage | 0 | - | VIN | V | - |
| Switch frequency | 0.9 | 1.0 | 1.1 | MHz | - |
| Rise time | - | 10 | 20 | ns | 10%-90% |
| Fall time | - | 10 | 20 | ns | 90%-10% |

**External Inductor Requirements:**
| Parameter | Value | Tolerance | Notes |
|-----------|-------|-----------|-------|
| Inductance | 2.2µH | ±20% | Per channel |
| DC resistance (DCR) | <20mΩ | - | Lower is better |
| Saturation current | >4A | - | Per channel |
| RMS current rating | >3.5A | - | Per channel |
| Type | Shielded | - | Recommended for low EMI |

**PCB Layout Considerations:**
- Minimize trace length from SW pin to inductor (<5mm)
- Keep SW traces away from sensitive signals
- Use thick traces (>50mil) or copper pours
- No vias in high-frequency path if possible

### 3.4 Ground (GND)

**Description:** Ground reference for all circuits.

**Requirements:**
- Multiple ground pins must all be connected to ground plane
- Thermal pad (EPAD) must be connected to ground
- Use solid ground plane (no splits near device)
- Separate power ground and signal ground with vias
- Kelvin sensing recommended for critical measurements

## 4. Digital Interface Specifications

### 4.1 I2C Interface

**Description:** Primary digital control and monitoring interface.

#### 4.1.1 Electrical Characteristics

| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Input high voltage (VIH) | 2.0 | - | 5.5 | V | - |
| Input low voltage (VIL) | -0.3 | - | 0.8 | V | - |
| Input current | - | - | ±10 | µA | Leakage |
| Output low voltage (VOL) | - | - | 0.4 | V | At 3mA sink |
| Pull-up resistor | 2.2 | 4.7 | 10 | kΩ | External, required |
| Bus capacitance | - | - | 400 | pF | Maximum |

#### 4.1.2 Timing Characteristics

**Standard Mode (100kHz):**
| Parameter | Symbol | Min | Max | Unit |
|-----------|--------|-----|-----|------|
| SCL clock frequency | fSCL | 0 | 100 | kHz |
| SCL low period | tLOW | 4.7 | - | µs |
| SCL high period | tHIGH | 4.0 | - | µs |
| SDA setup time | tSU;DAT | 250 | - | ns |
| SDA hold time | tHD;DAT | 0 | 3.45 | µs |
| SCL/SDA rise time | tr | - | 1000 | ns |
| SCL/SDA fall time | tf | - | 300 | ns |

**Fast Mode (400kHz):**
| Parameter | Symbol | Min | Max | Unit |
|-----------|--------|-----|-----|------|
| SCL clock frequency | fSCL | 0 | 400 | kHz |
| SCL low period | tLOW | 1.3 | - | µs |
| SCL high period | tHIGH | 0.6 | - | µs |
| SDA setup time | tSU;DAT | 100 | - | ns |
| SDA hold time | tHD;DAT | 0 | 0.9 | µs |
| SCL/SDA rise time | tr | - | 300 | ns |
| SCL/SDA fall time | tf | - | 300 | ns |

#### 4.1.3 I2C Address Configuration

**Base Address:** 0x40 (7-bit address)

**Address Selection via ADDR Pins:**
| ADDR1 | ADDR0 | I2C Address (7-bit) | I2C Address (8-bit W) | I2C Address (8-bit R) |
|-------|-------|---------------------|----------------------|----------------------|
| 0 | 0 | 0x40 | 0x80 | 0x81 |
| 0 | 1 | 0x41 | 0x82 | 0x83 |
| 1 | 0 | 0x42 | 0x84 | 0x85 |
| 1 | 1 | 0x43 | 0x86 | 0x87 |

**Address Pin Connection:**
- Logic 0: Connect to GND via 10kΩ resistor
- Logic 1: Connect to VDD (3.3V or 5V) via 10kΩ resistor
- Do not leave floating

#### 4.1.4 I2C Transaction Format

**Write Transaction:**
```
S | ADDR+W | ACK | REG | ACK | DATA | ACK | P
```

**Read Transaction:**
```
S | ADDR+W | ACK | REG | ACK | Sr | ADDR+R | ACK | DATA | NACK | P
```

**Legend:**
- S = Start condition
- Sr = Repeated start
- P = Stop condition
- ACK = Acknowledge (from slave)
- NACK = Not acknowledge (from master)

#### 4.1.5 I2C Features

- **Clock Stretching:** Supported during ADC conversion
- **General Call:** Not supported
- **10-bit Addressing:** Not supported
- **Multi-master:** Supported (as slave only)

### 4.2 Enable Input (EN)

**Description:** Master enable control input.

**Electrical Characteristics:**
| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Input high voltage (VIH) | 2.0 | - | 5.5 | V | Enable asserted |
| Input low voltage (VIL) | -0.3 | - | 0.8 | V | Disable |
| Input current | - | - | ±1 | µA | Leakage |
| Internal pull-down | - | 100 | - | kΩ | Can leave floating for disable |
| Response time (EN low→high) | - | - | 100 | µs | To start soft-start |
| Response time (EN high→low) | - | - | 10 | µs | To disable outputs |

**Functionality:**
- EN = LOW: All outputs disabled, device in low-power mode
- EN = HIGH: Outputs can be enabled via I2C or default configuration
- Overrides I2C enable commands when LOW
- Can be tied to VIN for always-enabled operation

### 4.3 Interrupt Output (INT#)

**Description:** Interrupt/alert output signal (active low, open-drain).

**Electrical Characteristics:**
| Parameter | Min | Typ | Max | Unit | Notes |
|-----------|-----|-----|-----|------|-------|
| Output low voltage (VOL) | - | - | 0.4 | V | At 3mA sink |
| Leakage current (high) | - | - | 1 | µA | - |
| Pull-up resistor | 4.7 | 10 | 47 | kΩ | External, required |
| Response time | - | - | 10 | µs | Fault to assertion |

**Trigger Conditions:**
- Over-voltage fault (any channel)
- Over-current fault (any channel)
- Thermal warning or shutdown
- Under-voltage lockout
- Power-good transition (if enabled)
- Any programmable threshold crossing (if enabled)

**Clearing:**
- Read fault status registers to clear interrupt
- Remains asserted until fault condition cleared AND status read
- Can be masked per-fault via interrupt mask register

## 5. Register Interface

### 5.1 Register Map Overview

**Address Space:** 8-bit addresses (0x00 - 0xFF)

**Register Types:**
- **RO:** Read-only
- **R/W:** Read/write
- **R/WC:** Read, write to clear
- **R/W1S:** Read, write 1 to set

### 5.2 Device Identification Registers

| Address | Name | Type | Default | Description |
|---------|------|------|---------|-------------|
| 0x00 | DEVICE_ID_0 | RO | 0x50 | Device ID byte 0 (ASCII 'P') |
| 0x01 | DEVICE_ID_1 | RO | 0x4D | Device ID byte 1 (ASCII 'M') |
| 0x02 | DEVICE_REV | RO | 0x01 | Device revision |
| 0x03 | FIRMWARE_VER | RO | 0x10 | Firmware version (1.0) |

### 5.3 Channel Control Registers (Repeated for CH1-4)

**Base addresses:** CH1=0x10, CH2=0x20, CH3=0x30, CH4=0x40

| Offset | Name | Type | Default | Description |
|--------|------|------|---------|-------------|
| +0x00 | CHx_ENABLE | R/W | 0x00 | Channel enable control |
| +0x01 | CHx_VSET_H | R/W | 0x0C | Voltage setpoint high byte |
| +0x02 | CHx_VSET_L | R/W | 0x80 | Voltage setpoint low byte (12-bit) |
| +0x03 | CHx_ILIMIT_H | R/W | 0x0C | Current limit high byte |
| +0x04 | CHx_ILIMIT_L | R/W | 0x00 | Current limit low byte (12-bit) |
| +0x05 | CHx_VMON_H | RO | - | Voltage monitor high byte (14-bit) |
| +0x06 | CHx_VMON_L | RO | - | Voltage monitor low byte |
| +0x07 | CHx_IMON_H | RO | - | Current monitor high byte (12-bit) |
| +0x08 | CHx_IMON_L | RO | - | Current monitor low byte |
| +0x09 | CHx_STATUS | RO | 0x00 | Channel status |
| +0x0A | CHx_FAULT | R/WC | 0x00 | Channel fault status |
| +0x0B | CHx_CONFIG | R/W | 0x00 | Channel configuration |

### 5.4 Voltage Setpoint Format

**12-bit DAC encoding (CHx_VSET):**
```
Voltage = 0.5V + (VSET / 4096) × 4.5V
VSET = ((Voltage - 0.5V) / 4.5V) × 4096
```

**Examples:**
| Voltage | VSET (decimal) | VSET (hex) |
|---------|----------------|------------|
| 0.5V | 0 | 0x000 |
| 1.0V | 455 | 0x1C7 |
| 1.8V | 1183 | 0x49F |
| 3.3V | 2548 | 0x9F4 |
| 5.0V | 4095 | 0xFFF |

### 5.5 Global Control Registers

| Address | Name | Type | Default | Description |
|---------|------|------|---------|-------------|
| 0x50 | GLOBAL_ENABLE | R/W | 0x00 | Global enable control |
| 0x51 | SEQUENCING | R/W | 0x00 | Sequencing configuration |
| 0x52 | SEQ_DELAY_1 | R/W | 0x00 | CH1-CH2 delay (ms) |
| 0x53 | SEQ_DELAY_2 | R/W | 0x00 | CH2-CH3 delay (ms) |
| 0x54 | SEQ_DELAY_3 | R/W | 0x00 | CH3-CH4 delay (ms) |
| 0x55 | RAMP_RATE | R/W | 0x04 | Voltage ramp rate |
| 0x56 | TEMP_MON | RO | - | Temperature monitor |
| 0x57 | VIN_MON | RO | - | Input voltage monitor |

### 5.6 Protection and Fault Registers

| Address | Name | Type | Default | Description |
|---------|------|------|---------|-------------|
| 0x60 | FAULT_STATUS | R/WC | 0x00 | Global fault status |
| 0x61 | FAULT_MASK | R/W | 0xFF | Fault interrupt mask |
| 0x62 | WARNING_STATUS | R/WC | 0x00 | Warning status |
| 0x63 | WARNING_MASK | R/W | 0xFF | Warning interrupt mask |
| 0x64 | OVP_CONFIG | R/W | 0x0A | OVP threshold config (% above setpoint) |
| 0x65 | OTP_CONFIG | R/W | 0x7D | Over-temp threshold (125°C default) |

### 5.7 Calibration Registers

| Address | Name | Type | Default | Description |
|---------|------|------|---------|-------------|
| 0x70-0x7F | CAL_DATA | R/W | (OTP) | Calibration coefficients |

**Note:** Calibration registers are loaded from OTP on power-up. Writing these registers affects current operation but does not modify OTP.

## 6. Mechanical Interface

### 6.1 Package Specifications

**Package Type:** QFN-48 (Quad Flat No-lead)

**Dimensions:**
| Parameter | Min | Nominal | Max | Unit |
|-----------|-----|---------|-----|------|
| Body length | 6.90 | 7.00 | 7.10 | mm |
| Body width | 6.90 | 7.00 | 7.10 | mm |
| Package height | 0.80 | 0.85 | 0.90 | mm |
| Pin pitch | - | 0.50 | - | mm |
| Pin width | 0.18 | 0.25 | 0.30 | mm |
| Pin length | 0.30 | 0.40 | 0.50 | mm |
| Exposed pad size | 5.10 | 5.20 | 5.30 | mm² |

### 6.2 Recommended PCB Land Pattern

**Pad dimensions:**
- Pin pads: 0.70mm × 0.25mm
- Pin pitch: 0.50mm
- Thermal pad: 5.20mm × 5.20mm with solder mask defined

**Thermal Vias:**
- Via diameter: 0.30mm (finished hole)
- Via pitch: 0.80mm
- Via count: 25-30 vias
- Via location: Under exposed pad
- Via fill: Plugged and capped (optional)

### 6.3 Solder Reflow Profile

**Lead-free (SAC305) profile:**
| Phase | Temperature | Time |
|-------|-------------|------|
| Preheat | 150-200°C | 60-120s |
| Soak | 200-250°C | 60-120s |
| Reflow | 250-260°C peak | 20-40s above 235°C |
| Cooling | <6°C/s | - |

**Peak temperature:** 260°C maximum  
**Time above 235°C:** 60 seconds maximum  
**Reflow cycles:** 3 maximum

## 7. Environmental Interface

### 7.1 Operating Conditions

| Parameter | Min | Typ | Max | Unit |
|-----------|-----|-----|-----|------|
| Ambient temperature | -40 | 25 | +85 | °C |
| Junction temperature | -40 | - | +125 | °C |
| Storage temperature | -55 | - | +150 | °C |
| Humidity (non-condensing) | 5 | - | 95 | %RH |

### 7.2 ESD Protection

| Parameter | Rating | Standard |
|-----------|--------|----------|
| HBM (Human Body Model) | ±2kV | JESD22-A114 |
| CDM (Charged Device Model) | ±500V | JESD22-C101 |
| IEC Contact discharge | ±4kV | IEC 61000-4-2 |
| IEC Air discharge | ±8kV | IEC 61000-4-2 |

## 8. Interface Guidelines

### 8.1 Power-Up Sequence

1. Apply VIN (5V-12V)
2. Wait for UVLO release (typ. 1ms)
3. Wait for device initialization (typ. 5ms)
4. Assert EN pin (if used)
5. Configure device via I2C (optional)
6. Enable channels via I2C or use defaults

### 8.2 Power-Down Sequence

1. Disable channels via I2C or EN pin
2. Wait for outputs to discharge (<10ms)
3. Remove VIN (optional)

### 8.3 Error Recovery

1. Read fault status registers to identify fault
2. Clear fault condition if possible
3. Clear fault status by reading registers
4. Re-enable affected channel

## 9. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | Interface Design | Initial release |
