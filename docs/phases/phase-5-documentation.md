# FÁZA 5: DOKUMENTÁCIA A ZDIEĽANIE (ongoing)

**Späť na:** [Project Plan](../../project-plan.md) | **Context:** [context.md](../../context.md)

---

## 5.1 Build Log
**Udržiavaj podrobný denník:**

### Čo zaznamenávať:
- Fotky každého kroku → `media/build-process/`
- Videa testov → `media/test-flights/`
- Problémy a ich riešenia → `logs/notes/YYYY-MM-DD-*.md`
- Zmeny v dizajne/kóde → Git commits
- CAD verzie → Fusion 360 versioning alebo `cad/versions/`

### Nástroje:
- **GitHub repo** pre kód
- **Google Drive/OneDrive** pre CAD a fotky
- **Notion/Obsidian** pre písomné záznamy
- **Tento projekt** už má štruktúru v `logs/`

### Template pre denný log:
```markdown
# Build Log - YYYY-MM-DD

## Dnešné úlohy:
- [ ] Úloha 1
- [ ] Úloha 2

## Progress:
- [x] Dokončené
- [ ] In progress

## Problémy:
1. **Problém:** Popis
   **Riešenie:** Ako vyriešené

## Poznámky:
- Poznatky, nápady, next steps

## Fotky:
- ![](../media/build-process/YYYY-MM-DD-01.jpg)
```

---

## 5.2 Final Showcase
**Vytvor:**

### 1. Build video
- Time-lapse konštrukcie
- Ukážka letu
- Vysvetlenie technických riešení
- Uložiť v `media/final-showcase/`

**Script outline:**
```
0:00 - Intro (čo sa bude stavať)
0:30 - CAD design showcase
1:00 - Parts overview
2:00 - Build montage (time-lapse)
4:00 - Electronics installation
5:00 - First flight attempt
6:00 - Tuning & improvements
7:00 - Final flight demo
8:00 - Lessons learned & outro
```

### 2. GitHub repository
```
/sword-drone-project
  /cad
    - frame.step
    - handle.stl
    - motor_mount.stl
  /code
    - glove_controller.ino
    - drone_receiver.ino
    - pid_tuning_notes.txt
  /docs
    - bom.csv (Bill of Materials)
    - wiring_diagram.pdf
    - build_guide.md
  README.md
  LICENSE
```

**README.md by mal obsahovať:**
- Project overview + demo GIF/video
- Features list
- Hardware requirements (BOM)
- Build instructions (link na phases)
- Usage guide
- Troubleshooting
- License & credits

### 3. Online prezentácia
Platformy:
- **Hackster.io** - Project page s detailným tutoriálom
- **Instructables** - Step-by-step guide
- **Reddit:** r/Multicopter, r/arduino, r/esp32
- **YouTube** - Build video
- **Twitter/X** - Progress updates + final reveal

**Social media strategy:**
- Tease progress počas buildu (WIP shots)
- Final reveal s video
- Engage s komunitou (answer questions)

---

## 5.3 Zdieľanie znalostí

### Write-ups:
1. **Technical deep-dive:** Ako funguje Y-config motor mix
2. **ESP-NOW tutorial:** Wireless drone control
3. **PID tuning guide:** Pre asymetrické drony
4. **Safety guide:** LiPo handling best practices

Uložiť v `docs/` alebo publikovať externally (Medium, Dev.to)

### Open-source contributions:
- Zdieľať CAD súbory (Thingiverse, Printables)
- Arduino libraries (ak vytvoríš custom gesture library)
- Betaflight preset pre sword-drone

---

## Checklist pre túto fázu:

- [ ] Build photos organizované
- [ ] Kód na GitHub
- [ ] CAD súbory zdieľané
- [ ] Video vytvorené
- [ ] README.md kompletný
- [ ] Online project page (Hackster/Instructables)
- [ ] Social media posts
- [ ] Community engagement (odpovede na otázky)

---

## License & Credits

**Odporúčaný license:**
- **MIT License** pre kód (open-source, permissive)
- **CC BY-SA 4.0** pre dokumentáciu a CAD

**Credits template:**
```markdown
## Credits

**Designer & Builder:** [Tvoje meno]
**Inspiration:** [Ak použiješ existujúce projekty]
**Libraries:**
- MPU9250 by hideakitai
- ESP-NOW (Espressif)
**Community help:**
- Betaflight Discord
- r/Multicopter

**Special thanks:** [Ľudia, ktorí pomohli]
```

---

**Previous:** [Phase 4 - Tuning](phase-4-tuning.md) | **Back to:** [Project Plan](../../project-plan.md)
