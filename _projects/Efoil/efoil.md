# E-Foil Project — Design & Build Log
**ChrisJan Daling • UVic Mechanical Engineering • 2025**
## Goal & Constraints
**Goal.** Design and build an electric hydrofoil that matches or outperforms mainstream consumer models while costing substantially less.  
**Targets.**
- Top speed: **≥ 35–45 km/h**
- Ride time: **≥ 45–60 min** at cruise

**Constraint.** Total project spend **≤ \$2,500** (materials, electronics, tooling, consumables).

---

## Requirements & Key Calculations *(approx., based on supplier specs)*

**Total system mass**  
95 (rider) + 18 (board) + 16 (battery) + 6 (system) = **135 kg**

**Buoyancy / Board volume**  
- Whole system (with rider) would need ~**135 L** to float → too large for a practical board.  
- **System without rider:** target **40–50 L**.  
- SolidWorks model volume: **84.75 L** → enough to float the system and aid takeoff.

**Battery pack** *(16S12P Li-ion, 3.2 V nominal, 2.4 Ah/cell)*  
- Nominal voltage: 16 × 3.2 = **57.6 V**  
- Capacity: 12 × 2.4 = **28.8 Ah**  
- Energy: 57.6 × 28.8 ≈ **1659 Wh**; usable (≈85%): **~1410 Wh**  
- Max discharge (15 A/cell claim): 15 × 12 = **180 A** → **~10.4 kW** @ 57.6 V

**Runtime estimates** *(η ≈ 0.9 for quick budgeting)*

| Rider mass | Approx. cruise power | Estimated cruise time \(t \approx \eta \cdot 1410 / P\) |
|---|---:|---:|
| 60–70 kg | 1 kW | ~**76 min** |
| 70–80 kg | 2 kW | ~**38 min** |
| 80–90 kg | 3 kW | ~**25 min** |

---

## Options Considered

### Motors
| Brand | Model | S-range | Rated kW | Peak kW | IP/Depth | Notes | Price (CAD) |
|---|---|---|---:|---:|---|---|---:|
| Flipsky | 65161 | 6–20S | 3.0 | 6 | IP68 | Popular but many report waterproofing/performance issues (Foil.zone discussions). | ~\$410 |
| Maytech | MTI65162-SF | 6–24S | 8.2 | 9+ | IP68 | Strong option; **shipping cost/time** also a concern. | ~\$471.85 |
| Freechobby | MP70131 | – | 6 | 12 | IP68 | Some waterproofing concerns; will add extra sealing steps. | ~\$310 |

### ESC (Electronic Speed Controller)
| Brand | Model | Voltage | Cont. A | Features | Notes | Cost (CAD) |
|---|---|---|---:|---|---|---:|
| Flipsky | **FSESC 75200** | 14–75 V (3–16S) | 200 | USB, CAN, sensors | Best price; includes **waterproof e-foil remote**. | \$244.99 |
| MakerX | Go-FOC HI200 | up to 75 V | 200 | VESC-based, CAN | Too expensive. | \$839.71 |
| TRAMPA | VESC 75/300 | up to 75 V | 300 | IMU, DRV-less | — | \$443.46 |
| HGLTECH/FIRDUO | Sea Lion 5–12 | 5–12S | 200 | PWM boat ESC | — | \$195.92 |

### BMS
| Brand | Model | S-range | Cont A | Peak A | Balance | Notes | Price |
|---|---|---|---:|---:|---|---|---:|
| **JK BMS** | **JK-B2A24S** | 8–24S | 150–200 | 350 | Constant | Chosen; found locally well under market price. | \$300 (paid \$100 used) |
| DALY Smart | 16S | 16S | 40 | 150 | Passive | Not feasible for 16S **current** needs. | \$250 |
| ANT Smart | — | 16–24 | 120 | 220 | Passive | Not feasible for 16S setup here. | \$230 |

### Cells
- **Modicel P42A (18650)** entries were considered (capacity, discharge, price pending vendor specifics).

---

## Design
- **Board:** foam core with fiberglass/epoxy skin; service hatches; hardpoints for mast plate and trays.
- **Motor mast mount:** custom-designed to shield the motor and manage flow; front mount included.
- **Power system:** battery → anti-spark → ESC; temp sensing; waterproof remote.
- **Waterproofing:** double O-rings, strain relief, external waterproof connectors; pressure/dunk tests.

---

## Build Process
1. **Battery pack** – choose S/P layout; spot-weld tabs & busbars; add BMS/connectors; insulate.  
2. **Power electronics** – select ESC; wire battery→anti-spark→ESC; add temp sensor; bench test.  
3. **Drive unit** – select motor; design/print mount; seals/bearings; prop; pressure-test housing.  
4. **Mast & foil integration** – route phase/signal cables; strain relief; waterproof connectors.  
5. **Board** – shape core; install hardpoints; cut hatches; fiberglass layup; sand/paint; gasketed lids.  
6. **Controls** – pair remote/receiver; throttle curves & safety limits; verify failsafe.  
7. **Wiring & assembly** – looms, ferrules, heat-shrink; secure with mounts.  
8. **Waterproofing tests** – IP checks; leak tests; “dry-run” with electronics in a test box.  
9. **Calibration & tuning** – ESC limits (current/voltage/temp), soft-start, braking.  
10. **Bench tests** – spin-up; current vs rpm logging.  
11. **Sea trials** – taxi → lift-off; log speed/current/temps; iterate ESC settings.  
12. **Maintenance** – rinse, corrosion protection, battery storage protocol.  
13. **Documentation** – photos, BOM & costs, test logs, lessons learned, next revs.

---

## Testing / Results

### Electrical & waterproofing
| Part | Test | Result | Notes |
|---|---|---|---|
| Battery | Discharge | **Pass ✅** | With motor load, delivered **~100 A** (per VESC Tool) for extended periods; motor heats first. |
| Battery | Charging | **Pass ✅** | Holds charge well; **disable active balancing** during storage. |
| Battery | Active balancing | **Pass ✅** | Works during charge. |
| ESC | Continuous power (100–150 A) | **Fail/Limit ❌** | Performs for a while but overheats; “200 A continuous” spec assumes **water-cooled case**. Plan: add dedicated water-cooled case and small loop. |
| Motor + Mount | Watertight | **Pass ✅** | Mount helps keep water off motor; more testing planned. |
| Hatch + Mast mount | Waterproof | **Pass ✅** | After prolonged submersion, no ingress into hatch. |

---

## Cost / Bill of Materials (BOM)
| Part | Model / Spec | Qty | Source | Notes | Unit \$ | Total \$ |
|---|---|---:|---|---|---:|---:|
| **Motor** | Freechobby MP70131 | 1 | Alibaba | — | 306.32 | **306.32** |
| **ESC** | Flipsky FSESC 75200 + Remote | 1 | Flipsky.net | \$200 + shipping ≈ \$244.99 | 244.99 | **244.99** |
| **BMS** | JK-BMS JK-B2A24S | 1 | Facebook Marketplace | Well below market (local) | 100.00 | **100.00** |
| **Cells** | 18650 (mix, mostly new) | 300 | Facebook Marketplace | Bulk negotiated | 1.00 | **300.00** |
| Cell holders | 18650 holders | 2 | Amazon | — | 14.99 | 30.00 |
| **Foam (board core)** | Owens Corning FOAMULAR NGX 2″×24″×96″ | 3 | Work offcuts | Free | 0.00 | **0.00** |
| **Fiberglass** | 6 oz × 50″ cloth | ~18.5 m² | Coast Fibre Tek | Overestimated qty | 13.8/m² | 250.00 |
| **Epoxy** | Aqua-Set Resin Kit | 6 L | Coast Fibre Tek | — | 258.40 | 258.40 |
| **Paint** | Pettit EZ-Poxy Topside Spray (20 oz) | 5 | Steveston Marine | — | 26.91 | 135.00 |
| Deck grip | Abahub EVA foam decking | 1 | Amazon | — | 61.99 | 61.99 |
| **Al plate** | 5083 alloy | 1 | Metal Supermarkets (Burnaby) | Offcuts | 48.00 | 48.00 |
| **Mast+foil** | Slingshot Hover Glide 35.4″ | 1 | Facebook Marketplace | — | 350.00 | **350.00** |
| Spot welder | Al-capable small welder | 1 | Amazon | — | 39.99 | 39.99 |
| Nickel strips | 18650 strip | — | Amazon | — | 30.00 | 30.00 |
| **Copper bars** | C110 (ETP) | — | Metal Supermarkets | Offcuts; punched holes | 80.00 | 80.00 |
| 3D filament | PLA/PETG | 4 | Amazon | Mixed | 25.00 | 100.00 |
| Connectors | XT150 (motor-ESC) | set | Amazon | — | 15.99 | 15.99 |
| Connectors | QS8-S anti-spark (battery-ESC) | set | Amazon | — | 27.00 | 27.00 |
| Fasteners & misc. | — | — | Amazon | — | — | **100.00** |
| **Estimated Total** |  |  |  |  |  | **\$2,477.68 (≈)** |

*Many tools/consumables (soldering iron, drills/bits, etc.) were already on hand and excluded.*

---

## Lessons Learned
- “Continuous current” claims on ESCs often assume **liquid-cooled cases**; plan thermal headroom.
- Active BMS balancing is great while charging—**disable for storage** to avoid parasitic drain.
- Mount/shroud design materially helps water management around the motor—continue iteration.
- Over-ordering fiberglass is easy; measure layups to control cost and weight.
- Keep a tight **photo + test log**; QR link to build album for documentation and portfolio.

---

## Next Steps
- Add **water-cooled ESC case** and compact pump/loop.
- More waterproofing cycles (vacuum → dunk → heat → re-dunk).
- Log **current vs. speed** on-water; refine prop/foil match for 1–3 kW cruise targets.

---

## Sources & Further Reading

- Reddit — *Making your own e-foil* [R].
- YouTube — *How to build an e-foil* [Y].
- Foil Zone — *eFoil Builders* [FZ].

[R]: https://www.reddit.com/r/eFoil/comments/s1bl4x/making_your_own_efoil/
[Y]: https://www.youtube.com/watch?v=bbcwL9_MjeY
[FZ]: https://foil.zone/c/efoil-builders/18
