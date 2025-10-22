# Performance Requirements - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document specifies the detailed performance requirements for the PM-5000 PMIC, focusing on electrical characteristics critical for test and measurement applications.

## 2. DC Performance Requirements

### 2.1 Output Voltage Specifications

#### PR-001: Output Voltage Accuracy
**Priority:** Critical  
**Requirement:** Output voltage shall be accurate to ±0.1% over full temperature range.

**Detailed Specifications:**
- Initial accuracy at 25°C: ±0.05% typical, ±0.1% maximum
- Temperature coefficient: ±25ppm/°C maximum
- Load regulation: <0.01%/A (0-3A load)
- Line regulation: <0.005%/V (5V-12V input)
- Long-term stability: <0.02% per 1000 hours

**Test Conditions:**
- Ambient temperature: -40°C to +85°C
- Input voltage: 5V to 12V
- Load current: 0A to 3A

#### PR-002: Output Voltage Resolution
**Priority:** High  
**Requirement:** Output voltage programming resolution shall be ≤1mV.

**Detailed Specifications:**
- DAC resolution: 12-bit minimum (14-bit target)
- LSB step size: ≤1mV
- Monotonic DAC operation
- No missing codes

#### PR-003: Output Voltage Range
**Priority:** Critical  
**Requirement:** Each channel shall provide 0.5V to 5.0V output.

**Detailed Specifications:**
- Minimum output voltage: 0.5V ±10mV
- Maximum output voltage: 5.0V ±10mV
- Programming steps: 4096 minimum (12-bit)
- Voltage step accuracy: ±0.5 LSB

### 2.2 Output Current Specifications

#### PR-010: Output Current Capability
**Priority:** Critical  
**Requirement:** Each channel shall deliver 3A continuous current.

**Detailed Specifications:**
- Continuous current: 3A at 85°C ambient
- Peak current (100ms): 3.5A
- Short-circuit current: 4A typical
- Current derating: 20mA/°C above 85°C ambient

#### PR-011: Current Monitoring Accuracy
**Priority:** High  
**Requirement:** Current monitoring shall be accurate to ±1% of reading.

**Detailed Specifications:**
- Accuracy at 25°C: ±0.5% typical, ±1% maximum
- Temperature coefficient: ±100ppm/°C
- Offset error: ±10mA maximum
- Gain error: ±1% maximum
- ADC resolution: 12-bit minimum

### 2.3 Load and Line Regulation

#### PR-020: Load Regulation
**Priority:** Critical  
**Requirement:** Output voltage shall vary <0.01% per ampere load change.

**Detailed Specifications:**
- Static load regulation: <0.01%/A (DC to 10Hz)
- Load current range: 0A to 3A
- Measured at output terminals
- All temperature conditions

#### PR-021: Line Regulation
**Priority:** Critical  
**Requirement:** Output voltage shall vary <0.005% per volt input change.

**Detailed Specifications:**
- Static line regulation: <0.005%/V
- Input voltage range: 5V to 12V
- Measured at output terminals
- All temperature conditions

## 3. AC Performance Requirements

### 3.1 Noise and Ripple

#### PR-030: Output Noise
**Priority:** Critical  
**Requirement:** Output noise shall be <50µVrms (20Hz to 20MHz).

**Detailed Specifications:**
- Broadband noise (20Hz-20MHz): <50µVrms typical, <75µVrms maximum
- Low-frequency noise (0.1Hz-10Hz): <5µVrms
- Spot noise at 10kHz: <2nV/√Hz
- Measurement bandwidth: 20Hz to 20MHz
- Measurement conditions: 2.5V output, 1A load, 12V input

#### PR-031: Output Ripple
**Priority:** High  
**Requirement:** Output ripple shall be <100µVpp at switching frequency.

**Detailed Specifications:**
- Ripple voltage: <100µVpp typical, <200µVpp maximum
- Measurement bandwidth: 20MHz
- All load conditions (0-3A)
- Switching frequency components only

#### PR-032: Power Supply Rejection Ratio
**Priority:** High  
**Requirement:** PSRR shall be >80dB at 1kHz.

**Detailed Specifications:**
- PSRR @ 120Hz: >90dB
- PSRR @ 1kHz: >80dB
- PSRR @ 10kHz: >70dB
- PSRR @ 100kHz: >60dB
- Input perturbation: 100mVpp

### 3.2 Transient Response

#### PR-040: Load Transient Response
**Priority:** Critical  
**Requirement:** Output shall settle to <0.1% within 10µs for load steps.

**Detailed Specifications:**
- Load step: 0.1A to 3A (or vice versa)
- Rise/fall time: 1µs
- Peak deviation: <100mV
- Settling time to 0.1%: <10µs
- Settling time to 0.01%: <50µs
- No ringing or oscillation

#### PR-041: Line Transient Response
**Priority:** High  
**Requirement:** Output shall settle to <0.1% within 20µs for line steps.

**Detailed Specifications:**
- Input step: 5V to 12V (or vice versa)
- Rise/fall time: 1µs
- Peak deviation: <50mV
- Settling time to 0.1%: <20µs
- No ringing or oscillation

#### PR-042: Output Slew Rate
**Priority:** Medium  
**Requirement:** Output voltage slew rate shall be programmable.

**Detailed Specifications:**
- Minimum slew rate: 0.1V/ms
- Maximum slew rate: 10V/ms (limited by stability)
- Slew rate accuracy: ±20%
- Programmable in 8 steps minimum

### 3.3 Stability and Loop Response

#### PR-050: Phase Margin
**Priority:** Critical  
**Requirement:** Control loop shall maintain >45° phase margin.

**Detailed Specifications:**
- Phase margin: >45° all conditions, >60° typical
- Gain margin: >10dB
- Load range: 0.1A to 3A
- All output capacitor combinations within spec
- Temperature range: -40°C to +125°C

#### PR-051: Bandwidth
**Priority:** Medium  
**Requirement:** Control loop bandwidth shall be >100kHz.

**Detailed Specifications:**
- Unity gain crossover: 100kHz minimum, 200kHz typical
- Bandwidth variation with load: <2:1 ratio
- Consistent across all output voltages

## 4. Timing Requirements

### 4.1 Startup and Shutdown

#### PR-060: Power-On Time
**Priority:** High  
**Requirement:** Outputs shall reach regulation within 5ms of enable.

**Detailed Specifications:**
- Enable to power-good: <5ms (with soft-start disabled)
- Enable to output valid: <2ms
- Soft-start time: 0.1ms to 10ms (programmable)

#### PR-061: Shutdown Time
**Priority:** Medium  
**Requirement:** Outputs shall discharge below 10% within 10ms.

**Detailed Specifications:**
- 90% to 10% discharge time: <10ms (with 10µF load)
- Active discharge enabled
- Emergency shutdown: <100µs to start discharge

#### PR-062: Sequencing Accuracy
**Priority:** Medium  
**Requirement:** Inter-channel timing shall be accurate to ±5%.

**Detailed Specifications:**
- Delay step size: 1ms
- Delay range: 0-100ms
- Timing accuracy: ±5% or ±1ms (whichever is greater)

### 4.2 Digital Interface Timing

#### PR-070: I2C Timing
**Priority:** High  
**Requirement:** I2C interface shall meet standard specifications.

**Detailed Specifications:**
- Standard mode (100kHz): Full compliance
- Fast mode (400kHz): Full compliance
- Setup time: Per I2C specification
- Hold time: Per I2C specification
- Rise/fall time: Per I2C specification

#### PR-071: Response Time
**Priority:** Medium  
**Requirement:** Register writes shall take effect within 100µs.

**Detailed Specifications:**
- Configuration register update: <100µs
- Output voltage change start: <100µs from DAC write
- Status register update rate: >1kHz
- ADC conversion time: <1ms

## 5. Efficiency and Power Consumption

### 5.1 Efficiency

#### PR-080: Power Conversion Efficiency
**Priority:** High  
**Requirement:** Efficiency shall be >90% at nominal load.

**Detailed Specifications:**
- Efficiency at 1.5A load: >90%
- Efficiency at 3A load: >88%
- Efficiency at 0.5A load: >85%
- Input voltage: 12V
- Output voltage: 3.3V
- All channels loaded

#### PR-081: Quiescent Current
**Priority:** Medium  
**Requirement:** Quiescent current shall be <5mA per channel.

**Detailed Specifications:**
- No-load quiescent current: <5mA per active channel
- Shutdown current: <50µA (all channels disabled)
- Digital interface current: <2mA

### 5.2 Thermal Performance

#### PR-090: Thermal Resistance
**Priority:** High  
**Requirement:** Thermal resistance shall support full power dissipation.

**Detailed Specifications:**
- θJA (with 2oz copper): <30°C/W
- θJC (junction to case): <5°C/W
- Maximum power dissipation: 4W per package @ 85°C ambient
- Thermal pad required for proper operation

#### PR-091: Operating Temperature
**Priority:** Critical  
**Requirement:** Device shall operate from -40°C to +125°C junction.

**Detailed Specifications:**
- Ambient temperature range: -40°C to +85°C
- Junction temperature range: -40°C to +125°C
- Storage temperature: -55°C to +150°C
- All specifications met -40°C to +85°C ambient

## 6. Measurement and Monitoring

### 6.1 Voltage Monitoring

#### PR-100: Voltage Measurement Accuracy
**Priority:** High  
**Requirement:** Voltage readback shall be accurate to ±0.05%.

**Detailed Specifications:**
- ADC resolution: 14-bit minimum
- Accuracy: ±0.05% typical, ±0.1% maximum
- Update rate: >1kHz per channel
- Conversion time: <1ms

### 6.2 Current Monitoring

#### PR-110: Current Measurement Accuracy
**Priority:** High  
**Requirement:** Current readback shall be accurate to ±1%.

**Detailed Specifications:**
- ADC resolution: 12-bit minimum
- Accuracy: ±0.5% typical, ±1% maximum
- Offset: <10mA
- Update rate: >1kHz per channel

### 6.3 Temperature Monitoring

#### PR-120: Temperature Measurement Accuracy
**Priority:** Medium  
**Requirement:** Temperature readback shall be accurate to ±5°C.

**Detailed Specifications:**
- Sensor accuracy: ±5°C over full range
- Resolution: 1°C or better
- Update rate: >10Hz
- Range: -40°C to +150°C

## 7. Environmental Requirements

### 7.1 EMC Performance

#### PR-130: Conducted Emissions
**Priority:** High  
**Requirement:** Shall meet CISPR 11 Class A limits.

**Detailed Specifications:**
- Frequency range: 150kHz to 30MHz
- Class A limits per CISPR 11
- Measured with standard LISN

#### PR-131: Radiated Emissions
**Priority:** High  
**Requirement:** Shall meet CISPR 11 Class A limits.

**Detailed Specifications:**
- Frequency range: 30MHz to 1GHz
- Class A limits per CISPR 11
- Measured at 3m distance

#### PR-132: ESD Immunity
**Priority:** High  
**Requirement:** Shall withstand ±4kV contact, ±8kV air discharge.

**Detailed Specifications:**
- Contact discharge: ±4kV (IEC 61000-4-2 Level 3)
- Air discharge: ±8kV (IEC 61000-4-2 Level 3)
- No latchup or damage
- No loss of function during/after event

## 8. Reliability Requirements

#### PR-140: MTBF
**Priority:** Medium  
**Requirement:** Mean Time Between Failures >1,000,000 hours.

**Detailed Specifications:**
- MTBF: >1M hours at 40°C
- Failure rate: <1 FIT
- Calculated per MIL-HDBK-217F

#### PR-141: Lifetime
**Priority:** Medium  
**Requirement:** 10-year operating life at maximum ratings.

**Detailed Specifications:**
- Operating life: >87,600 hours
- Temperature: 85°C maximum continuous
- No performance degradation >10%

## 9. Test Conditions

All specifications apply under the following conditions unless otherwise noted:
- Input voltage: 12V nominal (5V-12V range)
- Output voltage: 2.5V nominal (0.5V-5.0V range)  
- Load current: 1.5A nominal (0-3A range)
- Ambient temperature: 25°C (specifications valid -40°C to +85°C)
- Output capacitor: 22µF ceramic
- PCB: 4-layer, 2oz copper

## 10. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | System Architecture | Initial release |
