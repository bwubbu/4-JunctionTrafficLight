# 4-Junction Traffic Light Control System

A microcontroller-based traffic light control system written in Intel 8051 Assembly Language. This project implements a synchronized 4-way intersection traffic light system with automatic timing sequences for safe traffic management.

## 📋 Overview

This is an assignment for **WIX1003: Computer Systems & Organization** at the University of Malaya, FCSIT 2022/23. The project demonstrates practical microcontroller programming using the Intel 8051 processor to control traffic lights at a 4-way junction with precise timing and sequential light control.

## 🚦 System Features

- **4-Way Junction Control**: Manages traffic lights for Top, Right, Bottom, and Left directions
- **Three-Color Lights**: Red, Yellow, and Green lights for each direction
- **Automatic Sequencing**: Cycles through all 4 directions with proper timing
- **Precise Timing Control**: 
  - Red light: 6 seconds
  - Green light: 9 seconds
  - Yellow light: 3 seconds
- **7-Segment Display**: Real-time countdown timer display
- **Safe Transition**: All lights turn red before switching to next direction

## 🔌 Hardware Configuration

### Port Assignments

The system uses three 8-bit I/O ports for LED control and one port for display:

| Port | Function | Bits | Purpose |
|------|----------|------|---------|
| **P0** | Red Lights | P0.0-P0.3 | Top, Right, Bottom, Left red LEDs |
| **P1** | Yellow Lights | P1.0-P1.3 | Top, Right, Bottom, Left yellow LEDs |
| **P2** | Green Lights | P2.0-P2.3 | Top, Right, Bottom, Left green LEDs |
| **P3** | 7-Segment Display | P3.0-P3.6 | Timer countdown display |

### Direction Mapping

```
Bit 0 → Top
Bit 1 → Right
Bit 2 → Bottom
Bit 3 → Left
```

**Example**: 
- `0F0H` = all red lights ON (binary: 11110000)
- `0EH` = top direction only (binary: 11101110)
- `0DH` = right direction only (binary: 11011101)

## 🔄 Traffic Light Sequence

The system cycles through a continuous sequence:

```
TOP (6s red → 9s green → 3s yellow) 
  ↓
RIGHT (6s red → 9s green → 3s yellow) 
  ↓
BOTTOM (6s red → 9s green → 3s yellow) 
  ↓
LEFT (6s red → 9s green → 3s yellow) 
  ↓
[Repeat]
```

## 📁 Project Structure

```
4-JunctionTrafficLight/
├── README.md                           # This file
├── Code.txt                            # Assembly code reference
├── mcu8051ide-1.4.7-setup.exe         # MCU 8051 IDE installer
│
└── src/
    ├── TrafficLight.asm               # Main assembly source code
    ├── TrafficLight.adf               # MCU IDE project file
    ├── TrafficLight.hex               # Hexadecimal object code
    ├── TrafficLight.bin               # Binary executable
    ├── TrafficLight.lst               # Assembly listing file
    │
    └── [Backup files]
        ├── TrafficLight.asm~
        ├── TrafficLight.adf~
        ├── TrafficLight.hex~
        ├── TrafficLight.bin~
        └── TrafficLight.lst~
```

## 🛠️ Development Environment

### Requirements

- **Processor**: Intel 8051 Microcontroller (or compatible)
- **IDE**: MCU 8051 IDE version 1.4.7 or compatible
- **Assembler**: 8051 Cross-Assembler
- **Simulator**: MCU 8051 IDE simulator (included)

### Setup Instructions

1. **Install MCU 8051 IDE**:
   - Run `mcu8051ide-1.4.7-setup.exe`
   - Complete the installation wizard

2. **Open the Project**:
   - Launch MCU 8051 IDE
   - Open `src/TrafficLight.adf` project file

3. **Build the Project**:
   - Select: Project → Assemble
   - Verify no assembly errors
   - Generates: `.hex`, `.bin`, and `.lst` files

4. **Run Simulation**:
   - Select: Simulator → Run
   - Monitor P0, P1, P2, P3 ports in the simulator
   - Observe the 4-way traffic light sequence

## 📝 Key Assembly Routines

### START Initialization
Initializes all ports:
- Sets all red lights ON (P0 = 0F0H)
- Clears yellow lights (P1 = 0FH)
- Clears green lights (P2 = 0FH)

### SET Configuration
Configures timing registers:
- R4 = 06H (red timer value)
- R3 = 09H (green timer value)
- R5 = 03H (yellow timer value)
- DPTR points to 7-segment lookup table (SEG)

### DELAYRED - Red Light Delay (6 seconds)
- Displays countdown on 7-segment (6 → 5 → 4 → 3 → 2 → 1)
- Nested loop for precise timing
- Decrements R4 register

### DELAYGREEN - Green Light Delay (9 seconds)
- Displays countdown on 7-segment (9 → 8 → 7 → 6 → 5 → 4 → 3 → 2 → 1)
- Nested loop for 9-second duration
- Decrements R3 register

### DELAYYELLOW - Yellow Light Delay (3 seconds)
- Displays countdown on 7-segment (3 → 2 → 1)
- Nested loop for 3-second duration
- Decrements R5 register
- Ends with "-" display (040H)

### Junction Control Routines

**TOP**: TOP direction traffic light sequence
**RIGHT**: RIGHT direction traffic light sequence
**BOTTOM**: BOTTOM direction traffic light sequence
**LEFT**: LEFT direction traffic light sequence

Each routine:
1. Sets all other directions to red
2. Turns on red light (DELAYRED)
3. Turns on green light (DELAYGREEN)
4. Turns on yellow light (DELAYYELLOW)
5. Returns to main cycle

## 🔢 7-Segment Display Table

The SEG lookup table provides 7-segment codes for digits 0-9:

```
0: 3FH     5: 6DH
1: 06H     6: 7DH
2: 5BH     7: 07H
3: 4FH     8: 7FH
4: 66H     9: 6FH
```

These values are displayed on P3 during each timing phase.

## 💾 File Outputs

After assembly:
- **TrafficLight.hex** - Hexadecimal machine code (for programming)
- **TrafficLight.bin** - Binary machine code (for burning to ROM)
- **TrafficLight.lst** - Assembly listing (debugging reference)

## 🎓 Learning Objectives

This project demonstrates:
- Intel 8051 assembly language programming
- Microcontroller port I/O operations
- Timing and delay loop implementation
- Lookup table usage (7-segment encoding)
- Modular routine design
- Register manipulation and control flow
- Real-time system simulation

## 🧪 Testing & Simulation

### Simulation Steps

1. Set breakpoints at junction labels (TOP, RIGHT, BOTTOM, LEFT)
2. Monitor port values:
   - Watch P0 for red light states
   - Watch P1 for yellow light states
   - Watch P2 for green light states
   - Watch P3 for 7-segment display
3. Run step-by-step to verify:
   - Correct light sequences
   - Proper timing delays
   - Display countdown accuracy

### Expected Behavior

Each junction cycle:
- All lights red (6 seconds)
- Direction turns green (9 seconds with countdown)
- Direction turns yellow (3 seconds with countdown)
- All lights red again
- Next direction begins

## 📚 Assembly Instructions Used

- **Data Movement**: MOV, MOVC
- **Arithmetic**: DEC, DJNZ
- **Logical**: ANL, ORG, SETB
- **Bit Operations**: SETB
- **Control Flow**: ACALL, AJMP, RET
- **Data Definition**: DB, END

## 🔧 Troubleshooting

| Issue | Solution |
|-------|----------|
| "Unknown directive" error | Ensure MCU 8051 IDE is properly installed |
| Lights not changing | Verify port assignments (P0, P1, P2) in simulator |
| Timing incorrect | Check delay loop counts in DELAYRED/GREEN/YELLOW |
| Display not working | Verify 7-segment table (SEG) and P3 output |
| Compilation errors | Check syntax against 8051 instruction set manual |

## 📖 References

- Intel 8051 Microcontroller Datasheet
- MCU 8051 IDE Documentation
- 8051 Assembly Language Programming Guide
- University of Malaya WIX1003 Course Materials

## ✨ Key Innovations

- Efficient nested-loop timing without external timers
- Lookup table for 7-segment display control
- Modular design for easy modification of sequences
- Safe all-red state between direction changes
- Real-time countdown display for users

## 🤝 Contributing

This is an academic assignment project. Feel free to fork and experiment with:
- Extended timing sequences
- Additional directions (roundabout)
- Pedestrian crossing integration
- Vehicle detection simulation

## 📝 Course Information

- **Course Code**: WIX1003
- **Course Title**: Computer Systems & Organization
- **University**: University of Malaya (UM)
- **Faculty**: FCSIT (Faculty of Computer Science & Information Technology)
- **Academic Year**: 2022/23
- **Student ID**: U2002236

## ⚠️ Notes

- Code is optimized for MCU 8051 IDE simulator
- Actual hardware implementation may require timing calibration
- Port configurations assume standard 8051 pinout
- Assembly syntax follows Intel 8051 convention

## 📧 Contact

For questions about this traffic light control system, feel free to open an issue in the repository.

---

**Status**: Academic Assignment (Complete)  
**Language**: Intel 8051 Assembly (100%)  
**Last Updated**: November 2023
