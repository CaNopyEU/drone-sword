# Drone Sword - Context pre AI Agentov

## 🎯 Projekt Overview
**Cieľ:** Dron v tvare meča (Y-config, 3 motory), ovládaný gestami ruky cez IMU senzor.

**Aktuálna fáza:** FÁZA 0 - PRÍPRAVA
**Status:** Plánovanie, komponenty neobjednané
**Deadline:** ~2-3 mesiace (10h/týždeň)

---

## 📂 Rýchla navigácia

### Pre konkrétne úlohy:
- **Mechanika/CAD:** → `cad/` + `docs/phases/phase-2-mechanics.md`
- **Elektronika:** → `hardware/diagrams/wiring-diagram.md` + `docs/phases/phase-1-bench-test.md`
- **Programovanie:** → `code/` (2x ESP32 + Betaflight config)
- **Problémy:** → `docs/troubleshooting.md`
- **Bezpečnosť:** → `docs/safety-checklist.md` ⚠️ READ FIRST

### Kritické dokumenty:
1. `project-plan.md` - Kompletný master plán (archív, use fázy v docs/phases/)
2. `logs/progress.md` - Aktuálny stav checklistu
3. `hardware/bom.csv` - Shopping list

---

## 🔧 Technické špecifikácie

### Hardware Stack:
```
Flight Control: Matek F405-CTR
Motors:         3x T-Motor F40 Pro IV 2400KV (Y-config)
ESC:            Tekko32 F4
Battery:        Tattu 4S 1550mAh 75C
Sensors:        MPU9250 (IMU), BMP280 (barometer)
Comm:           ESP32 (glove) ←ESP-NOW→ ESP32 (drone) ←UART→ FC
```

### Software Stack:
```
FC Firmware:    Betaflight (custom motor mix)
Glove Code:     Arduino C++ (ESP-NOW transmitter)
Drone Code:     Arduino C++ (MSP protocol bridge)
Config Tools:   Betaflight Configurator, Arduino IDE
```

### Kritické parametre:
- **Hmotnosť:** 450-550g (max!)
- **CoG (centrum hmotnosti):** Musí byť v strede trojuholníka motorov (~250mm od konca rukoväte)
- **Update rate:** 50Hz (glove → drone)
- **Failsafe timeout:** 1s (auto throttle cut)

---

## 🚦 Aktuálny stav (Progress Tracker)

**Dokončené:**
- [x] Komplexný plán vytvorený
- [x] Modulárna štruktúra projektu

**V procese:**
- [ ] Žiadne (waiting for component orders)

**Next Steps:**
1. Objednať komponenty (zoznam: `hardware/bom.csv`)
2. Štúdium: PID tuning, ESP-NOW protokol
3. Setup testovacieho benchmarku

**Blocked by:**
- Čakáme na dodanie komponentov

---

## 🧩 Modulárne rozdelenie projektu

### Modul 1: Gesture Recognition System
**Owner:** Glove ESP32
**Dependencies:** MPU9250 library, ESP-NOW
**Outputs:** `ControlPacket` struct (throttle, roll, pitch, yaw, arm)
**Status:** ⏳ Design fáza
**Kód:** `code/glove-controller/`

### Modul 2: Wireless Communication
**Owner:** ESP-NOW bridge
**Dependencies:** WiFi 2.4GHz
**Latency target:** <20ms
**Status:** ⏳ Not started

### Modul 3: Flight Controller Integration
**Owner:** Drone ESP32 receiver
**Dependencies:** MSP protocol, Betaflight
**Outputs:** RC commands cez UART
**Status:** ⏳ Not started
**Kód:** `code/drone-receiver/`

### Modul 4: Mechanická konštrukcia
**Owner:** CAD design + výroba
**Dependencies:** Fusion 360, 3D printer, karbon
**Critical constraint:** CoG balance
**Status:** ⏳ CAD not started
**Files:** `cad/`

### Modul 5: PID Stabilizácia
**Owner:** Betaflight tuning
**Dependencies:** Functional hardware
**Method:** Iteratívne (Blackbox logs)
**Status:** ⏳ Čaká na build
**Config:** `code/betaflight-config/`

---

## ⚠️ Kritické riziká & mitigation

| Riziko | Impact | Pravdepodobnosť | Mitigation |
|--------|--------|-----------------|------------|
| Asymetrický tvar → nestabilný let | HIGH | HIGH | Extensive PID tuning, CoG balancing s závaží |
| Y-config motor mix nesprávne nastavený | HIGH | MEDIUM | Bench test bez props, postupné testovanie |
| ESP-NOW packet loss | MEDIUM | MEDIUM | Failsafe (1s timeout), distancia <10m pri testoch |
| LiPo fire pri nabíjaní | CRITICAL | LOW | LiPo bag, balance charging, never unattended |
| Gesture misinterpretation → crash | MEDIUM | HIGH | Deadzone tuning, gesture confirmation delay |

---

## 🤖 AI Agent Guidelines

### Keď pracuješ na tomto projekte:

1. **Vždy začni s:** "Aktuálna fáza: X" (check `logs/progress.md`)
2. **Pred úpravou kódu:** Over dependencies v `code/*/README.md`
3. **Pre nové nápady:** Validuj proti safety checklist
4. **Pri erroroch:** Cross-reference `docs/troubleshooting.md`
5. **Token management:** Prioritizuj aktuálnu fázu, avoid loading celého `project-plan.md`

### Typické dotazy & quick answers:

**Q:** "Ako mám zapojiť ESC?"
**A:** → `hardware/diagrams/wiring-diagram.md` + `docs/phases/phase-2-mechanics.md` sekcia 2.3

**Q:** "Prečo dron oscilluje?"
**A:** → `docs/troubleshooting.md` Problem 3 + znížiť P/D gains

**Q:** "Aké PID hodnoty použiť?"
**A:** → `code/betaflight-config/pid-settings.txt` (začiatočné), potom iteratívne tuning

**Q:** "Motor sa netočí správnym smerom"
**A:** → Betaflight CLI: `set motor_X_direction = reversed` alebo prehodiť 2 ESC wires

---

## 📊 Key Metrics & Targets

### Performance Targets:
- **Hover time:** 8-10 min (1550mAh batéria)
- **Max speed:** ~20 km/h (nie speed demon, stabilita > rýchlosť)
- **Response latency:** <50ms (gesture → motor change)
- **Gesture accuracy:** >95% (správna interpretácia)

### Build Metrics:
- **Budget:** ~480€ (detail: `hardware/bom.csv`)
- **Weight breakdown:**
  - Motors: 84g
  - FC+ESC: 30g
  - Battery: 180g
  - Frame: 100-120g
  - Ballast: 80-100g
  - **TOTAL:** 450-550g

---

## 🔗 External Resources

### Must-Read:
- Betaflight Wiki: https://github.com/betaflight/betaflight/wiki
- ESP-NOW Docs: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html
- Joshua Bardwell YT: PID tuning basics

### Recommended:
- RCGroups: Multirotors section
- Reddit: r/Multicopter, r/fpv
- Betaflight Discord community

---

## 📝 Version History
- **v1.0** (2024-12-06): Initial context document, project structure created
- [Future: Update po každej major milestone]

---

**Pre detailný popis akejkoľvek fázy:** → `docs/phases/phase-X-*.md`
**Pre kód štruktúru:** → `code/*/README.md`
**Pre progress tracking:** → `logs/progress.md`
