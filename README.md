Excellent project! This PID learning tool is perfect for building deep understanding. Let me help you set up the GitHub repository and all related content for **Project 2**.

---

## GitHub Repository Name Suggestions

| Name | Why |
|------|-----|
| **PID-Learning-Platform-TIA** | Clear educational purpose |
| **Analog-PID-Control-FactoryIO** | Shows tech stack |
| **PID-Tuning-Simulator-SCL** | Highlights custom PID + simulation |
| **Learn-PID-with-HMI-Animation** | Focus on learning + visual |

**My Recommendation:** `PID-Learning-Platform-TIA`

---

## Repository Description (350 Characters)

```
Custom PID controller written in SCL for learning PID tuning. Tank level control with Factory IO simulation. HMI shows P,I,D terms separately, real-time response, analog alarms. Perfect for understanding Kp, Ki, Kd effects.
```

---

## GitHub README.md for Project 2

```markdown
# PID Learning Platform | Custom SCL PID Controller

## Overview
A complete educational platform to learn PID control by doing. This project uses a **custom PID controller written entirely in SCL** (not the standard PID_Compact block) so you can see exactly how P, I, and D terms work together.

Control a tank level with filling valve, drain valve, and level sensor in Factory IO simulation. Change Kp, Ki, Kd gains from HMI and see the real-time response instantly.

---

## Why This Project?

After 13 years in industrial automation, I know that **PID is everywhere**:
- Temperature control
- Pressure regulation
- Speed control (VFDs)
- Flow control
- Tension control
- Torque control

Understanding PID deeply is a **core skill** for any automation engineer.

---

## Requirements

| Software | Version |
|----------|---------|
| TIA Portal | V16 or higher |
| PLCSIM | V16 or higher |
| Factory IO | V2.6.0 or higher |

---

## System Overview

### Process
```
Setpoint (HMI) → Error → PID Calculation → Output → Filling Valve
                        ↓
Level Sensor (0-10V) ← Tank Level ← Water Flow
                        ↓
                     Drain Valve
```

### Components
- **Tank** with level sensor (0-10V analog input)
- **Filling valve** (0-10V analog output - control valve)
- **Drain valve** (Digital output - ON/OFF)
- **Factory IO scene** with realistic tank dynamics

---

## Signal Scaling

| Signal | Raw Input | Scaled Value | Display |
|--------|-----------|--------------|---------|
| Level Sensor | 0 - 10V | 0 - 100% | 0 - 300 Units (HMI) |
| PID Setpoint | - | 0 - 100% | 0 - 300 Units (HMI) |
| PID Output | 0 - 100% | 0 - 10V | 0 - 100% (to Valve) |

### Scaling Blocks Used
- `NORM_X` - Convert raw analog to 0-100%
- `SCALE_X` - Convert 0-100% to engineering units
- Custom scaling for PID input/output

---

## Custom PID FB (SCL)

**Not using PID_Compact** - Written from scratch for learning:

```scl
// Custom PID Algorithm - Positional Form
error := setpoint - actual;

P_Term := Kp * error;

I_Term := I_Term + (Ki * error * dt);
// With anti-windup limiting

D_Term := Kd * (error - previous_error) / dt;

output := P_Term + I_Term + D_Term;

// Limit output 0-100%
IF output > 100 THEN output := 100; END_IF;
IF output < 0 THEN output := 0; END_IF;
```

### PID Parameters (HMI Adjustable)
| Parameter | Range | Description |
|-----------|-------|-------------|
| Kp (Gain) | 0 - 10.0 | Proportional gain |
| Ki (Integral) | 0 - 5.0 | Integral gain (sec⁻¹) |
| Kd (Derivative) | 0 - 2.0 | Derivative gain (sec) |
| Setpoint | 0 - 300 Units | Target tank level |

---

## HMI Features

### Main Screen
- Animated tank level (fills/drains in real-time)
- Live level display (Units and %)
- Setpoint input
- Current P, I, D term values (separately displayed)
- Output % to filling valve
- Valve animation (opening/closing)

### PID Tuning Screen
- Kp, Ki, Kd adjustment (numeric input + slider)
- Real-time response graph
- Error display
- Derivative of error display

### Analog Alarms (Focus Feature)
| Alarm | Condition | Display |
|-------|-----------|---------|
| High-High (Critical) | Level > 270 Units | Red + Buzzer |
| High | Level > 240 Units | Orange |
| Low | Level < 60 Units | Yellow |
| Low-Low (Critical) | Level < 30 Units | Red + Buzzer |

### Discrete Alarms
- **Valve Saturated** - Output at 0% or 100% for >10 sec
- **PID Response Alarm** - Error > 10% for >30 sec (valve not responding)

### Buzzer Control
- Triggers on Critical High / Critical Low
- HMI mute button

---

## Program Structure

```
Project 2: PID Learning Platform
│
├── PLC Tags
│   ├── Analog Input (Level Sensor - IW)
│   ├── Analog Output (Filling Valve - QW)
│   └── Digital Output (Drain Valve)
│
├── UDTs
│   └── UDT_PID_Parameters (Kp, Ki, Kd, Setpoint, Output, Error)
│
├── FBs
│   ├── FB_PID_Controller (Custom PID logic in SCL)
│   ├── FB_Scaling (NORM_X / SCALE_X utilities)
│   └── FB_AnalogAlarms (H, HH, L, LL with hysteresis)
│
├── DBs
│   ├── DB_PID (PID parameters and runtime values)
│   ├── DB_Alarms (Analog + Discrete alarms)
│   └── DB_HMI (HMI interface data)
│
└── OBs
    ├── OB100 (Startup - Initialize PID)
    ├── OB35 (Cyclic interrupt - PID calculation every 100ms)
    └── OB80 (Time error handling)
```

---

## HMI Display Values

| Value | Display Format |
|-------|----------------|
| Actual Level | 0 - 300 Units AND 0-100% |
| Setpoint | 0 - 300 Units AND 0-100% |
| Error | -100 to +100 Units |
| P Term | 0 - 100% |
| I Term | 0 - 100% |
| D Term | -100 to +100% |
| Output | 0 - 100% |

---

## Learning Exercises

### Exercise 1: Only P Control
- Set Ki = 0, Kd = 0
- Increase Kp gradually
- Observe: Steady-state error (offset)

### Exercise 2: Add I Control
- Start with P only (small Kp)
- Add small Ki
- Observe: Offset eliminated, but possible overshoot

### Exercise 3: Add D Control
- Start with P and I tuned
- Add small Kd
- Observe: Reduced overshoot, faster settling

### Exercise 4: Tuning Challenge
- Set setpoint = 200 Units
- Tune Kp, Ki, Kd for best response
- Try different setpoints (50, 150, 250)

### Exercise 5: Disturbance Rejection
- Open drain valve suddenly
- Watch PID respond to maintain level

---

## Analog Alarm Demo

```
Normal Range: 60 - 240 Units

Level 300 ─┬─ HH Alarm (Critical) → Red + Buzzer
           │
Level 270 ─┼─ H Alarm → Red
           │
Level 240 ─┼─ High Limit
           │
Level 60 ───┼─ Low Limit
           │
Level 30 ───┼─ L Alarm → Yellow
           │
Level 0 ────┴─ LL Alarm (Critical) → Red + Buzzer
```

---

## Factory IO Scene Setup

**Required objects:**
- Tank (with level visualization)
- Inlet valve (analog controlled)
- Outlet valve (digital - drain)
- Level sensor (analog output)

**Scene dynamics:**
- Inlet flow proportional to valve opening
- Drain flow constant when valve open
- Level changes based on (inlet - outlet) flow

---

## Quick Start

1. Download the `.ap16` file from this repository
2. Open in TIA Portal V16+
3. Load to PLCSIM
4. Open Factory IO scene (included)
5. Run HMI simulation
6. Start with Kp = 1.0, Ki = 0.1, Kd = 0
7. Watch tank level response
8. Tune and learn!

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No level change | Check PLCSIM and Factory IO connection |
| PID not responding | Verify OB35 is running (100ms cycle) |
| Valve saturation alarm | Reduce Kp or increase cycle time |
| Oscillation | Reduce Kp, increase Ki |

---

## Project Status

✅ **PROJECT 2: PID Learning Platform** - In Development

---

## Repository Contents

```
📁 PID-Learning-Platform.ap16    # TIA Portal project
📁 README.md                       # This file
📁 FactoryIO_Scene.json            # Tank scene
📁 PID_Tuning_Guide.pdf            # Tuning instructions
```

---

## Author

Building industrial automation skills from the floor up.

13 years commissioning ABB robots at Alok Industries Limited → Now programming PLCs.

---

## License

Educational purposes only.

---

## Next Project

🔜 **PROJECT 3: [To be announced]**
```

---

## LinkedIn Post for Project 2 Launch

```linkedin
🔜 PROJECT 2 LAUNCHED ON GITHUB

PID Learning Platform | Custom SCL PID Controller

After completing my Smart Conveyor System (Project 1), I'm moving to continuous process control.

---

WHAT I BUILT:

A complete PID learning tool with:
✅ Custom PID controller written in SCL (not PID_Compact)
✅ Tank level control with Factory IO simulation
✅ Real-time P, I, D term display on HMI
✅ Kp, Ki, Kd adjustable from HMI
✅ Full analog alarm system (H, HH, L, LL)
✅ HMI animation of tank filling/draining
✅ Scaling using NORM_X and SCALE_X
✅ 0-10V analog input (level sensor)
✅ 0-10V analog output (filling valve)

---

WHY THIS MATTERS:

PID is EVERYWHERE in industry:
• Temperature control
• Pressure regulation
• Speed control (VFDs)
• Flow control
• Tension control
• Torque control

Understanding P, I, D terms deeply is a core skill for any automation engineer.

---

WHAT YOU CAN LEARN:

Exercise 1: Only P Control → See steady-state error
Exercise 2: Add I Control → See offset eliminated
Exercise 3: Add D Control → See overshoot reduced
Exercise 4: Tuning Challenge → Find optimal gains
Exercise 5: Disturbance Rejection → Open drain valve

---

HMI FEATURES:

• Animated tank level (fills/drains live)
• Separate P, I, D term values displayed
• Analog alarms: H, HH, L, LL with buzzer
• Valve saturation alarm
• PID response alarm

---

📦 GitHub Repository: [Link]
📺 Demo Video: [Link]

Project 1: Conveyor (Discrete) ✅
Project 2: PID (Process) 🔜

#PLC #TIAportal #PIDControl #SCLProgramming #IndustrialAutomation #AnalogControl #FactoryIO #LearningPID #ProcessControl
```

---

## Hashtags for Project 2

```
#PLC #TIAportal #PIDControl #SCLProgramming #IndustrialAutomation #AnalogControl #FactoryIO #LearningPID #ProcessControl #PIDTuning #NORMX #SCALEX
```

---

