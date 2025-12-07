# Projekt: Lietajúci Meč Ovládaný Gestami Ruky
## Komplexný Plán Budovania pre AI Orchestrátora

**Verzia:** 1.0  
**Dátum:** December 2024  
**Cieľ:** Postaviť funkčný dron v tvare meča s Y-konfiguráciou motorov, ovládaný IMU senzorom na rukavici

---

## 📋 FÁZA 0: PRÍPRAVA A UČENIE (2-3 týždne)

### 0.1 Teoretické základy
**Cieľ:** Pochopiť princípy letu a stabilizácie

**Učebné materiály:**
- [ ] YouTube: "How Quadcopters Fly" - Physics Girl
- [ ] Dokumentácia Betaflight/INAV
- [ ] PID tuning základy (Joshua Bardwell kanál)
- [ ] ESC calibration tutoriály

**Kľúčové koncepty na pochopenie:**
- Thrust, Torque, Yaw, Pitch, Roll
- PID regulátor (Proportional-Integral-Derivative)
- PWM signály pre ESC
- LiPo batérie (safety, C-rating, cell voltage)
- Moment zotrvačnosti a centrum hmotnosti

**Výstup:** Dokument s poznámkami a otázkami pre AI asistenta

---

### 0.2 Obstaranie komponentov
**Akcia:** Objednaj všetky komponenty naraz

**Zoznam objednávok:**

```
HLAVNÉ KOMPONENTY:
□ Matek F405-CTR Flight Controller
□ 3x T-Motor F40 Pro IV 2400KV
□ Tekko32 F4 ESC (3x alebo 4in1)
□ 4x sady HQProp 5.1x4.1x3
□ Tattu 4S 1550mAh 75C batéria (kúp 2ks)
□ ISDT Q6 Nano nabíjačka
□ Matek PDB

SENZORY & ELEKTRONIKA:
□ MPU9250 (2ks - jeden záložný)
□ BMP280
□ ESP32 DevKit V1 (2ks)
□ 18650 batéria + holder
□ Konektory: XT60, JST, servo lead wires

MECHANIKA:
□ Karbon pláty: 2mm (200x300mm) a 3mm (150x150mm)
□ Hliníkové rúrky 10mm priemer, 500mm dĺžka
□ M3 skrutky a matice (sada)
□ Nylonové standoffs
□ Závaží (olovo/tungsten) ~100-150g
□ 3D printing filament (PETG preferované)

NÁRADIE:
□ Píla na kov
□ Vŕtačka + vrtáky pre karbon
□ Spájkovačka + cín
□ Multimeter
□ Izolepa, lepidlo (CA glue)
□ Ochranné okuliare
```

**Dodávatelia:**
- Banggood, AliExpress: motory, ESC, props
- Lokálne hobby obchody: LiPo batérie (bezpečnejšia doprava)
- 3D tlač: lokálna služba alebo vlastná tlačiareň

---

## 🔧 FÁZA 1: TESTOVACÍ BENCH (1 týždeň)

### 1.1 Základný test elektroniky
**Cieľ:** Overiť funkčnosť všetkých elektronických komponentov pred montážou

**Kroky:**
1. **Test Flight Controllera:**
   ```
   - Pripoj k PC cez USB
   - Nainštaluj Betaflight Configurator
   - Flashni najnovší firmware
   - Over: gyroskop, akcelerometer readings
   - Kalibrácia: akcelerometer calibration
   ```

2. **Test motorov a ESC:**
   ```
   - Pripoj 1 motor + ESC k FC (bez props!)
   - Napájanie z bench power supply (15V, 2A limit)
   - Motor test v Betaflight (5% throttle)
   - Over: správny smer otáčania
   - Calibrácia: ESC calibration (min/max throttle)
   - Opakuj pre všetky 3 motory
   ```

3. **Test batérie:**
   ```
   - Skontroluj voltage každého cell (ideálne 3.7-4.2V)
   - Nabíjanie test: 1C rate, balance charging
   - Discharge test: 30s pod záťažou (motory 10%)
   ```

**Safety Check:**
- [ ] LiPo bag pre nabíjanie
- [ ] Nikdy nenechávaj batériu bez dozoru pri nabíjaní
- [ ] Skontroluj PDB za shorts pred pripojením batérie

---

### 1.2 Rukavica - Prvá verzia
**Cieľ:** Funkčný prototyp rukavice s IMU

**Hardware setup:**
```
ESP32 pinout:
- MPU9250: SDA → GPIO21, SCL → GPIO22
- VCC → 3.3V, GND → GND
- Batéria → VIN pin
```

**Software (Arduino IDE):**

```cpp
// PSEUDOKÓD - Základná štruktúra

#include <Wire.h>
#include <MPU9250.h>
#include <esp_now.h>
#include <WiFi.h>

MPU9250 mpu;

struct GestureData {
  float pitch;
  float roll;
  float yaw;
  int throttle;
};

void setup() {
  Serial.begin(115200);
  Wire.begin(21, 22);
  
  // Inicializácia MPU9250
  mpu.setup(0x68);
  
  // ESP-NOW setup
  WiFi.mode(WIFI_STA);
  esp_now_init();
  // Registrácia peer (adresa dronu ESP32)
}

void loop() {
  mpu.update();
  
  GestureData data;
  data.pitch = mpu.getPitch();
  data.roll = mpu.getRoll();
  data.yaw = mpu.getYaw();
  data.throttle = mapThrottle(); // Funkcia na mapovanie gesta
  
  // Odoslať cez ESP-NOW
  esp_now_send(peerAddress, (uint8_t*)&data, sizeof(data));
  
  delay(20); // 50Hz update rate
}
```

**Test protokol:**
1. Upload kódu do ESP32
2. Pripoj k Serial Monitoru
3. Over čítanie hodnôt z MPU9250
4. Pohybuj rukou, sleduj zmeny pitch/roll/yaw
5. Kalibrácia: držať ruku v "neutrálnej" polohe 5s

---

## 🏗️ FÁZA 2: MECHANICKÁ KONŠTRUKCIA (2 týždne)

### 2.1 Návrh rámu v CAD
**Cieľ:** Presný 3D model pred rezaním materiálu

**Software:** Fusion 360 (free pre hobby) alebo OnShape

**Design špecifikácie:**
```
ROZMERY:
- Celková dĺžka: 600mm (čepeľ 450mm + rukoväť 150mm)
- Šírka čepele: 80mm v najširšom bode
- Motor spacing: Y-konfigurácia, 120° medzi motormi
- Vzdialenosť motorov od centra: 200mm

HMOTNOSTNÝ ROZPOČET:
- Cieľová celková hmotnosť: 450-550g
- Motory: 3x28g = 84g
- FC + ESC: 30g
- Batéria: 180g
- Rám: 100-120g
- Závaží (rukoväť): 80-100g

CENTRUM HMOTNOSTI:
- Musí byť v strede trojuholníka motorov
- Približne 250mm od konca rukoväte
```

**Návrh kroky:**
1. **Hlavná karbonová platňa (chrbtová kosť):**
    - Tvar: dlhý trojuholník/kopija
    - Hrúbka: 3mm karbon v strede, 2mm na koncoch
    - Výrezy pre motor mounts
    - Otvory pre M3 skrutky

2. **Motor mounting plates:**
    - 3ks, karbon 2mm
    - 16x16mm štandard pre motory
    - Uhol naklonenia: 5° von od centra (lepšia stabilita)

3. **Rukoväť:**
    - 3D tlačená PETG
    - Dutá pre batériu a ESP32 receiver
    - Závaží na konci (balancovanie)
    - Magnetické krytie pre prístup k elektronike

4. **Ochranný kryt elektroniky:**
    - 3D tlačený, transparentný PETG
    - Prístup k FC cez clips

**Výstup:**
- [ ] STL súbory pre 3D tlač
- [ ] DXF súbory pre CNC cutting karbonu (alebo ručné)
- [ ] Assembly drawing s rozmermi

---

### 2.2 Výroba a montáž
**Cieľ:** Zostavený rám pripravený na elektroniku

**Pracovný postup:**

**Deň 1-2: Karbon práca**
```
1. Vytlač CAD drawings v mierke 1:1
2. Prilepenie printov na karbon (spray adhesive)
3. Rezanie:
   - Hrubé rezy: píla na kov
   - Presné obrysy: Dremel s rezným kotúčom
   - Otvory: vŕtačka s karbónovými vrtákmi
4. Opracovanie hrán: brúsny papier 400-grit
5. Čistenie: IPA (isopropyl alcohol)

⚠️ Safety: maska proti prachu, okuliare, venkovanie
```

**Deň 3-4: 3D tlač**
```
Nastavenia pre PETG:
- Nozzle: 235°C
- Bed: 80°C
- Speed: 40mm/s
- Infill: 30% gyroid
- Walls: 3 perimeters

Súčasti na tlač:
1. Rukoväť (2 polovice, lepené)
2. Motor mounts (3ks)
3. FC kryt
4. Rukavica holder pre ESP32
5. Prop guard (voliteľné)
```

**Deň 5: Predmontáž**
```
1. Upevnenie motor mounts na karbon:
   - M3x10 skrutky + nylon nuts
   - Loctite 243 (removable threadlocker)
   
2. Test fit všetkých súčastí
3. Označenie pozícií pre káble

4. Balancovanie:
   - Zavesenie dronu za stred pomocou šnúrky
   - Pridanie/odobratie závaží v rukoväti
   - Cieľ: horizontálna rovnováha
```

---

### 2.3 Elektrická inštalácia
**Cieľ:** Všetky komponenty zapojené a ready to flash

**Wiring diagram:**
```
BATÉRIA (4S) → XT60 → PDB
PDB → 
  ├─ ESC1 (motor front) → FC Motor1 pad
  ├─ ESC2 (motor rear-left) → FC Motor2 pad
  ├─ ESC3 (motor rear-right) → FC Motor3 pad
  ├─ 5V BEC → FC 5V rail
  └─ GND → FC GND

FC:
  ├─ UART1 → ESP32 receiver (RX/TX)
  ├─ I2C → BMP280 (barometer)
  └─ USB → PC (configuration)

ESP32 receiver (v rukoväti):
  ├─ Power: 5V z BEC
  ├─ UART → FC
  └─ Anténa: 2.4GHz (built-in)
```

**Spájkovanie best practices:**
1. Tin všetky pads a konce káblov najprv
2. Heat shrink PRED spájkovaním
3. Strain relief: hot glue na kábel 5mm od spoja
4. Izoluj všetky spoje (krycia bužírka)
5. Kontinuita test každého spoja multimetrom

**Pre-flight electrical checks:**
- [ ] Žiadne skraty medzi +BAT a GND (multimeter)
- [ ] 5V rail: 4.9-5.1V (bez záťaže)
- [ ] Motor spinning test (bez props): všetky 3 otáčajú správne
- [ ] FC power on: LED indikuje správnu inicializáciu

---

## 💻 FÁZA 3: SOFTWARE & KALIBRÁCIA (1 týždeň)

### 3.1 Flight Controller Konfigurácia
**Cieľ:** Stabilný let v manuálnom režime (bez gest)

**Betaflight setup (podrobne):**

**Krok 1: Ports tab**
```
UART1: Serial RX (pre ESP32)
UART2: Disabled
UART3: Disabled
USB VCP: Enabled (pre tuning)
```

**Krok 2: Configuration tab**
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

**Krok 3: PID Tuning (začiatočné hodnoty)**
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

**Krok 4: Receiver tab**
```
Serial-based receiver: SBUS/IBUS emulation
Channel Map: AETR1234
Mid-point: 1500us
Min: 1000us, Max: 2000us
```

**Kalibrácia sekvencie:**
1. Accelerometer calibration (6-point)
2. Magnetometer calibration (ak použiješ kompas)
3. ESC calibration (min/max throttle)
4. Motor direction test (bez props!)
5. Failsafe nastavenie: Drop & disarm

---

### 3.2 ESP32 Gesture Recognition System
**Cieľ:** Real-time gesture → flight command preklad

**Architecture:**
```
[Rukavica ESP32] ---(ESP-NOW 2.4GHz)---> [Dron ESP32] ---(UART)---> [Flight Controller]
     MPU9250                                Filter/Map                    Betaflight
```

**Kód pre rukavicu (rozšírený):**

```cpp
// gesture_controller.ino
#include <Wire.h>
#include <MPU9250.h>
#include <esp_now.h>
#include <WiFi.h>

MPU9250 mpu;
uint8_t droneAddress[] = {0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF}; // Nahraď skutočnou

// Gesture data packet
struct ControlPacket {
  uint16_t throttle;  // 1000-2000
  uint16_t roll;      // 1000-2000
  uint16_t pitch;     // 1000-2000
  uint16_t yaw;       // 1000-2000
  uint8_t arm;        // 0 = disarm, 1 = arm
  uint32_t timestamp;
};

ControlPacket packet;

// Kalibračné hodnoty (vyplň po kalibrácii)
float pitchOffset = 0;
float rollOffset = 0;
float yawOffset = 0;

// Exponenciálna krivka pre smooth control
float applyExpo(float input, float expo) {
  return (expo * input * input * input) + ((1 - expo) * input);
}

// Mapovanie IMU na RC channels
void mapGesturesToRC() {
  mpu.update();
  
  float pitch = mpu.getPitch() - pitchOffset;
  float roll = mpu.getRoll() - rollOffset;
  float yaw = mpu.getYaw() - yawOffset;
  
  // Limits a deadband
  pitch = constrain(pitch, -45, 45);
  roll = constrain(roll, -45, 45);
  yaw = constrain(yaw, -90, 90);
  
  if (abs(pitch) < 3) pitch = 0;
  if (abs(roll) < 3) roll = 0;
  if (abs(yaw) < 5) yaw = 0;
  
  // Mapovanie s expo (60%)
  packet.pitch = map(applyExpo(pitch / 45.0, 0.6) * 500, -500, 500, 1000, 2000);
  packet.roll = map(applyExpo(roll / 45.0, 0.6) * 500, -500, 500, 1000, 2000);
  packet.yaw = map(applyExpo(yaw / 90.0, 0.6) * 500, -500, 500, 1000, 2000);
  
  // Throttle: akcelerometer Z-axis alebo fix hodnota
  packet.throttle = 1500; // TODO: Pridať throttle control (napr. flex senzor)
  
  packet.timestamp = millis();
}

// Arming gesture: špecifická sekvencia
bool detectArmGesture() {
  // Príklad: držať ruku plochú (roll ~ 0) po dobu 2s
  static unsigned long armStart = 0;
  
  if (abs(mpu.getRoll()) < 5 && abs(mpu.getPitch()) < 5) {
    if (armStart == 0) armStart = millis();
    if (millis() - armStart > 2000) {
      armStart = 0;
      return true;
    }
  } else {
    armStart = 0;
  }
  return false;
}

void setup() {
  Serial.begin(115200);
  Wire.begin(21, 22);
  
  if (!mpu.setup(0x68)) {
    Serial.println("MPU9250 connection failed!");
    while(1);
  }
  
  // Kalibrácia pri štarte
  Serial.println("Hold hand neutral for 5 seconds...");
  delay(1000);
  
  float pitchSum = 0, rollSum = 0, yawSum = 0;
  for (int i = 0; i < 100; i++) {
    mpu.update();
    pitchSum += mpu.getPitch();
    rollSum += mpu.getRoll();
    yawSum += mpu.getYaw();
    delay(10);
  }
  
  pitchOffset = pitchSum / 100;
  rollOffset = rollSum / 100;
  yawOffset = yawSum / 100;
  
  Serial.println("Calibration complete!");
  
  // ESP-NOW setup
  WiFi.mode(WIFI_STA);
  if (esp_now_init() != ESP_OK) {
    Serial.println("ESP-NOW init failed");
    return;
  }
  
  esp_now_peer_info_t peerInfo;
  memcpy(peerInfo.peer_addr, droneAddress, 6);
  peerInfo.channel = 0;
  peerInfo.encrypt = false;
  
  if (esp_now_add_peer(&peerInfo) != ESP_OK) {
    Serial.println("Failed to add peer");
    return;
  }
  
  Serial.println("Ready to fly!");
}

void loop() {
  mapGesturesToRC();
  
  if (detectArmGesture()) {
    packet.arm = 1;
    Serial.println("ARMED!");
  }
  
  // Odoslať každých 20ms (50Hz)
  esp_err_t result = esp_now_send(droneAddress, (uint8_t*)&packet, sizeof(packet));
  
  if (result != ESP_OK) {
    Serial.println("Send error");
  }
  
  // Debug print
  Serial.printf("T:%d R:%d P:%d Y:%d\n", 
                packet.throttle, packet.roll, packet.pitch, packet.yaw);
  
  delay(20);
}
```

**Kód pre dron ESP32 (receiver):**

```cpp
// drone_receiver.ino
#include <esp_now.h>
#include <WiFi.h>

#define FC_SERIAL Serial2  // UART na FC (TX2=GPIO17, RX2=GPIO16)

struct ControlPacket {
  uint16_t throttle;
  uint16_t roll;
  uint16_t pitch;
  uint16_t yaw;
  uint8_t arm;
  uint32_t timestamp;
};

ControlPacket lastPacket;
unsigned long lastReceivedTime = 0;

// SBUS output (Betaflight kompatibilný)
void sendSBUS() {
  // SBUS frame: 25 bytes
  // Pre jednoduchosť použijeme MSP protokol
  
  // TODO: Implementovať MSP_SET_RAW_RC správu
  // Alebo konfigurovať FC pre Serial RX
}

// Callback pri prijatí dát
void onDataRecv(const uint8_t *mac, const uint8_t *incomingData, int len) {
  memcpy(&lastPacket, incomingData, sizeof(lastPacket));
  lastReceivedTime = millis();
  
  // Odoslať do FC
  sendToFC();
}

void sendToFC() {
  // Príklad: MSP_SET_RAW_RC
  // 4 kanály: Roll, Pitch, Throttle, Yaw
  
  FC_SERIAL.write(0x24); // $
  FC_SERIAL.write(0x4D); // M
  FC_SERIAL.write(0x3C); // <
  FC_SERIAL.write(0x08); // payload size (8 bytes = 4 channels * 2 bytes)
  FC_SERIAL.write(0xC8); // MSP_SET_RAW_RC
  
  // Roll
  FC_SERIAL.write(lastPacket.roll & 0xFF);
  FC_SERIAL.write((lastPacket.roll >> 8) & 0xFF);
  
  // Pitch
  FC_SERIAL.write(lastPacket.pitch & 0xFF);
  FC_SERIAL.write((lastPacket.pitch >> 8) & 0xFF);
  
  // Throttle
  FC_SERIAL.write(lastPacket.throttle & 0xFF);
  FC_SERIAL.write((lastPacket.throttle >> 8) & 0xFF);
  
  // Yaw
  FC_SERIAL.write(lastPacket.yaw & 0xFF);
  FC_SERIAL.write((lastPacket.yaw >> 8) & 0xFF);
  
  // Checksum (XOR všetkých bytov okrem $M<)
  uint8_t checksum = 0x08 ^ 0xC8;
  checksum ^= (lastPacket.roll & 0xFF);
  checksum ^= ((lastPacket.roll >> 8) & 0xFF);
  checksum ^= (lastPacket.pitch & 0xFF);
  checksum ^= ((lastPacket.pitch >> 8) & 0xFF);
  checksum ^= (lastPacket.throttle & 0xFF);
  checksum ^= ((lastPacket.throttle >> 8) & 0xFF);
  checksum ^= (lastPacket.yaw & 0xFF);
  checksum ^= ((lastPacket.yaw >> 8) & 0xFF);
  
  FC_SERIAL.write(checksum);
}

// Failsafe: žiadny signál > 1s
void checkFailsafe() {
  if (millis() - lastReceivedTime > 1000) {
    // Nastav throttle na 1000 (minimum)
    lastPacket.throttle = 1000;
    lastPacket.roll = 1500;
    lastPacket.pitch = 1500;
    lastPacket.yaw = 1500;
    sendToFC();
  }
}

void setup() {
  Serial.begin(115200);
  FC_SERIAL.begin(115200, SERIAL_8N1, 16, 17); // RX2, TX2
  
  WiFi.mode(WIFI_STA);
  
  if (esp_now_init() != ESP_OK) {
    Serial.println("ESP-NOW init failed");
    return;
  }
  
  esp_now_register_recv_cb(onDataRecv);
  
  Serial.println("Drone receiver ready");
}

void loop() {
  checkFailsafe();
  delay(100);
}
```

---

### 3.3 Testovací protokol (dôležité!)
**Cieľ:** Bezpečné overenie každého subsystému

**Test 1: Bench test (bez props)**
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

**Test 2: Tethered test (s props, na uzde)**
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

**Test 3: Prvý voľný let**
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

## 🎮 FÁZA 4: LADENIE A OPTIMALIZÁCIA (1-2 týždne)

### 4.1 PID Tuning pre neštandardný tvar
**Problém:** Meč má asymetrický moment zotrvačnosti

**Metodika tuning:**
1. **Blackbox logging** (ak FC podporuje)
    - Zapni logging v Betaflight
    - Urob testovací let s rôznymi manévrami
    - Analyzuj logs v Blackbox Explorer
    - Hľadaj: oscillations, overshoots, sluggish response

2. **Iteratívny proces:**
   ```
   PITCH osa (najproblematickejšia):
   - Ak oscilluje → znížiť P gain
   - Ak pomalé → zvýšiť P gain
   - Ak overshoots → zvýšiť D gain
   - Ak drift → zvýšiť I gain
   
   Zmeny po 5-10% increments
   Test let po každej zmene
   ```

3. **Špecifické úpravy pre meč:**
    - **TPA (Throttle PID Attenuation):** Znížiť PID gains pri vysokom throttle (turbulencia od asymetrie)
    - **Angle mode expo:** Vyššie pre smooth gesto kontrolu
    - **Rate limits:** Nižšie než štandardné (pomalší roll/flip rate)

**Očakávané finálne hodnoty (orientačné):**
```
ROLL: P=45, I=90, D=35
PITCH: P=55, I=95, D=40 (vyššie kvôli asymetrii)
YAW: P=70, I=100, D=5
```

---

### 4.2 Gesture Command Optimization
**Cieľ:** Intuitívne, presné ovládanie

**Problémové oblasti:**
1. **Jitter v gesture readings**
    - Riešenie: Kalman filter alebo moving average
   ```cpp
   // Jednoduchý moving average filter
   float filterValue(float newValue, float* history, int size) {
     for (int i = size-1; i > 0; i--) {
       history[i] = history[i-1];
     }
     history[0] = newValue;
     
     float sum = 0;
     for (int i = 0; i < size; i++) {
       sum += history[i];
     }
     return sum / size;
   }
   ```

2. **Unintended commands**
    - Implementuj "gesture confirmation": hold polohu 200ms pred aplikovaním
    - Deadzone tuning: experimentuj s 2-5° deadband

3. **Throttle control**
    - **Možnosť A:** Flex sensor na palci (ohnutie = throttle up)
    - **Možnosť B:** Thumb joystick (malý analog stick)
    - **Možnosť C:** Akcelerometer Z-axis (zdvihnutie ruky = viac throttle)

---

### 4.3 Pokročilé funkcie (voliteľné)
**Ak základný systém funguje dobre:**

1. **Autonomous hover mode**
    - Použiť BMP280 (barometer) pre altitude hold
    - GPS modul pre position hold
    - One-handed "parking" gesto

2. **Predprogramované triky**
    - Špecifické gestá → akrobatické manévere
    - Napr: rýchly kruh rukou = 360° flip

3. **FPV integrácia**
    - Kamera na špici meča
    - VTX (video transmitter)
    - FPV okuliare pre pilota

4. **LED efekty**
    - WS2812B LED strip pozdĺž čepele
    - Farby podľa flight mode, batérie, atď.
    - Sync s gestami (napr. červená pri armed)

---

## 📊 FÁZA 5: DOKUMENTÁCIA A ZDIEĽANIE (ongoing)

### 5.1 Build Log
**Udržiavaj podrobný denník:**
- Fotky každého kroku
- Videa testov
- Problémy a ich riešenia
- Zmeny v dizajne/kóde
- CAD verzie (verziovanie!)

**Nástroje:**
- GitHub repo pre kód
- Google Drive/OneDrive pre CAD a fotky
- Notion/Obsidian pre písomné záznamy

---

### 5.2 Final Showcase
**Vytvor:**
1. **Build video**
    - Time-lapse konštrukcie
    - Ukážka letu
    - Vysvetlenie technických riešení

2. **GitHub repository**
   ```
   /sword-drone-project
     /cad
       - frame.step
       - handle.stl
       - motor_mount.stl
     /code
       - gesture_controller.ino
       - drone_receiver.ino
       - pid_tuning_notes.txt
     /docs
       - bom.csv (Bill of Materials)
       - wiring_diagram.pdf
       - build_guide.md
     README.md
     LICENSE
   ```

3. **Online prezentácia**
    - Hackster.io project page
    - Instructables tutorial
    - Reddit r/Multicopter, r/arduino

---

## 🛠️ TROUBLESHOOTING GUIDE

### Časté problémy a riešenia:

**Problém 1: Dron nechce vzlietnuť**
```
Diagnóza:
- Check: Batéria fully charged? (16.8V pre 4S)
- Check: Props správne orientované?
- Check: Motor direction correct?
- Check: Throttle channel funguje? (Betaflight Receiver tab)

Riešenie:
- Motor test v Betaflight (bez props)
- Kalibrácia ESC
- Zvýšiť motor_idle_throttle
```

**Problém 2: Krátke lety (battery depleted rýchlo)**
```
Diagnóza:
- Ampérová spotreba cez OSD/telemetry
- Hover current > 20A? (normálne 10-15A)

Riešenie:
- Příliš ťažký → odstrániť zbytočnú hmotnosť
- Props príliš aggressive → skúsiť nižší pitch
- ESC timing nastavenia
```

**Problém 3: Oscillácie/vibrácie**
```
Diagnóza:
- Soft mounting FC?
- Loose screws?
- PID gains príliš vysoké?

Riešenie:
- Retighten všetky skrutky (Loctite)
- Pridať vibration dampening pads pod FC
- Znížiť P a D gains
```

**Problém 4: Gesture lag/nereaguje**
```
Diagnóza:
- ESP-NOW packet loss? (Serial monitor check)
- MPU9250 DMP overflow?
- FC neprima data? (Betaflight CLI: serialpassthrough check)

Riešenie:
- Skrátiť vzdialenosť rukavica-dron
- Znížiť WiFi interference (vypnúť router nearby)
- Zvýšiť ESP-NOW transmit power
- Lower baudrate UART ak packet corruption
```

**Problém 5: Asymetrické lietanie**
```
Diagnóza:
- Nesymetrické center of gravity
- Jeden motor slabší
- Props damaged

Riešenie:
- Re-balance (zavesiť na šnúrku test)
- Swap motor positions, test again
- Replace all props naraz (matched set)
```

---

## 📈 PROGRESS TRACKING

**Použij tento checklist:**

```
□ FÁZA 0: PRÍPRAVA
  □ Všetky komponenty objednané
  □ Teoretické základy preštudované
  □ CAD software nainštalovaný
  □ Workspace pripravený

□ FÁZA 1: TESTOVACÍ BENCH
  □ FC flashnutý a funkčný
  □ Motory otáčajú správnym smerom
  □ ESC calibrated
  □ Rukavica ESP32 číta IMU data
  □ ESP-NOW komunikácia funguje

□ FÁZA 2: MECHANIKA
  □ CAD model finalizovaný
  □ Karbon vyrezaný
  □ 3D diely vytlačené
  □ Rám zomontovaný
  □ Balancing complete (±5g)
  □ Elektronika nainštalovaná
  □ Wiring check passed

□ FÁZA 3: SOFTWARE
  □ Betaflight nakonfigurovaný
  □ Custom motor mix tested
  □ Gesture code uploadnutý
  □ Receiver code uploadnutý
  □ Bench test passed (no props)
  □ Tethered test passed
  □ První voľný let úspešný

□ FÁZA 4: TUNING
  □ PID tuning complete
  □ Gesture response optimalizovaná
  □ Stabilný hover 60s+
  □ Forward/backward flight smooth
  □ Emergency procedures tested

□ FÁZA 5: DOKUMENTÁCIA
  □ Build photos organizované
  □ Kód na GitHub
  □ CAD súbory zdieľané
  □ Video vytvorené
```

---

## 🎯 MILESTONES & TIMELINE

**Týždeň 1-3:** Príprava + Objednávky  
**Týždeň 4:** Testovací bench  
**Týždeň 5-6:** Mechanická konštrukcia  
**Týždeň 7:** Software setup  
**Týždeň 8-9:** Tuning a testy  
**Týždeň 10+:** Advanced features & showcase

**Celkový čas: 2-3 mesiace** (pri práci 5-10h/týždeň)

---

## 💰 FINAL BUDGET SUMMARY

| Kategória | Cena |
|-----------|------|
| Flight controller & sensors | 60€ |
| Motory (3x) | 105€ |
| ESC | 50€ |
| Props | 15€ |
| Batérie (2x) | 70€ |
| Nabíjačka | 45€ |
| Elektronika (ESP32, MPU, atď.) | 40€ |
| Mechanika (karbon, 3D print) | 50€ |
| Káble, konektory, náradie | 30€ |
| Backup parts (props, screws) | 15€ |
| **TOTAL** | **~480€** |

---

## 🚀 FINAL TIPS FOR SUCCESS

1. **Neboj sa zničiť props** - kúp extra, budú crashes
2. **Dokumentuj všetko** - budeš rád neskôr
3. **Komunita je kľúč** - Reddit, Discord, FB groups
4. **Safety first** - LiPo fires sú reálne, respect batérie
5. **Iteruj** - prvá verzia nikdy nebude perfektná
6. **Maj fun!** - toto je o učení sa a building cool stuff

---

## 📞 RESOURCES & CONTACTS

**Forums:**
- RCGroups.com - Multirotors section
- Reddit: r/Multicopter, r/fpv, r/arduino
- Betaflight Discord

**YouTube kanály:**
- Joshua Bardwell (tuning guru)
- Albert Kim (unique builds)
- Painless360 (INAV expert)
- Project Air

**Dokumentácie:**
- Betaflight Wiki: github.com/betaflight/betaflight/wiki
- ESP-NOW: docs.espressif.com
- MPU9250: invensense.com

---

## 🤖 NOTES FOR AI ORCHESTRATOR

**Tento dokument je živý plán.** Pre AI asistenta pracujúceho s užívateľom:

- **Fáza tracking:** Vždy spomeň aktuálnu fázu na začiatku session
- **Adaptive guidance:** Ak užívateľ застraaguje, vráť sa o krok späť
- **Safety reminders:** Pri KAŽDEJ work session s elektrikou/motormi
- **Detailed help:** Keď užívateľ hlási problém, začni troubleshooting guide
- **Celebrate milestones:** Povzbuď po dokončení každej fázy!

**Key decision points requiring AI consultation:**
1. PID tuning values (iteratívne, založené na feedback)
2. Motor mix adjustments (ak nesymetrický let)
3. Gesture mapping optimization (user preference dependent)
4. Center of gravity balancing (fyzické meranie needed)

**Version history:**
- v1.0 (Dec 2024) - Initial comprehensive plan
- [Future updates as project progresses]

---

**Veľa šťastia s projektom! 🚁⚔️**

*Pre otázky alebo pomoc s konkrétnym krokom, always refer back to this document and ask AI asistenta for clarification.*