# Wiring Diagram - Drone Sword

**Späť na:** [Context](../../context.md) | [Phase 2](../../docs/phases/phase-2-mechanics.md#23-elektrická-inštalácia)

---

## 🔌 Complete System Wiring

### Main Power Distribution

```
┌─────────────────┐
│  4S LiPo Battery│  16.8V (fully charged)
│  1550mAh 75C    │  14.8V (nominal)
└────────┬────────┘
         │ XT60 Connector
         ▼
┌─────────────────┐
│       PDB       │  Power Distribution Board
│  (or 4-in-1 ESC)│
└─┬─┬─┬─┬─┬─┬─┬──┘
  │ │ │ │ │ │ │
  │ │ │ │ │ │ └──► GND (common ground)
  │ │ │ │ │ └────► 5V BEC → Flight Controller 5V rail
  │ │ │ └────────► VBAT → ESC3 (Motor 3 - rear right)
  │ │ └──────────► VBAT → ESC2 (Motor 2 - rear left)
  │ └────────────► VBAT → ESC1 (Motor 1 - front)
  └──────────────► VBAT → Voltage monitor (optional)
```

---

## 🎮 Flight Controller Connections

### Matek F405-CTR Pinout

```
┌──────────────────────────────────────┐
│         Matek F405-CTR FC            │
│                                      │
│  [USB]  ← PC (config/tuning)         │
│                                      │
│  MOTOR OUTPUTS:                      │
│  ├─ M1 ──────► ESC1 signal (front)   │
│  ├─ M2 ──────► ESC2 signal (rear-L)  │
│  └─ M3 ──────► ESC3 signal (rear-R)  │
│                                      │
│  UART PORTS:                         │
│  ├─ UART1 TX ──────► ESP32 RX        │
│  ├─ UART1 RX ──────► ESP32 TX        │
│  └─ (UART2/3 unused)                 │
│                                      │
│  I2C:                                │
│  ├─ SCL ──────► BMP280 SCL           │
│  └─ SDA ──────► BMP280 SDA           │
│                                      │
│  POWER:                              │
│  ├─ 5V  ◄──── BEC from PDB           │
│  ├─ GND ◄──── Common ground          │
│  └─ VBAT ◄─── Battery voltage sense  │
└──────────────────────────────────────┘
```

---

## 📡 ESP32 Receiver (on drone)

### ESP32 #1 - Drone Receiver

```
┌────────────────────────────┐
│    ESP32 DevKit V1         │
│    (v rukoväti dronu)      │
│                            │
│  POWER:                    │
│  ├─ VIN ◄─── 5V (from BEC) │
│  └─ GND ◄─── Common GND    │
│                            │
│  UART (to FC):             │
│  ├─ TX2 (GPIO17) ──► FC RX │
│  ├─ RX2 (GPIO16) ──► FC TX │
│  └─ GND ──────────► FC GND │
│                            │
│  WIRELESS:                 │
│  └─ Built-in 2.4GHz antenna│
│     (receives from glove)  │
└────────────────────────────┘
```

**Placement:** Vnútri rukoväte, close to FC for short UART wires

---

## 🧤 ESP32 Glove Controller

### ESP32 #2 - Glove Transmitter

```
┌─────────────────────────────────┐
│    ESP32 DevKit V1              │
│    (na rukavici)                │
│                                 │
│  POWER:                         │
│  ├─ VIN ◄─── 18650 battery (~4V)│
│  └─ GND ◄─── Battery GND        │
│                                 │
│  I2C (to IMU):                  │
│  ├─ GPIO21 (SDA) ──► MPU9250 SDA│
│  ├─ GPIO22 (SCL) ──► MPU9250 SCL│
│  ├─ 3.3V ──────────► MPU9250 VCC│
│  └─ GND ───────────► MPU9250 GND│
│                                 │
│  OPTIONAL INPUTS:               │
│  ├─ GPIO34 (ADC) ──► Flex sensor│
│  │   (throttle control)         │
│  └─ GPIO35 (ADC) ──► Thumb joy  │
│                                 │
│  WIRELESS:                      │
│  └─ Built-in 2.4GHz antenna     │
│     (transmits to drone)        │
└─────────────────────────────────┘
```

**Placement:** Attached to glove back, IMU on hand dorsum

---

## ⚙️ Motor & ESC Wiring

### Y-Configuration Layout

```
              FRONT
                │
                │
            [Motor 1]
                │
                │
         ┌──────┴──────┐
         │             │
         │             │
    [Motor 2]     [Motor 3]
    (Rear-Left)   (Rear-Right)
         │             │
         │             │
         └──────┬──────┘
                │
              HANDLE
```

### ESC → Motor Connections

```
ESC1 (Front motor):
├─ Phase A ──► Motor 1 wire A
├─ Phase B ──► Motor 1 wire B
└─ Phase C ──► Motor 1 wire C

ESC2 (Rear-left motor):
├─ Phase A ──► Motor 2 wire A
├─ Phase B ──► Motor 2 wire B
└─ Phase C ──► Motor 2 wire C

ESC3 (Rear-right motor):
├─ Phase A ──► Motor 3 wire A
├─ Phase B ──► Motor 3 wire B
└─ Phase C ──► Motor 3 wire C
```

**Note:** Motor direction set via Betaflight (software reversing supported)

### ESC Signal Wiring

```
ESC1 Signal cable (3-pin):
├─ Signal (white/yellow) ──► FC Motor 1 pad
├─ +5V (red) ───────────────► (usually not connected)
└─ GND (black/brown) ───────► FC GND

ESC2, ESC3: Same pattern
```

**Protocol:** DSHOT600 (digital, no need for calibration after initial setup)

---

## 🔋 Battery & Power Flow

### Voltage Rails

```
VBAT (14.8V nominal):
├─► ESC1, ESC2, ESC3 (direct)
├─► PDB input
└─► FC VBAT sense pad

5V Rail (from BEC):
├─► FC 5V input
├─► ESP32 receiver VIN
└─► (Optional) LED strip, camera

3.3V Rail:
└─► MPU9250 (from ESP32 glove 3.3V pin)
```

### Current Flow (estimated)

```
Hover (~12A total):
├─ Motor 1: ~4A
├─ Motor 2: ~4A
├─ Motor 3: ~4A
├─ FC: ~0.2A
└─ ESP32 receiver: ~0.1A

Full throttle (~30A total):
├─ Motors: ~28A combined
├─ Electronics: ~0.3A
```

---

## 🛠️ Wire Gauge Recommendations

| Connection | Wire Gauge | Color Code |
|------------|------------|------------|
| Battery → PDB | 14 AWG (2.5mm²) | Red (+), Black (-) |
| PDB → ESC | 18 AWG (1.0mm²) | Red (+), Black (-) |
| ESC signal | 22-26 AWG | Per ESC spec |
| FC UART | 26-28 AWG | Any (twisted pair) |
| I2C (to BMP280) | 26-28 AWG | Color coded (SDA/SCL) |

---

## 📋 Wiring Checklist

### Pre-soldering:
- [ ] All components laid out, test fit
- [ ] Wire lengths measured (+20mm margin)
- [ ] Heat shrink pre-installed na wires
- [ ] Tinning: all pads + wire ends

### During soldering:
- [ ] Good ventilation (fumes)
- [ ] Stable "third hand" holder
- [ ] Each joint: shiny, no cold joints
- [ ] No solder bridges

### Post-soldering:
- [ ] Continuity test každého spoja (multimeter)
- [ ] Insulation: heat shrink applied
- [ ] Strain relief: hot glue 5mm from joint
- [ ] **CRITICAL:** Check for shorts:
  - [ ] VBAT ↔ GND: OPEN (∞ resistance)
  - [ ] 5V ↔ GND: OPEN when unpowered

### Power-on test (bez props!):
- [ ] Connect battery (via smoke stopper ak máš)
- [ ] FC LEDs: normal boot sequence
- [ ] Multimeter check: 5V rail = 4.9-5.1V
- [ ] No hot components (touch test after 30s)
- [ ] No magic smoke 🚭

---

## 🔍 Troubleshooting

**Symptom:** FC doesn't boot
- Check 5V rail voltage
- Verify GND connections
- Re-flash firmware via USB

**Symptom:** Motor doesn't spin
- Check ESC signal cable orientation
- Verify motor wire connections (3 phases)
- Test motor+ESC separately (bench power supply)

**Symptom:** ESP32 not powering on
- Check VIN voltage (should be 4-5V from BEC)
- Verify USB upload works (isolate power issue)

**More:** → [Troubleshooting Guide](../../docs/troubleshooting.md)

---

## 📸 Reference Images

> TODO: Add annotated photos po build

- [ ] PDB soldering
- [ ] FC installation
- [ ] ESP32 placement v rukoväti
- [ ] Complete wiring overview

---

**Next:** [Phase 3 - Software Configuration](../../docs/phases/phase-3-software.md)
