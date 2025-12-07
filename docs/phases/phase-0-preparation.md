# FÁZA 0: PRÍPRAVA A UČENIE (2-3 týždne)

**Späť na:** [Project Plan](../../project-plan.md) | **Context:** [context.md](../../context.md)

---

## 0.1 Teoretické základy
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

## 0.2 Obstaranie komponentov
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

**Výstup:** Kompletný BOM uložený v `hardware/bom.csv`

---

## Checklist pre túto fázu:

- [ ] Všetky komponenty objednané
- [ ] Teoretické základy preštudované
- [ ] CAD software nainštalovaný (Fusion 360 / OnShape)
- [ ] Workspace pripravený (dobre vetraný, dobrá osvit)
- [ ] Safety equipment: okuliare, maska proti prachu, LiPo bag

**Next:** [Phase 1 - Bench Test](phase-1-bench-test.md)
