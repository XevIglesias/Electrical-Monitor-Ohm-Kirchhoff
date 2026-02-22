# Electrical Monitor - Ohm & Kirchhoff Laws

## Description
Function Block for Siemens S7-1200 that calculates electrical 
parameters and monitors cable voltage drop in industrial 
installations. Based on Ohm's Law and Kirchhoff's Voltage Law.

## PLC Platform
- **Controller:** Siemens S7-1200 (CPU 1214C DC/DC/DC)
- **Software:** TIA Portal V18
- **Language:** SCL (Structured Control Language)
- **Simulation:** S7-PLCSIM

## Features
- Real-time power calculation (P = V x I)
- Resistance calculation (R = V / I) with division-by-zero protection
- Cable voltage drop calculation using copper resistivity
- Configurable alarms:
  - Over-power warning
  - Over-current warning
  - Voltage drop > 5% warning (industrial standard)

## Block Interface

### Inputs
| Name | Type | Description |
|------|------|-------------|
| i_xEnable | BOOL | Enable calculations |
| i_rVoltage_V | REAL | Measured voltage (V) |
| i_rCurrent_A | REAL | Measured current (A) |
| i_rMaxPower_W | REAL | Maximum allowed power (W) |
| i_rMaxCurrent_A | REAL | Maximum allowed current (A) |
| i_rCableLength_m | REAL | Cable length (m) |
| i_rCableSection_mm2 | REAL | Cable cross-section (mm2) |

### Outputs
| Name | Type | Description |
|------|------|-------------|
| o_rPower_W | REAL | Calculated power (W) |
| o_rResistance_Ohm | REAL | Calculated resistance (Ohm) |
| o_rVoltageDrop_V | REAL | Cable voltage drop (V) |
| o_rVoltageAtLoad_V | REAL | Voltage at load (V) |
| o_rVoltageDrop_Percent | REAL | Voltage drop (%) |
| o_xPowerWarning | BOOL | Power exceeded alarm |
| o_xCurrentWarning | BOOL | Current exceeded alarm |
| o_xVoltageDropWarning | BOOL | Voltage drop > 5% alarm |
| o_xError | BOOL | Calculation error |

## Test Results

### Scenario 1 - Normal Operation (50m, 1.5mm2, 1.5A)
| Parameter | Value | Status |
|-----------|-------|--------|
| Power | 36.0 W | OK |
| Resistance | 16.0 Ohm | OK |
| Voltage Drop | 1.75V (7.29%) | ALARM |
| Voltage at Load | 22.25V | Warning |

### Scenario 2 - Corrected Cable (50m, 2.5mm2, 1.5A)
| Parameter | Value | Status |
|-----------|-------|--------|
| Voltage Drop | 1.05V (4.37%) | OK |
| Voltage at Load | 22.95V | OK |

### Scenario 3 - Overcurrent (50m, 1.5mm2, 3.0A)
| Parameter | Value | Status |
|-----------|-------|--------|
| Power | 72.0 W | OK |
| Current Alarm | TRUE | ALARM (3.0A > 2.5A) |
| Voltage Drop | 3.5V (14.58%) | ALARM |

### Scenario 4 - Long Cable (200m, 1.5mm2, 1.5A)
| Parameter | Value | Status |
|-----------|-------|--------|
| Voltage Drop | 7.0V (29.17%) | CRITICAL |
| Voltage at Load | 17.0V | CRITICAL |

## Formulas Used
- **Ohm's Law:** V = I x R
- **Power:** P = V x I
- **Cable Voltage Drop:** Vdrop = I x (2 x rho x L / S)
  - rho (copper) = 0.0175 Ohm.mm2/m
  - Factor 2 = forward + return path
- **Kirchhoff's Voltage Law:** V_source = V_cable + V_load

## Industrial Application
- Verify cable sizing during engineering phase
- Monitor real-time electrical parameters via analog sensors
- Generate alarms for preventive maintenance
- Comply with voltage drop standards (max 5% for 24VDC)

## Author
Xavier Iglesias Borland - PLC Programming Portfolio

## License
MIT
