# Betaflight Configuration

**Späť na:** [Context](../../context.md) | [Phase 3](../../docs/phases/phase-3-software.md#31-flight-controller-konfigurácia)

---

## 📖 Overview

Custom Betaflight configuration pre Y-config tricopter (3 motors) v tvare meča. Obsahuje custom motor mix, PID settings, receiver config.

---

## 🔧 Hardware Setup

- **Flight Controller:** Matek F405-CTR
- **Firmware:** Betaflight 4.x (latest stable)
- **Motor config:** Y-configuration (tricopter)
- **Receiver:** ESP32 via UART (Serial RX)

---

## 📂 Files

- `motor-mix.txt` - Custom Y-config motor mix
- `pid-settings.txt` - Initial + tuned PID values
- `cli-dump.txt` - Full Betaflight configuration backup
- `ports-config.txt` - UART port assignments

---

## ⚙️ Configuration Steps

### 1. Firmware Flash

1. Download Betaflight Configurator
2. Connect FC via USB
3. Backup current config (CLI → `dump all`)
4. Flash latest Betaflight 4.x
5. **DO NOT** apply defaults (will do manually)

### 2. Ports Tab

```
UART1: Serial RX (for ESP32 receiver)
UART2: Disabled
UART3: Disabled
USB VCP: Enabled (for configuration/tuning)
```

**Save & Reboot**

---

### 3. Configuration Tab

#### ESC/Motor Features:
```
ESC/Motor Protocol: DSHOT600
Motor Poles: 14 (T-Motor F40 Pro IV)
Motor Idle Throttle: 5.0%
Digital Idle Offset: 4.5
```

#### System Configuration:
```
Gyro Update Frequency: 8kHz
PID Loop Frequency: 4kHz
```

#### Arming:
```
Maximum Arming Angle: 25°
```

**Save & Reboot**

---

### 4. Motor Mix (CRITICAL!)

**Go to:** CLI tab

**Paste:**
```
# Custom Y-config motor mix
mixer CUSTOM
mmix reset

# Motor 1 (Front)
mmix 0  1.000  0.000  1.000  0.000

# Motor 2 (Rear-Left, 240° from front)
mmix 1  1.000 -0.866 -0.500 -1.000

# Motor 3 (Rear-Right, 120° from front)
mmix 2  1.000  0.866 -0.500  1.000

save
```

**Explanation:**
- Column 1: Throttle (all 1.0 = equal thrust)
- Column 2: Roll (X-axis)
- Column 3: Pitch (Y-axis)
- Column 4: Yaw (rotation)

**⚠️ Important:** These are theoretical values. May need tuning based on actual CoG and motor placement!

---

### 5. PID Tuning Tab

#### Initial Values (Start here):
```
ROLL:
  P: 40
  I: 80
  D: 30
  F: 100

PITCH:
  P: 45  (higher due to asymmetric shape)
  I: 85
  D: 32
  F: 110

YAW:
  P: 60
  I: 90
  D: 0
  F: 100
```

#### Rate Profile:
```
RC Rate: 1.00
Super Rate: 0.70
RC Expo: 0.00 (expo handled in glove code)

Max Velocity:
  Roll: 300°/s
  Pitch: 300°/s
  Yaw: 200°/s
```

**Save**

---

### 6. Receiver Tab

```
Receiver Type: Serial-based receiver
Serial Receiver Provider: SBUS (or IBUS)

Channel Map: AETR1234
  - A = Aileron (Roll)
  - E = Elevator (Pitch)
  - T = Throttle
  - R = Rudder (Yaw)

RSSI Channel: Disabled (no RSSI from ESP-NOW yet)
```

#### Stick Endpoints:
```
Min: 1000us
Center: 1500us
Max: 2000us
```

**Test:** Move glove → channels should update in real-time

**Save**

---

### 7. Modes Tab (Optional)

**Arming:** Handled by glove gesture (sends AUX channel)

**Angle Mode** (recommended for initial flights):
```
AUX 1 (from glove)
Range: 1300-2100 (always active)
```

**Horizon Mode** (after stable):
```
AUX 2
Range: 1700-2100
```

---

### 8. Failsafe Tab

```
Stage 1: Drop
  - After 1.0s signal loss
  - Throttle → Minimum (1000us)
  - Other channels → Hold last value

Stage 2: Land (after 5s)
  - Auto-disarm

Procedure:
  - Drop stage
  - Auto-land (if supported)
```

**Save**

---

### 9. Motors Tab (Testing)

**⚠️ REMOVE PROPS FIRST! ⚠️**

1. Enable motor test (checkbox)
2. Slowly increase master slider to 5%
3. Verify:
   - All 3 motors spin
   - Direction correct (check with hand gently)
4. Individual motor test (10% each)

**Expected:**
- Motor 1: Front (toward blade tip)
- Motor 2: Rear-left
- Motor 3: Rear-right

**If wrong direction:**
```
CLI:
set motor_1_direction = reversed
(or swap 2 motor wires)
save
```

---

### 10. Calibration

#### Accelerometer:
1. Modes tab → Calibrate Accelerometer
2. Place drone on level surface
3. Click Calibrate
4. Wait for beep/LED confirmation

#### Magnetometer (optional):
- Not required for acro/angle mode
- Only if using GPS/compass features

---

## 🧪 Bench Testing

### Pre-flight Checks:
1. **Motors tab:** All spin correct direction (no props!)
2. **Receiver tab:** All channels 1000-2000 range
3. **Setup tab:** Accelerometer shows level when drone flat
4. **Stick test:**
   - Tilt glove left → roll channel decreases
   - Tilt right → roll increases
   - Forward → pitch increases
   - Etc.

### Motor Mix Validation:
1. Receiver tab, increase throttle
2. Switch to Motors tab
3. Tilt drone physically (props OFF!):
   - Tilt left → Motor 3 should increase (compensate)
   - Tilt right → Motor 2 should increase
   - Tilt forward → Motors 2+3 increase, Motor 1 decrease

**If incorrect:** Adjust motor mix values

---

## 📊 Advanced Tuning (Phase 4)

### Blackbox Logging:
```
Configuration tab:
- Enable Blackbox
- Log rate: 1kHz
- Device: Flash (onboard)

After flight:
- Download logs via Blackbox tab
- Analyze in Blackbox Explorer
```

### TPA (Throttle PID Attenuation):
```
PID Tuning → TPA
Breakpoint: 1500 (50% throttle)
TPA: 0.5 (reduce P/D by 50% at full throttle)
```

Reason: Asymmetric shape causes more turbulence at high throttle

### Filter Settings (if oscillations):
```
Filter Settings tab:
Gyro Lowpass 1: ON (dynamic)
Gyro Lowpass 2: OFF
D-term Lowpass 1: ON
```

---

## 🐛 Troubleshooting

**Problem:** Motors don't spin
- Check ESC calibration (Motors tab → calibrate)
- Verify DSHOT600 supported by ESC
- Try DSHOT300 or Multishot

**Problem:** Drone flips on takeoff
- Motor mix incorrect (swap values)
- Motor direction reversed
- Props wrong orientation

**Problem:** Oscillations
- Too high P gain → reduce by 10%
- Too high D gain → reduce
- Enable filters

---

## 💾 Backup & Restore

### Backup (IMPORTANT - do before every change):
```
CLI tab → Type:
dump all

Copy output → save to `cli-dump-YYYY-MM-DD.txt`
```

### Restore:
```
CLI tab → Paste dump
Type: save
```

---

## 📝 Version History

### v0.1 - Initial Config
- Motor mix entered (theoretical)
- PID initial values
- Receiver configured

### v0.2 - After First Flight (TBD)
- Motor mix adjusted
- PID first iteration

### v1.0 - Final Tuned (TBD)
- Optimized PID values
- TPA configured
- Blackbox validated

---

## 📚 Resources

- **Betaflight Wiki:** https://betaflight.com/docs/wiki
- **Joshua Bardwell PID Tuning:** YouTube
- **Motor Mix Calculator:** https://www.rcgroups.com/forums/showthread.php?2348574

---

**Next:** [Testing Protocol](../../docs/phases/phase-3-software.md#33-testovací-protokol-dôležité)
