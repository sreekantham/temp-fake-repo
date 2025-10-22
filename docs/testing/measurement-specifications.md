# Measurement Specifications - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document defines measurement methodologies and specifications for characterizing the PM-5000 PMIC in test and measurement applications. It provides guidance for system designers integrating the PMIC into precision instruments.

## 2. Measurement Capability Overview

The PM-5000 provides comprehensive built-in measurement capabilities for monitoring power delivery in real-time.

### 2.1 Measurement Channels

| Parameter | Channels | Resolution | Accuracy | Update Rate |
|-----------|----------|------------|----------|-------------|
| Output Voltage | 4 | 14-bit | ±0.05% | 1kHz |
| Output Current | 4 | 12-bit | ±1% | 1kHz |
| Input Voltage | 1 | 12-bit | ±0.5% | 100Hz |
| Junction Temperature | 1 | 10-bit | ±5°C | 10Hz |

## 3. Voltage Measurement

### 3.1 Voltage Measurement Architecture

**Implementation:**
- Direct sensing at output terminal
- 14-bit successive approximation ADC
- Differential input configuration
- Kelvin sensing capability (dedicated sense pins)

**Signal Chain:**
```
VOUT → Sense Resistor Network → Input Buffer → ADC → Digital Filter → Register
```

### 3.2 Voltage Measurement Specifications

| Parameter | Min | Typ | Max | Unit | Conditions |
|-----------|-----|-----|-----|------|------------|
| Measurement range | 0 | - | 5.5 | V | - |
| Resolution | - | 14 | - | bits | 0.34mV LSB |
| Absolute accuracy | - | ±0.05 | ±0.1 | % of reading | 25°C |
| Temperature coefficient | - | 25 | - | ppm/°C | -40°C to 85°C |
| Offset error | - | ±0.5 | ±1.0 | mV | - |
| Gain error | - | ±0.05 | ±0.1 | % | - |
| Noise (RMS) | - | - | 0.5 | mV | 1Hz BW |
| Conversion time | - | 0.8 | 1.0 | ms | Per channel |
| Sample rate | 500 | 1000 | - | Hz | Per channel |

### 3.3 Voltage Readback Format

**Register Format:**
- 16-bit register (14-bit data, MSB aligned)
- Two's complement representation (for differential sensing)
- Scaling: 1 LSB = 5.5V / 16384 = 0.336mV

**Calculation:**
```
VOUT = (ADC_CODE × 5.5V) / 16384
ADC_CODE = (VOUT × 16384) / 5.5V
```

**Example:**
```
VOUT = 3.300V
ADC_CODE = (3.300 × 16384) / 5.5 = 9830 = 0x2666
Reading registers 0x15-16 (CH1_VMON): 0x26, 0x66
```

### 3.4 Voltage Measurement Accuracy Enhancement

**Calibration:**
- Two-point calibration (offset and gain)
- Factory calibration stored in OTP
- User calibration via registers

**Calibration Procedure:**
1. Measure VOUT with external reference DMM
2. Read internal ADC value
3. Calculate offset and gain errors
4. Write calibration coefficients to registers

**Calibration Registers:**
```
CHx_VCAL_OFFSET: 0x70 + (channel × 4)
CHx_VCAL_GAIN: 0x71 + (channel × 4)
```

## 4. Current Measurement

### 4.1 Current Measurement Architecture

**Implementation:**
- Integrated current sense amplifier
- In-line sense resistor (5mΩ typical)
- 12-bit successive approximation ADC
- Bidirectional measurement capability

**Signal Chain:**
```
IOUT → Sense Resistor → Current Sense Amp → ADC → Digital Filter → Register
```

### 4.2 Current Measurement Specifications

| Parameter | Min | Typ | Max | Unit | Conditions |
|-----------|-----|-----|-----|------|------------|
| Measurement range | -0.5 | - | 4.0 | A | Bidirectional |
| Resolution | - | 12 | - | bits | 1.1mA LSB |
| Absolute accuracy | - | ±0.5 | ±1.0 | % of reading | 25°C, >100mA |
| Offset error | - | ±5 | ±10 | mA | Near zero current |
| Gain error | - | ±0.5 | ±1.0 | % | - |
| Temperature coefficient | - | 100 | - | ppm/°C | -40°C to 85°C |
| Noise (RMS) | - | - | 5 | mA | 1Hz BW |
| Conversion time | - | 0.8 | 1.0 | ms | Per channel |
| Sample rate | 500 | 1000 | - | Hz | Per channel |
| Sense resistor | - | 5 | - | mΩ | Integrated |
| Sense resistor TCR | - | ±50 | - | ppm/°C | - |

### 4.3 Current Readback Format

**Register Format:**
- 16-bit register (12-bit data, MSB aligned)
- Two's complement for bidirectional measurement
- Scaling: 1 LSB = 4.5A / 4096 = 1.1mA

**Calculation:**
```
IOUT = (ADC_CODE × 4.5A) / 4096
ADC_CODE = (IOUT × 4096) / 4.5A
```

**Example:**
```
IOUT = 1.500A
ADC_CODE = (1.500 × 4096) / 4.5 = 1365 = 0x555
Reading registers 0x17-18 (CH1_IMON): 0x55, 0x50
```

### 4.4 Current Measurement Accuracy Enhancement

**Calibration:**
- Two-point calibration (zero current and full scale)
- Automatic zero calibration on enable
- User calibration available

**Auto-Zero Procedure:**
1. Channel enabled with no load
2. Measure current sense ADC
3. Store offset in calibration register
4. Subtract offset from all subsequent readings

## 5. Temperature Measurement

### 5.1 Temperature Sensor Architecture

**Implementation:**
- On-die bandgap temperature sensor
- Proportional to absolute temperature (PTAT)
- 10-bit ADC
- Digital temperature compensation

### 5.2 Temperature Measurement Specifications

| Parameter | Min | Typ | Max | Unit | Conditions |
|-----------|-----|-----|-----|------|------------|
| Measurement range | -55 | - | 150 | °C | - |
| Resolution | - | 10 | - | bits | 0.2°C LSB |
| Absolute accuracy | - | ±3 | ±5 | °C | -40°C to 125°C |
| Conversion time | - | 10 | - | ms | - |
| Sample rate | - | 10 | 100 | Hz | Programmable |

### 5.3 Temperature Readback Format

**Register Format:**
- 8-bit register
- Signed integer representation
- Scaling: 1 LSB = 1°C

**Calculation:**
```
Temperature (°C) = ADC_CODE (signed)
```

**Example:**
```
Temperature = 75°C
ADC_CODE = 75 = 0x4B
Reading register 0x56 (TEMP_MON): 0x4B

Temperature = -25°C  
ADC_CODE = -25 = 0xE7 (two's complement)
Reading register 0x56 (TEMP_MON): 0xE7
```

## 6. Input Voltage Measurement

### 6.1 Input Voltage Monitoring

**Implementation:**
- Resistive divider from VIN
- 12-bit ADC
- Used for UVLO and monitoring

### 6.2 Input Voltage Specifications

| Parameter | Min | Typ | Max | Unit |
|-----------|-----|-----|-----|------|
| Measurement range | 0 | - | 16 | V |
| Resolution | - | 12 | - | bits |
| Absolute accuracy | - | ±0.5 | ±1.0 | % |
| Sample rate | - | 100 | - | Hz |

## 7. Measurement Synchronization

### 7.1 Simultaneous Sampling

**Feature:**
- All voltage and current ADCs sample simultaneously
- Synchronization within 1µs
- Enables accurate power calculation

**Application:**
```
Power = VOUT × IOUT (measured at same instant)
```

### 7.2 Measurement Timing

**Trigger Modes:**
- Free-running (continuous conversion)
- I2C triggered (on-demand conversion)
- External trigger (via GPIO, optional)

**Timing Diagram:**
```
I2C Trigger ─┐     ┌─────────────┐
             └─────┘             └─────

Sample Point        ↓
                    
ADC Conv    ─────┌─────┐
                 └─────┘ (<1ms)

Data Ready         └────┐
                        └─────────
```

## 8. Measurement Filtering

### 8.1 Digital Filtering

**Implementation:**
- IIR filter (single-pole)
- Programmable bandwidth
- Reduces noise without latency

**Filter Configurations:**
| Setting | Bandwidth | Settling Time | Noise Reduction |
|---------|-----------|---------------|-----------------|
| 0 (Off) | Full (1kHz) | 1ms | None |
| 1 | 500Hz | 2ms | 3dB |
| 2 | 100Hz | 10ms | 10dB |
| 3 | 10Hz | 100ms | 20dB |

### 8.2 Averaging

**Hardware Averaging:**
- Selectable 1x, 4x, 16x, 64x averaging
- Reduces noise at expense of update rate

**Example:**
```
64x Averaging:
- Sample rate: 1000Hz / 64 = 15.6Hz
- Noise reduction: √64 = 8x (18dB)
```

## 9. Power Calculation

### 9.1 Instantaneous Power

**Calculation:**
```
P_inst = VOUT × IOUT
```

**Example:**
```
VOUT = 3.300V (from ADC)
IOUT = 1.500A (from ADC)
P_inst = 3.300 × 1.500 = 4.950W
```

### 9.2 Average Power

**Calculation:**
```
P_avg = (1/N) × Σ(VOUT[i] × IOUT[i])
```

**Implementation:**
- Accumulate N samples
- Calculate running average
- Configurable accumulation period

## 10. Measurement Uncertainty Analysis

### 10.1 Voltage Measurement Uncertainty

**Contributors:**
1. ADC quantization: ±0.5 LSB = ±0.17mV
2. ADC INL: ±1 LSB = ±0.34mV
3. Reference error: ±0.05%
4. Gain error: ±0.05%
5. Offset error: ±1mV
6. Temperature drift: ±25ppm/°C

**Combined Uncertainty (RSS):**
```
At 3.3V, 25°C:
U_voltage = √[(0.34mV)² + (1mV)² + (3.3V×0.05%)² + (3.3V×0.05%)²]
         = √[0.12 + 1.00 + 2.72 + 2.72]
         = 2.5mV = 0.076%
```

### 10.2 Current Measurement Uncertainty

**Contributors:**
1. ADC quantization: ±0.5 LSB = ±0.55mA
2. Sense resistor tolerance: ±1%
3. Sense amplifier gain error: ±0.5%
4. Offset error: ±10mA
5. Temperature drift: ±100ppm/°C

**Combined Uncertainty (RSS):**
```
At 1.5A, 25°C:
U_current = √[(0.55mA)² + (1.5A×1%)² + (1.5A×0.5%)² + (10mA)²]
         = √[0.30 + 225 + 56.25 + 100]
         = 19.5mA = 1.3%
```

### 10.3 Power Measurement Uncertainty

**Calculation:**
```
U_power = √(U_voltage² + U_current²)
        = √(0.076%² + 1.3%²)
        = 1.30%

At P = 4.95W (3.3V, 1.5A):
U_power = 4.95W × 1.30% = ±64mW
```

## 11. Application Examples

### 11.1 Example 1: Precision Voltage Monitoring

**Application:** Monitor 3.3V rail with ±10mV tolerance

**Configuration:**
```
I2C Write: 0x11-12 = 0x9F4  // Set 3.3V
I2C Write: 0x19 = 0x03      // Enable filter, averaging

Continuous monitoring:
Loop:
  I2C Read: 0x15-16         // Read voltage
  Convert to voltage
  Check if within 3.290V - 3.310V
  If out of range, trigger alert
End Loop
```

### 11.2 Example 2: Power Monitoring

**Application:** Calculate and log power consumption

**Configuration:**
```
I2C Write: Configure for simultaneous sampling

Periodic measurement:
Loop:
  I2C Read: 0x15-16         // Read voltage
  I2C Read: 0x17-18         // Read current
  Calculate: Power = V × I
  Accumulate energy: E += P × Δt
  Log data
  Wait 100ms
End Loop
```

### 11.3 Example 3: Efficiency Measurement

**Application:** Measure converter efficiency

**Configuration:**
```
Measure input power:
  VIN × IIN (external measurement)

Measure output power:
  VOUT × IOUT (internal measurement)

Calculate efficiency:
  η = (VOUT × IOUT) / (VIN × IIN) × 100%
```

## 12. Measurement Best Practices

### 12.1 Calibration Recommendations

1. **Factory Calibration:**
   - Performed at final test
   - Stored in OTP memory
   - Covers full temperature range

2. **Field Calibration:**
   - Optional for highest accuracy
   - Use precision reference equipment
   - Store coefficients in external memory

3. **Calibration Frequency:**
   - Annual verification recommended
   - Recalibrate after thermal stress
   - Check if accuracy degrades

### 12.2 Noise Reduction Techniques

1. **Hardware:**
   - Use recommended filter capacitors
   - Proper PCB layout (Kelvin sensing)
   - Shielded cables for remote sensing

2. **Software:**
   - Enable digital filtering
   - Use averaging
   - Discard first sample after mode change

3. **Measurement:**
   - Allow settling time after changes
   - Multiple measurements for critical readings
   - Statistical analysis (min, max, mean, σ)

### 12.3 Error Handling

**Invalid Readings:**
```
If (VOUT_reading > VOUT_setpoint × 1.2):
  // Possible OVP event or measurement error
  Read fault status register
  If (OVP_bit set):
    Handle OVP condition
  Else:
    Retry measurement
```

**Saturation Detection:**
```
If (ADC_CODE == 0xFFFF) or (ADC_CODE == 0x0000):
  // ADC saturated
  Check for fault condition
  Verify correct configuration
```

## 13. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | Applications Engineering | Initial release |
