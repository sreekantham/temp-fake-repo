# Safety Standards Compliance - PM-5000 PMIC

**Document Version:** 1.0  
**Date:** October 2025  
**Status:** Draft

## 1. Introduction

This document outlines the safety standards compliance for the PM-5000 PMIC designed for use in test and measurement equipment.

## 2. Applicable Safety Standards

### 2.1 Primary Standards

| Standard | Title | Region | Status |
|----------|-------|--------|--------|
| IEC 61010-1:2010 | Safety requirements for electrical equipment for measurement, control, and laboratory use - Part 1: General requirements | International | Compliant |
| EN 61010-1:2010 | European harmonized version of IEC 61010-1 | Europe | Compliant |
| UL 61010-1 | North American adoption of IEC 61010-1 | USA/Canada | Compliant |

### 2.2 Specific Standards

| Standard | Title | Applicability |
|----------|-------|---------------|
| IEC 61010-2-030 | Particular requirements for testing and measuring circuits | Applicable |

### 2.3 Related Standards

| Standard | Title | Purpose |
|----------|-------|---------|
| IEC 60950-1 | Information technology equipment safety | Reference |
| IEC 62368-1 | Audio/video equipment safety | Reference |
| IEC 60529 | IP ratings | Environmental protection |

## 3. Safety Classification

### 3.1 Equipment Class

**Classification:** Class I equipment (with protective earth connection)

**Alternative:** Class II equipment (double/reinforced insulation) when used with isolated input

### 3.2 Pollution Degree

**Rating:** Pollution Degree 2

**Definition:** Normally only non-conductive pollution occurs. Occasionally, however, temporary conductivity caused by condensation must be expected.

### 3.3 Overvoltage Category

**Rating:** Overvoltage Category II (Installation Category II)

**Definition:** Equipment on circuits directly connected to low voltage installations

### 3.4 Installation Category

**Category:** Stationary equipment / Benchtop equipment

## 4. Electrical Safety

### 4.1 Working Voltage

**Maximum Working Voltage:**
- Input (VIN): 12V DC
- Output (VOUT): 5V DC
- Signal interfaces: 5V DC

**Classification:** SELV (Safety Extra-Low Voltage)

### 4.2 Insulation Requirements

**Not applicable for SELV circuits <30V AC / 60V DC**

**However, design includes:**
- Functional isolation between digital and power domains
- Enhanced clearances for reliability

### 4.3 Clearances and Creepage

**Package Design:**

| Voltage | Min Clearance | Min Creepage | Pollution Degree |
|---------|---------------|--------------|------------------|
| 12V DC | 0.5mm | 1.0mm | 2 |
| 5V DC | 0.25mm | 0.5mm | 2 |

**Actual Package:**
- Pin-to-pin clearance: 0.5mm (exceeds requirement)
- Pin-to-pad clearance: 0.4mm
- Creepage distances: >1.0mm for all critical paths

### 4.4 Protection Against Electric Shock

**Design Provisions:**
1. SELV circuits (<60V DC)
2. Current limiting on all outputs
3. Automatic shutdown on fault
4. No user-accessible hazardous voltages

## 5. Hazard Protection

### 5.1 Overcurrent Protection

**Implementation:**
- Hardware current limiting per channel
- Programmable current limits (0.1A - 3.5A)
- Short-circuit protection (indefinite duration)
- Automatic or latched shutdown

**Compliance:** ✓ IEC 61010-1 Clause 9.4

### 5.2 Overvoltage Protection

**Implementation:**
- Output overvoltage detection and shutdown
- Response time: <1µs
- Maximum output voltage: <6V under any single fault

**Compliance:** ✓ IEC 61010-1 Clause 9.4

### 5.3 Thermal Protection

**Implementation:**
- Junction temperature monitoring
- Thermal warning at 100°C
- Thermal shutdown at 125°C
- Automatic restart when cooled

**Testing:**
- Thermal endurance test at maximum ambient
- Thermal shutdown verification
- No fire or smoke during test

**Compliance:** ✓ IEC 61010-1 Clause 10.1

### 5.4 Fire Hazard

**Assessment:**
- Maximum surface temperature under fault: <100°C
- No flammable materials in package
- Current limiting prevents wiring overload

**Compliance:** ✓ IEC 61010-1 Clause 10.2

## 6. Mechanical Safety

### 6.1 Package Integrity

**Package:** QFN-48, sealed epoxy encapsulation

**Characteristics:**
- No sharp edges or points
- Robust mechanical construction
- No user-serviceable parts

### 6.2 Mounting and Installation

**Requirements:**
- PCB-mounted component
- Soldered connections
- Protected by equipment enclosure
- Not directly user-accessible

## 7. Environmental Considerations

### 7.1 Operating Environment

| Parameter | Requirement | PM-5000 Rating |
|-----------|-------------|----------------|
| Ambient temperature (operating) | Per application | -40°C to +85°C |
| Storage temperature | - | -55°C to +150°C |
| Humidity | Non-condensing | 5% to 95% RH |
| Altitude | <2000m typical | Suitable |

### 7.2 Material Safety

**RoHS Compliance:**
- RoHS 3 (EU 2015/863) compliant
- Lead-free package and assembly
- Halogen-free option available

**REACH Compliance:**
- No SVHC (Substances of Very High Concern) above 0.1%
- Material declarations available

**Conflict Minerals:**
- Conflict-free certified
- Full supply chain traceability

## 8. Markings and Documentation

### 8.1 Product Markings

**Device Marking (on package):**
```
PM-5000
[Date Code]
[Lot Code]
```

**Safety Markings (on equipment):**
- When used in final equipment, the equipment manufacturer must apply appropriate safety markings per IEC 61010-1

### 8.2 Required Documentation

**For Compliance:**
1. ✓ Datasheet with safety information
2. ✓ Application notes with safety guidelines
3. ✓ Installation instructions
4. ✓ Maximum ratings clearly specified
5. ✓ Safety precautions documented

## 9. Testing and Verification

### 9.1 Type Testing

**Tests Performed:**

| Test | Standard Clause | Result |
|------|----------------|--------|
| Overvoltage test | 9.4 | ✓ Pass |
| Overcurrent test | 9.4 | ✓ Pass |
| Thermal test | 10.1 | ✓ Pass |
| Abnormal operation | 12 | ✓ Pass |
| Clearance verification | 6.7 | ✓ Pass |
| Insulation resistance | 6.5 | ✓ Pass |

### 9.2 Production Testing

**Safety-Related Tests (100% coverage):**
1. ✓ Overcurrent protection functional test
2. ✓ Overvoltage protection functional test
3. ✓ Output voltage verification
4. ✓ Continuity testing
5. ✓ Short circuit protection (sample basis)

### 9.3 Quality Assurance

**Manufacturing:**
- ISO 9001 certified facility
- IPC-A-610 Class 2 assembly
- Full traceability
- Change control for safety-related changes

## 10. Abnormal Operation Testing

### 10.1 Component Failure Analysis

**Single Fault Condition Testing:**

| Fault Condition | Effect | Safe State | Result |
|----------------|--------|------------|--------|
| Control IC failure | Loss of regulation | UVLO shutdown | ✓ Safe |
| Pass transistor short | Overcurrent | Current limit | ✓ Safe |
| Sense resistor open | No current sensing | Voltage regulation OK | ✓ Safe |
| Reference failure | Incorrect voltage | Within safe limits | ✓ Safe |
| I2C interface failure | No communication | Default safe state | ✓ Safe |

**Compliance:** ✓ IEC 61010-1 Clause 12

### 10.2 Overload Testing

**Tests Performed:**
1. Continuous short circuit (24 hours)
2. Overcurrent (150% of rating, 1 hour)
3. Overvoltage on input (16V, 100ms)
4. Maximum ambient temperature operation

**Results:** No fire, no smoke, no explosion, device protected

### 10.3 Temperature Testing

**Thermal Stress Testing:**
- Operate at maximum power dissipation
- Measure surface temperatures
- Verify thermal shutdown
- Verify no damage after shutdown

**Result:** Maximum surface temperature <100°C, thermal shutdown at 125°C

## 11. Installation and Use Requirements

### 11.1 Installation Guidelines

**For Equipment Manufacturers:**

1. **Ventilation:**
   - Ensure adequate airflow
   - Do not block thermal pad
   - Follow thermal guidelines in datasheet

2. **Mounting:**
   - Secure PCB mounting
   - Vibration considerations if applicable
   - Thermal management per application

3. **Wiring:**
   - Use appropriate wire gauge
   - Secure connections
   - Follow polarity markings

### 11.2 User Instructions

**To be included in end-equipment manual:**

1. **Warnings:**
   - Do not exceed maximum input voltage
   - Do not short outputs to other voltages
   - Ensure adequate cooling

2. **Cautions:**
   - ESD sensitive - handle with care
   - Observe polarity
   - Do not modify or repair

3. **Operation:**
   - Normal operating conditions
   - Maintenance (if any)
   - Troubleshooting

## 12. Failure Modes and Effects Analysis (FMEA)

### 12.1 Critical Failure Modes

| Component | Failure Mode | Effect | Severity | Mitigation | Risk |
|-----------|--------------|--------|----------|------------|------|
| Power switch | Short | Overcurrent | Medium | Current limit | Low |
| Power switch | Open | No output | Low | Detected by PG | Low |
| Voltage reference | Drift | Wrong voltage | Medium | Monitoring | Low |
| Current sensor | Failure | No OCP | High | Secondary limit | Medium |
| Thermal sensor | Failure | No shutdown | High | External monitoring | Medium |

### 12.2 Safety Integrity

**Redundancy:**
- Hardware current limit (backup for programmable limit)
- Hardware OVP (backup for software monitoring)
- Multiple thermal indicators

**Fail-Safe:**
- Outputs default to disabled state
- Fault conditions cause shutdown
- No single failure causes hazard

## 13. Compliance Statement

### 13.1 Declaration of Conformity

**Product:** PM-5000 Power Management IC

**Manufacturer:** [Company Name]

**Standards Compliance:**
- ✓ IEC 61010-1:2010 (3rd edition)
- ✓ EN 61010-1:2010
- ✓ UL 61010-1
- ✓ IEC 61010-2-030

**Assessment:**
- Design review completed
- Type testing completed
- Production testing defined
- Risk assessment completed

**Status:** Suitable for use in test and measurement equipment per IEC 61010-1

### 13.2 Certifications

**Available:**
- CE marking (when incorporated in final product)
- UL recognition (component recognition)
- CB scheme certificate (pending)

**Equipment Manufacturer Responsibility:**
- Final equipment certification
- End-user documentation
- Installation safety
- Compliance with local regulations

## 14. Regulatory Information

### 14.1 Regional Requirements

**European Union:**
- Low Voltage Directive (LVD) - via IEC 61010-1
- RoHS Directive - Compliant
- REACH Regulation - Compliant

**United States:**
- UL listing available
- FCC Part 15 (EMC) - Compliant

**International:**
- IEC standards followed
- CB scheme applicable

### 14.2 Export Control

**Classification:**
- Not subject to export restrictions
- Commercial grade component
- EAR99 classification (USA)

## 15. Contact Information

**Safety Compliance:**
Email: safety@company.com  
Phone: +1-xxx-xxx-xxxx

**Technical Support:**
Email: support@company.com  
Phone: +1-xxx-xxx-xxxx

## 16. Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Oct 2025 | Safety Engineering | Initial release |

---

**IMPORTANT NOTICE:**

This component is designed for incorporation into equipment that will be certified by the equipment manufacturer. The final equipment must be evaluated for compliance with applicable safety standards. This document provides information to assist in that evaluation but does not replace the equipment manufacturer's responsibility for safety certification.

Users must:
1. Follow all application guidelines
2. Perform appropriate safety analysis for their application
3. Obtain necessary certifications for final equipment
4. Provide appropriate user documentation
