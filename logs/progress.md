# Build Progress Tracker

**Last Updated:** 2024-12-06
**Current Phase:** FÁZA 0 - PRÍPRAVA
**Overall Progress:** 5%

---

## 📊 Phase Overview

| Phase | Status | Progress | Start Date | End Date |
|-------|--------|----------|------------|----------|
| 0 - Preparation | 🟡 In Progress | 10% | 2024-12-06 | - |
| 1 - Bench Test | ⚪ Not Started | 0% | - | - |
| 2 - Mechanics | ⚪ Not Started | 0% | - | - |
| 3 - Software | ⚪ Not Started | 0% | - | - |
| 4 - Tuning | ⚪ Not Started | 0% | - | - |
| 5 - Documentation | 🟡 In Progress | 20% | 2024-12-06 | - |

---

## ✅ FÁZA 0: PRÍPRAVA (2-3 týždne)

### 0.1 Teoretické základy
- [ ] YouTube: "How Quadcopters Fly" - Physics Girl
- [ ] Dokumentácia Betaflight/INAV preštudovaná
- [ ] PID tuning základy (Joshua Bardwell kanál)
- [ ] ESC calibration tutoriály
- [ ] **Kľúčové koncepty pochopené:**
  - [ ] Thrust, Torque, Yaw, Pitch, Roll
  - [ ] PID regulátor princípy
  - [ ] PWM signály pre ESC
  - [ ] LiPo batérie safety
  - [ ] Moment zotrvačnosti a centrum hmotnosti

**Poznámky:** _TBD_

---

### 0.2 Obstaranie komponentov

#### Hlavné komponenty:
- [ ] Matek F405-CTR Flight Controller
- [ ] 3x T-Motor F40 Pro IV 2400KV
- [ ] Tekko32 F4 ESC (3x alebo 4in1)
- [ ] 4x sady HQProp 5.1x4.1x3
- [ ] Tattu 4S 1550mAh 75C batéria (kúp 2ks)
- [ ] ISDT Q6 Nano nabíjačka
- [ ] Matek PDB

#### Senzory & Elektronika:
- [ ] MPU9250 (2ks - jeden záložný)
- [ ] BMP280
- [ ] ESP32 DevKit V1 (2ks)
- [ ] 18650 batéria + holder
- [ ] Konektory: XT60, JST, servo lead wires

#### Mechanika:
- [ ] Karbon pláty: 2mm (200x300mm) a 3mm (150x150mm)
- [ ] Hliníkové rúrky 10mm priemer, 500mm dĺžka (optional)
- [ ] M3 skrutky a matice (sada)
- [ ] Nylonové standoffs
- [ ] Závaží (olovo/tungsten) ~100-150g
- [ ] 3D printing filament (PETG preferované)

#### Náradie:
- [ ] Píla na kov
- [ ] Vŕtačka + vrtáky pre karbon
- [ ] Spájkovačka + cín
- [ ] Multimeter
- [ ] Izolepa, lepidlo (CA glue)
- [ ] Ochranné okuliare
- [ ] Respirator mask (karbon dust!)

**Objednávky:**
- Banggood order #: _TBD_
- AliExpress order #: _TBD_
- Local hobby shop: _TBD_

**Dodacie termíny:** _TBD_

---

### 0.3 Príprava prostredia
- [x] CAD software nainštalovaný (Fusion 360 / OnShape)
- [x] Workspace pripravený
- [ ] Arduino IDE + libraries (ESP32, MPU9250)
- [ ] Betaflight Configurator installed
- [ ] Safety equipment ready (glasses, mask, LiPo bag)

---

## ⚪ FÁZA 1: TESTOVACÍ BENCH (1 týždeň)

### 1.1 Základný test elektroniky
- [ ] FC flashnutý Betaflight firmware
- [ ] Gyroskop & accelerometer readings OK
- [ ] Accelerometer calibrated
- [ ] Motor 1 test passed (bez props)
- [ ] Motor 2 test passed
- [ ] Motor 3 test passed
- [ ] ESC calibration completed
- [ ] Battery charge/discharge test OK

### 1.2 Rukavica - Prvá verzia
- [ ] MPU9250 pripojený k ESP32
- [ ] IMU readings visible v Serial Monitor
- [ ] Kalibrácia IMU completed
- [ ] ESP-NOW basic test (2x ESP32)
- [ ] Gesture data transmission working

**Notes:** _TBD_

---

## ⚪ FÁZA 2: MECHANICKÁ KONŠTRUKCIA (2 týždne)

### 2.1 CAD Návrh
- [ ] Main carbon plate designed (600mm sword shape)
- [ ] Motor mounts designed (3ks, Y-config)
- [ ] Handle design completed (PETG 3D print)
- [ ] FC cover designed
- [ ] Assembly drawing s rozmermi
- [ ] STL files exported
- [ ] DXF files pre carbon cutting

**CAD Files:** `cad/`

### 2.2 Výroba
- [ ] Carbon plates cut
- [ ] Holes drilled
- [ ] Edges sanded smooth
- [ ] Handle 3D printed (2 halves)
- [ ] Motor mounts 3D printed (3x)
- [ ] FC cover 3D printed
- [ ] All parts test fit

### 2.3 Montáž
- [ ] Motor mounts attached to carbon
- [ ] Motors installed (M3 screws + Loctite)
- [ ] Frame assembled
- [ ] Center of Gravity balanced (±5g)
- [ ] Ballast weight positioned

### 2.4 Elektrická inštalácia
- [ ] PDB soldered
- [ ] ESCs soldered to PDB
- [ ] FC installed with vibration dampening
- [ ] Motors connected to ESCs
- [ ] ESP32 receiver installed v handle
- [ ] BMP280 connected (I2C)
- [ ] All wiring secured (zip ties, hot glue)
- [ ] **Pre-flight checks:**
  - [ ] No shorts (VBAT ↔ GND)
  - [ ] 5V rail voltage OK
  - [ ] Motor spin test (bez props)
  - [ ] FC boot LED normal

**Photos:** `media/build-process/phase-2/`

---

## ⚪ FÁZA 3: SOFTWARE & KALIBRÁCIA (1 týždeň)

### 3.1 Flight Controller
- [ ] Betaflight Configurator connected
- [ ] Ports configured (UART1 for ESP32)
- [ ] ESC protocol: DSHOT600
- [ ] Custom motor mix entered
- [ ] PID values (initial)
- [ ] Receiver configured (Serial RX)
- [ ] Failsafe: Drop & disarm
- [ ] CLI dump saved

**Config backup:** `code/betaflight-config/cli-dump.txt`

### 3.2 ESP32 Gesture System
- [ ] Glove controller code uploaded
- [ ] Drone receiver code uploaded
- [ ] ESP-NOW pairing successful
- [ ] Gesture → RC mapping tested
- [ ] Deadband tuned (3° initial)
- [ ] Expo applied (60%)
- [ ] Arming gesture works

**Code:** `code/glove-controller/` + `code/drone-receiver/`

### 3.3 Testing
- [ ] **Bench test (bez props):**
  - [ ] All 3 motors respond
  - [ ] Roll/pitch/yaw mapping correct
  - [ ] Throttle control smooth
- [ ] **Tethered test (s props, 1m rope):**
  - [ ] Stable hover attempted
  - [ ] No oscillations
  - [ ] Response to gestures accurate
- [ ] **First free flight:**
  - [ ] Hover 1m, 10 seconds
  - [ ] Forward/backward 2m
  - [ ] Left/right 1m
  - [ ] Safe landing

**Flight logs:** `logs/flight-logs/`

---

## ⚪ FÁZA 4: LADENIE A OPTIMALIZÁCIA (1-2 týždne)

### 4.1 PID Tuning
- [ ] Blackbox logging enabled
- [ ] Test flights recorded
- [ ] Logs analyzed (oscillations, overshoots)
- [ ] **Iterative tuning:**
  - [ ] Iteration 1: PITCH axis
  - [ ] Iteration 2: ROLL axis
  - [ ] Iteration 3: YAW axis
  - [ ] Iteration 4: Fine-tuning
- [ ] TPA (Throttle PID Attenuation) configured
- [ ] Final PID values stable

**Final PIDs:** `code/betaflight-config/pid-settings-final.txt`

### 4.2 Gesture Optimization
- [ ] Jitter filtering implemented
- [ ] Gesture confirmation delay tuned
- [ ] Deadzone optimized
- [ ] Throttle control finalized (joystick/flex sensor)
- [ ] Emergency disarm gesture tested

### 4.3 Performance Validation
- [ ] Hover 60s+ stable
- [ ] Forward/backward flight smooth
- [ ] Response latency <50ms
- [ ] Battery life: 8-10 min achieved
- [ ] Emergency procedures practiced

**Notes:** `logs/notes/tuning-results.md`

---

## 🟡 FÁZA 5: DOKUMENTÁCIA (ongoing)

### 5.1 Build Log
- [x] Project structure created
- [x] Documentation templates
- [ ] Daily logs počas build
- [ ] Photos každého kroku
- [ ] Videos testov
- [ ] Problems & solutions documented

### 5.2 Showcase
- [ ] Build video script written
- [ ] Time-lapse footage compiled
- [ ] Flight demo recorded
- [ ] Video edited
- [ ] GitHub repo public
- [ ] README completed
- [ ] CAD files uploaded

### 5.3 Sharing
- [ ] Hackster.io project page
- [ ] Instructables tutorial
- [ ] Reddit posts (r/Multicopter, r/arduino)
- [ ] YouTube video published
- [ ] Social media updates

---

## 🎯 Milestones

- [ ] **Milestone 1:** All components delivered (Target: _TBD_)
- [ ] **Milestone 2:** Bench test successful (Target: _TBD_)
- [ ] **Milestone 3:** Frame assembled (Target: _TBD_)
- [ ] **Milestone 4:** First hover achieved (Target: _TBD_)
- [ ] **Milestone 5:** Stable 60s hover (Target: _TBD_)
- [ ] **Milestone 6:** Project completed & documented (Target: _TBD_)

---

## 📝 Build Log Entries

### 2024-12-06
**Phase:** 0 - Preparation
**Progress:** Project structure created, documentation initialized

**Completed:**
- Created comprehensive project structure
- Split project-plan.md into modular phase files
- Written context.md for AI agents
- Created BOM, wiring diagram, safety checklist
- README.md completed

**Next:**
- Order components
- Start learning (PID tuning, ESP-NOW)
- Begin CAD design

**Notes:**
- Using token-efficient structure for AI collaboration
- All critical safety info in dedicated file
- Ready to start actual build when components arrive

---

## 📊 Time Tracking

| Phase | Estimated | Actual | Variance |
|-------|-----------|--------|----------|
| Phase 0 | 2-3 weeks | - | - |
| Phase 1 | 1 week | - | - |
| Phase 2 | 2 weeks | - | - |
| Phase 3 | 1 week | - | - |
| Phase 4 | 1-2 weeks | - | - |
| Phase 5 | Ongoing | - | - |
| **Total** | **2-3 months** | - | - |

**Hours logged:** 2h (documentation setup)

---

## 🚧 Current Blockers

- ⏳ Waiting for component orders
- ⏳ CAD learning curve (if new to Fusion 360)

---

## 💡 Ideas & Improvements

- [ ] Add LED strip along blade (WS2812B)
- [ ] FPV camera integration (future)
- [ ] GPS position hold (advanced)
- [ ] Custom gesture library (open-source contribution)

---

**Update this file po každej work session!**
