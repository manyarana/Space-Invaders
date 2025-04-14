# 🚀 Space Invaders - Open Source Project Presentation

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

### 📁 File & Module Structure

The project follows a modular file structure, organizing game components into separate scripts for better maintainability and clarity:

- `main.py`: Entry point of the game. Initializes everything.
- `player.py`: Manages the player spaceship class.
- `enemy.py`: Handles alien logic and movement.
- `bullet.py`: Bullet mechanics for player and enemy fire.
- `game.py`: Manages game loop, collision detection, score updates.
- `utils.py`: Utility functions like loading images and sound.


### 🔄 Game Flow Overview

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

### 🕹️ Controls and Interaction

- **Arrow Keys (← / →):** Move player horizontally
- **Spacebar:** Fire bullets
- **Close Button:** Exit game

Input is managed through `pygame.event.get()` inside the loop, checking for both keydown and quit events.


### 🎯 Gameplay Elements

- **Player**: A spaceship that can move left and right and shoot upward
- **Enemies**: Aliens descending toward the player; random respawning
- **Bullets**: Fired by the player, travel upward and destroy enemies on contact
- **Game Over**: Occurs if an enemy collides with the player or reaches the bottom

---

## 🔍 Depth-Wise Analysis

This section explores the internal architecture and engineering choices behind the Space Invaders project. It focuses on the approaches taken during development, the data structures implemented, and key trade-offs considered by the original developer.

---

### 1️⃣ Approaches Taken

#### 🔧 a. Object-Oriented Design (OOP)

The project is designed using **object-oriented programming**, a core software engineering principle. Each game entity is modeled as a class with attributes and behaviors:

- `Player`: Handles spaceship movement, firing, and screen bounds.
- `Enemy`: Controls alien movement logic and random repositioning.
- `Bullet`: Manages bullet movement and lifecycle.

This approach improves:
- Code readability
- Reusability
- Extensibility (easy to add power-ups, new enemies, etc.)

#### 🔁 b. Main Game Loop (Real-Time Frame Handling)

A central loop maintains a steady frame rate and updates all game objects every frame. This loop is responsible for:

- Processing user input
   `pygame.event.get()`
- Updating entity states
- Collision detection
- Rendering updated visuals
- Controlling frame rate via
   `pygame.time.Clock()`

This loop allows real-time game progression and is the **heart of the game**.

#### ⌨️ c. Event-Driven Input Handling

The project listens to system and keyboard events using Pygame’s event system:

```python
for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False
 ```
Movement is detected with `pygame.key.get_pressed()`, enabling smooth, continuous actions.

### 🎯 d. Manual Collision Detection

Rather than using a physics engine, the game relies on manual bounding box collision detection using `pygame.Rect.colliderect()`.

This method is:
- Lightweight and fast for simple 2D games
- Sufficient for rectangle-shaped hitboxes (like bullets/enemies)

### 2️⃣ Data Structures Used

#### 🧱 a. Classes (OOP Abstractions)

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

#### 📚 b. Lists (Collections of Game Entities)

- enemies[]: List storing active enemy instances
- bullets[]: List storing all fired bullets
- removed_bullets[]: Temporary list to avoid modifying lists during iteration

These structures allow easy iteration for movement updates and collision checks.

#### 📐 c. Pygame Rectangles for Hitboxes

- Each sprite is associated with a `pygame.Rect`
- Used in:
``` python
if bullet.rect.colliderect(enemy.rect):
    # Handle hit
```
- Enables fast and reliable overlap detection without external libraries

#### 🖼️ d. Assets: Images and Sounds

- Images: `pygame.image.load()`
- Sounds: `pygame.mixer.Sound()`
- Managed through utility functions (`utils.py`)
- Loaded once during initialization
- Stored in variables and reused to avoid repeated file I/O

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

## 💡 Key Learnings

- Understood the game loop architecture in real-world Pygame projects.
- Gained insight into class-based design for game elements.
- Learned how Pygame handles input, sound, image rendering, and FPS control.
- Improved ability to read, analyze, and explain existing code.

---

## 👨‍💻 Credits
 
 - Original Author: 
 - Investigated by:
   - Manya Rana (202401115)
   - Mrunali Parmar (202401122)
   - Krishna Parmar (202401100)
   - Krisha Bhuva (202401099)

---











  
