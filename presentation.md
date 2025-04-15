# 🚀 Space Invaders - Project 3

---

## 👾 Project Overview

- **Project Name:** Space Invaders
- **Language & Framework:** C++
- **Graphics Library:** SFML (Simple and Fast Multimedia Library)
- **What It Does:** A classic 2D arcade-style shooter game. The player controls a spaceship, shoots incoming enemies, and tries to survive as long as possible.
- **Why This Project?**
  - Simple enough to understand thoroughly.
  - Demonstrates fundamental game design patterns.
  - Offers clean code structure with learning opportunities.

  ---

## 🎯 Project Objectives

- Analyze and document a functional C++ open-source game project.
- Understand SFML's use for rendering, input, audio, and game state management.
- Learn the game’s internal structure and class-based design.
- Present technical depth with structured visuals and explanations.

  ---

## 🧠 Breadth-wise Understanding

### File & Module Structure

The project follows a modular file structure, organizing game components into separate scripts for better maintainability and clarity:

- `main.cpp`: Entry point of the game. Initializes everything.
- `player.cpp / .h`: Manages the player spaceship class.
- `enemy.cpp / .h`: Handles alien logic and movement.
- `missile.cpp / .h`: Bullet mechanics for player and enemy fire.
- `game.cpp / .h`: Manages game loop, collision detection, score updates.
- `Shield.cpp / .h` : Manages destructible shields.


### Game Flow Overview

1. Initialization
- SFML Window created using `sf::RenderWindow`.
- Textures, fonts, sounds are loaded once using AssetManager.
- Entities initialized: Player, enemy grid, shields, etc.

2. Main Game Loop
``` cpp
while (window.isOpen()) {
    handleInput();
    update();
    detectCollisions();
    render();
}
```
a. Event Handling
- Polls SFML events using ```window.pollEvent()```.
- Keyboard inputs manage ship movement and missile firing.

b. State Updates
- Player updates movement based on input flags.
- All bullets move upwards each frame.
- Enemies move downwards and may respawn if destroyed or offscreen.
- Game difficulty may scale based on score or enemy count.
- Ensures enemies are removed or respawned, bullets are deleted.

d. Rendering
- ```window.clear()``` followed by drawing all game entities.
- UI: score, level, lives drawn using ```sf::Text```.
- ```window.display()``` to update screen.

3. Game Over Condition
If an enemy reaches the player or bottom edge of the screen, the game ends.  

### Controls and Interaction

| Key            | Action                                  |
|----------------|-----------------------------------------|
| ← Left Arrow   | Move player spaceship left              |
| → Right Arrow  | Move player spaceship right             |
| Spacebar       | Fire a bullet                           |
| Escape (ESC)   | Optional: Exit / pause (if implemented) |


### Gameplay Elements

- **Player:** Controlled by user. Can move and shoot.
- **Enemies:** Appear in a 10x5 grid, descend over time.
- **Missiles:** Fired by player and enemies.
- **Shields:** Block incoming fire; take damage in 4 stages.
- **UI Elements:** Score, lives, and level shown with sf::Text.

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

The game follows a continuous real-time loop pattern located in main.cpp, responsible for the full lifecycle of each frame.

Loop Responsibilities:
- RenderWindow: A 950x950 `sf::RenderWindow` that handles display and input events.
- Texture Management: Textures for the player ship, enemies, missiles, and shields are loaded 
 and managed using `sf::Texture` and `sf::Sprite`.
- Text Rendering: Game UI such as score, lives, and level are displayed using `sf::Text` objects 
and a loaded `sf::Font` (e.g., `font.ttf`).
- Sound Management:
  - `sf::SoundBuffer` for sound effects (shoot, hit, kill).
  - `sf::Sound` plays effects in response to collisions and user actions.
  - Optional background music via `sf::Music`.

#### c. Event-Driven Input Handling

Input handling is implemented using SFML's own event system with `sf::Event`. Keyboard events are captured and used to move the player ship or trigger actions like firing missiles.

cpp 
```
sf::Event event;
while (window.pollEvent(event)) {
    if (event.type == sf::Event::Closed)
        window.close();

    if (event.type == sf::Event::KeyPressed) {
        if (event.key.code == sf::Keyboard::Left)
            player.moveLeft();
        if (event.key.code == sf::Keyboard::Right)
            player.moveRight();
        if (event.key.code == sf::Keyboard::Space)
            player.shoot();
    }
}
```
This approach allows for:
- Immediate and responsive interaction with the user.
- Clean separation between input processing and entity behavior.
- Easily extendable to include new actions (e.g., pause menu with Escape).

####  d. Manual Collision Detection 
 SFML uses `sf::FloatRect::intersects()` for collision detection, typically done using `getGlobalBounds()` from any `sf::Sprite` or `sf::Text`.
cpp
```
if (bullet.getBounds().intersects(enemy.getBounds())) {
    // Bullet hit the enemy
    bullet.setActive(false);
    enemy.destroy();
}
```
Benefits:
- Lightweight: No need for a physics engine, keeping frame rates high.
- Simple: Ideal for sprite-based 2D games with rectangular shapes.
- Flexible: Can be extended with flags for health, destruction effects, etc.


### 2️⃣ Data Structures Used

#### a. Classes (OOP Abstractions)
The core gameplay revolves around three main C++ classes, each encapsulating data and behavior:
- Player Class
  - Attributes:
    - `sf::Sprite sprite`: The visual representation of the player ship.
    - `sf::Texture texture`: Image used for the player's sprite.
    - `float speed`: Movement speed (typically pixels/frame).
  - Methods:
    - `moveLeft()` and `moveRight()`: Updates position with boundary checks
    - `shoot(std::vector<Missile>& missiles)`: Spawns a missile at player's location
    - `draw(sf::RenderWindow&)`: Draws player to screen

- Enemy Class
  - Attributes:
    - `sf::Sprite sprite`: Visual for each enemy.
    - `sf::Texture texture`: Texture shared across enemy grid.
    - `float speed`: Movement rate (changes on level progression).
  - Methods:
    - `move(Direction dir)`: Moves the enemy downward
    - `draw(sf::RenderWindow&)`: Renders enemy on screen
    - `respawn()`: Repositions off-screen enemy back into formation.

- Missile Class
  - Attributes:
    - `sf::RectangleShape shape`: Graphical representation of missile (or could be a sprite).
    - `float velocity`: Upward speed for player missiles, downward for enemies.
    - `bool active`: Used to check if missile should be updated or drawn.
  - Methods:
    - `update()`: Moves missile each frame according to velocity.
    - `isOffScreen()`: Checks if bullet has exited screen
    - `draw(sf::RenderWindow&)`: Renders Missile if active
This abstraction allows:
- Better encapsulation with `sf::Sprite` & `sf::Texture`.
- Easier to debug and maintain modular code.
- Extendable for effects like animations, scaling, rotation.

#### b. Vectors (Collections of Game Entities)

The game uses C++ STL containers to manage dynamic lists of objects:

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

#### c. SFML Bounding Boxes for Collision Detection

- Each entity (missile, enemy, ship) uses getGlobalBounds() to get its AABB for collision detection.
``` cpp
 if (missile.getBounds().intersects(enemy.getBounds())) {
    // Collision response
 }
  ```
- `sf::FloatRect` returned by `getGlobalBounds()` is used with `.intersects()`.
- Ideal for fast collision checks in 2D arcade games.

Why AABB in SFML?
- Simple and efficient.
- Works well for sprite-based axis-aligned shapes.
- Can be extended to pixel-perfect if needed later.

#### d. Assets: Images and Textures

Instead of using raw pointers, SFML uses RAII (Resource Acquisition Is Initialization) for managing graphical and audio resources.
``` cpp
sf::Texture playerTexture;
playerTexture.loadFromFile("assets/player.png");

sf::Sprite playerSprite;
playerSprite.setTexture(playerTexture);
```
- For rendering:
  `window.draw(playerSprite);`
- For sounds and music
  ``` cpp
  sf::SoundBuffer buffer;
  buffer.loadFromFile("assets/shoot.wav");

  sf::Sound shootSound;
  shootSound.setBuffer(buffer);
  shootSound.play();
  ```
- For fonts and text
  ``` cpp
  sf::Font font;
  font.loadFromFile("assets/font.ttf");

  sf::Text scoreText;
  scoreText.setFont(font);
  scoreText.setString("Score: 0");
  scoreText.setCharacterSize(24);
  scoreText.setFillColor(sf::Color::White);
  ```

#### Analysis of all the classes
1. Player

| Element           | Description                                                              |
|-------------------|--------------------------------------------------------------------------|
| Purpose           | Manages the player spaceship — movement, firing missiles, and rendering  |
| Attributes        | `sf::Sprite sprite`, `sf::Texture texture`, `float speed`, `int lives`   |
| Methods           |`moveLeft()`, `moveRight()`, `shoot(std::vector<Missile>&)`, `draw()`     |
| Behavior          | Moves horizontally based on user input, can fire missiles from top-center|
| Input Handling    |Controlled via `sf::Keyboard::isKeyPressed` inside event handling loop    |

2. Enemy

| Element           | Description                                                             |
|-------------------|-------------------------------------------------------------------------|
| Purpose           | Represents enemy invaders falling from the top                          |
| Attributes        | `sf::Sprite sprite`, `sf::Texture texture`, `float speed`, `bool active`|
| Methods           | `move(Direction dir)`, `dropDown()`, `respawn()`, `draw()`              |
| Behavior          | Moves down the screen at a constant speed; respawns when destroyed      |
| Collision Logic   | If hit by a missile, it's either reset or marked inactive               |

3. Missile

| Element           | Description                                                            |
|-------------------|------------------------------------------------------------------------|
| Purpose           | Represents missiles fired by the player                                |
| Attributes        | `sf::RectangleShape shape`, `float velocity`, `bool active`            |
| Methods           | `update()`, `isOffScreen()`, `draw()`, `deactivate()`                  |
| Behavior          | Moves vertically upward and deactivates on collision or when off-screen|
| Lifecycle         | Fired from the player's position and removed when no longer needed     |

4. AssetManager 

| Element           | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| Purpose           | Handles centralized loading and access to textures, fonts, and sounds |
| Methods           | `loadTexture()`, `getTexture()`, `loadFont()`, `getSound()`           |
| Behavior          | Loads assets once; allows reuse across all entities                   |

## 📄 Header File Conventions

Even though each class has its own `.h` and `.cpp` file,  following typical SFML-based modular structure. Here’s what the includes generally look like: :

| Header Element                   | Purpose                                                          |
|----------------------------------|------------------------------------------------------------------|
| `#include <SFML/Graphics.hpp>`   | SFML's rendering, sprites, textures, and fonts                   |
| `#include <SFML/Audio.hpp>`      | For sound effects and background music                           |
| `#include <vector>`              | STL container used for missiles, enemies, etc.                   |
| `#include "Player.h"`            | Declares Player class                                            |
| `#include "Enemy.h"`             | Declares the Enemy class                                         |
| `#include "Missile.h"`           | Declares the Bullet class                                        |
| `#include "Shield.h"`	           | Declares Shield class                                            |
| `#include "AssetManager.h"`      | Singleton or namespace for loading/retrieving textures and sounds|

#### Main Game Loop

| Element                  | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `sf::RenderWindow`       |Creates a 950x950 game window; handles rendering and input events            |
| `sf::Texture`            | Loads images for ship, alien, missiles, and shields                         |
| `Music` & `SoundBuffer`  |  Loads images for ship, alien, missiles, and shields                        |
| `sf::Text`               | Displays score, level, and status messages (Game Over, Level Up, etc.)      |
| `sf::Font`               | 	Loads "font.ttf" used to render `sf::Text`                                 |



| Entity        | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| `ship`        | The player-controlled ship                                                  |
| `enemies`     | 10x5 grid of alien enemies                                                  |
| `missiles`    | Stores player-fired missiles                                                |
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











  
