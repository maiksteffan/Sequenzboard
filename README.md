## 🌟 Firmware Development Briefing – *Sequenzboard*


### ⚙️ Hardware Overview

| Component        | Description                            |
|------------------|----------------------------------------|
| MCU              | ESP32-P4-Module-DEV-KIT-C              |
| LED Strips       | 2 × WS2812                             |
| Touch Sensors    | 26 × Custom PCBs (connected via I²C)   |

---

### 🧰 Tech Stack

- **Framework**: ESP-IDF  
- **Editor**: VS Code  
- **Core Libraries**:  
  - LVGL (UI)  
  - FreeRTOS (Multitasking)  

---

### 🧠 General Firmware Requirements

- Robust and stable in real-world use
- Modular and maintainable structure
- OTA support (Remote firmware updates)
- Clean codebase with documentation
- Performance-efficient (touch latency, LED feedback)

---

### 🧱 Logical Structure Overview (UML-style)

#### 🧹 Hold Model
```cpp
struct Hold {
  char id;         // 'A' - 'Z'
  int ledPos;      // Physical LED position
  int touchPos;    // Physical sensor position
};
```

---

### 🖛 TouchStripController

Handles input from 26 I²C touch sensors.

#### Required Methods:
```cpp
std::vector<Hold> touched(); 
// Returns currently touched holds (max. 2 at once)
```

---

### 💡 LedStripController

Controls WS2812 visual feedback.

#### Required Methods:
```cpp
void display(const Hold& hold);               
// Lights up LED at hold.ledPos

void displayTouched(const Hold& hold);        
// Animation: Expand while touched, contract when released

void displaySuccess(const Hold& hold);        
// Animation for hold touched ≥ 1s

void displayLostBothTouchpoints(const Hold& h1, const Hold& h2);
// Called when user leaves both grips (fall)

void displayVictoryAnimation();               
void displayWelcomeAnimation();               
```

---

### 🤖 InteractionController

Coordinates user interaction:
- Reads `TouchStripController`
- Controls `LedStripController`
- Updates `DisplayController`

---

### 🔁 Sequence Logic

A sequence = ordered list of holds, e.g.
```cpp
Sequence s = {"A", "G", "N", "P", "O", "R"};
```

#### Flow:
1. `display(A)` lights up hold A  
2. While hold A is touched: `displayTouched(A)`  
3. If touched for ≥1s → `displaySuccess(A)` → `display(G)`  
4. If contact breaks: `displayLostTouch(A)`  
5. If broken for ≥1s → `displayFailedAnimation(A)`  

---

### 💻 Display Logic (via LVGL)

#### Main Menu
- Structured Workouts
- High-Score Challenges
- Multiplayer Mode

---

### 🧘 Structured Workouts

- Select from predefined workouts (via JSON)
- Each workout contains sequential sequences
- Show total + current move
```json
{
  "WorkoutTitle": "Warm-Up Routine",
  "Sequences": [["A", "B"], ["C", "E", "F"], ["G", "H", "I"]]
}
```

---

### 🧠 High-Score Challenges

- *Memory Mode*: How many sequences can you remember?
- *Reaction Mode*: How fast do you respond to the next light-up?
- *Streak Mode*: Longest successful sequence in a row

---

### 👫 Multiplayer Mode

- Two players alternate turns
- Add one move each round (“I packed my suitcase” style)
- Detects modes like:
  - `normal`
  - `doubleDyno`
  - `oneHandDyno`

---

Feel free to reach out with questions, suggestions, or requests for further clarifications — we're happy to iterate together!

