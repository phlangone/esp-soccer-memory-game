# Memory Game: Football Teams with Multiple OLED Displays

## Project Description

This project implements a **physical interactive memory game**, developed in **C/C++ for microcontrollers**. The system uses **8 individual OLED displays** to show football team emblems from several international tournaments.

The player selects a tournament, memorizes the position of the emblems temporarily displayed on the 8 screens, and then uses dedicated **physical buttons** to try to find the matching pairs. The game includes **audio feedback (buzzer)** for correct and incorrect guesses, as well as a **victory melody** when the game is completed.

**Author:** Paulo H. Langone – SENAI Anchieta (Industrial Electronics Technology Program – EXTUN4)

---

## Features

* **Tournament Selection Menu:** 8 league options (Brasileirão, Bundesliga, World Cup, La Liga, MLS, Premier League, Serie A, UAFA).
* **Random Shuffling:** Algorithm that ensures team positions change in every new round.
* **Display Multiplexing:** Control of 8 I2C displays with the same address (0x3C) using **hardware selection via a multiplexer/demultiplexer on the SDA/SCL bus**.
* **Audiovisual Feedback:** Distinct sound signals for correct and incorrect moves, plus animation and music when the game is completed.

---

## Required Hardware

* 1× Microcontroller (e.g., **ESP32**)
* 8× **SSD1306 OLED Displays** (128×64 pixels, I2C interface)
* 8× Push-buttons
* 1× Buzzer
* 1× Multiplexer (e.g., **CD74HC4051** or similar) to manage the I2C communication between displays.

---

### Pin Mapping (Pinout)

| Component | Microcontroller Pin |
|-----------|--------------------|
| **Buzzer** | GPIO 16 |
| **Button 0** | GPIO 13 |
| **Button 1** | GPIO 12 |
| **Button 2** | GPIO 14 |
| **Button 3** | GPIO 27 |
| **Button 4** | GPIO 26 |
| **Button 5** | GPIO 25 |
| **Button 6** | GPIO 33 |
| **Button 7** | GPIO 32 |
| **Mux Select A** | GPIO 15 |
| **Mux Select B** | GPIO 2 |
| **Mux Select C** | GPIO 5 |

---

## Dependencies and Libraries

To compile and run this project, the following libraries must be installed in your IDE:

* `Wire.h` (built-in)
* `Adafruit_GFX.h`
* `Adafruit_SSD1306.h`

**Note:** The project depends on a local file named `teams.h`.  
This file must contain the **byte arrays (hexadecimal bitmaps)** used to render the team and tournament logos on the OLED displays.

---

## System Logic

1. **Initialization**  
   Pins are configured and all displays are initialized by scanning through the multiplexer.

2. **Tournament Selection**  
   Each display shows a tournament logo. The player presses the button corresponding to the desired tournament.

3. **Memorization Phase**  
   The system randomly assigns positions, shuffles the teams of the selected tournament, and displays them for **2 seconds (`TEMPO_EXIBICAO`)**.

4. **Game Loop**  
   All displays are cleared. The system waits for the player to press two buttons.

   * **Match:** The corresponding displays remain revealed and a success sound is played.
   * **Mismatch:** An error sound is played and the displays are cleared again.

5. **Game Completion**  
   When all **4 pairs** are found, a celebration image is displayed simultaneously on all **8 screens**, accompanied by a programmed melody.
