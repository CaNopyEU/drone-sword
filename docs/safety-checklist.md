# Safety Checklist

**⚠️ ČÍTAJ PRED KAŽDOU WORK SESSION ⚠️**

**Späť na:** [Context](../context.md) | [Troubleshooting](troubleshooting.md)

---

## 🔋 LiPo Battery Safety

### Pred nabíjaním:
- [ ] Batéria v LiPo safety bag
- [ ] Balance charging enabled
- [ ] Correct cell count selected (4S)
- [ ] Charge rate ≤ 1C (1.55A pre 1550mAh)
- [ ] Nabíjačka na nehorľavom povrchu (kovový/ceramic tray)
- [ ] Smoke detector funkčný v miestnosti

### Počas nabíjania:
- [ ] **NIKDY nenechávaj bez dozoru**
- [ ] Check každých 10-15 min
- [ ] Nabíjaj v dobre vetranej miestnosti
- [ ] Žiadne horľavé materiály do 1m
- [ ] Pripravená nádoba s pieskom/dirt (fire extinguisher)

### Po nabití:
- [ ] Voltage check: každý cell 4.15-4.20V
- [ ] Odpoj batériu do 30 min po dokončení
- [ ] Storage v LiPo bag (nie v drone)
- [ ] Storage voltage: 3.8V/cell ak nepoužívaš >1 týždeň

### Warning signs - STOP using battery ak:
- ❌ Puffed/swollen (akýkoľvek bubble)
- ❌ Physical damage (dent, puncture)
- ❌ Cell voltage difference >0.05V
- ❌ Abnormálne teplé počas nabíjania (>40°C)
- ❌ Hissing/zapach

**Disposal:** Discharge do 0V (salt water 24h), recykluj v elektronike obchode

---

## ⚙️ Pre-flight Checklist

### T-24h (Príprava):
- [ ] Battery fully charged (check voltage)
- [ ] Props integrity check (no cracks)
- [ ] All screws tight (M3 especially)
- [ ] FC firmware up-to-date
- [ ] Glove ESP32 battery charged
- [ ] Backup props v case

### T-15min (On-site):
- [ ] Weather check: vietor <15 km/h, no rain
- [ ] Flight area clear (min 10m radius bez ľudí)
- [ ] Soft ground (tráva preferovaná)
- [ ] Backup plan (kde pristáť pri problémoch)
- [ ] Asistent prítomný (emergency battery disconnect)
- [ ] First aid kit accessible

### T-5min (Power-on):
- [ ] Visual inspection: žiadne loose wires
- [ ] Battery voltage: >15.6V (3.9V/cell)
- [ ] Battery secure v drone (no wobble)
- [ ] Props správne orientation (check twice!)
- [ ] FC boot sequence normal (LED pattern)
- [ ] Glove-drone connection established (Serial check)
- [ ] Gesture test: roll/pitch response bez props

### T-0 (Takeoff):
- [ ] **Ochranné okuliare ON**
- [ ] Clear airspace (birds, people)
- [ ] Arm gesture recognized
- [ ] Throttle response smooth (no jerks)
- [ ] Stabilita pri hover (3s test)
- [ ] Emergency disarm gesture ready

---

## 🔧 Workshop Safety

### Soldering:
- [ ] Vetraná miestnosť (open window)
- [ ] Soldering iron v stojane (nie na stole)
- [ ] Pomoc "third hand" pre stabilitu
- [ ] Ochrana očí (safety glasses)
- [ ] Power off components pred spájkovaním

### Karbon rezanie:
- [ ] **MANDATORY:** Respirator mask (karbon prach = zdravotné riziko)
- [ ] Ochranné okuliare
- [ ] Vonku alebo extremely well ventilated
- [ ] Long sleeves (karbon splinters)
- [ ] Vacuum cleanup (nie compressed air - rozfúka prach)

### 3D printing (PETG):
- [ ] Printer v vetranej miestnosti
- [ ] Bed adhesion check (prevent warping)
- [ ] First layer watch (5 min)
- [ ] Fire safety: printer nie unattended >2h

### Power tools:
- [ ] Drill/Dremel secure grip
- [ ] Workpiece clamped (nie držané rukou)
- [ ] Correct bit/blade pre material
- [ ] Unplug pred bit change

---

## ✈️ Flight Safety Rules

### Nikdy nelietaj:
- ❌ Nad ľuďmi / crowd
- ❌ V daždi / sneh
- ❌ Pri vetre >20 km/h (beginner)
- ❌ V noci (bez FPV lighting)
- ❌ Blízko letiska (<5km bez povolenia)
- ❌ Nad autami / cesta
- ❌ Keď si unavený / pod vplyvom

### Vždy:
- ✅ Line of sight (vidieť dron)
- ✅ Escape route (kde pristať pri failsafe)
- ✅ Respect privacy (no cameras pointing at windows)
- ✅ Follow local laws (Czech: max 120m AGL, register >250g)
- ✅ Announce letov susedom (courtesy)

### Distance rules:
- **Ľudia:** Min 50m horizontal
- **Budovy:** Min 10m
- **Pilot:** Max 100m (gesture control range limited)
- **Altitude:** Max 30m (testing phase), legal max 120m

---

## 🩺 Personal Safety

### Protective equipment:
- [ ] Safety glasses (MANDATORY pri flight)
- [ ] Closed-toe shoes (no sandals)
- [ ] Long pants (protect from props pri ground handling)
- [ ] Gloves pri karbon work

### First aid:
- [ ] First aid kit on-site
- [ ] Know location of nearest hospital
- [ ] Phone charged (emergency call)

### Prop injuries prevention:
- **NEVER** reach near spinning props
- **ALWAYS** disarm pred handling
- **WAIT** 5 sec after disarm (motors stop)
- **APPROACH** from behind (nie front)

---

## 🧯 Emergency Equipment

**Pri každom flight mať:**
- [ ] Fire extinguisher / sand bucket (LiPo fire)
- [ ] Thick gloves (emergency battery removal)
- [ ] Wire cutters (cut power quickly)
- [ ] Phone (emergency call)
- [ ] First aid kit

---

## 📋 Pre-Storage Checklist

**Po session:**
- [ ] Battery discharged to storage (3.8V/cell)
- [ ] Props removed (safe storage)
- [ ] Visual damage check
- [ ] Clean dust/dirt z electronics
- [ ] Log flight time / issues
- [ ] Charge glove battery (ready for next time)

**Long-term storage (>1 month):**
- [ ] Remove battery z drone
- [ ] Store battery v fireproof container
- [ ] Temperature: 5-25°C
- [ ] Check battery voltage monthly

---

## 🚨 Know Your Limits

### Stop flying ak:
- Battery <3.5V/cell (20% capacity)
- Wind picks up (>20 km/h)
- Rain starts
- Loss of visual
- Control lag >500ms
- ANY unusual behavior

### Progressive testing:
1. ✅ Bench test (no props)
2. ✅ Tethered test (1m rope)
3. ✅ Ground effect hover (<1m)
4. ✅ Low altitude flight (1-2m)
5. ✅ Normal altitude (5-10m)
6. ⚠️ Advanced maneuvers (only after 10+ successful flights)

---

## 📞 Emergency Contacts

**Priprav si:**
- [ ] Local fire department: ___________
- [ ] Nearest hospital: ___________
- [ ] Emergency contact person: ___________
- [ ] Insurance (ak máš liability coverage): ___________

---

## ✍️ Safety Acknowledgement

**Pred prvým letom, confirm:**
- [ ] Čítal som celý safety checklist
- [ ] Rozumiem rizikám LiPo batteries
- [ ] Mám protective equipment
- [ ] Mám emergency plan
- [ ] Flight area je bezpečný
- [ ] **Som pripravený DISARM okamžite pri probléme**

---

**Bezpečnosť > Cool tricks. Always.**

**Ďalšie čítanie:**
- [Troubleshooting Guide](troubleshooting.md)
- [Phase 3 - Testing Protocols](phases/phase-3-software.md#33-testovací-protokol-dôležité)
