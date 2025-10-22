# System Architecture - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document describes the high-level system architecture of the PM-5000 PMIC, including major functional blocks, interfaces, and design decisions.

## 2. System Overview

The PM-5000 PMIC is a highly integrated power management solution designed for precision test and measurement applications. The architecture is optimized for:

- High accuracy and low noise
- Fast transient response
- Comprehensive monitoring and telemetry
- Flexible digital control
- Robust protection mechanisms

### 2.1 Key Architecture Decisions

| Decision | Rationale |
|----------|-----------|
| Synchronous buck topology | High efficiency, low heat, good transient response |
| Digital control loop | Flexibility, programmability, telemetry integration |
| Dedicated per-channel LDO | Ultra-low noise final stage for test applications |
| Multi-phase operation | Reduced input ripple, better thermal distribution |
| Integrated ADCs | Accurate monitoring without external components |
| I2C primary interface | Industry standard, minimal pin count, adequate speed |

## 3. Top-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         PM-5000 PMIC                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌─────────┐      ┌──────────────────────────────────────┐     │
│  │  Input  │      │         Power Stages                  │     │
│  │  Power  │──────┤  ┌────────┐  ┌────────┐  ┌────────┐ │     │
│  │  Stage  │      │  │ Buck 1 │  │ Buck 2 │  │ Buck 3 │ │     │
│  └─────────┘      │  │+ LDO 1 │  │+ LDO 2 │  │+ LDO 3 │ │────▶VOUT1-4
│                    │  └────────┘  └────────┘  └────────┘ │     │
│  ┌─────────┐      │  ┌────────┐                          │     │
│  │ Voltage │      │  │ Buck 4 │                          │     │
│  │   Ref   │──────┤  │+ LDO 4 │                          │     │
│  └─────────┘      │  └────────┘                          │     │
│                    └──────────────────────────────────────┘     │
│                                   │                              │
│  ┌─────────────────────────────────┴───────────────────────┐   │
│  │              Digital Control Core                        │   │
│  │  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────────┐   │   │
│  │  │Digital │  │ Analog │  │  PWM   │  │ Sequencer  │   │   │
│  │  │Control │──│  ADCs  │──│Generators│─│  & Timing  │   │   │
│  │  │  Loop  │  │ (V,I,T)│  └────────┘  └────────────┘   │   │
│  │  └────────┘  └────────┘                                │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                   │                              │
│  ┌─────────────────────────────────┴───────────────────────┐   │
│  │           Protection & Monitoring                        │   │
│  │  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐  ┌─────────┐  │   │
│  │  │  OVP │  │  OCP │  │ UVLO │  │Thermal│  │ Power   │  │   │
│  │  │      │  │      │  │      │  │ Prot. │  │  Good   │  │   │
│  │  └──────┘  └──────┘  └──────┘  └──────┘  └─────────┘  │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                   │                              │
│  ┌─────────────────────────────────┴───────────────────────┐   │
│  │              I2C Interface & Registers                   │   │
│  │  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────────┐   │   │
│  │  │  I2C   │  │Register│  │ EEPROM │  │  Interrupt │   │   │
│  │  │  Slave │──│  Map   │──│  (Cal) │──│   Logic    │   │   │
│  │  └────────┘  └────────┘  └────────┘  └────────────┘   │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
         │         │          │          │          │       │
        VIN       SDA        SCL        EN        INT#    GND
```

## 4. Major Functional Blocks

### 4.1 Input Power Stage

**Function:** Conditions and distributes input power to all downstream blocks.

**Key Components:**
- Input filter (external capacitors required)
- Under-voltage lockout (UVLO) comparator
- Input reverse polarity protection (external)
- Power-on-reset (POR) circuit
- Bandgap voltage reference

**Specifications:**
- Input voltage range: 5V to 12V
- UVLO threshold: 4.5V ±100mV
- UVLO hysteresis: 200mV typical
- Input current: <50mA quiescent + load current

### 4.2 Voltage Reference

**Function:** Provides precision voltage reference for all analog circuits.

**Key Components:**
- Bandgap reference core
- Temperature compensation
- Buffer amplifiers
- Trim and calibration

**Specifications:**
- Reference voltage: 1.25V nominal
- Initial accuracy: ±0.5%
- Temperature coefficient: <25ppm/°C
- Load regulation: <100ppm/mA
- Noise (0.1Hz-10Hz): <5µVrms

### 4.3 Power Stages (4 Channels)

Each channel consists of two stages for optimal performance:

#### 4.3.1 Buck Converter Stage

**Function:** Efficient step-down conversion from VIN to intermediate voltage.

**Key Components:**
- High-side P-MOSFET switch
- Low-side N-MOSFET switch
- Gate drivers
- Current sense amplifier
- PWM comparator

**Specifications:**
- Switching frequency: 1MHz nominal
- Output voltage: 0.7V to 5.5V
- Peak efficiency: >92%
- Current limit: 3.5A per channel

#### 4.3.2 Low-Dropout (LDO) Post-Regulator

**Function:** Final stage regulation for ultra-low noise output.

**Key Components:**
- Low-noise pass transistor
- Error amplifier
- Soft-start circuit
- Current limit

**Specifications:**
- Dropout voltage: <200mV @ 3A
- Output noise: <20µVrms (20Hz-20MHz)
- PSRR @ 1kHz: >80dB
- Bandwidth: >1MHz

**Design Rationale:**
The two-stage architecture (buck + LDO) provides:
- High efficiency from buck converter
- Ultra-low noise from LDO
- Fast transient response from buck
- High PSRR from LDO
- Optimal for test equipment power supplies

### 4.4 Digital Control Core

**Function:** Implements control loops, sequencing, and housekeeping functions.

**Key Components:**
- Digital compensator (PID control)
- PWM generators (4 channels)
- Sequencing state machine
- Soft-start/ramp control
- Calibration engine

**Implementation:**
- Custom digital logic (ASIC)
- Operates at 50MHz core clock
- 12-bit voltage DACs (14-bit target)
- Lookup tables for non-linear compensation

**Features:**
- Configurable control loop parameters
- Adaptive compensation based on load
- Phase management for multi-channel operation
- Programmable sequencing and timing

### 4.5 Analog-to-Digital Converters

**Function:** Digitize voltage, current, and temperature for monitoring and control.

**Configuration:**
- 4× voltage sense ADCs (14-bit, 1kSPS)
- 4× current sense ADCs (12-bit, 1kSPS)
- 1× temperature sense ADC (10-bit, 100SPS)

**Specifications:**
- Voltage ADC: 14-bit, ±0.05% accuracy
- Current ADC: 12-bit, ±1% accuracy
- Temperature ADC: 10-bit, ±5°C accuracy
- Simultaneous sampling on all channels
- Averaging and filtering in digital domain

### 4.6 Protection Circuits

#### 4.6.1 Over-Voltage Protection (OVP)

**Implementation:**
- Hardware comparator per channel
- Threshold: 110% of programmed voltage
- Response time: <1µs
- Action: Immediate shutdown of affected channel

#### 4.6.2 Over-Current Protection (OCP)

**Implementation:**
- Cycle-by-cycle current limiting in buck stage
- Average current monitoring via ADC
- Hardware current limit at 4A
- Configurable hiccup or fold-back mode

#### 4.6.3 Thermal Protection

**Implementation:**
- On-die temperature sensor
- Warning threshold: 100°C
- Shutdown threshold: 125°C
- Hysteretic auto-restart at 110°C

#### 4.6.4 Under-Voltage Lockout (UVLO)

**Implementation:**
- Input voltage monitoring
- Threshold: 4.5V with 200mV hysteresis
- All outputs disabled when UVLO active
- Digital status reporting

### 4.7 I2C Interface

**Function:** Digital communication and control interface.

**Implementation:**
- I2C slave compliant with I2C specification
- 7-bit addressing
- Standard mode (100kHz) and Fast mode (400kHz)
- Configurable address via pin strapping (4 addresses)

**Features:**
- Complete register access
- Status and fault reporting
- Interrupt generation
- Clock stretching for ADC reads

### 4.8 Register Map

**Organization:**
```
0x00-0x0F: Device ID and configuration
0x10-0x1F: Channel 1 control and status
0x20-0x2F: Channel 2 control and status
0x30-0x3F: Channel 3 control and status
0x40-0x4F: Channel 4 control and status
0x50-0x5F: Global control and sequencing
0x60-0x6F: Protection and fault status
0x70-0x7F: Calibration and trim
0x80-0xFF: Reserved
```

**Key Registers:**
- Voltage set point (R/W, 16-bit)
- Current limit (R/W, 12-bit)
- Enable control (R/W, per channel)
- Voltage readback (RO, 14-bit)
- Current readback (RO, 12-bit)
- Fault status (RO, clear on read)
- Interrupt mask (R/W)
- Sequencing configuration (R/W)

### 4.9 Calibration and Trim

**Function:** Store and apply calibration data for accuracy.

**Implementation:**
- One-time programmable (OTP) EEPROM
- Per-channel voltage offset and gain
- Per-channel current sense calibration
- Temperature sensor calibration
- Bandgap reference trim

**Storage:**
- 256 bytes OTP memory
- Factory programming during test
- CRC protection
- Redundant storage for critical values

## 5. Power Management Strategy

### 5.1 Startup Sequence

```
1. VIN applied
2. UVLO releases when VIN > 4.7V
3. Bandgap and reference startup (1ms)
4. Digital core initialization (5ms)
   - Load calibration from OTP
   - Initialize registers to defaults
   - Configure PWM generators
5. Wait for EN pin assertion or I2C enable
6. Sequenced channel startup per configuration
7. Assert power-good when all enabled channels in regulation
```

### 5.2 Shutdown Sequence

```
1. Disable command received (I2C or EN pin)
2. Active discharge enabled on all outputs
3. Channels disabled in programmed sequence
4. Digital core remains active for monitoring
5. If VIN removed, UVLO triggers full shutdown
```

### 5.3 Fault Handling

```
Fault Detection
      ↓
Fault Classification (Critical vs. Warning)
      ↓
Critical Fault → Immediate Shutdown
Warning → Log and Continue
      ↓
Assert Interrupt
      ↓
Update Status Registers
      ↓
Auto-Recovery (if enabled) or Wait for I2C Clear
```

## 6. Thermal Management

### 6.1 Power Dissipation

**Sources of Heat:**
- Buck converter switching losses: ~300mW per channel
- Buck converter conduction losses: ~200mW per channel
- LDO dropout power: (VIN - VOUT) × ILOAD per channel
- Quiescent power: ~150mW

**Worst Case (per channel at 3A):**
- Buck losses: 500mW
- LDO dropout (12V in, 5V out at 3A): Minimal (buck output ~5.2V)
- LDO dropout (12V in, 0.5V out at 3A): ~2.1W
  
**Worst case is low output voltage with maximum input voltage.**

### 6.2 Thermal Design

**Package:** QFN-48 with exposed thermal pad

**Thermal Resistance:**
- θJC (junction to case): <5°C/W
- θJA (junction to ambient, 2oz Cu): <30°C/W

**Operating Limits:**
- Maximum junction temperature: 125°C
- Ambient temperature range: -40°C to +85°C
- Thermal shutdown: 125°C

**Thermal Management:**
- Exposed pad MUST be soldered to PCB
- Recommend 4-layer PCB with thermal vias
- Recommend 2oz copper on power planes
- Heat spreading area: >10cm² recommended

## 7. External Components

### 7.1 Required External Components (per channel)

| Component | Value | Purpose |
|-----------|-------|---------|
| Input Capacitor | 10µF ceramic | Input filtering |
| Buck Inductor | 2.2µH | Energy storage |
| Buck Output Cap | 10µF ceramic | Buck output filtering |
| LDO Output Cap | 22µF ceramic | LDO stability & filtering |
| Soft-start Cap | 1nF | Soft-start timing |

### 7.2 I2C Interface Components

| Component | Value | Purpose |
|-----------|-------|---------|
| SDA Pull-up | 4.7kΩ | I2C bus pull-up |
| SCL Pull-up | 4.7kΩ | I2C bus pull-up |

## 8. Design Trade-offs

### 8.1 Buck + LDO vs. Buck Only

**Decision:** Implement buck + LDO topology

**Rationale:**
- Test equipment requires <50µVrms noise (LDO provides this)
- Buck alone cannot achieve noise target
- Buck + LDO common in precision applications
- Efficiency acceptable for target applications

### 8.2 Analog vs. Digital Control

**Decision:** Digital control loop

**Rationale:**
- Flexibility for compensation tuning
- Enables adaptive control
- Integrated telemetry easier
- Programmability required for test equipment

### 8.3 Switching Frequency

**Decision:** 1MHz switching frequency

**Rationale:**
- Allows small external components
- Good efficiency (>90%)
- Manageable EMI
- Proven frequency for similar applications

## 9. Design Validation

### 9.1 Simulation Requirements

- SPICE simulation of complete power stages
- Control loop stability analysis (AC analysis)
- Transient response simulation
- Thermal simulation
- EMI pre-compliance simulation

### 9.2 Prototype Validation

- Electrical characterization per specifications
- Thermal testing at maximum power
- EMC testing (conducted and radiated)
- Reliability testing (HTOL, TC, HAST)
- Safety testing per IEC 61010-1

## 10. Block Diagram Reference

Detailed block diagrams for each subsystem are provided in:
- [Block Diagram Document](block-diagram.md)
- Interface specifications: [Interface Specifications](interface-specifications.md)

## 11. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | System Architecture | Initial release |
