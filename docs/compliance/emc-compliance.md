# EMC Compliance - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document outlines the electromagnetic compatibility (EMC) compliance strategy and test results for the PM-5000 PMIC in test and measurement applications.

## 2. Applicable Standards

### 2.1 Emissions Standards

| Standard | Description | Class | Region |
|----------|-------------|-------|--------|
| CISPR 11 | EMC for industrial, scientific and medical equipment | Class A | International |
| EN 55011 | European harmonized version of CISPR 11 | Class A | Europe |
| FCC Part 15 | Radio frequency devices | Class A | USA |

### 2.2 Immunity Standards

| Standard | Description | Level | Region |
|----------|-------------|-------|--------|
| IEC 61000-4-2 | Electrostatic discharge (ESD) | Level 3 | International |
| IEC 61000-4-3 | Radiated RF immunity | Level 3 | International |
| IEC 61000-4-4 | Electrical fast transient (EFT) | Level 3 | International |
| IEC 61000-4-5 | Surge immunity | Level 3 | International |
| IEC 61000-4-6 | Conducted RF immunity | Level 3 | International |
| IEC 61000-4-8 | Power frequency magnetic field | Level 4 | International |
| IEC 61000-4-11 | Voltage dips and interruptions | - | International |

### 2.3 Product-Specific Standards

| Standard | Description |
|----------|-------------|
| IEC 61326-1 | EMC for electrical equipment for measurement, control and laboratory use |
| EN 61326-1 | European version of IEC 61326-1 |

## 3. Conducted Emissions

### 3.1 CISPR 11 Class A Limits

**Measurement Bandwidth:** 9kHz (150kHz-30MHz)

| Frequency Range | Quasi-Peak Limit | Average Limit |
|-----------------|------------------|---------------|
| 150kHz - 500kHz | 79 dBµV | 66 dBµV |
| 500kHz - 5MHz | 79 dBµV | 66 dBµV |
| 5MHz - 30MHz | 73 dBµV | 60 dBµV |

### 3.2 Test Configuration

**Setup:**
- Line Impedance Stabilization Network (LISN)
- 50Ω termination
- Evaluation board with standard application circuit
- VIN = 12V, all channels enabled at 50% load

**Measurement Conditions:**
- Ambient temperature: 25°C
- Humidity: 45% RH
- Grounded metal test table

### 3.3 Test Results (Typical)

**Frequency Scan Results:**

| Frequency | Measured (QP) | Measured (AV) | Limit (QP) | Limit (AV) | Margin |
|-----------|---------------|---------------|------------|------------|--------|
| 150kHz | 55 dBµV | 42 dBµV | 79 dBµV | 66 dBµV | 24 dB |
| 500kHz | 58 dBµV | 45 dBµV | 79 dBµV | 66 dBµV | 21 dB |
| 1.0MHz (Fsw) | 64 dBµV | 52 dBµV | 79 dBµV | 66 dBµV | 14 dB |
| 2.0MHz (2×Fsw) | 52 dBµV | 40 dBµV | 79 dBµV | 66 dBµV | 26 dB |
| 5.0MHz | 48 dBµV | 36 dBµV | 73 dBµV | 60 dBµV | 24 dB |
| 10MHz | 45 dBµV | 33 dBµV | 73 dBµV | 60 dBµV | 27 dB |
| 30MHz | 38 dBµV | 26 dBµV | 73 dBµV | 60 dBµV | 34 dB |

**Status:** ✓ PASS - All measurements below limits with >10dB margin

### 3.4 EMI Reduction Techniques

**Implemented in PM-5000:**
1. **Spread Spectrum Frequency Modulation:**
   - ±10% frequency dithering around 1MHz
   - Triangular modulation profile
   - Reduces peak emissions by 8-12dB

2. **Optimized Switching Edges:**
   - Controlled gate drive slew rate
   - Minimizes high-frequency content
   - Balances efficiency and EMI

3. **Internal Filtering:**
   - Input capacitor integration guidance
   - Output filter recommendations
   - Common-mode filtering

4. **Package and Layout:**
   - Minimized loop areas
   - Multiple ground pins
   - Exposed pad for low-inductance ground

**PCB Design Guidelines:**
- Use 4-layer board minimum
- Solid ground plane (layer 2)
- Separate power plane (layer 3)
- Input filter near VIN pins (<5mm)
- Output filters near VOUT pins (<5mm)
- Keep SW traces <5mm length
- Use ground vias liberally

## 4. Radiated Emissions

### 4.1 CISPR 11 Class A Limits

**Measurement Distance:** 3 meters

| Frequency Range | Quasi-Peak Limit |
|-----------------|------------------|
| 30MHz - 230MHz | 40 dBµV/m |
| 230MHz - 1GHz | 47 dBµV/m |

### 4.2 Test Configuration

**Setup:**
- Semi-anechoic chamber
- 3-meter measurement distance
- Turntable for azimuth scan
- Antenna heights: 1m - 4m
- Horizontal and vertical polarizations

**DUT Configuration:**
- Evaluation board in application circuit
- VIN = 12V, all channels enabled
- 150mm input/output cables
- Grounded reference plane

### 4.3 Test Results (Typical)

| Frequency | Measured (H-Pol) | Measured (V-Pol) | Limit | Margin |
|-----------|------------------|------------------|-------|--------|
| 30MHz | 28 dBµV/m | 25 dBµV/m | 40 dBµV/m | 12 dB |
| 100MHz | 32 dBµV/m | 30 dBµV/m | 40 dBµV/m | 8 dB |
| 230MHz | 35 dBµV/m | 33 dBµV/m | 40 dBµV/m | 5 dB |
| 500MHz | 38 dBµV/m | 36 dBµV/m | 47 dBµV/m | 9 dB |
| 1GHz | 34 dBµV/m | 32 dBµV/m | 47 dBµV/m | 13 dB |

**Status:** ✓ PASS - All measurements below limits

### 4.4 Mitigation Techniques

**Design Features:**
- Shielded inductor recommendation
- Minimized loop areas
- Differential routing where applicable
- Proper grounding strategy

**Application Notes:**
- Use shielded cables for critical signals
- Metal enclosure recommended for Class B
- Ferrite beads on I2C lines (optional)

## 5. ESD Immunity

### 5.1 IEC 61000-4-2 Requirements

**Test Levels:**
- Contact Discharge: ±4kV (Level 3)
- Air Discharge: ±8kV (Level 3)

**Performance Criteria:**
- Criterion A: Normal operation during and after test
- Criterion B: Temporary loss of function, self-recoverable
- Criterion C: Temporary loss of function, requires intervention

### 5.2 Test Configuration

**Test Points:**
- All accessible pins and surfaces
- 10 discharges per point, each polarity
- Discharge interval: 1 second

**DUT State:**
- All channels enabled
- Continuous monitoring of outputs
- I2C communication active

### 5.3 Test Results

| Pin/Surface | Contact (±4kV) | Air (±8kV) | Result | Notes |
|-------------|----------------|------------|--------|-------|
| VIN | Pass (A) | Pass (A) | ✓ | No upset |
| VOUT1-4 | Pass (A) | Pass (A) | ✓ | No upset |
| SDA/SCL | Pass (B) | Pass (B) | ✓ | I2C recovers |
| EN | Pass (A) | Pass (A) | ✓ | No upset |
| INT# | Pass (A) | Pass (A) | ✓ | No upset |
| GND | Pass (A) | Pass (A) | ✓ | Reference |
| Package body | - | Pass (A) | ✓ | No upset |

**Status:** ✓ PASS - No damage, acceptable criteria met

### 5.4 ESD Protection Features

**On-Chip Protection:**
- All pins: >2kV HBM (Human Body Model)
- Power pins: Enhanced protection
- I2C pins: Bus-hold circuitry
- CDM rating: >500V

**External Protection:**
- TVS diodes recommended for harsh environments
- PCB clearances per IPC standards

## 6. EFT/Burst Immunity

### 6.1 IEC 61000-4-4 Requirements

**Test Levels:**
- Power lines: ±2kV (Level 3)
- Signal lines: ±1kV (Level 3)
- Burst frequency: 5kHz
- Burst duration: 15ms

### 6.2 Test Configuration

**Coupling:**
- Capacitive clamp for power lines
- Capacitive coupling for signal lines

**DUT State:**
- Normal operation
- All channels enabled
- Continuous monitoring

### 6.3 Test Results

| Test Point | Level | Result | Criterion | Notes |
|------------|-------|--------|-----------|-------|
| VIN | ±2kV | Pass | A | No upset |
| VOUT | ±2kV | Pass | A | No upset |
| I2C | ±1kV | Pass | B | Recoverable |

**Status:** ✓ PASS

## 7. Surge Immunity

### 7.1 IEC 61000-4-5 Requirements

**Test Levels:**
- Line-to-ground: ±1kV (Level 3)
- Line-to-line: ±0.5kV (Level 3)

### 7.2 Test Configuration

**Coupling Network:**
- 2Ω source impedance
- 9µF coupling capacitor

**DUT State:**
- Operational during test
- Monitor for damage after test

### 7.3 Test Results

| Test | Level | Polarity | Result | Notes |
|------|-------|----------|--------|-------|
| VIN to GND | ±1kV | Both | Pass (B) | Temporary upset, auto-recovery |
| VIN to VOUT | ±0.5kV | Both | Pass (A) | No upset |

**Status:** ✓ PASS

**Note:** External surge protection recommended for ±2kV (Level 4) applications

## 8. Conducted RF Immunity

### 8.1 IEC 61000-4-6 Requirements

**Test Level:** 10V (rms), 80% AM (1kHz) - Level 3

**Frequency Range:** 150kHz to 80MHz

### 8.2 Test Configuration

**Injection:**
- EM clamp or current injection probe
- Sweep frequency range
- Monitor DUT performance

### 8.3 Test Results

| Frequency Range | Level | Result | Criterion |
|----------------|-------|--------|-----------|
| 150kHz - 1MHz | 10V | Pass | A |
| 1MHz - 10MHz | 10V | Pass | A |
| 10MHz - 80MHz | 10V | Pass | A |

**Status:** ✓ PASS - No upset across full frequency range

## 9. Radiated RF Immunity

### 9.1 IEC 61000-4-3 Requirements

**Test Level:** 10V/m (Level 3)

**Frequency Range:** 80MHz to 1GHz

**Modulation:** 80% AM (1kHz)

### 9.2 Test Configuration

**Chamber:**
- Semi-anechoic or fully anechoic
- 3-meter distance
- Horizontal and vertical polarizations

### 9.3 Test Results

| Frequency Range | Level | Result | Notes |
|----------------|-------|--------|-------|
| 80MHz - 1GHz | 10V/m | Pass (A) | No upset |

**Status:** ✓ PASS

## 10. Magnetic Field Immunity

### 10.1 IEC 61000-4-8 Requirements

**Test Level:** 30A/m continuous (Level 4)

**Frequency:** 50Hz / 60Hz

### 10.2 Test Results

| Frequency | Level | Result | Notes |
|-----------|-------|--------|-------|
| 50Hz | 30A/m | Pass (A) | No upset |
| 60Hz | 30A/m | Pass (A) | No upset |

**Status:** ✓ PASS

## 11. Voltage Dips and Interruptions

### 11.1 IEC 61000-4-11 Requirements

**Test Conditions:**
- 30% dip for 10ms
- 60% dip for 100ms
- >95% dip for 5000ms (interruption)

### 11.2 Test Results

| Test | Result | Criterion | Notes |
|------|--------|-----------|-------|
| 30% dip, 10ms | Pass | A | No upset |
| 60% dip, 100ms | Pass | B | UVLO triggers, auto-recovery |
| >95% dip, 5000ms | Pass | C | Full restart required |

**Status:** ✓ PASS

## 12. Compliance Summary

### 12.1 Emissions Compliance

| Standard | Class | Status | Certificate |
|----------|-------|--------|-------------|
| CISPR 11 | A | ✓ PASS | Pending |
| EN 55011 | A | ✓ PASS | Pending |
| FCC Part 15 | A | ✓ PASS | Pending |

### 12.2 Immunity Compliance

| Standard | Level | Status |
|----------|-------|--------|
| IEC 61000-4-2 (ESD) | 3 | ✓ PASS |
| IEC 61000-4-3 (Radiated RF) | 3 | ✓ PASS |
| IEC 61000-4-4 (EFT) | 3 | ✓ PASS |
| IEC 61000-4-5 (Surge) | 3 | ✓ PASS |
| IEC 61000-4-6 (Conducted RF) | 3 | ✓ PASS |
| IEC 61000-4-8 (Magnetic) | 4 | ✓ PASS |
| IEC 61000-4-11 (Dips) | - | ✓ PASS |

### 12.3 Overall Status

**EMC Compliance:** ✓ COMPLETE

**Certifications:**
- CE Mark: Ready for application
- FCC: Ready for application
- IEC 61326-1: Compliant

## 13. Application Guidelines

### 13.1 PCB Layout for EMC

**Critical Requirements:**
1. Use 4-layer PCB minimum
2. Continuous ground plane
3. Input filter within 5mm of VIN
4. Short, wide power traces
5. Minimize loop areas

### 13.2 External Filtering

**Recommended Components:**
```
Input:
- 22µF ceramic capacitor (X7R, 25V) at VIN
- 47µF electrolytic (optional, for bulk)
- Common-mode choke (optional, for enhanced EMI reduction)

Output:
- 47µF ceramic capacitor (X7R) at each VOUT
- Additional 10µF for transient response

I2C Interface:
- 100Ω series resistors (optional)
- 22pF capacitors to ground (optional)
```

### 13.3 Enclosure Considerations

**Class A Compliance:**
- Open PCB: Compliant with guidelines
- Basic enclosure: Improved margin

**Class B Compliance:**
- Metal enclosure required
- I/O filtering required
- Additional ferrites may be needed

## 14. Test Reports

**Available Documentation:**
- Full EMC test report (200+ pages)
- FCC test report
- CE technical file
- IEC 61326-1 compliance report

**Contact:** [compliance@company.com](mailto:compliance@company.com)

## 15. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | EMC Engineering | Initial release |
