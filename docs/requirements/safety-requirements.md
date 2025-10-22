# Safety Requirements - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document defines safety requirements for the PM-5000 PMIC to ensure safe operation in test and measurement equipment and compliance with relevant safety standards.

## 2. Applicable Standards

The PM-5000 PMIC shall comply with the following safety standards:

- **IEC 61010-1**: Safety requirements for electrical equipment for measurement, control, and laboratory use
- **IEC 61010-2-030**: Particular requirements for testing and measuring circuits
- **UL 61010-1**: North American equivalent to IEC 61010-1
- **EN 61010-1**: European harmonized standard

## 3. General Safety Requirements

### 3.1 Electrical Safety

#### SR-001: Overvoltage Protection
**Priority:** Critical  
**Requirement:** The PMIC shall protect against overvoltage conditions.

**Implementation:**
- Output overvoltage protection at 110% of programmed voltage
- Input overvoltage protection at 14V
- Protection response time: <1µs
- Automatic shutdown with latched fault status
- No damage up to 16V input for 100ms

**Verification:**
- Apply 110% of maximum programmed output voltage
- Apply up to 16V to input for 100ms
- Verify shutdown occurs and no damage results

#### SR-002: Overcurrent Protection
**Priority:** Critical  
**Requirement:** The PMIC shall limit current to prevent damage.

**Implementation:**
- Programmable current limit per channel (0.1A-3.5A)
- Hardware current limit at 4A (non-programmable)
- Fold-back or hiccup mode operation
- Current limit accuracy: ±5%
- Fault status indication

**Verification:**
- Short-circuit testing on all outputs
- Verify current limiting engages
- Verify no device damage or thermal runaway

#### SR-003: Short Circuit Protection
**Priority:** Critical  
**Requirement:** All outputs shall be short-circuit proof.

**Implementation:**
- Continuous short circuit to ground supported
- Short circuit current limited to <4A
- No damage after 1 hour continuous short
- Automatic recovery when short removed (if configured)

**Verification:**
- Connect each output to ground for 1 hour minimum
- Verify no damage or performance degradation
- Test at minimum and maximum ambient temperatures

### 3.2 Thermal Safety

#### SR-010: Thermal Shutdown
**Priority:** Critical  
**Requirement:** The PMIC shall prevent thermal runaway.

**Implementation:**
- Thermal shutdown at 125°C junction temperature
- Thermal warning at 100°C junction temperature
- Automatic hysteretic restart at 110°C
- Shutdown response time: <10µs
- Fault status reporting

**Verification:**
- Thermal chamber testing to trigger shutdown
- Verify shutdown temperature accuracy
- Verify automatic restart operation

#### SR-011: Thermal Derating
**Priority:** High  
**Requirement:** Safe operating area shall be defined and enforced.

**Implementation:**
- Maximum power dissipation limits documented
- Current derating above 85°C ambient
- Junction temperature monitoring
- Thermal warning before shutdown

**Verification:**
- Create power dissipation curves
- Verify derating calculations
- Test at elevated ambient temperatures

#### SR-012: Package Thermal Characteristics
**Priority:** High  
**Requirement:** Thermal design shall prevent hazardous temperatures.

**Implementation:**
- Maximum case temperature: 125°C
- Thermal pad for heat dissipation
- θJA specification with recommended PCB layout
- No accessible surface >100°C under fault conditions

**Verification:**
- Thermal imaging under maximum load
- Temperature measurements per IEC 61010-1
- Verify surface temperatures under fault conditions

### 3.3 Input Power Safety

#### SR-020: Input Voltage Range
**Priority:** Critical  
**Requirement:** Safe operation over specified input voltage range.

**Implementation:**
- Operating input range: 5V to 12V
- No damage range: -0.3V to 16V
- Under-voltage lockout at 4.5V typical
- UVLO hysteresis: 200mV minimum

**Verification:**
- Functional testing 5V to 12V
- No-damage testing -0.3V to 16V
- UVLO threshold verification

#### SR-021: Input Reverse Polarity
**Priority:** High  
**Requirement:** Device shall withstand reverse polarity on input.

**Implementation:**
- Reverse polarity protection via external diode or P-FET
- No damage with -12V applied to input
- No damage for reverse polarity up to 1 minute

**Verification:**
- Apply -12V to input for 1 minute
- Verify no damage or degradation
- Test with and without recommended protection

#### SR-022: Input Transient Protection
**Priority:** High  
**Requirement:** Device shall withstand input voltage transients.

**Implementation:**
- Withstand transients per IEC 61000-4-5
- Surge immunity: ±1kV (Level 3)
- Fast transient burst: ±2kV (Level 3)
- No latchup or damage

**Verification:**
- Surge testing per IEC 61000-4-5
- EFT/Burst testing per IEC 61000-4-4
- Verify continued operation after transients

### 3.4 Output Safety

#### SR-030: Output Short Circuit Duration
**Priority:** Critical  
**Requirement:** Outputs shall withstand indefinite short circuit.

**Implementation:**
- Continuous short circuit protection
- Current fold-back to safe level
- No thermal damage during short circuit
- Automatic or manual recovery options

**Verification:**
- 24-hour short circuit test
- Monitor junction temperature during test
- Verify functionality after test

#### SR-031: Output Discharge
**Priority:** High  
**Requirement:** Outputs shall safely discharge when disabled.

**Implementation:**
- Active discharge on disable
- Discharge to <10% within 10ms
- Discharge circuit rated for full output voltage
- Automatic discharge on fault conditions

**Verification:**
- Measure discharge time with various load capacitances
- Verify discharge under fault conditions
- Test with maximum rated output capacitance

#### SR-032: Output Voltage Limiting
**Priority:** Critical  
**Requirement:** Output voltage shall not exceed safe limits.

**Implementation:**
- Hardware overvoltage clamp at 5.5V
- Overvoltage protection triggers at 110% of set voltage
- Fail-safe design prevents output >6V under any fault
- Redundant voltage sensing

**Verification:**
- Fault injection testing
- Open-loop testing with disabled feedback
- Verify maximum possible output voltage

### 3.5 Isolation and Creepage

#### SR-040: Electrical Isolation
**Priority:** High  
**Requirement:** Appropriate isolation between functional circuits.

**Implementation:**
- Digital interface isolated from power outputs (optional)
- Isolation rating: 1kV (if isolated version)
- Withstand voltage: 1.5kV for 1 minute
- Meets IEC 61010-1 functional isolation

**Verification:**
- Hi-pot testing at 1.5kV
- Insulation resistance measurement
- Partial discharge testing

#### SR-041: Creepage and Clearance
**Priority:** High  
**Requirement:** Package design shall meet safety spacing requirements.

**Implementation:**
- Minimum creepage: 2mm (pollution degree 2)
- Minimum clearance: 1.5mm
- Per IEC 61010-1 requirements
- Package design verification

**Verification:**
- Package dimensional inspection
- Review mechanical drawings
- Production sample verification

### 3.6 Fault Handling

#### SR-050: Fault Detection and Reporting
**Priority:** High  
**Requirement:** All fault conditions shall be detected and reported.

**Implementation:**
- Overcurrent fault detection and reporting
- Overvoltage fault detection and reporting
- Thermal fault detection and reporting
- UVLO status reporting
- Fault status registers
- Interrupt output for fault conditions

**Verification:**
- Trigger each fault condition
- Verify fault detection and reporting
- Verify interrupt assertion
- Verify status register updates

#### SR-051: Safe State on Fault
**Priority:** Critical  
**Requirement:** Device shall enter safe state on any fault.

**Implementation:**
- All outputs disabled on critical fault
- Latched shutdown for critical faults
- Auto-restart for non-critical faults (configurable)
- Fail-safe default configuration

**Verification:**
- Trigger each fault condition
- Verify outputs disabled
- Verify safe state maintained
- Test fault recovery mechanisms

#### SR-052: Fault Recovery
**Priority:** Medium  
**Requirement:** Safe and predictable fault recovery behavior.

**Implementation:**
- Automatic recovery with configurable delay
- Manual recovery via register write
- Recovery attempt counter
- Persistent fault lockout after N attempts

**Verification:**
- Test automatic recovery
- Test manual recovery
- Verify attempt counting
- Test persistent fault lockout

## 4. Digital Interface Safety

#### SR-060: Interface Pin Protection
**Priority:** High  
**Requirement:** Digital interface pins shall be protected.

**Implementation:**
- ESD protection on all pins (HBM: ±2kV minimum)
- Input voltage limits: -0.3V to 5.5V
- Input current limiting
- Latchup immunity: ±100mA (JESD78)

**Verification:**
- ESD testing per JESD22-A114
- Latchup testing per JESD78
- Overvoltage stress testing

#### SR-061: I2C Bus Safety
**Priority:** Medium  
**Requirement:** I2C interface shall not create hazardous conditions.

**Implementation:**
- I2C pins tolerate master failure (hung bus)
- Watchdog timeout for I2C communication (optional)
- Safe default state if I2C fails
- I2C pins do not source/sink excessive current

**Verification:**
- Hung bus testing
- Communication timeout testing
- Current limit verification

## 5. EMC Safety Aspects

#### SR-070: ESD Immunity
**Priority:** High  
**Requirement:** Device shall withstand ESD per IEC 61000-4-2.

**Implementation:**
- Contact discharge: ±4kV (Level 3)
- Air discharge: ±8kV (Level 3)
- All pins protected
- No latchup or damage
- Functional during and after discharge

**Verification:**
- ESD testing per IEC 61000-4-2
- Test all exposed pins and surfaces
- Verify functionality during/after events

#### SR-071: EFT/Burst Immunity
**Priority:** High  
**Requirement:** Device shall withstand fast transients per IEC 61000-4-4.

**Implementation:**
- ±2kV on power lines (Level 3)
- ±1kV on signal lines (Level 3)
- No upset or damage
- Functional during and after bursts

**Verification:**
- EFT testing per IEC 61000-4-4
- Test power and signal lines
- Verify operation during/after test

## 6. Software Safety

#### SR-080: Watchdog Timer
**Priority:** Medium  
**Requirement:** Optional watchdog to detect communication failures.

**Implementation:**
- Configurable timeout (1s to 60s)
- Safe state on timeout
- Watchdog refresh via I2C
- Can be disabled for static configurations

**Verification:**
- Timeout testing
- Verify safe state entry
- Test refresh mechanism

#### SR-081: Register Write Protection
**Priority:** Medium  
**Requirement:** Critical registers protected from accidental writes.

**Implementation:**
- Write-protect enable for configuration registers
- Unlock sequence required for protected registers
- Read-only status and telemetry registers
- Power-on default to safe configuration

**Verification:**
- Attempt writes to protected registers
- Verify unlock sequence requirement
- Verify read-only register protection

## 7. Manufacturing and Quality

#### SR-090: Production Testing
**Priority:** Critical  
**Requirement:** 100% production testing for safety-critical functions.

**Implementation:**
- Overvoltage protection tested 100%
- Overcurrent protection tested 100%
- Thermal shutdown tested (sample basis)
- Short circuit protection tested 100%

**Verification:**
- Review production test plan
- Verify coverage of safety functions
- Audit production test execution

#### SR-091: Quality Management
**Priority:** High  
**Requirement:** Manufacturing under quality management system.

**Implementation:**
- ISO 9001 certified manufacturing
- Traceability for all safety-critical components
- Change control for safety-related changes
- Failure analysis for safety-related returns

**Verification:**
- Review quality certifications
- Audit manufacturing facility
- Review change control procedures

## 8. Documentation and Labeling

#### SR-100: Safety Documentation
**Priority:** High  
**Requirement:** Comprehensive safety information in datasheet.

**Implementation:**
- Maximum ratings clearly specified
- Safe operating area defined
- Fault protection mechanisms documented
- Application circuit examples
- PCB layout guidelines for safety

**Verification:**
- Review datasheet for completeness
- Verify all safety information included
- Check for clarity and accuracy

#### SR-101: Warning Labels
**Priority:** Medium  
**Requirement:** Appropriate warnings in documentation.

**Implementation:**
- Warnings for maximum ratings
- Cautions for ESD sensitivity
- Notes on thermal management
- Short circuit protection limitations

**Verification:**
- Review all product documentation
- Ensure appropriate warning levels
- Verify compliance with standards

## 9. Environmental Safety

#### SR-110: RoHS Compliance
**Priority:** High  
**Requirement:** Compliance with hazardous substance restrictions.

**Implementation:**
- RoHS 3 (EU 2015/863) compliant
- Lead-free package and solder
- Halogen-free (optional)
- Material composition documented

**Verification:**
- Material declaration from suppliers
- X-ray fluorescence testing
- Third-party compliance certification

#### SR-111: REACH Compliance
**Priority:** Medium  
**Requirement:** Compliance with REACH regulation.

**Implementation:**
- No SVHC above 0.1% by weight
- Material disclosure available
- Supply chain compliance

**Verification:**
- Review material declarations
- SVHC screening
- Supplier compliance verification

## 10. Maintenance and Support

#### SR-120: Field Failures
**Priority:** Medium  
**Requirement:** Safe behavior under field failure conditions.

**Implementation:**
- Graceful degradation where possible
- No hazardous failure modes
- Failure mode analysis completed
- FMEA documented

**Verification:**
- Review FMEA documentation
- Fault injection testing
- Verify no hazardous failures identified

## 11. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | Safety Engineering | Initial release |
