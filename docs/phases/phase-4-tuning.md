# FÁZA 4: LADENIE A OPTIMALIZÁCIA (1-2 týždne)

**Späť na:** [Project Plan](../../project-plan.md) | **Context:** [context.md](../../context.md)

---

## 4.1 PID Tuning pre neštandardný tvar
**Problém:** Meč má asymetrický moment zotrvačnosti

### Metodika tuning:

#### 1. Blackbox logging (ak FC podporuje)
- Zapni logging v Betaflight
- Urob testovací let s rôznymi manévrami
- Analyzuj logs v Blackbox Explorer
- Hľadaj: oscillations, overshoots, sluggish response

#### 2. Iteratívny proces:
```
PITCH osa (najproblematickejšia):
- Ak oscilluje → znížiť P gain
- Ak pomalé → zvýšiť P gain
- Ak overshoots → zvýšiť D gain
- Ak drift → zvýšiť I gain

Zmeny po 5-10% increments
Test let po každej zmene
```

#### 3. Špecifické úpravy pre meč:
- **TPA (Throttle PID Attenuation):** Znížiť PID gains pri vysokom throttle (turbulencia od asymetrie)
- **Angle mode expo:** Vyššie pre smooth gesto kontrolu
- **Rate limits:** Nižšie než štandardné (pomalší roll/flip rate)

### Očakávané finálne hodnoty (orientačné):
```
ROLL: P=45, I=90, D=35
PITCH: P=55, I=95, D=40 (vyššie kvôli asymetrii)
YAW: P=70, I=100, D=5
```

**Logging výsledkov:** `logs/flight-logs/` + `code/betaflight-config/pid-settings.txt`

---

## 4.2 Gesture Command Optimization
**Cieľ:** Intuitívne, presné ovládanie

### Problémové oblasti:

#### 1. Jitter v gesture readings
**Riešenie:** Kalman filter alebo moving average

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

**Implementácia:** `code/glove-controller/gesture-mapping.cpp`

#### 2. Unintended commands
- Implementuj "gesture confirmation": hold polohu 200ms pred aplikovaním
- Deadzone tuning: experimentuj s 2-5° deadband

#### 3. Throttle control
**Možnosti:**
- **A:** Flex sensor na palci (ohnutie = throttle up)
- **B:** Thumb joystick (malý analog stick)
- **C:** Akcelerometer Z-axis (zdvihnutie ruky = viac throttle)

**Odporúčanie:** Začni s fixným throttle, potom pridaj joystick (najspoľahlivejšie)

---

## 4.3 Pokročilé funkcie (voliteľné)
**Ak základný systém funguje dobre:**

### 1. Autonomous hover mode
- Použiť BMP280 (barometer) pre altitude hold
- GPS modul pre position hold
- One-handed "parking" gesto

### 2. Predprogramované triky
- Špecifické gestá → akrobatické manévere
- Napr: rýchly kruh rukou = 360° flip

### 3. FPV integrácia
- Kamera na špici meča
- VTX (video transmitter)
- FPV okuliare pre pilota

### 4. LED efekty
- WS2812B LED strip pozdĺž čepele
- Farby podľa flight mode, batérie, atď.
- Sync s gestami (napr. červená pri armed)

**Plány:** `logs/notes/advanced-features.md`

---

## Checklist pre túto fázu:

- [ ] PID tuning complete
- [ ] Gesture response optimalizovaná
- [ ] Stabilný hover 60s+
- [ ] Forward/backward flight smooth
- [ ] Emergency procedures tested
- [ ] (Voliteľné) Advanced features implementované

**Previous:** [Phase 3 - Software](phase-3-software.md) | **Next:** [Phase 5 - Documentation](phase-5-documentation.md)
