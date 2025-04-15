# 🚀 Space Invaders - Project 3

---

## 👾 Project Overview

- **Project Name:** Space Invaders
- **Language & Framework:** Python + Pygame
- **What It Does:** A classic 2D arcade-style shooter game. The player controls a spaceship, shoots incoming enemies, and tries to survive as long as possible.
- **Why This Project?**
  - Simple enough to understand thoroughly.
  - Demonstrates fundamental game design patterns.
  - Offers clean code structure with learning opportunities.

  ---

## 🎯 Project Objectives

- Understand and document the internal working of a small open-source game.
- Explore how Pygame handles rendering, input, collisions, and game loops.
- Gain experience reading and analyzing real-world Python code.
- Prepare a technical presentation covering both breadth and depth of the project.

  ---

## 🧠 Breadth-wise Understanding

### File & Module Structure

The project follows a modular file structure, organizing game components into separate scripts for better maintainability and clarity:

- `main.py`: Entry point of the game. Initializes everything.
- `player.py`: Manages the player spaceship class.
- `enemy.py`: Handles alien logic and movement.
- `bullet.py`: Bullet mechanics for player and enemy fire.
- `game.py`: Manages game loop, collision detection, score updates.
- `utils.py`: Utility functions like loading images and sound.


### Game Flow Overview

The gameplay is structured around a **central game loop** running at a fixed frame rate. The steps are:

1. **Initialization**
   - Set up screen, FPS, fonts, and load images/sounds
   - Create initial objects (player, enemies)

2. **Game Loop (Repeats Every Frame)**
   - **Input Handling**: Arrow keys and spacebar for movement and shooting
   - **Game State Update**: Move bullets and enemies, spawn enemies if needed
   - **Collision Detection**: Check bullet hits and game-over scenarios
   - **Rendering**: Clear screen and draw all game elements
   - **Frame Control**: Delay next frame to maintain smooth performance

3. **Exit Handling**
   - Triggered by window close or collision end state

### Controls and Interaction

- **Arrow Keys (← / →):** Move player horizontally
- **Spacebar:** Fire bullets
- **Close Button:** Exit game

Input is managed through `pygame.event.get()` inside the loop, checking for both keydown and quit events.


### Gameplay Elements

- **Player**: A spaceship that can move left and right and shoot upward
- **Enemies**: Aliens descending toward the player; random respawning
- **Bullets**: Fired by the player, travel upward and destroy enemies on contact
- **Game Over**: Occurs if an enemy collides with the player or reaches the bottom

---

## 🔍 Depth-Wise Analysis

This section explores the internal architecture and engineering choices behind the Space Invaders project. It focuses on the approaches taken during development, the data structures implemented, and key trade-offs considered by the original developer.

---

### 1️⃣ Approaches Taken

####  a. Object-Oriented Design (OOP)

The project is designed using **object-oriented programming**, a core software engineering principle. Each game entity is modeled as a class with attributes and behaviors:

- `Player`: Handles spaceship movement, firing, and screen bounds.
- `Enemy`: Controls alien movement logic and random repositioning.
- `Bullet`: Manages bullet movement and lifecycle.

This approach improves:
- Code readability
- Reusability
- Extensibility (easy to add power-ups, new enemies, etc.)

#### b. Main Game Loop (Real-Time Frame Handling)

A central loop maintains a steady frame rate and updates all game objects every frame. This loop is responsible for:

- Processing user input
   `pygame.event.get()`
- Updating entity states
- Collision detection
- Rendering updated visuals
- Controlling frame rate via
   `pygame.time.Clock()`

This loop allows real-time game progression and is the **heart of the game**.

#### c. Event-Driven Input Handling

The project listens to system and keyboard events using Pygame’s event system:

```python
for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False
 ```
Movement is detected with `pygame.key.get_pressed()`, enabling smooth, continuous actions.

### d. Manual Collision Detection

Rather than using a physics engine, the game relies on manual bounding box collision detection using `pygame.Rect.colliderect()`.

This method is:
- Lightweight and fast for simple 2D games
- Sufficient for rectangle-shaped hitboxes (like bullets/enemies)

### 2️⃣ Data Structures Used

#### a. Classes (OOP Abstractions)

Encapsulated into three main game object types:

-> Player Class
  - Attributes: position, image, speed
  - Methods: move_left(), move_right(), shoot()
-> Enemy Class
  - Attributes: position, image, direction
  - Methods: move(), respawn()
-> Bullet Class
  - Attributes: position, velocity, source (player/enemy)
  - Method: update(), check_bounds()

This abstraction simplifies managing multiple instances and separates responsibilities.

#### b. Lists (Collections of Game Entities)

- enemies[]: List storing active enemy instances
- bullets[]: List storing all fired bullets
- removed_bullets[]: Temporary list to avoid modifying lists during iteration

These structures allow easy iteration for movement updates and collision checks.

#### c. Pygame Rectangles for Hitboxes

- Each sprite is associated with a `pygame.Rect`
- Used in:
``` python
if bullet.rect.colliderect(enemy.rect):
    # Handle hit
```
- Enables fast and reliable overlap detection without external libraries

#### d. Assets: Images and Sounds

- Images: `pygame.image.load()`
- Sounds: `pygame.mixer.Sound()`
- Managed through utility functions (`utils.py`)
- Loaded once during initialization
- Stored in variables and reused to avoid repeated file I/O

#### Analysis of all the classes
1.Character 
| Element         | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| Inherits From   | `Sprite` (SFML class for 2D graphics)                                       |
| Purpose         | Wrapper over `Sprite` to simplify creating game characters                  |
| Constructor     | Takes `position`, `texture`, and `scale` as input                           |
| Initialization  | Calls `Sprite(texture)` to initialize the base class                        |
| setPosition     | Sets the character's position on the screen                                 |
| setScale        | Sets the character's size by scaling the sprite                             |

2.Enemy
| Element        | Description                                                                  |
|----------------|----------------------------------------------------------------------------- |
| Inherits From  | `Character`                                                                  |
| Purpose        | Represents an enemy in the game                                              |
| Constructor    | Initializes the enemy using position, texture, and scale; sets `alive = true`|
| `isAlive()`    | Returns the current alive status (`true` or `false`)                         |
| `kill()`       | Marks the enemy as dead (`alive = false`)                                    |
| `reset()`      | Resets the enemy to alive state (`alive = true`)                             |
| Private Field  | `bool alive` — tracks if the enemy is active                                 |

3.Menu
| Element            | Description                                                                     |
|--------------------|---------------------------------------------------------------------------------|
| Purpose            | Displays the main menu and handles user selection                               |
| Constructor        | Initializes `userChoice` (default = 0)                                          |
| `displayMenu()`    | Renders game instructions and options; updates score, lives, and game state     |
| `getUserChoice()`  | Returns the current user menu selection                                         |
| `setUserChoice()`  | Sets a new menu selection value                                                 |
| Private Field      | `int userChoice` — stores the user's selected option (1: Play, 2: Test, 3: Exit)|

4.Missile
| Element        | Description                                                         |
|----------------|---------------------------------------------------------------------|
| Inherits From  | `Character`                                                         |
| Purpose        | Represents a missile (projectile) in the game                       |
| Constructor    | Initializes missile using position, texture, and scale              |
| Behavior       | Currently inherits all behavior from `Character` (no new methods)   |

5.User
| Element        | Description                                                         |
|----------------|---------------------------------------------------------------------|
| Inherits From  | `Character`                                                         |
| Purpose        | Represents the player (user-controlled character)                   |
| Constructor    | Initializes the user using position, texture, and scale             |
| Behavior       | Inherits all behavior from `Character`; no additional methods yet   |

#### Header File Explanation (`header.hpp`)

| Element                        | Description                                                                |
|--------------------------------|----------------------------------------------------------------------------|
| `#include <iostream>`          | For basic I/O operations (`cout`, `endl`)                                  |
| `#include <string>`            | For using `std::string`                                                    |
| `#include <vector>`            | For using `std::vector`                                                    |
| `#include <time.h>`            | For timing functions (e.g., random seed setup)                             |
| `#include <SFML/Graphics.hpp>` | Imports SFML graphics module (sprites, textures, windows)                  |
| `#include <SFML/Audio.hpp>`    | Imports SFML audio module (sound effects, music)                           |
| `using namespace sf`           | Allows direct use of SFML types like `RenderWindow`, `Texture`, etc.       |
| `using std::...`               | Allows direct use of common STL items (`cout`, `vector`, etc.)             |

#### Main Game Loop

| Element                  | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `RenderWindow`           | Creates a 950x950 game window                                               |
| `Textures`               | Loads images for ship, alien, missiles, and shields                         |
| `Music` & `SoundBuffer`  | Loads background music and sound effects (`shoot`, `kill`, `hit`)           |
| `Text`                   | Displays score, lives, level, and messages using `SFML::Text`               |
| `Fonts`                  | Loads "font.ttf" for text rendering                                         |


| Entity        | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| `ship`        | The player-controlled ship (a `User`, subclass of `Character`)              |
| `enemies`     | 10x5 grid of alien enemies (`Enemy`, subclass of `Character`)               |
| `missiles`    | Stores player-fired missiles (`Missile`)                                    |
| `enemyM`      | Stores enemy-fired missiles                                                 |
| `shields`     | 3 shield sprites with 4-stage damage textures                               |


| Section                   | Description                                                                |
|---------------------------|----------------------------------------------------------------------------|
| `Menu`                    | Displays instructions and gets user input (Play / Test / Exit)             |
| `ship_movement()`         | Moves the ship left/right with A/D or arrow keys                           |
| `missile_movement()`      | Moves user missiles up and enemy missiles down                             |
| `draw_missiles()`         | Renders missiles to the window                                             |
| `Enemy Movement`          | Enemies move side to side, descend when hitting screen edge                |
| `Collision Detection`     | Handles missile-enemy, missile-shield, and missile-ship interactions       |
| `Level Progression`       | Increases level when all enemies are defeated                              |
| `Game Over`               | Triggers when lives reach 0 or enemies reach the bottom                    |


| Test Case                 | Description                                                                 |
|---------------------------|-----------------------------------------------------------------------------|
| Test 1 - Missile Deletion | Checks if missiles are correctly removed when leaving screen                |
| Test 2 - Ship Hit         | Verifies life loss when enemy missile hits the player                       |
| Test 3 - Shield Damage    | Confirms shield takes damage in stages when hit                             |
| Test 4 - Enemy Movement   | Displays and tests enemy movement across screen                             |
| Test 5 - Enemy Shooting   | Ensures random enemies fire projectiles correctly                           |

### 3️⃣ Trade-offs Made

| Trade-off                    | Benefits                                | Limitations                                     |
|------------------------------|-----------------------------------------|-------------------------------------------------|
| Flat structure               | Easy to follow and ideal for learning   | Less scalable for large games                   |
| Manual collision detection   | Fast and sufficient for 2D games        | Not precise for complex shapes                  |
| Hardcoded parameters         | Quick prototyping                       | Low flexibility; harder to balance gameplay     |
| No scene manager             | Simplifies main loop                    | Can't switch between game states                |
| Single-player only           | Focused logic, minimal complexity       | No support for multiplayer or AI behavior       |
| One-direction bullet flow    | Avoids logic duplication                | No enemy bullets or special attacks             |

---
## Demo Video

---

## 💡 Key Learnings

- Understood the game loop architecture in real-world Pygame projects.
- Gained insight into class-based design for game elements.
- Learned how Pygame handles input, sound, image rendering, and FPS control.
- Improved ability to read, analyze, and explain existing code.

---

## 👨‍💻 Credits
 
 - Original Author: Subham, Ryan, Danielle, Manjesh   
 - Investigated by:
   - Manya Rana (202401115)
   - Mrunali Parmar (202401122)
   - Krishna Parmar (202401100)
   - Krisha Bhuva (202401099)

---











  
