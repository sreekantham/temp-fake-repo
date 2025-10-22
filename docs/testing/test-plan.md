# Test Plan - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document defines the comprehensive test plan for the PM-5000 PMIC, covering all phases from design verification through production testing.

### 1.1 Test Objectives

- Verify compliance with all functional requirements
- Validate performance specifications across operating conditions
- Ensure safety and protection features function correctly
- Confirm reliability and quality targets are met
- Enable efficient production testing

### 1.2 Test Phases

1. **Design Verification Testing (DVT)** - Prototype validation
2. **Production Validation Testing (PVT)** - Pre-production validation
3. **Production Testing** - 100% manufacturing test
4. **Qualification Testing** - Reliability and compliance
5. **Ongoing Reliability Testing** - Production monitoring

## 2. Design Verification Testing (DVT)

### 2.1 Functional Verification

#### TC-F001: Power-Up Sequence
**Objective:** Verify correct power-up behavior

**Test Steps:**
1. Apply VIN = 12V
2. Monitor UVLO release
3. Verify bandgap startup
4. Check digital core initialization
5. Assert EN pin
6. Verify channels enable in sequence
7. Check power-good assertion

**Pass Criteria:**
- UVLO releases at 4.5V ±100mV
- Initialization completes within 5ms
- Channels enable in programmed order
- Power-good asserts when all channels in regulation

**Requirements Traced:** FR-040, FR-041

#### TC-F002: Output Voltage Programming
**Objective:** Verify voltage can be programmed correctly

**Test Steps:**
1. Enable channel 1
2. Program voltage to 0.5V, verify output
3. Program voltage to 1.0V, verify output
4. Program voltage to 1.8V, verify output
5. Program voltage to 3.3V, verify output
6. Program voltage to 5.0V, verify output
7. Repeat for all channels

**Pass Criteria:**
- Output voltage within ±0.1% of programmed value
- All channels achieve all test voltages
- No interaction between channels

**Requirements Traced:** FR-002, FR-004, PR-001

#### TC-F003: Current Limiting
**Objective:** Verify current limiting functions correctly

**Test Steps:**
1. Enable channel 1 at 3.3V
2. Program current limit to 1.0A
3. Apply load to draw >1.0A
4. Verify current limits to programmed value
5. Check fault status register
6. Repeat with different current limits
7. Test all channels

**Pass Criteria:**
- Current limits to within ±5% of programmed value
- Fault status set correctly
- Device does not overheat
- Automatic recovery when overload removed

**Requirements Traced:** FR-003, FR-020, PR-010

#### TC-F004: Over-Voltage Protection
**Objective:** Verify OVP shuts down channel

**Test Steps:**
1. Enable channel 1 at 3.3V
2. Force output voltage above OVP threshold
3. Verify channel shuts down
4. Check fault status register
5. Verify interrupt assertion
6. Clear fault and re-enable
7. Repeat for all channels

**Pass Criteria:**
- OVP triggers at 110% ±2% of programmed voltage
- Response time <1µs
- Fault status set correctly
- Channel can be re-enabled after fault cleared

**Requirements Traced:** FR-021, SR-001

#### TC-F005: I2C Communication
**Objective:** Verify I2C interface functions correctly

**Test Steps:**
1. Write device ID registers
2. Read back and verify
3. Write configuration registers
4. Read back and verify
5. Test all readable/writable registers
6. Verify read-only registers cannot be written
7. Test clock stretching during ADC conversion
8. Test all 4 I2C addresses

**Pass Criteria:**
- All registers accessible via I2C
- Data integrity maintained
- Read-only registers protected
- Clock stretching works correctly
- All addresses selectable

**Requirements Traced:** FR-030, FR-032

### 2.2 Performance Verification

#### TC-P001: DC Accuracy
**Objective:** Measure output voltage accuracy

**Test Procedure:**
1. Program output to each of the following voltages:
   - 0.5V, 1.0V, 1.8V, 2.5V, 3.3V, 5.0V
2. Measure with calibrated 6.5-digit DMM
3. Record measurements at each load current:
   - 0A, 0.5A, 1.5A, 3.0A
4. Test at temperatures: -40°C, 25°C, 85°C
5. Test at input voltages: 5V, 8.5V, 12V
6. Repeat for all channels

**Pass Criteria:**
- Accuracy ±0.05% typical, ±0.1% maximum at 25°C
- Accuracy ±0.1% maximum over full temperature range
- Load regulation <0.01%/A
- Line regulation <0.005%/V

**Requirements Traced:** PR-001, PR-020, PR-021

#### TC-P002: Output Noise Measurement
**Objective:** Measure output noise and ripple

**Test Procedure:**
1. Configure channel to 2.5V output
2. Apply 1A load
3. Measure noise with oscilloscope:
   - Bandwidth: 20Hz to 20MHz
   - 50Ω input termination
   - AC coupling
4. Record RMS noise
5. Record peak-to-peak ripple
6. Measure at switching frequency
7. Repeat at 0.1A, 1.5A, 3.0A loads
8. Test all channels

**Pass Criteria:**
- Broadband noise <50µVrms typical, <75µVrms maximum
- Output ripple <100µVpp typical, <200µVpp maximum
- Switching frequency components identifiable

**Requirements Traced:** PR-030, PR-031

#### TC-P003: Load Transient Response
**Objective:** Measure transient response to load steps

**Test Procedure:**
1. Configure channel to 3.3V output
2. Apply electronic load in dynamic mode
3. Program load step: 0.1A → 3.0A, 1µs edge
4. Capture output voltage waveform
5. Measure:
   - Peak deviation
   - Settling time to 0.1%
   - Settling time to 0.01%
   - Overshoot/undershoot
6. Repeat for negative step: 3.0A → 0.1A
7. Test all channels

**Pass Criteria:**
- Peak deviation <100mV
- Settling time to 0.1% <10µs
- Settling time to 0.01% <50µs
- No sustained oscillation

**Requirements Traced:** PR-040

#### TC-P004: PSRR Measurement
**Objective:** Measure power supply rejection ratio

**Test Procedure:**
1. Configure channel to 2.5V output, 1A load
2. Inject 100mVpp sine wave on VIN
3. Measure output ripple amplitude
4. Calculate PSRR = 20×log(Vin_ripple/Vout_ripple)
5. Sweep frequency: 120Hz, 1kHz, 10kHz, 100kHz
6. Repeat at different output voltages
7. Test all channels

**Pass Criteria:**
- PSRR @ 120Hz: >90dB
- PSRR @ 1kHz: >80dB
- PSRR @ 10kHz: >70dB
- PSRR @ 100kHz: >60dB

**Requirements Traced:** PR-032

#### TC-P005: Efficiency Measurement
**Objective:** Measure power conversion efficiency

**Test Procedure:**
1. Configure channel to 3.3V output
2. Measure input voltage and current
3. Measure output voltage and current
4. Calculate efficiency = (Vout×Iout)/(Vin×Iin)
5. Sweep load current: 0.1A, 0.5A, 1.0A, 1.5A, 2.0A, 2.5A, 3.0A
6. Test at VIN = 5V, 8.5V, 12V
7. Test all channels

**Pass Criteria:**
- Efficiency >90% at 1.5A load, 12V input
- Efficiency >88% at 3.0A load, 12V input
- Efficiency >85% at 0.5A load, 12V input

**Requirements Traced:** PR-080

### 2.3 Monitoring and Telemetry Verification

#### TC-M001: Voltage Monitoring Accuracy
**Objective:** Verify voltage readback accuracy

**Test Procedure:**
1. Program output voltage to known value
2. Measure with calibrated DMM
3. Read voltage via I2C ADC
4. Compare DMM reading to I2C reading
5. Repeat at 0.5V, 1.0V, 1.8V, 2.5V, 3.3V, 5.0V
6. Test all channels
7. Test at -40°C, 25°C, 85°C

**Pass Criteria:**
- Voltage readback accurate to ±0.05% typical, ±0.1% maximum
- 14-bit resolution achieved
- Update rate >1kHz

**Requirements Traced:** FR-010, PR-100

#### TC-M002: Current Monitoring Accuracy
**Objective:** Verify current readback accuracy

**Test Procedure:**
1. Apply known load current
2. Measure with calibrated ammeter
3. Read current via I2C ADC
4. Compare readings
5. Repeat at 0.1A, 0.5A, 1.0A, 1.5A, 2.0A, 2.5A, 3.0A
6. Test all channels

**Pass Criteria:**
- Current readback accurate to ±0.5% typical, ±1% maximum
- 12-bit resolution achieved
- Offset <10mA

**Requirements Traced:** FR-011, PR-110

#### TC-M003: Temperature Monitoring
**Objective:** Verify temperature sensor accuracy

**Test Procedure:**
1. Place device in temperature chamber
2. Set chamber to -40°C, wait for thermal equilibrium
3. Read temperature via I2C
4. Compare to thermocouple measurement
5. Repeat at 0°C, 25°C, 50°C, 85°C, 100°C, 125°C
6. Verify thermal warning at 100°C
7. Verify thermal shutdown at 125°C

**Pass Criteria:**
- Temperature accurate to ±5°C
- Warning threshold 100°C ±5°C
- Shutdown threshold 125°C ±5°C

**Requirements Traced:** FR-012, PR-120

### 2.4 Protection Verification

#### TC-S001: Thermal Shutdown
**Objective:** Verify thermal shutdown protects device

**Test Procedure:**
1. Enable all channels at maximum load
2. Raise ambient temperature or apply localized heating
3. Monitor junction temperature
4. Verify shutdown occurs at 125°C ±5°C
5. Allow device to cool
6. Verify automatic restart at 110°C
7. Check device for damage

**Pass Criteria:**
- Shutdown occurs at 125°C ±5°C
- Auto-restart at 110°C ±5°C
- No damage to device
- Device operates normally after cooling

**Requirements Traced:** FR-023, SR-010

#### TC-S002: Short Circuit Protection
**Objective:** Verify short circuit protection works indefinitely

**Test Procedure:**
1. Enable channel at 5V
2. Short output to ground
3. Verify current limiting engages
4. Monitor for 1 hour continuous
5. Monitor junction temperature
6. Remove short circuit
7. Verify channel recovers
8. Check for damage

**Pass Criteria:**
- Short circuit current limited to <4A
- No thermal damage after 1 hour
- Junction temperature <125°C
- Channel operates normally after short removed

**Requirements Traced:** SR-002, SR-003, SR-030

## 3. Environmental Testing

### 3.1 Temperature Testing

#### TC-E001: Temperature Sweep
**Objective:** Verify operation across temperature range

**Test Procedure:**
1. Configure device with typical settings
2. Place in temperature chamber
3. Run at -40°C for 1 hour, verify all specs
4. Ramp to 85°C, verify during ramp
5. Hold at 85°C for 1 hour, verify all specs
6. Cycle -40°C to 85°C, 100 cycles
7. Verify no degradation

**Pass Criteria:**
- All specifications met -40°C to 85°C
- No failures during temperature cycling
- No performance degradation

**Requirements Traced:** PR-091

### 3.2 EMC Testing

#### TC-EMC001: Conducted Emissions
**Objective:** Verify conducted emissions compliance

**Test Procedure:**
1. Configure DUT per CISPR 11
2. Use standard LISN
3. Measure emissions 150kHz to 30MHz
4. Record peak and quasi-peak values
5. Compare to Class A limits

**Pass Criteria:**
- All emissions below CISPR 11 Class A limits
- Margin >6dB recommended

**Requirements Traced:** PR-130

#### TC-EMC002: Radiated Emissions
**Objective:** Verify radiated emissions compliance

**Test Procedure:**
1. Configure DUT per CISPR 11
2. Perform 3m radiated emissions test
3. Measure 30MHz to 1GHz
4. Record peak values
5. Compare to Class A limits

**Pass Criteria:**
- All emissions below CISPR 11 Class A limits
- Margin >6dB recommended

**Requirements Traced:** PR-131

#### TC-EMC003: ESD Testing
**Objective:** Verify ESD immunity

**Test Procedure:**
1. Test all pins per IEC 61000-4-2
2. Contact discharge: ±4kV
3. Air discharge: ±8kV
4. Monitor device operation during test
5. Verify functionality after test
6. 10 discharges per pin, both polarities

**Pass Criteria:**
- No latchup
- No damage
- No loss of function during or after discharge

**Requirements Traced:** PR-132, SR-070

## 4. Reliability Testing

### 4.1 HTOL (High Temperature Operating Life)

**Test Conditions:**
- Temperature: 125°C junction
- Duration: 1000 hours minimum
- Sample size: 77 units (per JESD47)
- Bias: All channels enabled, 50% load
- Measurements: 0hr, 168hr, 500hr, 1000hr

**Pass Criteria:**
- Zero failures
- <10% parameter drift

### 4.2 Temperature Cycling

**Test Conditions:**
- Temperature: -55°C to 150°C
- Ramp rate: <15°C/min
- Dwell time: 15 minutes
- Cycles: 1000 minimum
- Sample size: 77 units

**Pass Criteria:**
- Zero failures
- No package cracks or delamination

### 4.3 HAST (Highly Accelerated Stress Test)

**Test Conditions:**
- Temperature: 130°C
- Humidity: 85% RH
- Pressure: 2 atmospheres
- Duration: 96 hours
- Sample size: 77 units

**Pass Criteria:**
- Zero failures
- No corrosion or contamination

## 5. Production Testing

### 5.1 Wafer Level Testing

**Tests Performed:**
- Continuity testing
- Basic functional test
- Key parametric measurements
- Yield binning

### 5.2 Final Test

**Test Flow:**
1. Contact test (continuity)
2. Power-up test
3. I2C communication test
4. Output voltage accuracy test (all channels)
5. Current limit test (sample channels)
6. Monitoring accuracy test
7. Protection test (OVP, thermal)
8. Marking and packing

**Test Limits:**
- Tighter than datasheet specifications
- Based on process capability
- Reviewed quarterly

### 5.3 Test Coverage

| Requirement Category | Coverage |
|---------------------|----------|
| Critical Safety | 100% |
| Functional | 100% |
| Performance (key parameters) | 100% |
| Performance (secondary) | Sample basis |

## 6. Traceability Matrix

| Requirement ID | Test Case(s) | Test Phase |
|----------------|--------------|------------|
| FR-001 | TC-F002 | DVT, Production |
| FR-002 | TC-F002, TC-P001 | DVT, Production |
| FR-003 | TC-F003 | DVT, Production |
| FR-004 | TC-F002 | DVT, Production |
| FR-010 | TC-M001 | DVT, Production |
| FR-011 | TC-M002 | DVT, Production |
| FR-012 | TC-M003 | DVT |
| FR-020 | TC-F003 | DVT, Production |
| FR-021 | TC-F004 | DVT, Production |
| FR-023 | TC-S001 | DVT, Qualification |
| FR-030 | TC-F005 | DVT, Production |
| PR-001 | TC-P001 | DVT, Production |
| PR-030 | TC-P002 | DVT |
| PR-031 | TC-P002 | DVT |
| PR-032 | TC-P004 | DVT |
| PR-040 | TC-P003 | DVT |
| PR-080 | TC-P005 | DVT |
| SR-001 | TC-F004 | DVT, Production |
| SR-002 | TC-F003 | DVT, Production |
| SR-003 | TC-S002 | DVT, Qualification |
| SR-010 | TC-S001 | DVT, Qualification |

## 7. Test Equipment Requirements

### 7.1 DVT Test Equipment

| Equipment | Specifications | Quantity |
|-----------|---------------|----------|
| Precision Power Supply | 0-20V, 10A, <1mV ripple | 2 |
| 6.5 Digit DMM | 0.01% accuracy | 2 |
| Oscilloscope | 500MHz, 4 channels | 1 |
| Electronic Load | 0-5A, dynamic mode | 4 |
| Temperature Chamber | -55°C to 150°C | 1 |
| I2C Analyzer | - | 1 |
| Function Generator | 20MHz | 1 |
| Spectrum Analyzer | 1GHz | 1 |

### 7.2 Production Test Equipment

| Equipment | Specifications | Quantity |
|-----------|---------------|----------|
| ATE System | Custom test program | 1 |
| Load Boards | Custom design | 4 |
| Precision DMM Module | 0.05% accuracy | 1 |
| Power Supply Module | 0-20V, 5A | 1 |

## 8. Test Data Management

### 8.1 DVT Data
- Record all measurements in test database
- Generate test reports for each unit
- Statistical analysis of results
- Correlation analysis between tests

### 8.2 Production Data
- 100% data logging
- Statistical Process Control (SPC)
- Automatic limit checking
- Traceability to wafer lot and date code

## 9. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | Test Engineering | Initial release |
