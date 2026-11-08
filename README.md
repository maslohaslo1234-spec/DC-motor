# DC Motor

A small electric motor that I design from scratch: a 3D-printed rotor with two neodymium magnets spins on two bearings. A coil of enamelled copper wire sits next to the path of the magnets. A Hall sensor detects each magnet, and a MOSFET switches the coil on at the right moment, so the coil pulls the magnet forward and keeps the rotor spinning. It is powered from a 12 V bench supply.

**Status:** design phase. The schematic and the CAD model are done. The motor has not been built or tested yet.

## How it works

1. The rotor is a disc with two magnets, one on each side of the axis. Both have the same pole facing down, towards the coil.
2. A Hall sensor (A3144) stands in the path of the magnets, a little **before** the coil in the direction of rotation. It reacts only to the south pole of a magnet.
3. When a magnet passes the sensor, the sensor output goes low. This opens the P-channel MOSFET (IRF9540N), so current flows through the coil.
4. The coil becomes an electromagnet and pulls the magnet forward, towards the coil. When the magnet moves on, the sensor releases, a 10 kOhm resistor pulls the gate up, and the MOSFET switches off.
5. A diode (1N5822) across the coil absorbs the voltage spike that appears when the current is switched off.
6. The second magnet does the same half a turn later, so the rotor gets two pushes per revolution.

A motor with one coil cannot start itself from every position, so it may need a push with a finger. The direction of rotation is set by the sensor being in front of the coil.

## Electronics

<img width="583" height="736" alt="image" src="https://github.com/user-attachments/assets/8a64b20e-db46-47d2-822c-c99e2bab06ca" />

The schematic is drawn in KiCad (`schematic/`). There is no custom PCB, because the circuit has only a few parts and will be wired on a small perfboard.

| Part | Purpose |
|---|---|
| A3144 Hall sensor | Detects the magnets (supply 4.5-24 V, open-collector output) |
| IRF9540N P-channel MOSFET | Switches the coil. The Hall sensor pulls its gate low, which opens it |
| 10 kOhm resistor | Holds the gate high (MOSFET off) when the sensor sees nothing |
| 1N5822 diode (3 A Schottky) | Flyback diode across the coil |
| 100 uF 25 V capacitor | Smooths the current pulses of the supply |
| Coil: about 450 turns of 0.25 mm enamelled wire (~13 Ω estimated) | The electromagnet |
| 2x neodymium magnets 10 x 3 mm (N52) | Get attracted by the coil |
| 12 V bench supply, current limit 1 A | Power |

I chose the P-channel MOSFET because the A3144 pulls its output low when it sees a magnet, and a high-side P-channel MOSFET turns on exactly then. It needs about 10 V on the gate to be fully open, so the circuit runs from 12 V.

## Mechanical design

The model is made in Onshape:
https://cad.onshape.com/documents/5e1b1a74ffba5d8179451b37/w/749f37bce197d5164e74821c/e/9d904d3edbaf5cf01d2fa46c?renderMode=0&uiState=6af06d5d6e01b752608595d5 

(STEP and STL files are in `cad/`).

<img width="1457" height="765" alt="Zrzut ekranu 2026-11-08 124604" src="https://github.com/user-attachments/assets/47786014-04c9-40d7-81dc-756c31434f1f" />

- **Rotor:** a printed disc (80 mm in diameter, 8 mm thick) with a hub. It has a pocket for a 608 bearing on each side (22.1 mm), a 16 mm hole through the middle so that only the outer rings of the bearings touch it, and two 10.2 mm pockets for the magnets 30 mm from the axis.

<img width="1098" height="545" alt="image" src="https://github.com/user-attachments/assets/e99d2648-d5cf-4d74-b033-798b0a4396c1" />


- **Axis:** an M8 x 40 bolt with its head sunk into the base, a 9 mm spacer under the lower bearing, a 2 mm spacer between the bearings, a washer and a nut. The nut is tightened only so much that the inner rings are held and the rotor spins freely.

<img width="422" height="799" alt="image" src="https://github.com/user-attachments/assets/818ba15a-5a8b-4791-bdb1-9e48840c2617" />


- **Base:** a 120 x 120 x 8 mm plate with four 4 mm mounting holes, 8.2mm hole in the middle for the M8 bolt, slot for the coil, a place to put the hall sensor in and a hexagonal hole in the bottom for the M8 bold head

<img width="1241" height="832" alt="image" src="https://github.com/user-attachments/assets/8bd9e7a8-487b-4ee3-85c1-785b4701007b" />


- **Coil bobbin:** a printed spool (flanges 32 mm in diameter) that plugs into a square hole in the base.

<img width="655" height="372" alt="image" src="https://github.com/user-attachments/assets/edd0a519-f86b-402c-904e-8374fb5df8cd" />


<img width="1278" height="676" alt="Zrzut ekranu 2026-11-08 124648" src="https://github.com/user-attachments/assets/6fff8bee-7434-4c41-a524-eaae588fa7cd" />

The gap between the magnets and the coil is about 2 mm, and between the magnets and the sensor also about 2 mm. A3144 is not very sensitive, so the gap has to be small.

## Safety notes

- Set the bench supply to **12 V** and the current limit to **1 A** before connecting the circuit. The gate of the MOSFET allows 20 V at most, so never go above 15 V.
- The coil gets warm if the rotor is blocked. Do not leave the circuit powered and unattended.
- Neodymium magnets are strong and can pinch fingers. Keep them away from the electronics and from other magnets.

## Project files

| Path | Description |
|---|---|
| `schematic/` | KiCad project and the PDF of the schematic |
| `cad/STEP/` | Complete model and separate parts in STEP |
| `cad/STL/` | Printable parts: rotor, base, bobbin and spacers |
| `BOM.csv` | Bill of materials with prices and vendors |
| `JOURNAL.md` | Development journal mirrored from Half Life |

## Progress

- [x] Idea and choice of parts
- [x] KiCad schematic
- [x] Onshape model with the full assembly
- [ ] Order the parts
- [ ] Print the parts, wind the coil and build the motor
- [ ] Test and tune (position of the sensor, direction of the coil)
- [ ] Optional: add more magnets (the sensor will have to move to about 25 degrees before the coil)

