# Drone Receiver - ESP32 to Flight Controller Bridge

**Späť na:** [Context](../../context.md) | [Phase 3](../../docs/phases/phase-3-software.md)

---

## 📖 Overview

ESP32 receiver mounted in drone handle. Receives gesture data via ESP-NOW from glove, converts to MSP protocol, sends to Betaflight FC via UART.

---

## 🔧 Hardware Requirements

- ESP32 DevKit V1
- Connection to Flight Controller UART
- 5V power from BEC

### Wiring

```
ESP32          Flight Controller
------         -----------------
TX2 (GPIO17) ───► UART1 RX
RX2 (GPIO16) ───► UART1 TX
GND          ───► GND

ESP32          Power (from PDB/BEC)
------         -------------------
VIN          ◄─── 5V
GND          ◄─── GND
```

---

## 💻 Software Setup

### Arduino IDE Configuration

Same as glove controller:
- Board: ESP32 Dev Module
- Upload Speed: 921600
- CPU: 240MHz

### Libraries:
- ESP-NOW (built-in)
- No additional libraries needed

---

## 📂 Files

- `drone-receiver.ino` - Main sketch (TBD)
- `msp-protocol.cpp/h` - MSP message builder (TBD)
- `failsafe.cpp/h` - Failsafe logic (TBD)

---

## 🔌 Communication Protocols

### ESP-NOW (Receive from Glove)

**Packet Structure:**
```cpp
struct ControlPacket {
  uint16_t throttle;  // 1000-2000us
  uint16_t roll;      // 1000-2000us
  uint16_t pitch;     // 1000-2000us
  uint16_t yaw;       // 1000-2000us
  uint8_t arm;        // 0=disarm, 1=arm
  uint32_t timestamp; // millis() from glove
};
```

### MSP Protocol (Send to FC)

**MSP_SET_RAW_RC (0xC8):**
```
Frame format:
$M< [size] [cmd] [data...] [checksum]

Example (4 channels):
$M<
0x08              // size (8 bytes = 4 channels × 2 bytes)
0xC8              // MSP_SET_RAW_RC
[roll LSB] [roll MSB]
[pitch LSB] [pitch MSB]
[throttle LSB] [throttle MSB]
[yaw LSB] [yaw MSB]
[checksum]
```

**Update rate:** 50Hz (20ms period)

---

## 🛡️ Failsafe Logic

### Timeout Detection:
```cpp
if (millis() - lastReceivedTime > 1000) {
  // No signal for 1 second
  FAILSAFE_ACTIVE = true;

  // Set safe values:
  throttle = 1000;  // Minimum (motors off if disarmed)
  roll = 1500;      // Center
  pitch = 1500;     // Center
  yaw = 1500;       // Center
}
```

### Recovery:
- When signal restored → resume normal operation
- Log failsafe events to Serial

---

## 🔍 Debugging

### Serial Monitor Output:
```
Drone receiver ready
MAC Address: AA:BB:CC:DD:EE:FF

Packet received from glove:
  T:1523 R:1505 P:1498 Y:1502 ARM:1
  Latency: 18ms
Sent to FC via MSP

FAILSAFE! No signal for 1000ms
Failsafe values sent to FC
...
Signal restored
```

### Common Issues:
- **No packets received:** Check glove MAC in drone code matches glove's actual MAC
- **FC not responding:** Verify UART port in Betaflight (Ports tab)
- **Checksum errors:** Debug MSP frame construction

---

## ⚙️ Configuration

```cpp
// config.h (template)

// Serial (to FC)
#define FC_SERIAL Serial2
#define FC_BAUD 115200
#define FC_TX_PIN 17  // GPIO17
#define FC_RX_PIN 16  // GPIO16

// Failsafe
#define FAILSAFE_TIMEOUT 1000  // ms
#define FAILSAFE_THROTTLE 1000 // us
#define FAILSAFE_CENTER 1500   // us

// MSP
#define MSP_SET_RAW_RC 200  // 0xC8
#define MSP_HEADER_SIZE 5   // $M< + size + cmd
```

---

## 🚀 Upload Instructions

1. Connect ESP32 via USB
2. Upload code via Arduino IDE
3. Open Serial Monitor to see MAC address
4. **Copy MAC address** → paste into glove controller code
5. Re-upload glove code with correct MAC
6. Test ESP-NOW pairing (Serial monitor na both ESP32s)

---

## 🧪 Testing

### Bench Test (bez props):
1. Power drone (battery connected)
2. ESP32 receiver boots
3. Turn on glove
4. Verify "Packet received" v Serial Monitor
5. Open Betaflight Configurator → Receiver tab
6. Move glove → channels update v Betaflight

### Expected Receiver Tab values:
- Neutral hand: all channels ~1500us
- Tilt left: Roll → 1000us
- Tilt right: Roll → 2000us
- Etc.

---

## 📊 Performance Metrics

| Metric | Target | Actual |
|--------|--------|--------|
| Receive latency | <20ms | _TBD_ |
| MSP send rate | 50Hz | _TBD_ |
| Packet loss | <1% | _TBD_ |
| Failsafe trigger | 1s | _TBD_ |

---

## 🔄 Development Roadmap

- [ ] Phase 1: ESP-NOW receive setup
- [ ] Phase 2: Basic Serial output test
- [ ] Phase 3: MSP protocol implementation
- [ ] Phase 4: Failsafe logic
- [ ] Phase 5: Integration test with FC
- [ ] Phase 6: Latency optimization

---

## 🐛 Troubleshooting

**Problem:** Betaflight shows "No RX signal"
- Check UART port configured (Ports tab → Serial RX)
- Verify TX/RX not swapped
- Try lower baud rate (57600)

**Problem:** Channels jittering
- Add smoothing filter
- Check ESP-NOW packet loss
- Verify checksum calculation

**Problem:** Failsafe not triggering
- Test by turning off glove
- Check timeout value (1000ms)
- Monitor Serial for "FAILSAFE!" message

---

## 📝 Version History

- **v0.1** (TBD): ESP-NOW receive only
- **v0.2** (TBD): MSP implementation
- **v1.0** (TBD): Failsafe + full integration

---

**Next:** Test with [Glove Controller](../glove-controller/) and configure [Betaflight](../betaflight-config/)
