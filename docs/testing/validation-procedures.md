# Validation Procedures - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document provides detailed step-by-step validation procedures for the PM-5000 PMIC. These procedures are used during design verification and qualification testing.

## 2. Test Setup

### 2.1 Standard Test Configuration

**Equipment Required:**
- Precision power supply (0-20V, 10A)
- Two 6.5-digit DMMs
- Four-channel oscilloscope (500MHz, 1GSa/s)
- Electronic loads (4 units, 0-5A each)
- Temperature chamber (-55°C to 150°C)
- I2C controller/analyzer
- Evaluation board for PM-5000

**DUT Configuration:**
- VIN = 12V (unless specified otherwise)
- All channels enabled
- Default I2C address (0x40)
- Output capacitors: 47µF ceramic per channel
- Input capacitor: 22µF ceramic

### 2.2 Measurement Points

```
Test Board Measurement Points:
- TP1: VIN
- TP2-TP5: VOUT1-4
- TP6-TP9: SW1-4 (switch nodes)
- TP10: GND
- TP11: I2C SDA
- TP12: I2C SCL
- TP13: EN
- TP14: INT#
```

## 3. DC Performance Validation

### 3.1 VP-001: Output Voltage Accuracy Test

**Objective:** Validate output voltage accuracy meets ±0.1% specification

**Equipment:**
- 6.5-digit DMM (calibrated, ±0.01% accuracy)
- Precision power supply
- Electronic load

**Procedure:**

1. **Setup:**
   ```
   - Connect VIN to precision supply set to 12V
   - Connect DMM to VOUT1 (TP2)
   - Connect electronic load to VOUT1
   - Configure I2C communication
   ```

2. **Initial Configuration:**
   ```
   I2C Write: 0x10 = 0x01    // Enable CH1
   I2C Write: 0x11-12 = TBD  // Set voltage (see table)
   ```

3. **Test Matrix:**
   
   For each voltage in the table below:
   
   | Voltage | VSET Value | Expected Reading | Tolerance |
   |---------|------------|------------------|-----------|
   | 0.50V | 0x000 | 0.500V | ±0.5mV |
   | 1.00V | 0x1C7 | 1.000V | ±1.0mV |
   | 1.80V | 0x49F | 1.800V | ±1.8mV |
   | 2.50V | 0x71C | 2.500V | ±2.5mV |
   | 3.30V | 0x9F4 | 3.300V | ±3.3mV |
   | 5.00V | 0xFFF | 5.000V | ±5.0mV |

4. **Load Regulation Test:**
   
   For each voltage setting:
   ```
   a. Set load to 0A, record voltage
   b. Set load to 1.5A, record voltage
   c. Set load to 3.0A, record voltage
   d. Calculate load regulation:
      Load_Reg = (VOUT_0A - VOUT_3A) / VOUT_nominal / 3A
   ```

5. **Line Regulation Test:**
   
   At VOUT = 3.3V, ILOAD = 1.5A:
   ```
   a. Set VIN to 5V, record VOUT
   b. Set VIN to 8.5V, record VOUT
   c. Set VIN to 12V, record VOUT
   d. Calculate line regulation:
      Line_Reg = (VOUT_12V - VOUT_5V) / VOUT_nominal / 7V
   ```

6. **Temperature Coefficient:**
   ```
   a. Place DUT in temperature chamber
   b. Set VOUT = 3.3V, ILOAD = 1.5A, VIN = 12V
   c. Measure VOUT at -40°C, 25°C, 85°C
   d. Calculate tempco:
      Tempco = (VOUT_85C - VOUT_25C) / VOUT_25C / 60°C
   ```

**Pass Criteria:**
- ✓ Voltage accuracy: ±0.05% typical, ±0.1% max at 25°C
- ✓ Load regulation: <0.01%/A
- ✓ Line regulation: <0.005%/V
- ✓ Temperature coefficient: <25ppm/°C

**Data Recording:**
```
Test Date: __________
DUT S/N: __________
Temperature: ________°C
Tested by: __________

VOUT Settings    0A        1.5A      3.0A      Load Reg
0.50V           ____V     ____V     ____V     ____%/A
1.00V           ____V     ____V     ____V     ____%/A
1.80V           ____V     ____V     ____V     ____%/A
2.50V           ____V     ____V     ____V     ____%/A
3.30V           ____V     ____V     ____V     ____%/A
5.00V           ____V     ____V     ____V     ____%/A

Line Regulation Test (3.3V, 1.5A):
VIN=5V:    ____V
VIN=8.5V:  ____V
VIN=12V:   ____V
Line Reg:  ____%/V

Temperature Coefficient (3.3V, 1.5A, 12V):
-40°C:  ____V
25°C:   ____V
85°C:   ____V
Tempco: ____ppm/°C

PASS / FAIL
```

### 3.2 VP-002: Output Noise and Ripple Test

**Objective:** Validate output noise <50µVrms and ripple <100µVpp

**Equipment:**
- Oscilloscope with low-noise probe
- 50Ω feed-through terminator (optional for ultra-low noise)
- Spectrum analyzer (optional)

**Procedure:**

1. **Oscilloscope Setup:**
   ```
   - Bandwidth: 20MHz
   - Coupling: AC (DC for ripple measurement)
   - Vertical scale: 50µV/div for noise, 100µV/div for ripple
   - Timebase: 500µs/div for noise, 1µs/div for ripple
   - Averaging: 16 averages
   - Probing: Use ground spring or coax
   ```

2. **Measurement Setup:**
   ```
   - Configure VOUT1 = 2.5V
   - Set ILOAD = 1.0A
   - VIN = 12V
   - Allow 5 minutes warm-up
   - Ensure low-noise environment
   ```

3. **Noise Measurement:**
   ```
   a. Set oscilloscope to AC coupling, 20MHz BW limit
   b. Measure RMS noise (use scope statistics function)
   c. Record for 60 seconds minimum
   d. Calculate average RMS value
   e. Repeat at ILOAD = 0.1A, 1.5A, 3.0A
   ```

4. **Ripple Measurement:**
   ```
   a. Set oscilloscope to AC coupling, 20MHz BW
   b. Trigger on switching frequency (~1MHz)
   c. Measure peak-to-peak ripple
   d. Identify frequency components (FFT)
   e. Record at multiple load currents
   ```

5. **Frequency Spectrum (Optional):**
   ```
   a. Use spectrum analyzer or oscilloscope FFT
   b. Frequency range: 10Hz to 20MHz
   c. Identify noise peaks
   d. Check for spurious frequencies
   ```

**Pass Criteria:**
- ✓ Broadband noise (20Hz-20MHz): <50µVrms typical, <75µVrms max
- ✓ Low frequency noise (0.1Hz-10Hz): <5µVrms
- ✓ Ripple at switching frequency: <100µVpp typical, <200µVpp max

**Data Recording:**
```
Test Date: __________
DUT S/N: __________
VOUT = 2.5V, VIN = 12V

Load Current    RMS Noise    Pk-Pk Ripple    Switching Freq
0.1A           ____µVrms    ____µVpp         ____MHz
1.0A           ____µVrms    ____µVpp         ____MHz
1.5A           ____µVrms    ____µVpp         ____MHz
3.0A           ____µVrms    ____µVpp         ____MHz

Notes:
_________________________________________________________________

PASS / FAIL
```

### 3.3 VP-003: Load Transient Response Test

**Objective:** Validate settling time <10µs to 0.1% for load transients

**Equipment:**
- Oscilloscope (500MHz, 1GSa/s)
- Electronic load with dynamic mode
- Current probe (optional)

**Procedure:**

1. **Load Setup:**
   ```
   - Configure electronic load in dynamic mode
   - Load levels: 0.1A / 3.0A
   - Edge time: 1µs
   - Frequency: 1kHz (500µs period)
   ```

2. **Oscilloscope Setup:**
   ```
   - CH1: VOUT1 (AC coupled, 50mV/div)
   - CH2: Load current (current probe)
   - Timebase: 5µs/div
   - Trigger: Rising edge on load current
   - Sample rate: 1GSa/s minimum
   - Single shot mode or persistence
   ```

3. **DUT Configuration:**
   ```
   - VOUT1 = 3.3V
   - VIN = 12V
   - Output cap = 47µF ceramic
   ```

4. **Rising Load Step (0.1A → 3.0A):**
   ```
   a. Trigger oscilloscope on rising current edge
   b. Capture waveform
   c. Measure:
      - Peak undershoot voltage
      - Time to settle within 0.1% (3.3mV)
      - Time to settle within 0.01% (0.33mV)
      - Presence of ringing/oscillation
   d. Save waveform
   ```

5. **Falling Load Step (3.0A → 0.1A):**
   ```
   a. Trigger on falling current edge
   b. Capture waveform
   c. Measure:
      - Peak overshoot voltage
      - Time to settle within 0.1%
      - Time to settle within 0.01%
      - Presence of ringing/oscillation
   d. Save waveform
   ```

6. **Variation Testing:**
   ```
   a. Repeat at VOUT = 1.8V and 5.0V
   b. Repeat at VIN = 5V and 12V
   c. Repeat with different output capacitors if applicable
   ```

**Pass Criteria:**
- ✓ Peak deviation: <100mV
- ✓ Settling time to 0.1%: <10µs
- ✓ Settling time to 0.01%: <50µs
- ✓ No sustained oscillation

**Data Recording:**
```
Test Date: __________
DUT S/N: __________

Rising Edge (0.1A → 3.0A):
VOUT Setting: 3.3V
Peak Undershoot: ____mV
Settling to 0.1% (3.3mV): ____µs
Settling to 0.01% (0.33mV): ____µs
Ringing observed: YES / NO

Falling Edge (3.0A → 0.1A):
VOUT Setting: 3.3V
Peak Overshoot: ____mV
Settling to 0.1%: ____µs
Settling to 0.01%: ____µs
Ringing observed: YES / NO

Waveform files saved: ________________

PASS / FAIL
```

## 4. Protection Validation

### 4.1 VP-010: Over-Current Protection Test

**Objective:** Validate current limiting protects device and load

**Equipment:**
- Precision power supply
- Electronic load or resistive load
- DMM for current measurement
- Oscilloscope

**Procedure:**

1. **Setup:**
   ```
   - VOUT1 = 3.3V
   - VIN = 12V
   - Program current limit to 1.0A
   I2C Write: 0x13-14 = 0x400  // 1.0A limit
   ```

2. **Gradual Overload Test:**
   ```
   a. Start with no load
   b. Gradually increase load current
   c. Monitor output voltage and current
   d. Observe behavior when current reaches limit:
      - Voltage should drop (fold-back)
      - or Current should limit (constant current)
   e. Record current limit accuracy
   ```

3. **Short Circuit Test:**
   ```
   a. Remove load
   b. Verify output at 3.3V
   c. Short output to ground with low-resistance wire
   d. Monitor:
      - Short circuit current
      - Junction temperature
      - Time to fault indication
   e. Maintain short for 60 seconds
   f. Remove short, verify recovery
   ```

4. **Fault Status Test:**
   ```
   a. Trigger overcurrent condition
   b. Read fault status register:
      I2C Read: 0x1A (CH1_FAULT)
   c. Verify OCP bit is set
   d. Verify interrupt asserted (INT# = LOW)
   e. Clear fault by reading register
   f. Verify interrupt clears
   ```

5. **Hiccup Mode Test (if applicable):**
   ```
   a. Configure for hiccup mode
   b. Apply short circuit
   c. Observe hiccup behavior on oscilloscope
   d. Measure:
      - On time
      - Off time
      - Current during on time
   ```

**Pass Criteria:**
- ✓ Current limit accuracy: ±5%
- ✓ Short circuit current: <4A
- ✓ No thermal damage after 60s short
- ✓ Fault status correctly reported
- ✓ Device recovers after fault removed

**Data Recording:**
```
Test Date: __________
DUT S/N: __________

Current Limit Test:
Programmed Limit: 1.0A
Measured Limit: ____A
Accuracy: ____%

Short Circuit Test:
Short Circuit Current: ____A
Duration: 60 seconds
Junction Temp (estimated): ____°C
Recovery: SUCCESS / FAIL

Fault Reporting:
OCP Bit Set: YES / NO
INT# Asserted: YES / NO
Fault Clears: YES / NO

PASS / FAIL
```

### 4.2 VP-011: Thermal Shutdown Test

**Objective:** Validate thermal shutdown at 125°C prevents damage

**Equipment:**
- Temperature chamber
- Thermocouple
- Power supply and loads
- I2C controller

**Procedure:**

1. **Thermal Chamber Method:**
   ```
   a. Place DUT in temperature chamber
   b. Configure all channels enabled, maximum load
   c. Set chamber to 25°C, verify operation
   d. Gradually increase temperature
   e. Monitor operation and junction temperature
   f. Record temperature at thermal warning (expect 100°C)
   g. Continue heating
   h. Record temperature at shutdown (expect 125°C)
   i. Allow cooling, verify auto-restart at 110°C
   ```

2. **Self-Heating Method:**
   ```
   a. Configure for maximum power dissipation:
      - All channels enabled
      - Low VOUT, high current (maximize LDO dropout)
      - High VIN = 12V
      - Example: VOUT = 0.5V, IOUT = 3A per channel
   b. Monitor junction temperature via I2C
   c. Observe thermal warning at 100°C
   d. Continue operation
   e. Observe shutdown at 125°C
   f. Reduce load, verify restart
   ```

3. **Fault Status Verification:**
   ```
   a. During thermal event, read status:
      I2C Read: 0x56 (TEMP_MON)
      I2C Read: 0x60 (FAULT_STATUS)
   b. Verify thermal warning bit set at 100°C
   c. Verify thermal shutdown bit set at 125°C
   d. Verify INT# assertion
   ```

**Pass Criteria:**
- ✓ Thermal warning: 100°C ±5°C
- ✓ Thermal shutdown: 125°C ±5°C
- ✓ Auto-restart: 110°C ±5°C (if enabled)
- ✓ No damage to device
- ✓ Proper fault reporting

**Data Recording:**
```
Test Date: __________
DUT S/N: __________

Thermal Warning:
Readback Temperature: ____°C
Thermocouple Reading: ____°C
Warning Asserted: YES / NO

Thermal Shutdown:
Readback Temperature: ____°C
Thermocouple Reading: ____°C
Shutdown Occurred: YES / NO

Auto-Restart:
Restart Temperature: ____°C
Successful Restart: YES / NO

Post-Test Verification:
Device operates normally: YES / NO
No visible damage: YES / NO

PASS / FAIL
```

## 5. Digital Interface Validation

### 5.1 VP-020: I2C Communication Test

**Objective:** Validate I2C interface compliance and functionality

**Equipment:**
- I2C analyzer/controller
- Oscilloscope (for timing analysis)
- Pull-up resistors (4.7kΩ)

**Procedure:**

1. **Basic Communication:**
   ```
   a. Connect I2C controller
   b. Scan I2C bus for devices
   c. Verify device responds at correct address
   d. Read device ID registers:
      I2C Read: 0x00 → expect 0x50 ('P')
      I2C Read: 0x01 → expect 0x4D ('M')
   ```

2. **Register Access:**
   ```
   a. Write to all writable registers
   b. Read back and verify data
   c. Attempt write to read-only registers
   d. Verify read-only registers don't change
   e. Test all channel register banks
   ```

3. **Timing Compliance:**
   ```
   Standard Mode (100kHz):
   a. Set I2C clock to 100kHz
   b. Capture transaction on oscilloscope
   c. Measure timing parameters:
      - tLOW (SCL low period): ≥4.7µs
      - tHIGH (SCL high period): ≥4.0µs
      - tSU;DAT (data setup): ≥250ns
      - tHD;DAT (data hold): ≤3.45µs
   
   Fast Mode (400kHz):
   d. Set I2C clock to 400kHz
   e. Repeat timing measurements
   f. Verify compliance with fast mode specs
   ```

4. **Clock Stretching:**
   ```
   a. Initiate ADC read during conversion
   b. Monitor SCL line
   c. Verify clock stretching occurs
   d. Measure stretch duration (<1ms expected)
   ```

5. **Address Selection:**
   ```
   a. Test all 4 I2C addresses:
      ADDR[1:0] = 00 → 0x40
      ADDR[1:0] = 01 → 0x41
      ADDR[1:0] = 10 → 0x42
      ADDR[1:0] = 11 → 0x43
   b. Change ADDR pins, verify address changes
   ```

**Pass Criteria:**
- ✓ Device responds at all 4 addresses
- ✓ All registers accessible
- ✓ Read-only registers protected
- ✓ Timing compliant with I2C spec
- ✓ Clock stretching works correctly

**Data Recording:**
```
Test Date: __________
DUT S/N: __________

Device ID:
Address 0x00: ____ (expect 0x50)
Address 0x01: ____ (expect 0x4D)
Address 0x02: ____ (revision)

Register Access:
All writable registers: PASS / FAIL
Read-only protection: PASS / FAIL

Timing (100kHz):
tLOW: ____µs (spec ≥4.7µs)
tHIGH: ____µs (spec ≥4.0µs)
tSU;DAT: ____ns (spec ≥250ns)

Timing (400kHz):
tLOW: ____µs (spec ≥1.3µs)
tHIGH: ____µs (spec ≥0.6µs)
tSU;DAT: ____ns (spec ≥100ns)

Clock Stretching:
Observed: YES / NO
Duration: ____ms

Address Selection:
0x40: PASS / FAIL
0x41: PASS / FAIL
0x42: PASS / FAIL
0x43: PASS / FAIL

PASS / FAIL
```

## 6. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | Test Engineering | Initial release |
