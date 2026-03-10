---
layout: page
title: Garage door remote
---

## Part numbers

Code hopping encoder:
- Microchip HCS200/SN

SPDT (2:1) switch:
- Texas Instruments TMUX6219RQXR (slightly lower leakage current)
- Texas Instruments TMUX7219RQXR
Both devices are identical except for the input leakage current.

Linear regulator:
- Texas Instruments TPS7A25 (0.3 A)
- Texas Instruments TPS7A26 (0.5 A)
Both devices are identical except for the maximum output current.

Schmitt-trigger inverter:
- SN74LVC1G14DPW (5-pin X2SON)
- SN74LVC1G14DSF (6-pin SON)

Resistors:
- CRCW0402120KFKED (0402,  120 kOhm, 1%, +-100ppm/degC)
- CRCW0402150KFKED (0402,  150 kOhm, 1%, +-100ppm/degC)
- CRCW0402360KFKED (0402,  360 kOhm, 1%, +-100ppm/degC)
- CRCW04021M00FKED (0402, 1.00 MOhm, 1%, +-100ppm/degC)
- CRCW04021M30FKED (0402, 1.30 MOhm, 1%, +-100ppm/degC)

Capacitors:
- GRM155R61E105KE11 (0402, 1.00 uF, 10%, X5R, 25V)
- GRM155R61E225KE11 (0402, 2.20 uF, 10%, X5R, 25V)

## Calculations

### 12 V linear regulator

Sources:
- TPS7A25 datasheet, section 9.2

Requirements:
- U\_IN  = 11.80 - 14.80 V
- U\_OUT = 12.00 V

IC parameters:
- U\_FB  =  1.24 V

TI recommends to set the feedback divider current to 100 times the FB pin current:
    R\_T + R\_B <= U\_OUT / (I\_FB x 100) =
               = 12.00 / (10n  x 100) =
               = 12.00 MOhm

Using the formula for the feedback divider ratio:
    R\_T / R\_B = (U\_OUT - U\_FB) / U\_FB =
              = (12.00 - 1.24) / 1.24 =
              = 8.68 / 1

So the resistances should be:
    R\_T <= 10.76 MOhm
    R\_B <=  1.24 MOhm

Aiming for the feedback divider current 1000 times the FB pin current (resistor values 10 times
smaller than the calculated limits), and using E24 (5%) series values:
    R\_T = 1.30 MOhm
    R\_B =  150 kOhm

TI recommends using X7R ceramic capacitors with the following capacitances:
    C\_IN  = 1.00 uF
    C\_OUT = 2.20 uF
Package dimensions suggest 0201 (0603 metric) or 0402 (1005 metric) to be appropriate sizes.

### 5 V linear regulator

Sources:
- TPS7A25 datasheet, section 9.2

Requirements:
- U\_IN  = 11.80 - 14.80 V
- U\_OUT =  5.00 V

IC parameters:
- U\_FB  =  1.24 V

TI recommends to set the feedback divider current to 100 times the FB pin current:
    R\_T + R\_B <= U\_OUT / (I\_FB x 100) =
               = 5.00  / (10n  x 100) =
               = 5.00 MOhm

Using the formula for the feedback divider ratio:
    R\_T / R\_B = (U\_OUT - U\_FB) / U\_FB =
              = (5.00  - 1.24) / 1.24 =
              = 3.03 / 1

So the resistances should be:
    R\_T <= 3.76 MOhm
    R\_B <= 1.24 MOhm

Aiming for the feedback divider current 1000 times the FB pin current (resistor values 10 times
smaller than the calculated limits), and using E24 (5%) series values:
    R\_T = 360 kOhm
    R\_B = 120 kOhm

TI recommends using X7R ceramic capacitors with the following capacitances:
    C\_IN  = 1.00 uF
    C\_OUT = 2.20 uF
Package dimensions suggest 0201 (0603 metric) or 0402 (1005 metric) to be appropriate sizes.

### Schmitt-trigger relaxation oscillator

Sources:
- [Exactly how Schmitt-trigger oscillators work](allaboutcircuits.com/technical-articles/exactly-how-schmitt-trigger-oscillators-work/)

Requirements:
- U\_DD = 5.00 V
- U\_SS = 0.00 V
- T    = 0.50 - 1.00 s

IC parameters:
- U\_T+ = 2.71 V
- U\_T- = 1.84 V
- U\_T- = 1.89 V (DPW package)

    t\_H = ln((U\_DD - U\_T-) / (U\_DD - U\_T+)) x R x C =
        = ln((5.00 - 1.84) / (5.00 - 2.71)) x R x C =
        = 0.322 x R x C

    t\_L = ln(U\_T+ / U\_T-) x R x C =
        = ln(2.71 / 1.84) x R x C =
        = 0.387 x R x C

    T = t\_H           + t\_L           =
      = 0.322 x R x C + 0.387 x R x C =
      = 0.709 x R x C

Using:
- R = 1.00 MOhm
- C = 1.00 uF
we get a period within the required range:
    T = 0.709 x R  x C  =
      = 0.709 x 1M x 1u =
      = 0.709 s
