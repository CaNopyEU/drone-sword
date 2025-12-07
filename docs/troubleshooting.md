# Troubleshooting Guide

**Späť na:** [Context](../context.md) | [Project Plan](../project-plan.md)

---

## Časté problémy a riešenia

### ⚡ Elektrické problémy

#### Problém 1: Dron nechce vzlietnuť
**Symptómy:** Motory sa krútia, ale nedostatok thrust

**Diagnóza:**
- [ ] Check: Batéria fully charged? (16.8V pre 4S)
- [ ] Check: Props správne orientované?
- [ ] Check: Motor direction correct?
- [ ] Check: Throttle channel funguje? (Betaflight Receiver tab)

**Riešenie:**
1. Motor test v Betaflight (bez props)
2. Kalibrácia ESC (min/max throttle)
3. Zvýšiť `motor_idle_throttle` v Betaflight
4. Over prop installation (pushed down až na motor bell)

---

#### Problém 2: Krátke lety (battery depleted rýchlo)
**Symptómy:** <3 min hover time namiesto 8-10 min

**Diagnóza:**
- [ ] Ampérová spotreba cez OSD/telemetry
- [ ] Hover current > 20A? (normálne 10-15A)

**Riešenie:**
- Príliš ťažký → odstrániť zbytočnú hmotnosť
- Props príliš aggressive → skúsiť nižší pitch (napr. 5x4 namiesto 5x4.5)
- ESC timing nastavenia (znížiť PWM frequency)
- Check motor temps (prehrievanie = neefektívnosť)

---

#### Problém 3: Jeden motor sa netočí
**Symptómy:** Motor test v Betaflight nepohne motorom

**Diagnóza:**
- [ ] Multimeter test: ESC output voltage pri throttle up?
- [ ] Motor spinning manually (by hand) ľahko?
- [ ] ESC beep sequence normal? (2 tóny = ready)

**Riešenie:**
1. Swap motor s working one → izoluj problém (motor vs ESC)
2. Re-solder connections (cold joint?)
3. ESC calibration znova
4. Replace ESC/motor ak hardvérový fault

---

### 🎮 Ovládanie & Gesture problémy

#### Problém 4: Gesture lag/nereaguje
**Symptómy:** 500ms+ delay medzi gesture a drone response

**Diagnóza:**
- [ ] ESP-NOW packet loss? (Serial monitor check)
- [ ] MPU9250 DMP overflow?
- [ ] FC neprima data? (Betaflight CLI: `serialpassthrough` check)
- [ ] WiFi interference? (many 2.4GHz devices nearby)

**Riešenie:**
- Skrátiť vzdialenosť rukavica-dron (<10m pri testoch)
- Znížiť WiFi interference (vypnúť router nearby)
- Zvýšiť ESP-NOW transmit power:
  ```cpp
  esp_wifi_set_max_tx_power(84); // Max power (21dBm)
  ```
- Lower baudrate UART ak packet corruption (115200 → 57600)
- Add acknowledgement v ESP-NOW (reliability mode)

---

#### Problém 5: Nechtené commands (twitchy control)
**Symptómy:** Dron reaguje na malé pohyby ruky

**Diagnóza:**
- [ ] Deadzone príliš malý?
- [ ] IMU drift (calibration needed)?
- [ ] Jitter v sensor readings?

**Riešenie:**
1. Increase deadband (3° → 5°)
2. Re-calibrate IMU pri štarte
3. Implement moving average filter (window size 5-10)
4. Add "gesture confirmation" delay (200ms hold)

```cpp
// Example: Gesture confirmation
if (abs(currentPitch - targetPitch) < 2 && millis() - holdStart > 200) {
  // Apply gesture
}
```

---

### 🛠️ Mechanické problémy

#### Problém 6: Oscillácie/vibrácie vo flight
**Symptómy:** High-frequency shaking, instabilné video

**Diagnóza:**
- [ ] Soft mounting FC?
- [ ] Loose screws?
- [ ] PID gains príliš vysoké?
- [ ] Props balanced?

**Riešenie:**
1. Retighten všetky skrutky (Loctite 243)
2. Pridať vibration dampening pads pod FC
3. Znížiť P a D gains (5-10% decrements)
4. Replace props (damaged props → vibrácie)
5. Check motor bearings (worn → wobble)

---

#### Problém 7: Asymetrické lietanie
**Symptómy:** Dron sa nakláňa jedným smerom pri hover

**Diagnóza:**
- [ ] Nesymetrické center of gravity?
- [ ] Jeden motor slabší?
- [ ] Props damaged/different?
- [ ] Motor mount angle incorrect?

**Riešenie:**
1. **Re-balance test:**
   - Zavesiť dron na šnúrku cez CoG
   - Pridať/odobrať závaží až horizontal
2. **Motor swap test:**
   - Swap suspected weak motor s iným
   - Ak problém sa presunie → motor fault
3. **Props:**
   - Replace all props naraz (matched set)
   - Over correct rotation (CW vs CCW)
4. **Motor mix adjustment:**
   - Fine-tune motor mix values v Betaflight
   - Použiť Blackbox data pre diagnostiku

---

### 💻 Software problémy

#### Problém 8: Betaflight CLI nedostupný
**Symptómy:** Cannot connect cez USB

**Diagnóza:**
- [ ] Driver issues? (CP2102/STM32 VCP)
- [ ] FC v DFU mode stuck?
- [ ] USB cable data capable? (not just power)

**Riešenie:**
1. Install/reinstall drivers (CP210x/STM32 VCP)
2. Try different USB port
3. Boot into DFU mode → flash firmware znova
4. Check Device Manager (Windows) / lsusb (Linux) pre recognition

---

#### Problém 9: ESP32 upload failed
**Symptómy:** "Failed to connect to ESP32" pri upload

**Diagnóza:**
- [ ] Boot mode (GPIO0 pulled LOW pri upload)?
- [ ] Correct COM port selected?
- [ ] Baud rate too high?

**Riešenie:**
1. Hold BOOT button počas upload start
2. Lower upload speed (921600 → 115200)
3. Install CH340/CP2102 drivers
4. Try esptool.py manually:
   ```bash
   esptool.py --port /dev/ttyUSB0 erase_flash
   ```

---

## Emergency Procedures

### 🚨 In-flight emergency

**Ak dron:**
1. **Osciluje nekontolovateľne** → DISARM okamžite (gesture alebo kill switch)
2. **Letí preč** → DISARM, radšej crash než flyaway
3. **Smoke/zápach** → DISARM + disconnect battery ASAP
4. **Low battery alarm** → Land immediately (tráva preferovaná)

### 🔥 LiPo fire

**NIKDY nepoužívaj vodu!**

1. Evakuuj okolitých ľudí
2. Ak malý fire → sand/dirt na batériu
3. Ak veľký → call fire department, mention LiPo
4. Nechaj vyhorieť v bezpečnej vzdialenosti

**Prevention:**
- Balance charging vždy
- LiPo bag pri nabíjaní
- Never leave unattended
- Storage voltage: 3.8V/cell

---

## Diagnostic Tools

### Hardware:
- **Multimeter:** Voltage/continuity checks
- **Battery voltage checker:** Quick cell voltage check
- **Oscilloscope:** (advanced) PWM signal analysis

### Software:
- **Betaflight Blackbox:** Flight data logging
- **Serial Monitor:** ESP32 debug output
- **Logic analyzer:** (advanced) Protocol debugging (UART, I2C)

### Useful Betaflight CLI commands:
```
status          # Overall system status
dump            # Full configuration export
get motor       # Motor-related settings
tasks           # CPU usage per task
version         # Firmware version
```

---

## Ak nič nepomôže...

### Community pomoc:
1. **Betaflight Discord** - Real-time help
2. **r/Multicopter Reddit** - Post s Blackbox log
3. **RCGroups forum** - Experienced builders

### Čo pripraviť pre help request:
- [ ] Blackbox log (ak je k dispozícii)
- [ ] Betaflight `diff all` output
- [ ] Fotky wiring
- [ ] Video problému
- [ ] Detailed popis (čo sa stalo, kedy, za akých podmienok)

---

**Pre preventívne maintenance:** → `docs/safety-checklist.md`
