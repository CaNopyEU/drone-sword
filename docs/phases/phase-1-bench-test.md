# FÁZA 1: TESTOVACÍ BENCH (1 týždeň)

**Späť na:** [Project Plan](../../project-plan.md) | **Context:** [context.md](../../context.md)

---

## 1.1 Základný test elektroniky
**Cieľ:** Overiť funkčnosť všetkých elektronických komponentov pred montážou

### Kroky:

#### 1. Test Flight Controllera:
```
- Pripoj k PC cez USB
- Nainštaluj Betaflight Configurator
- Flashni najnovší firmware
- Over: gyroskop, akcelerometer readings
- Kalibrácia: akcelerometer calibration
```

#### 2. Test motorov a ESC:
```
- Pripoj 1 motor + ESC k FC (bez props!)
- Napájanie z bench power supply (15V, 2A limit)
- Motor test v Betaflight (5% throttle)
- Over: správny smer otáčania
- Calibrácia: ESC calibration (min/max throttle)
- Opakuj pre všetky 3 motory
```

#### 3. Test batérie:
```
- Skontroluj voltage každého cell (ideálne 3.7-4.2V)
- Nabíjanie test: 1C rate, balance charging
- Discharge test: 30s pod záťažou (motory 10%)
```

### Safety Check:
- [ ] LiPo bag pre nabíjanie
- [ ] Nikdy nenechávaj batériu bez dozoru pri nabíjaní
- [ ] Skontroluj PDB za shorts pred pripojením batérie

---

## 1.2 Rukavica - Prvá verzia
**Cieľ:** Funkčný prototyp rukavice s IMU

### Hardware setup:
```
ESP32 pinout:
- MPU9250: SDA → GPIO21, SCL → GPIO22
- VCC → 3.3V, GND → GND
- Batéria → VIN pin
```

### Software (Arduino IDE):

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

**Kompletný kód:** `code/glove-controller/glove-controller.ino`

### Test protokol:
1. Upload kódu do ESP32
2. Pripoj k Serial Monitoru
3. Over čítanie hodnôt z MPU9250
4. Pohybuj rukou, sleduj zmeny pitch/roll/yaw
5. Kalibrácia: držať ruku v "neutrálnej" polohe 5s

---

## Checklist pre túto fázu:

- [ ] FC flashnutý a funkčný
- [ ] Motory otáčajú správnym smerom
- [ ] ESC calibrated
- [ ] Rukavica ESP32 číta IMU data
- [ ] ESP-NOW komunikácia funguje (basic test medzi 2x ESP32)

**Previous:** [Phase 0 - Preparation](phase-0-preparation.md) | **Next:** [Phase 2 - Mechanics](phase-2-mechanics.md)
