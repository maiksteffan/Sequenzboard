Prototype: 
https://www.youtube.com/watch?v=Yh3gl3W50hQ

Communication
A 30 min Meeting every week where you can show the progress of the project. 
A shared Github Repo. 

*This gives a general Idea for the firmware structure.
If something is unclear, please don’t hesitate to ask :)
Hardware
Logic+Display: ESP32-P4-Module-DEV-KIT-C
Led-Strips: 2X WS2812
Touch-Sensors: 26X Custom PCBs (I2C BUS)

Techstack
Framework: ESP-IDF
Editor: VSCode
Libraries: LVGL, FreeRTOS

General Software Requirements
Reliable
Maintainable
Remote Update Management
Well Documented
Efficient

UML diagram

Hold Layout (Logic)
Holds( A-Z ) -> 26 Holds

Touch Sensors (User Input)
A comprehensive TouchStripController class with these obligatory methods:
touched() 
-> returns the holds, that are currently being touched (should be max. 2)


LED-Strip (Visual Feedback)
A comprehensive LedStripController class with these obligatory methods:
display(hold)
-> lights up the LedStrip at position “hold.ledPos”
displayTouched(hold) 
-> makes a small led animation around hold.pos, that indicates, the hold is being currently touched. (Expansion as long as someone has contact with the hold/ contraction, while someone has no contact)
displaySuccess(hold)
-> makes a small led animation around hold.pos, that indicates, the hold has been touched for 1 second. 
displayLostBothTouchpoints(hold_1, hold_2)
	(when someone steps of the board or falls down)
displayVictoryAnimation()
displayWelcomeAnimation()

InteractionController (Logic)
Manages the User Input via the Touch-Sensors and gives Visual Feedback via the LED-Strip

Sequence (Logic)
A Sequence consists of a List of holds with a predetermined order. 
For example:
Sequence a = generate Sequence(A,G,N,P,O,R) 
(Each letter represents a different hold from the Layout from A-Z)

Process: (Überarbeiten)
The Led-Strip lights up the led at the position of Hold “A”  -> display(hold)
When and as long as Hold “A” is being touched -> displayTouched(A) 
If Hold “A” has been touched for at least one second -> displaySuccess(A) -> display(G)
If hold “A” lost contact-> displayLostTouch(G)
If hold “A” has lost contact for at least 1 second -> displayFailedAnimation(A)


Display: (Visual Feedback + User Input)
A Menu that lets you choose from 3 different Modi’s:
Structured Workouts*
High-Score-Challenges*
Multiplayer Game* 

*Structured Workouts (Logic)
When you clicked this modi, you will see a list of predefined workouts (JSON Objects) to choose from.
A workout consists of multiple sequences that follow upon each other like levels. 
During the workouts you should see the total moves and your current move. 
Example JSON: 
{ WorkoutTitle: …., Sequences[]...}

*High-Score-Challenges (Logic)
How many grip sequences can you remember?
Today's best: 15
How fast is your reaction time when a new grip lights up?
Today's best: 300 ms
How many grip sequences can you complete in a row?
It works kind of like the game “I packed my suitcase”, except each player decides how many grips to add.
Current Sequence: 35 long

*Multiplayer-Game (Logic)
Scenario: Two friends duel by taking turns adding a new move to the sequence.
Like “I packed my suitcase” – the system automatically detects the different modes.
You decide how many grip sequences to add.
Different modes are -> oneHandDyno, DoubleDyno, Normal

