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
- Users can browse and select from different training modes
- Upon selection, the training starts automatically (starts a placeholder program at this stage)

---

### 3. Training Programm Logic
The User can choose from The following Training-Modes: 
(The different Modes will be explained later)
- **Guided Workouts**
- 3 **High Score Challenge modes**
- 1 **Multiplayer Game**
  
Those Modes will use similar Methods from the controller classes but orchestrate them differntly. 
Once a training session is running, real-time interaction between the following components is orchestrated:
- LED strips → visual instruction & feedback  
- Touch sensors → user input  
- Display → progress indication + additional input

**Important: The different interactions shouldn't block each other. 
For example, while listening to touch inputs from the grips, the user should still be able to use the touchdisplay**

---

### 4. Remote Updates

The system must support **OTA (Over-the-Air)** updates, allowing firmware, content, and UI enhancements to be remotely delivered.

---

## 🛠️ Hardware Overview

| Component         | Details                          |
|------------------|----------------------------------|
| **MCU**          | ESP32-P4-Module-DEV-KIT-C        |
| **Display**      | Capacitive Touchscreen    |
| **LED Strips**   | 1x WS2812                        |
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
displayHold();
displayTouched();
displaySuccess();
displayWelcomeAnimation();
displayVictoryAnimation();
```
---

## 🔁 Sequence Logic

A sequence is a list of ordered holds (e.g. A → G → N → P).

### Example Process:

1. `display(A)`
2. while `A` is touched → `displayTouched(A)`
3. If held ≥ 1s → `displaySuccess(A)` → show next
4. If contact lost → `displayFailed()

---

## 🖥️ Training Modes

### 🧗Structured Workouts (Workouts are stored as JSON objects)

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
How many consecutive Moves can you remember?  
**Today’s best:** 15

#### Reaction Time  
How fast can you react to a lit grip?  
**Today’s best:** 300 ms

#### Chain Sequences  
How many consecutive moves can you do without stepping off?
**Current streak:** 35

---

### 🧑‍🤝‍🧑 Multiplayer Game: *"Packing List"*

- Players take turns adding a move to the sequence  
- System automatically detects player actions:
  - `oneHandDyno`
  - `DoubleDyno`
  - `Normal Move`

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
