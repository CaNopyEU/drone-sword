# FÁZA 2: MECHANICKÁ KONŠTRUKCIA (2 týždne)

**Späť na:** [Project Plan](../../project-plan.md) | **Context:** [context.md](../../context.md)

---

## 2.1 Návrh rámu v CAD
**Cieľ:** Presný 3D model pred rezaním materiálu

**Software:** Fusion 360 (free pre hobby) alebo OnShape

### Design špecifikácie:
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

### Návrh kroky:

#### 1. Hlavná karbonová platňa (chrbtová kosť):
- Tvar: dlhý trojuholník/kopija
- Hrúbka: 3mm karbon v strede, 2mm na koncoch
- Výrezy pre motor mounts
- Otvory pre M3 skrutky

#### 2. Motor mounting plates:
- 3ks, karbon 2mm
- 16x16mm štandard pre motory
- Uhol naklonenia: 5° von od centra (lepšia stabilita)

#### 3. Rukoväť:
- 3D tlačená PETG
- Dutá pre batériu a ESP32 receiver
- Závaží na konci (balancovanie)
- Magnetické krytie pre prístup k elektronike

#### 4. Ochranný kryt elektroniky:
- 3D tlačený, transparentný PETG
- Prístup k FC cez clips

### Výstup:
- [ ] STL súbory pre 3D tlač → `cad/handle/`
- [ ] DXF súbory pre CNC cutting karbonu (alebo ručné) → `cad/frame/`
- [ ] Assembly drawing s rozmermi → `cad/assembly.step`

---

## 2.2 Výroba a montáž
**Cieľ:** Zostavený rám pripravený na elektroniku

### Pracovný postup:

#### Deň 1-2: Karbon práca
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

#### Deň 3-4: 3D tlač
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

#### Deň 5: Predmontáž
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

## 2.3 Elektrická inštalácia
**Cieľ:** Všetky komponenty zapojené a ready to flash

### Wiring diagram:
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

**Detailná schéma:** `hardware/diagrams/wiring-diagram.md`

### Spájkovanie best practices:
1. Tin všetky pads a konce káblov najprv
2. Heat shrink PRED spájkovaním
3. Strain relief: hot glue na kábel 5mm od spoja
4. Izoluj všetky spoje (krycia bužírka)
5. Kontinuita test každého spoja multimetrom

### Pre-flight electrical checks:
- [ ] Žiadne skraty medzi +BAT a GND (multimeter)
- [ ] 5V rail: 4.9-5.1V (bez záťaže)
- [ ] Motor spinning test (bez props): všetky 3 otáčajú správne
- [ ] FC power on: LED indikuje správnu inicializáciu

---

## Checklist pre túto fázu:

- [ ] CAD model finalizovaný
- [ ] Karbon vyrezaný
- [ ] 3D diely vytlačené
- [ ] Rám zomontovaný
- [ ] Balancing complete (±5g)
- [ ] Elektronika nainštalovaná
- [ ] Wiring check passed

**Previous:** [Phase 1 - Bench Test](phase-1-bench-test.md) | **Next:** [Phase 3 - Software](phase-3-software.md)
