# 🚀 Sequenzboard Firmware Repository

Welcome! This repository contains the firmware development for the **Sequenzboard** – an interactive, LED-guided training board inspired by climbing movements.

Watch the prototype in action: 
---

## 🎯 Project Goal

Develop a modular, reliable firmware stack for the Sequenzboard enabling responsive interactions, visual feedback, and OTA (Over-the-Air) remote updates.

---

## 📌 Key Components

### 1. Hardware Abstraction Layer

Implement foundational helper methods abstracting control of:

* **LED Strips:** WS2812 (Visual Feedback)
* **Touch Sensors:** 26x I²C custom PCBs (User Input)
* **Display:** LVGL-based capacitive touchscreen (UI & Touch Input)

These methods serve as building blocks for all training logic.

---

### 2. Navigable User Interface

* Users select training modes through an intuitive UI.
* Training begins automatically after selection.

**UI Design & Specifications:** [Interactive UI Prototype](https://msv8v1.axshare.com)

---

### 3. Training Program Logic

The firmware supports multiple training modes, each orchestrating LED, sensor, and display interactions:

* **Guided Workouts**
* **High Score Challenges**
* **Multiplayer Game**

**Important:** All interactions must be non-blocking—e.g., touch display input and grip sensor inputs operate concurrently.

---

### 4. Remote OTA Updates

Firmware, content, and UI enhancements must support secure and efficient OTA updates.

---

## 🛠️ Hardware Overview

| Component         | Specification                 |
| ----------------- | ----------------------------- |
| **MCU**           | ESP32-P4-Module-DEV-KIT-B     |
| **Display**       | Capacitive Touchscreen (LVGL) |
| **LED Strips**    | WS2812 (2 strips)              |
| **Touch Sensors** | 25x I²C custom PCBs           |

---

## 🧰 Tech Stack

* **Framework:** ESP-IDF
* **RTOS:** FreeRTOS
* **UI Library:** LVGL
* **Development Environment:** Visual Studio Code

---

## 📦 General Software Requirements

* ✅ Reliable
* ✅ Maintainable
* ✅ OTA Update-Ready
* ✅ Efficient
* ✅ Well Documented

---

## 🔄 Core Interaction: Sequences

Sequences define interactions for all training modes:

* Holds separated by commas must be touched sequentially. Example: `"A,R,T,E,V,Y"`
* Holds without commas indicate simultaneous touches within 500 ms tolerance. Example: `"AR,TE,VY"`
* Combination interactions possible. Example: `"A,R,T,E,VY"`

**Interactive Sequence Demo:** [Sequence Simulator](https://editor.p5js.org/maikstf/full/QCS-UwFKY)

Example sequences to try:

* `Sequence("A,R,T,E,V,Y")`
* `Sequence("AR,TE,VY")`
* `Sequence("A,R,T,E,VY")`

Upon successful completion, trigger LED strip victory animations.

---

## 🚩 Training Modes & Implementation

### 🏋️ Guided Workouts

* Stored locally as JSON:

  * List of sequences (levels)
  * Instructional GIF + additional info
  * Category, duration, difficulty, move count
* Real-time UI:

  * Animated hand indicators for next move (alternating hands)
  * Current move and level displayed
  * Success (green) or failure (red) feedback per sequence

### 🏅 High Score Challenges

**Daily Reset at 00:00:**

* **Reaction Challenge:**

  * Random delay between holds
  * Total reaction time recorded

* **Speed Challenge:**

  * Complete a 10-move sequence quickly
  * Fastest time recorded

* **Endurance Challenge:**

  * 1000-move sequence starting at random index
  * Counts consecutive moves without errors

**Highscore feedback:**

* Victory animations on LED strip and display
* User enters name for leaderboard

### 🎨 Creative Mode

* Records user-generated sequences automatically
* Replay and restart functionality

### 🎮 Multiplayer Climbing Game ("Kofferpacken")

* Players sequentially build sequences by repeating previous moves and adding new ones
* Single or simultaneous dual-hand moves allowed
* Losing conditions: failing to replicate sequence
* Game continues until one player remains

---
