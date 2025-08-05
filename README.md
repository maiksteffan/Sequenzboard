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
## Sequences 
Sequences are the main Part of every program.  
They consist of a List of Holds, seperated by a comma, that need to be touched in a consecutive order. 
For example "A,R,T,E,V,Y".
If two Holds are not seperated by a comma, they should be touched simoulatiously within a tolerance of 500ms. 
For example "AR,TE,VY"
The two different Touch-LED Interactions can also be combined like this:
In this example you only touch the last two holds simoulatiously: "A,R,T,E,VY"

Try out how Sequeces are interpreted from a Sequence-String:
https://editor.p5js.org/maikstf/full/QCS-UwFKY
(The hands in the simoulation show, which touches are expected. If the expected touches differ from the actual touches, it is detected as "stepping of".)

Try those examples to see how the algorithm should work:
- Sequence("A,R,T,E,V,Y")
- Sequence("AR,TE,VY")

In case a Sequence has been completed sucessfully, a victory animation should be played on the led strips. 

## Workouts
- Local Storage for Workouts as JSON objects
- Workout Objects consist of:
  - A List of Sequences (Levels)
  - A Animation.GIF, that displays the Movement instructions
  - Additional Gif-Info
  - A Category that describes the Workout
  - Duration of the Workout in minutes
  - Difficulty of the Workout
  - Amount of Moves (calculated from List of Sequences)
- When in Workout view:
  - The current hand is animated. 
  - The current Move and Level is displayed.
  - When completing a Sequence, the level is marked as success(green) and you get to the next Level.
  - When stepping down before the Sequence is done, the Level is marked as failed(red) and you move on to the next Level. 
## Highscore Challenges
- Local Storage for different Sequences for the Reaction Highscore Challenge with a length of 5
- Local Storage for different Sequences for the Speed Highscore Challenge with a length of 10
- Local Storage for a Sequence for the Endurance Highscore Challenge with a length of 1000, that always starts at a different index
- The current Highscore for every challenge gets reset everyday at 0.00 o'clock
  ### Speed Highscore
  - measures how long you take to complete the Sequence
  - The time someone needs to complete is the Speed Highscore
  ## Endurance Highscore
  - measures how many moves you can climb without stepping down.
  - The amount of moves makes the highscore.
  ### Reaction Highscore
  - the holds appear with a random delay
  - the reaction time between the appearing and the touching of a hold is measured
  - the sum of the reaction time is the Reaction Highscore
## Creative Mode
- as soon as you start climbing, the moves are being detected and added to your sequence. This counts up the record-counter. 
- As soon as you step down, you can repeat your recoreded Sequence as many times as you want.
- When clicking the restart button, you can record a new Sequence
## Climbing Game (Kofferpacken)
- after selecting the amount of players and pressing play, the game starts
- the first player has to touch the first two holds
- the next player has to repeat touching the first two holds and add the next two touches....(and so on)
- each player can decide, whether he wants to make a single move or wants to move with both hands simoultaniously.
- If you can't repeat the current Sequence and step down, you lose a life.
- The Game is over, when there is only one remeining player. 



