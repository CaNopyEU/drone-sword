# FÁZA 3: SOFTWARE & KALIBRÁCIA (1 týždeň)

**Späť na:** [Project Plan](../../project-plan.md) | **Context:** [context.md](../../context.md)

---

## 3.1 Flight Controller Konfigurácia
**Cieľ:** Stabilný let v manuálnom režime (bez gest)

### Betaflight setup (podrobne):

#### Krok 1: Ports tab
```
UART1: Serial RX (pre ESP32)
UART2: Disabled
UART3: Disabled
USB VCP: Enabled (pre tuning)
```

#### Krok 2: Configuration tab
```
ESC/Motor Protocol: DSHOT600
Motor Idle Throttle: 5%
Mixer: Custom motor mix pre Y-config

Custom Motor Mix (teoretické hodnoty):
Motor 1 (front):
  Throttle: 1.0
  Roll: 0.0
  Pitch: 1.0
  Yaw: 0.0

Motor 2 (rear-left):
  Throttle: 1.0
  Roll: -0.866
  Pitch: -0.5
  Yaw: -1.0

Motor 3 (rear-right):
  Throttle: 1.0
  Roll: 0.866
  Pitch: -0.5
  Yaw: 1.0

(Tieto hodnoty budú vyžadovať tuning!)
```

**Config file:** `code/betaflight-config/motor-mix.txt`

#### Krok 3: PID Tuning (začiatočné hodnoty)
```
ROLL:
  P: 40
  I: 80
  D: 30

PITCH:
  P: 45 (vyššie kvôli neštandardnému tvaru)
  I: 85
  D: 32

YAW:
  P: 60
  I: 90
  D: 0
```

**Config file:** `code/betaflight-config/pid-settings.txt`

#### Krok 4: Receiver tab
```
Serial-based receiver: SBUS/IBUS emulation
Channel Map: AETR1234
Mid-point: 1500us
Min: 1000us, Max: 2000us
```

### Kalibrácia sekvencie:
1. Accelerometer calibration (6-point)
2. Magnetometer calibration (ak použiješ kompas)
3. ESC calibration (min/max throttle)
4. Motor direction test (bez props!)
5. Failsafe nastavenie: Drop & disarm

---

## 3.2 ESP32 Gesture Recognition System
**Cieľ:** Real-time gesture → flight command preklad

### Architecture:
```
[Rukavica ESP32] ---(ESP-NOW 2.4GHz)---> [Dron ESP32] ---(UART)---> [Flight Controller]
     MPU9250                                Filter/Map                    Betaflight
```

### Kód pre rukavicu (rozšírený):

Pozri kompletný kód v `code/glove-controller/glove-controller.ino`

**Kľúčové funkcie:**
- `mapGesturesToRC()` - Mapovanie IMU na RC channels
- `applyExpo()` - Exponenciálna krivka pre smooth control
- `detectArmGesture()` - Arming gesture recognition

**Features:**
- Kalibrácia pri štarte (5s hold neutral)
- Deadband (3° pre pitch/roll, 5° pre yaw)
- Expo 60% pre jemnejšiu kontrolu
- 50Hz update rate

### Kód pre dron ESP32 (receiver):

Pozri kompletný kód v `code/drone-receiver/drone-receiver.ino`

**Kľúčové funkcie:**
- `onDataRecv()` - ESP-NOW callback
- `sendToFC()` - MSP_SET_RAW_RC message builder
- `checkFailsafe()` - 1s timeout protection

**Features:**
- MSP protocol communication s FC
- Failsafe: throttle → 1000 po 1s loss
- Checksum validation

---

## 3.3 Testovací protokol (dôležité!)
**Cieľ:** Bezpečné overenie každého subsystému

### Test 1: Bench test (bez props)
```
1. Pripoj batériu
2. Arm systém cez rukavicu
3. Pomaly zvyšuj throttle, over:
   - Všetky 3 motory štartujú súčasne
   - Otáčky sa zvyšujú plynule
   - Žiadne zvláštne vibrácie/noise
4. Test roll/pitch:
   - Nakloň ruku doľava → motor 3 zrýchli
   - Nakloň ruku doprava → motor 2 zrýchli
   - Nakloň ruku dopredu → motory 2+3 zrýchlia
5. Test yaw:
   - Otočiť zápästie → všetky motory zmena rpm
```

### Test 2: Tethered test (s props, na uzde)
```
⚠️ KRITICKÉ BEZPEČNOSTNÉ OPATRENIA:
- Props guards nainštalované
- Dron priviazaný silnou šnúrkou (max 1m)
- Ochranné okuliare
- Vonku, ďaleko od ľudí
- Asistent pre núdzové odpojenie batérie

1. Arm, pomaly throttle až sa dron odľahčí
2. Over stabilitu: pokus o hover
3. Malé roll/pitch inputy, sleduj response
4. Ak stabilný → zvýšiť throttle, krátky lift-off (20cm)
5. Okamžité disarm pri problémoch
```

### Test 3: Prvý voľný let
```
PODMIENKY:
- Bezvetrný deň
- Otvoreté pole, tráva (soft landing)
- Batéria fully charged
- Asistent s vysielačkou (backup control?)

FLIGHT PLAN:
1. Hover 1m nad zemou, 10 sekúnd
2. Pomaly dopredu 2m, stop, hover
3. Dozadu späť, hover
4. Doľava/doprava, každý 1m
5. Land

Pri AKÝCHKOĽVEK problémoch: DISARM okamžite
```

---

## Checklist pre túto fázu:

- [ ] Betaflight nakonfigurovaný
- [ ] Custom motor mix tested
- [ ] Gesture code uploadnutý
- [ ] Receiver code uploadnutý
- [ ] Bench test passed (no props)
- [ ] Tethered test passed
- [ ] Prvý voľný let úspešný

**Previous:** [Phase 2 - Mechanics](phase-2-mechanics.md) | **Next:** [Phase 4 - Tuning](phase-4-tuning.md)
