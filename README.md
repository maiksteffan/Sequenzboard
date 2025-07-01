# Sequenzboard Firmware Repository

Welcome! This repository contains the firmware development for the **Sequenzboard** – an interactive, LED-guided training board inspired by climbing movement.  
This document outlines the system architecture, milestones, and expectations to help guide the firmware implementation process.

---

## 🎯 Project Goal

Develop a modular, reliable firmware stack for the Sequenzboard that enables responsive training interactions, visual feedback, and remote updates.

---

## 📌 Milestones & Features

### 1. Helper Methods (Hardware Abstraction)
Provide foundational helper methods for implementing training program logic. These methods abstract the control of:

- **LED Strips** – Visual Feedback (WS2812)
- **Touch Sensors** – User Input (26 I²C-capable custom PCBs)
- **Display** – User Interface + Touch Input (LVGL-based)

These methods will form the backbone of the training program logic and allow for scalable implementation of training types.

---

### 2. Navigable UI

- Touch-based menu system
- Users can browse and select from different training modes
- Upon selection, the training starts automatically

Supported modes:
-  Structured Workouts
-  Highscore Challenges
-  Multiplayer Game ("Packing List")

---

### 3. Interaction Handling

Once a training session is running, real-time interaction between the following components is orchestrated:

- LED strips → visual instruction & feedback  
- Touch sensors → user input tracking  
- Display → progress indication + additional input

---

### 4. Program Content (v1.0)
Initial firmware should support:

- 10 **Workouts** (each with 3–10 levels)
- 3 **High Score Challenge** modes
- 1 **Multiplayer Game**: *"Packing List"* (Kofferpacken)

> 💡 UI should be **functionally complete**, but design aesthetics can be refined later.

---

### 5. Remote Updates

The system must support **OTA (Over-the-Air)** updates, allowing firmware, content, and UI enhancements to be remotely delivered.

---

## 🛠️ Hardware Overview

| Component         | Details                          |
|------------------|----------------------------------|
| **MCU**          | ESP32-P4-Module-DEV-KIT-C        |
| **Display**      | Capacitive Touchscreen (LVGL)    |
| **LED Strips**   | 2x WS2812                        |
| **Touch Sensors**| 26x I²C Custom PCBs              |

---

## 🧰 Tech Stack

- **Framework:** ESP-IDF  
- **Editor:** Visual Studio Code  
- **UI Library:** LVGL  
- **RTOS:** FreeRTOS

---

## 📦 General Software Requirements

- ✅ Reliable  
- ✅ Maintainable  
- ✅ OTA Update-Ready  
- ✅ Efficient  
- ✅ Well Documented

---

# Sequenzboard Firmware – Component Overview

This section outlines the core components and logic structure of the Sequenzboard firmware. It serves as a reference for implementing the interaction system and training modes.

### Hold Layout
```cpp
// Logical mapping A-Z for 26 holds
struct Hold {
    char id;         // e.g. 'A'
    int ledPos;
    int touchPos;
};
```

---

## 🧩 Touch Controller

**`TouchStripController` Class**

```cpp
touched(); // Returns currently touched holds (max. 2)
```

---

## 💡 LED Controller

**`LedStripController` Class**

```cpp
display(hold);
displayTouched(hold);
displaySuccess(hold);
displayLostBothTouchpoints(hold_1, hold_2);
displayVictoryAnimation();
displayWelcomeAnimation();
```

---

## 🎮 Interaction Controller

Handles user input via the touch sensors and triggers visual feedback via the LED strip.

---

## 🔁 Sequence Logic

A sequence is a list of ordered holds (e.g. A → G → N → P).

```cpp
Sequence sequence = generateSequence({A, G, N, P});
```

### Example Process:

1. `display(A)`
2. If `A` is touched → `displayTouched(A)`
3. If held ≥ 1s → `displaySuccess(A)` → show next
4. If contact lost → `displayLostTouch(A)`

---

## 🖥️ UI & Training Modes

### Menu Navigation

Users can select from:

- Structured Workouts  
- High-Score Challenges  
-  Multiplayer Game  

---

### 🧗 Workouts (JSON-based)

Each workout is a list of sequences:

```json
{
  "WorkoutTitle": "Core Blast",
  "Sequences": [
    ["A", "B", "D"],
    ["G", "N", "P", "O"]
  ]
}
```

---

### ⏱ Highscore Challenges

#### Memory Challenge  
How many sequences can you remember?  
**Today’s best:** 15

#### Reaction Time  
How fast can you react to a lit grip?  
**Today’s best:** 300 ms

#### Chain Sequences  
Like “I packed my suitcase” — repeat and extend sequences.  
**Current streak:** 35

---

### 🧑‍🤝‍🧑 Multiplayer Game: *"Packing List"*

- Players take turns adding a move to the sequence  
- Works similarly to memory games  
- System automatically detects player actions  
- Includes game modes:
  - `oneHandDyno`
  - `DoubleDyno`
  - `Normal`

---

## 🔄 Update Process

- Firmware must support **remote OTA updates**
- Future extension: sync workouts/challenges from a server

---

## 📎 Notes for Developer

- Use modular, reusable classes for hardware components  
- Avoid hard-coded logic in UI or training modes  
- If anything is unclear → **please ask!** 🙏

---

## 📐 UML Diagram

_To be added in the next update._

---

## 🤝 Collaboration

We’re happy to support any questions or discuss improvements.  
Let’s build a solid foundation for a great firmware experience.
