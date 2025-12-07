# Glove Controller - ESP32 Transmitter

**Späť na:** [Context](../../context.md) | [Phase 3](../../docs/phases/phase-3-software.md)

---

## 📖 Overview

ESP32-based gesture controller worn on glove. Reads MPU9250 IMU data, maps gestures to RC commands, transmits via ESP-NOW to drone receiver.

---

## 🔧 Hardware Requirements

- ESP32 DevKit V1
- MPU9250 (9-axis IMU)
- 18650 Li-ion battery + holder
- (Optional) Flex sensor for throttle
- (Optional) Thumb joystick

### Wiring

```
ESP32          MPU9250
------         -------
3.3V    ───►   VCC
GND     ───►   GND
GPIO21  ───►   SDA
GPIO22  ───►   SCL

ESP32          Power
------         -----
VIN     ◄───   18650 battery (+)
GND     ◄───   Battery (-)
```

---

## 💻 Software Setup

### Arduino IDE Configuration

1. **Install ESP32 Board:**
   - File → Preferences → Additional Boards Manager URLs:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Tools → Board → Boards Manager → Search "ESP32" → Install

2. **Install Libraries:**
   - Sketch → Include Library → Manage Libraries
   - Install:
     - `MPU9250` by hideakitai
     - (ESP-NOW is built-in)

3. **Board Settings:**
   - Board: "ESP32 Dev Module"
   - Upload Speed: 921600 (or 115200 if fails)
   - CPU Frequency: 240MHz
   - Flash Frequency: 80MHz
   - Port: Select your COM port

---

## 📂 Files

- `glove-controller.ino` - Main sketch (TBD)
- `config.h` - Configuration constants (TBD)
- `gesture-mapping.cpp/h` - Gesture → RC mapping logic (TBD)

---

## 🎮 Gesture Mapping

### Planned Gestures:

| Hand Position | Output | Range |
|---------------|--------|-------|
| Tilt forward | Pitch forward | 1000-2000us |
| Tilt backward | Pitch backward | 1000-2000us |
| Tilt left | Roll left | 1000-2000us |
| Tilt right | Roll right | 1000-2000us |
| Rotate wrist | Yaw | 1000-2000us |
| Hold flat 2s | ARM motors | Binary |
| Quick flip down | DISARM | Binary |

### Throttle Control (TBD):
- **Option A:** Thumb joystick (analog input GPIO34)
- **Option B:** Flex sensor on index finger
- **Option C:** Z-axis acceleration (lift hand up)

---

## ⚙️ Configuration Parameters

```cpp
// config.h (template)

// ESP-NOW
#define DRONE_MAC_ADDRESS {0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF} // Replace with actual

// IMU
#define IMU_ADDRESS 0x68
#define CALIBRATION_SAMPLES 100

// Gesture Mapping
#define DEADBAND_PITCH 3.0  // degrees
#define DEADBAND_ROLL 3.0
#define DEADBAND_YAW 5.0
#define EXPO_FACTOR 0.6     // 0-1, higher = more expo

// Update Rate
#define LOOP_FREQUENCY 50   // Hz (20ms period)

// Arming
#define ARM_HOLD_TIME 2000  // ms
#define ARM_TOLERANCE 5.0   // degrees (flat hand)
```

---

## 🔍 Debugging

### Serial Monitor Output:
```
Calibration complete!
Offsets: Pitch=2.3° Roll=-1.2° Yaw=0.5°
Ready to fly!

T:1500 R:1520 P:1480 Y:1500 ARM:0
T:1500 R:1525 P:1475 Y:1500 ARM:0
...
ARMED!
T:1600 R:1530 P:1470 Y:1505 ARM:1
```

### Common Issues:
- **MPU9250 not found:** Check I2C wiring, try address 0x69
- **Jittery readings:** Add moving average filter
- **Send errors:** Check drone MAC address, reduce distance

---

## 🚀 Upload Instructions

1. Connect ESP32 via USB
2. Hold BOOT button (if upload fails)
3. Click Upload in Arduino IDE
4. Wait for "Hard resetting via RTS pin..."
5. Open Serial Monitor (115200 baud)
6. Perform calibration (hold hand neutral 5s)

---

## 📊 Performance Targets

- **Update rate:** 50Hz (achieved: _TBD_)
- **Latency:** <20ms IMU → transmit (measured: _TBD_)
- **Battery life:** 4+ hours (tested: _TBD_)
- **Range:** 50m+ (tested: _TBD_)

---

## 🔄 Development Roadmap

- [ ] Phase 1: Basic IMU reading
- [ ] Phase 2: ESP-NOW transmission
- [ ] Phase 3: Gesture mapping
- [ ] Phase 4: Calibration routine
- [ ] Phase 5: Expo & deadband
- [ ] Phase 6: Arming logic
- [ ] Phase 7: Throttle integration
- [ ] Phase 8: Filtering (moving average)

---

## 📝 Version History

- **v0.1** (TBD): Initial version, basic IMU reading
- **v0.2** (TBD): ESP-NOW added
- **v1.0** (TBD): Full gesture control

---

**Next:** Upload to ESP32 and test with [Drone Receiver](../drone-receiver/)
