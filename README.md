# ⚔️ Drone Sword - Gesture-Controlled Flying Sword

> Funkčný dron v tvare meča s Y-konfiguráciou motorov, ovládaný gestami ruky cez IMU senzor

![Project Status](https://img.shields.io/badge/status-planning-yellow)
![Phase](https://img.shields.io/badge/phase-0%20preparation-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## 📖 O projekte

**Drone Sword** je custom-built multirotor v tvare meča, ktorý sa ovláda gestami ruky namiesto tradičného RC vysielača. Používa Y-konfiguráciu s 3 motormi, MPU9250 IMU senzor v rukavici a ESP-NOW wireless protokol pre real-time komunikáciu.

### ✨ Features
- 🎮 **Gesture control** - Ovládanie nakláňaním ruky (roll, pitch, yaw)
- ⚡ **Low latency** - <50ms response time (ESP-NOW)
- 🎯 **Stable flight** - Custom PID tuning pre asymetrický tvar
- 🔋 **8-10 min flight time** - 4S 1550mAh LiPo
- 🛠️ **Open-source** - Všetok kód, CAD, dokumentácia dostupná

---

## 🎥 Demo

> *Coming soon - projekt v plánovacej fáze*

---

## 🔧 Technické špecifikácie

### Hardware
| Komponent | Špecifikácia |
|-----------|--------------|
| **Flight Controller** | Matek F405-CTR |
| **Motors** | 3x T-Motor F40 Pro IV 2400KV |
| **ESC** | Tekko32 F4 (4-in-1 alebo 3x individuálne) |
| **Battery** | Tattu 4S 1550mAh 75C |
| **IMU Sensor** | MPU9250 (9-axis) |
| **Microcontrollers** | 2x ESP32 DevKit V1 |
| **Frame** | Custom karbon fiber + 3D printed PETG |
| **Rozmery** | 600mm dĺžka, 450-550g celková hmotnosť |

### Software
- **FC Firmware:** Betaflight (custom motor mix)
- **Glove Code:** Arduino C++ (ESP-NOW transmitter)
- **Drone Receiver:** Arduino C++ (MSP protocol bridge)
- **Communication:** ESP-NOW (2.4GHz, 50Hz update rate)

---

## 🚀 Quick Start

### 1. Prečítaj dokumentáciu
```bash
# Začni tu:
docs/
├── context.md              # High-level overview pre AI agentov
├── safety-checklist.md     # ⚠️ MANDATORY pred začatím!
├── troubleshooting.md      # Riešenia častých problémov
└── phases/                 # Detailné build guides
    ├── phase-0-preparation.md
    ├── phase-1-bench-test.md
    ├── phase-2-mechanics.md
    ├── phase-3-software.md
    ├── phase-4-tuning.md
    └── phase-5-documentation.md
```

### 2. Objednaj komponenty
Pozri kompletný Bill of Materials: [`hardware/bom.csv`](hardware/bom.csv)

**Budget:** ~480€ total

### 3. Build fázy
1. **Príprava** (2-3 týždne) - Učenie, objednávky
2. **Bench test** (1 týždeň) - Elektronika testing
3. **Mechanika** (2 týždne) - CAD, výroba, montáž
4. **Software** (1 týždeň) - Betaflight config, ESP32 kód
5. **Tuning** (1-2 týždne) - PID optimization, gesture tuning

**Celkový čas:** 2-3 mesiace (10h/týždeň)

---

## 📂 Štruktúra projektu

```
drone-sword/
├── README.md                    # Tento súbor
├── context.md                   # AI agent context
├── project-plan.md              # Kompletný master plán (archív)
│
├── docs/                        # Dokumentácia
│   ├── phases/                  # Build guides po fázach
│   ├── troubleshooting.md
│   ├── safety-checklist.md
│   └── learning-resources.md
│
├── code/                        # Software
│   ├── glove-controller/        # ESP32 rukavica
│   ├── drone-receiver/          # ESP32 na drone
│   ├── betaflight-config/       # FC konfigurácia
│   └── testing/                 # Test scripts
│
├── cad/                         # CAD súbory
│   ├── frame/                   # Karbon frame
│   ├── handle/                  # 3D printed handle
│   └── assembly.step
│
├── hardware/                    # Hardvérové špecifikácie
│   ├── bom.csv                  # Bill of Materials
│   └── diagrams/                # Wiring diagrams
│
├── logs/                        # Build log & progress
│   ├── progress.md
│   ├── flight-logs/
│   └── notes/
│
└── media/                       # Fotky, videá
    ├── build-process/
    ├── test-flights/
    └── final-showcase/
```

---

## 🛠️ Použitie

### Gesture mapping (plánované):
| Gesto | Akcia |
|-------|-------|
| Nakloň ruku dopredu | Pitch forward |
| Nakloň ruku dozadu | Pitch backward |
| Nakloň ruku doľava | Roll left |
| Nakloň ruku doprava | Roll right |
| Otočiť zápästie | Yaw |
| Drž plochú 2s | ARM motors |
| Rýchle sklopenie | DISARM (emergency) |

**Throttle control:** TBD (thumb joystick / flex sensor / Z-axis accel)

---

## ⚠️ Safety

**KRITICKÉ:**
- ✅ Prečítaj [`docs/safety-checklist.md`](docs/safety-checklist.md) pred začatím
- ✅ LiPo batteries = požiarové riziko, nikdy nenechávaj bez dozoru
- ✅ Props = sharp, vždy disarm pred handling
- ✅ First flights: tethered test, ochranné okuliare, asistent prítomný
- ✅ Testuj iba vonku, open field, min 50m od ľudí

---

## 📊 Current Status

**Aktuálna fáza:** FÁZA 0 - PRÍPRAVA

**Dokončené:**
- [x] Komplexný plán vytvorený
- [x] Modulárna štruktúra projektu
- [x] Dokumentácia initialized

**Next steps:**
- [ ] Objednať komponenty
- [ ] Štúdium: PID tuning, ESP-NOW
- [ ] CAD design začať

**Progress tracker:** [`logs/progress.md`](logs/progress.md)

---

## 🤝 Prispievanie

Tento projekt je momentálne v early stage. Ak máš nápady, feedback alebo chceš prispieť:

1. Open issue s návrhom
2. Fork repo, urob zmeny
3. Submit PR s popisom

**Areas where help is welcome:**
- CAD design review
- PID tuning algoritmy
- Gesture recognition optimization
- Testing & feedback

---

## 📚 Resources

### Learning materials:
- **Betaflight Wiki:** https://github.com/betaflight/betaflight/wiki
- **ESP-NOW Docs:** https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_now.html
- **Joshua Bardwell (YouTube):** PID tuning tutorials
- **RCGroups:** Multicopter section

### Inspiration:
- Y-configuration drones
- Gesture-controlled projects (Myo armband era)
- FPV freestyle drones

---

## 📄 License

- **Code:** MIT License
- **Documentation & CAD:** CC BY-SA 4.0

Pozri [LICENSE](LICENSE) pre detaily.

---

## 👨‍💻 Author

**CaNopy**

- GitHub: [@CaNopy](https://github.com/CaNopyEU)

---

## 🙏 Acknowledgments

- **Betaflight Team** - Awesome FC firmware
- **Espressif** - ESP32 platform & ESP-NOW
- **hideakitai** - MPU9250 Arduino library
- **r/Multicopter community** - Inspiration & support

---

## 📞 Contact

**Questions?** Open an issue alebo kontaktuj cez GitHub.

**Safety concerns?** Pozri [`docs/safety-checklist.md`](docs/safety-checklist.md)

---

**⚔️ Fly safe, fly smart! ⚔️**
