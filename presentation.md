# 🚀 Space Invaders - Project 3

---

## 👾 Project Overview

- **Project Name:** Space Invaders
- **Language & Framework:** C++
- **Graphics Library:** SDL2 (Simple DirectMedia Layer)
- **What It Does:** A classic 2D arcade-style shooter game. The player controls a spaceship, shoots incoming enemies, and tries to survive as long as possible.
- **Why This Project?**
  - Simple enough to understand thoroughly.
  - Demonstrates fundamental game design patterns.
  - Offers clean code structure with learning opportunities.

  ---

## 🎯 Project Objectives

- Analyze and document a functional C++ open-source game project.
- Understand SDL’s role in rendering, input handling, and collision detection.
- Learn the game’s internal structure and class-based design.
- Present technical depth with structured visuals and explanations.

  ---

## 🧠 Breadth-wise Understanding

### File & Module Structure

The project follows a modular file structure, organizing game components into separate scripts for better maintainability and clarity:

- `main.cpp`: Entry point of the game. Initializes everything.
- `player.cpp`: Manages the player spaceship class.
- `enemy.cpp`: Handles alien logic and movement.
- `bullet.cpp`: Bullet mechanics for player and enemy fire.
- `game.cpp`: Manages game loop, collision detection, score updates.
- `texturemanager.cpp` : Loads and renders textures.
- `assets `: Contains image files (PNG/JPG).
- `include/SDL2 `:SDL2 headers (external).

### Game Flow Overview

**1. Initialization (`main.cpp` & `game.cpp`)**
- Initializes SDL subsystems (`SDL_Init`, `IMG_Init`, `TTF_Init`).
- Creates an SDL Window and Renderer (`SDL_CreateWindow`, `SDL_CreateRenderer`).
- Loads textures using `IMG_LoadTexture()` through a custom `TextureManager` class.
- Initializes game entities: `Player`, multiple `Enemy` instances, and an empty bullet pool.

**2. Main Game Loop**
- The loop continues while `isRunning == true`.
- Each iteration performs the following in sequence:

a. Event Handling
- Captures keyboard input using SDL_PollEvent for real-time controls.
- Spacebar triggers a bullet spawn from the player’s position.

b. State Updates
- Player updates movement based on input flags.
- All bullets move upwards each frame.
- Enemies move downwards and may respawn if destroyed or offscreen.
- Game difficulty may scale based on score or enemy count.

c. Collision Detection
- Bullet-to-enemy intersection checked using `SDL_HasIntersection()`
- Ensures enemies are removed or respawned, bullets are deleted.

d. Rendering
- Clears the screen using SDL_RenderClear().
- Renders player, enemies, and bullets using SDL_RenderCopy().
- Displays score or game-over messages using SDL_ttf.

e. Frame Rate Control
 Uses SDL_Delay() to lock frame rate (typically ~16ms delay for 60 FPS).

3. Game Over Condition
- If an enemy reaches the player or bottom edge of the screen, the game ends.
- Game cleanup is triggered with SDL_DestroyRenderer, SDL_DestroyWindow, and SDL_Quit().


### Controls and Interaction

The game uses **keyboard input** captured via SDL’s event system. 
These controls are mapped in `main.cpp` and passed to the `Player` class for behavior.

| Key            | Action                                  |
|----------------|-----------------------------------------|
| ← Left Arrow   | Move player spaceship left              |
| → Right Arrow  | Move player spaceship right             |
| Spacebar       | Fire a bullet                           |
| Escape (ESC)   | Optional: Exit / pause (if implemented) |
| Window Close   | Quit game                               |


### Gameplay Elements

- **Player**: A spaceship that can move left and right and shoot upward
- **Enemies**: Aliens descending toward the player; random respawning
- **Bullets**: Fired by the player, travel upward and destroy enemies on contact
- **Game Over**: Occurs if an enemy collides with the player or reaches the bottom

---

## 🔍 Depth-Wise Analysis

This section explores the internal architecture and engineering choices behind the Space Invaders project. It focuses on the approaches taken during development, the data structures implemented, and key trade-offs considered by the original developer.

### 1️⃣ Approaches Taken

#### a. Object-Oriented Design (OOP)

The project is structured using **object-oriented principles**. Each core game entity is represented as a C++ class encapsulating its data and behavior:

- **`Player`**: Handles movement, screen boundaries, shooting bullets, and drawing itself.
- **`Enemy`**: Controls movement patterns, collision behavior, and respawning.
- **`Bullet`**: Manages bullet direction, velocity, rendering, and off-screen cleanup.

Benefits of OOP in this context:
- **Encapsulation**: Keeps functionality modular and isolated.
- **Reusability**: Enables use of similar logic across multiple objects.
- **Extensibility**: Easy to add new features like enemies, power-ups, or effects.

---

#### b. Main Game Loop (Real-Time Frame Control)

The game uses a traditional **infinite loop pattern**, which cycles through all essential steps of game execution per frame. This loop is located in `main.cpp`.

The loop manages:
- **Input processing** using SDL’s event system.
- **State updates** for game entities like enemies, player, and bullets.
- **Collision detection** between bullets and enemies.
- **Rendering** all textures to the screen.
- **Frame delay** using `SDL_Delay()` to maintain consistent FPS (~60).

```cpp
while (isRunning) {
    handleInput();
    updateEntities();
    checkCollisions();
    render();
    SDL_Delay(16); // ~60 FPS
}
```

#### c. Event-Driven Input Handling

Input handling is implemented using SDL’s event-driven model. Keyboard inputs are captured by polling SDL’s event queue with SDL_PollEvent.
cpp 
```
SDL_Event event;
while (SDL_PollEvent(&event)) {
    if (event.type == SDL_QUIT) {
        isRunning = false;
    }

    if (event.type == SDL_KEYDOWN) {
        if (event.key.keysym.sym == SDLK_LEFT) {
            player.moveLeft();
        }
        if (event.key.keysym.sym == SDLK_RIGHT) {
            player.moveRight();
        }
        if (event.key.keysym.sym == SDLK_SPACE) {
            player.shoot();
        }
    }
}
```
This approach allows for:
- Immediate reaction to user input
- Clean separation of input logic and entity behavior
- Easy mapping of keys to different actions

####  d. Manual Collision Detection 
Instead of relying on a third-party physics engine, the project uses manual collision detection with axis-aligned bounding boxes (AABB) using SDL's built-in function:
cpp
```
if (SDL_HasIntersection(&bullet.rect, &enemy.rect)) {
    // Collision occurred
}
```
Benefits:
- Lightweight and performant for 2D gameplay
- Easy to understand and debug
- Perfectly suited for rectangular objects like sprites
This technique ensures fast and accurate collision checks with minimal performance cost.

### 2️⃣ Data Structures Used

#### a. Classes (OOP Abstractions)
The core gameplay revolves around three main C++ classes, each encapsulating data and behavior:
- Player Class
  - Attributes:
    - `SDL_Rect rect`: Position and size of the spaceship
    - `SDL_Texture* texture`: Sprite used for rendering
    - `int speed`: Movement speed of the player
  - Methods:
    - `moveLeft()` and `moveRight()`: Updates position with boundary checks
    - `shoot()`: Spawns a bullet at player's location
    - `render(SDL_Renderer*)`: Draws player to screen

- Enemy Class
  - Attributes:
    - `SDL_Rect rect`: Position and size of the enemy
    - `SDL_Texture* texture`: Enemy sprite
    - `int speed`: Vertical descent rate
  - Methods:
    - `move()`: Moves the enemy downward
    - `respawn()`: Randomizes position and resets Y-axis
    - `render(SDL_Renderer*)`: Draws enemy to screen

- Bullet Class
  - Attributes:
    - `SDL_Rect rect`: Bullet position and size
    - `int velocity`: Speed at which it travels
    - `bool active`: Status of bullet (used/unused)
  - Methods:
    - `move()`: Updates Y-position each frame
    - `isOffScreen()`: Checks if bullet has exited screen
    - `render(SDL_Renderer*)`: Draws bullet if active
This abstraction allows:
- Cleaner code and single-responsibility logic per class
- Reusability (e.g., bullet logic could be reused for enemies)
- Easier future enhancements like adding power-ups or different enemies

#### b. Vectors (Collections of Game Entities)

The game uses **C++ STL containers** to manage dynamic lists of objects:

```cpp
std::vector<Enemy> enemies;
std::vector<Bullet> bullets;
```
- enemies: Stores all active enemy objects, updated and drawn every frame.
- bullets: Stores bullets fired by the player. Each bullet is updated and drawn independently.
- Inactive/expired bullets are removed after collision or going off-screen using erase-remove idiom or a boolean flag.

Benefits of std::vector:
- Dynamic memory allocation
- Easy iteration with range-based for loops
- Fast access by index

#### c. SDL Rectangles for Hitboxes

-  Each entity (Player, Enemy, Bullet) is represented by an SDL_Rect for its bounding box.
- These rectangles are used for both positioning and collision detection.
  ```
  if (SDL_HasIntersection(&bullet.rect, &enemy.rect)) {
    // Collision detected
  }
  ```
This method performs Axis-Aligned Bounding Box (AABB) checks, which are:
- Extremely fast
- Well-suited for sprite-based 2D games
- Sufficient for simple rectangular hitboxes

#### d. Assets: Images and Textures

Images are loaded into SDL_Texture* using the SDL_image library:
```cpp
SDL_Texture* playerTexture = IMG_LoadTexture(renderer, "assets/player.png");
```
Textures are stored as pointers and passed to the rendering context using:
```cpp
SDL_RenderCopy(renderer, texture, NULL, &rect);
```
- Asset loading is typically done once at the start of the game via helper functions (e.g., TextureManager::Load()).
- Memory Management: Textures must be cleaned up at the end using SDL_DestroyTexture() to prevent memory leaks.
- While this version of the game doesn't include sound, it could be easily integrated using SDL_mixer.

#### Analysis of all the classes
1. Player

| Element           | Description                                                                                |
|-------------------|--------------------------------------------------------------------------------------------|
| Purpose           | Manages the player spaceship — movement, firing bullets, and rendering                     |
| Attributes        | `SDL_Rect rect`, `int speed`, `SDL_Texture* texture`                                       |
| Methods           | `moveLeft()`, `moveRight()`, `shoot(std::vector<Bullet>& bullets)`, `render(SDL_Renderer*)`|
| Behavior          | Moves horizontally based on user input, can fire bullets from top-center                   |
| Input Handling    | Movement is triggered by arrow keys, shooting by spacebar                                  |

2. Enemy

| Element           | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| Purpose           | Represents enemy invaders falling from the top                              |
| Attributes        | `SDL_Rect rect`, `int speed`, `SDL_Texture* texture`, `bool active`         |
| Methods           | `moveDown()`, `respawn()`, `render(SDL_Renderer*)`, `deactivate()`          |
| Behavior          | Moves down the screen at a constant speed; respawns when destroyed          |
| Collision Logic   | If hit by a bullet, it's either reset or marked inactive                    |

3. Bullet

| Element           | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| Purpose           | Represents bullets fired by the player                                      |
| Attributes        | `SDL_Rect rect`, `int velocity`, `bool active`                              |
| Methods           | `move()`, `isOffScreen()`, `render(SDL_Renderer*)`, `deactivate()`          |
| Behavior          | Moves vertically upward and deactivates on collision or when off-screen     |
| Lifecycle         | Fired from the player's position and removed when no longer needed          |

4. TextureManager (Utility Class)

| Element           | Description                                                                            |
|-------------------|----------------------------------------------------------------------------------------|
| Purpose           | Simplifies loading and drawing textures using SDL2                                     |
| Methods           | `static SDL_Texture* LoadTexture(const char*, SDL_Renderer*)`, `static void Draw(...)` |
| Behavior          | Loads textures once, then renders them efficiently with `SDL_RenderCopy()`             |

## 📄 Header File Conventions

Even though each class has its own `.h` and `.cpp` file, the overall structure follows these SDL + C++ conventions:

| Header Element                    | Purpose                                                                      |
|----------------------------------|-------------------------------------------------------------------------------|
| `#include <SDL.h>`               | SDL core graphics and event handling                                          |
| `#include <SDL_image.h>`         | Image loading support (PNG, JPG, etc.)                                        |
| `#include <vector>`              | STL container used for managing bullets and enemies                           |
| `#include "Player.h"`            | Declares the Player class                                                     |
| `#include "Enemy.h"`             | Declares the Enemy class                                                      |
| `#include "Bullet.h"`            | Declares the Bullet class                                                     |
| `#include "TextureManager.h"`    | For loading and rendering textures                                            |

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











  
