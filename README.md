# Sequenzboard Firmware Repository

Welcome! This repository contains the firmware development for the **Sequenzboard** – an interactive, LED-guided training board inspired by climbing movement.  
This document outlines the system architecture, milestones, and expectations to help guide the firmware implementation process.

---

## 🎯 Project Goal

Develop a modular, reliable firmware stack for the Sequenzboard that enables responsive training interactions, visual feedback, and remote updates.

---

## Overview

### 1. Helper Methods (Hardware Abstraction)
Provide foundational helper methods for implementing training program logic. These methods abstract the control of:

- **LED Strips** – Visual Feedback (WS2812)
- **Touch Sensors** – User Input (26 I²C-capable custom PCBs)
- **Display** – User Interface + Touch Input (LVGL-based)

These methods will form the backbone of the training program logic and allow for scalable implementation of training types.

---

### 2. Navigable UI
- Users can browse and select from different training modes
- Upon selection, the training starts automatically 

---

### 3. Training Programm Logic
The User can choose from The following Training-Modes: 
(The different Modes will be explained later)
- **Guided Workouts**
- **High Score Challenge modes**
- **Multiplayer Game**
  
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
| **MCU**          | ESP32-P4-Module-DEV-KIT-B        |
| **Display**      | Capacitive Touchscreen           |
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

# Programms to implement

## Workouts
- Local Storage for Workouts as JSON objects
- Workouts consist of: ...
- When in Workout view 

## Highscore Challenges
- Local Storage for different Sequences, that change for every attempt
### Speed Challenge
- measures how long you take to complete the Sequence
### Reaction Challenge
- measures the time between the time the 

## Creative Mode

## Climbing Game


