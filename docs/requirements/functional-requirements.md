# Functional Requirements - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document defines the functional requirements for the PM-5000 series Power Management Integrated Circuit (PMIC) designed for test and measurement applications.

## 2. Scope

The PM-5000 PMIC shall provide precision power delivery, monitoring, and control capabilities suitable for automated test equipment and precision measurement instruments.

## 3. Functional Requirements

### 3.1 Power Output Requirements

#### FR-001: Multi-Channel Output
**Priority:** Critical  
**Description:** The PMIC shall provide 4 independent programmable voltage output channels.

**Acceptance Criteria:**
- Each channel operates independently
- Channels can be enabled/disabled individually
- No cross-talk between channels >60dB

#### FR-002: Voltage Range
**Priority:** Critical  
**Description:** Each output channel shall provide programmable voltage from 0.5V to 5.0V.

**Acceptance Criteria:**
- Minimum output voltage: 0.5V ±10mV
- Maximum output voltage: 5.0V ±10mV
- Voltage step resolution: 1mV or better

#### FR-003: Current Capability
**Priority:** Critical  
**Description:** Each channel shall deliver up to 3A continuous current.

**Acceptance Criteria:**
- Continuous current: 3A per channel
- Peak current (100ms): 3.5A per channel
- Current limiting: Programmable from 0.1A to 3.5A

#### FR-004: Voltage Programming
**Priority:** Critical  
**Description:** Output voltage shall be programmable via digital interface.

**Acceptance Criteria:**
- 12-bit DAC resolution minimum
- Programming range covers full 0.5V-5.0V span
- Voltage setting accuracy: ±0.1%

### 3.2 Monitoring and Telemetry

#### FR-010: Voltage Monitoring
**Priority:** High  
**Description:** The PMIC shall monitor actual output voltage on each channel.

**Acceptance Criteria:**
- Real-time voltage readback via ADC
- 14-bit ADC resolution minimum
- Update rate: ≥1kHz per channel
- Measurement accuracy: ±0.05%

#### FR-011: Current Monitoring
**Priority:** High  
**Description:** The PMIC shall monitor load current on each channel.

**Acceptance Criteria:**
- Real-time current readback via ADC
- 12-bit ADC resolution minimum
- Current sensing range: 0-3.5A
- Measurement accuracy: ±1%

#### FR-012: Temperature Monitoring
**Priority:** High  
**Description:** The PMIC shall monitor junction temperature.

**Acceptance Criteria:**
- Internal temperature sensor
- Temperature accuracy: ±5°C
- Update rate: ≥10Hz
- Over-temperature warning at 100°C
- Over-temperature shutdown at 125°C

#### FR-013: Power Good Indication
**Priority:** High  
**Description:** Each channel shall provide a power-good status signal.

**Acceptance Criteria:**
- Power-good asserts when output is within ±2% of target
- Power-good de-asserts when output exceeds ±3% of target
- Hysteresis prevents chattering
- Available as both register bit and GPIO output

### 3.3 Protection Features

#### FR-020: Over-Current Protection
**Priority:** Critical  
**Description:** Each channel shall provide over-current protection.

**Acceptance Criteria:**
- Programmable current limit (0.1A to 3.5A)
- Current limit accuracy: ±5%
- Hiccup mode or latched shutdown (configurable)
- Fault status reported via register

#### FR-021: Over-Voltage Protection
**Priority:** Critical  
**Description:** Each channel shall provide over-voltage protection.

**Acceptance Criteria:**
- OVP threshold: 110% of programmed voltage
- Response time: <1µs
- Automatic shutdown on OVP event
- Fault status reported via register

#### FR-022: Under-Voltage Lockout
**Priority:** Critical  
**Description:** The PMIC shall include under-voltage lockout on input supply.

**Acceptance Criteria:**
- UVLO threshold: 4.5V typical
- Hysteresis: 200mV minimum
- All outputs disabled when UVLO active
- UVLO status available via register

#### FR-023: Thermal Protection
**Priority:** Critical  
**Description:** The PMIC shall provide thermal shutdown protection.

**Acceptance Criteria:**
- Thermal shutdown at 125°C junction temperature
- Thermal warning at 100°C
- Automatic restart when temperature drops to 110°C
- Fault status reported via register

### 3.4 Digital Interface

#### FR-030: I2C Interface
**Priority:** Critical  
**Description:** The PMIC shall provide an I2C slave interface for control and monitoring.

**Acceptance Criteria:**
- I2C standard mode (100kHz) support
- I2C fast mode (400kHz) support
- 7-bit addressing
- Configurable I2C address (via pin strapping)

#### FR-031: SPI Interface (Optional)
**Priority:** Medium  
**Description:** The PMIC may provide an SPI interface as alternative to I2C.

**Acceptance Criteria:**
- SPI mode 0 and mode 3 support
- Clock rates up to 10MHz
- 4-wire SPI (CS, CLK, MOSI, MISO)

#### FR-032: Register Map
**Priority:** Critical  
**Description:** All configuration and status shall be accessible via register interface.

**Acceptance Criteria:**
- Complete register map documentation
- Read/write registers for configuration
- Read-only registers for status
- Reserved bits return 0 when read

#### FR-033: Interrupt/Alert
**Priority:** High  
**Description:** The PMIC shall provide an interrupt/alert output signal.

**Acceptance Criteria:**
- Open-drain output
- Asserts on any fault condition
- Asserts on any programmable threshold crossing
- Cleared by reading fault status registers

### 3.5 Sequencing and Timing

#### FR-040: Power-Up Sequencing
**Priority:** High  
**Description:** The PMIC shall support programmable power-up sequencing.

**Acceptance Criteria:**
- Configurable delay between channel enables (0-100ms)
- Configurable ramp rate per channel (0.1-10V/ms)
- Hardware and software enable options

#### FR-041: Power-Down Sequencing
**Priority:** High  
**Description:** The PMIC shall support programmable power-down sequencing.

**Acceptance Criteria:**
- Configurable order of channel shutdown
- Configurable discharge behavior
- Emergency shutdown <10µs on all channels

#### FR-042: Soft-Start
**Priority:** Medium  
**Description:** Each channel shall implement soft-start on enable.

**Acceptance Criteria:**
- Programmable soft-start time (0.1ms-10ms)
- Linear or exponential ramp (configurable)
- Prevents input current surge

### 3.6 Calibration and Compensation

#### FR-050: Factory Calibration
**Priority:** High  
**Description:** The PMIC shall support factory calibration data storage.

**Acceptance Criteria:**
- Non-volatile memory for calibration coefficients
- Per-channel voltage offset and gain calibration
- Per-channel current sense calibration
- Temperature sensor calibration

#### FR-051: User Calibration
**Priority:** Medium  
**Description:** Users shall be able to perform field calibration.

**Acceptance Criteria:**
- Software-accessible calibration registers
- Calibration can be saved to external NVM
- Default factory calibration preserved

## 4. Interface Requirements

### 4.1 Electrical Interfaces

| Interface | Type | Description |
|-----------|------|-------------|
| VIN | Power Input | Main input power (5V-12V) |
| VOUT1-4 | Power Output | Four independent outputs |
| SDA, SCL | I2C | Digital control interface |
| INT# | Digital Output | Interrupt/alert signal |
| EN | Digital Input | Master enable input |
| ADDR0-1 | Digital Input | I2C address configuration |

### 4.2 Mechanical Interfaces

| Parameter | Specification |
|-----------|--------------|
| Package | QFN-48 (7mm x 7mm) |
| Pin Pitch | 0.5mm |
| Thermal Pad | Yes, exposed die attach pad |
| Package Height | 0.9mm maximum |

## 5. Performance Requirements

Performance requirements are detailed in [performance-requirements.md](performance-requirements.md).

## 6. Verification and Validation

Each functional requirement shall be verified through:
- Design review
- Simulation analysis
- Prototype testing
- Production testing

Traceability matrix mapping requirements to test cases is maintained in [test-plan.md](../testing/test-plan.md).

## 7. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | System Architecture | Initial release |
