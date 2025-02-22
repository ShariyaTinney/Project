# Brick Breaker Game in Java

## Overview
Brick Breaker is a classic arcade game where the player controls a paddle to bounce a ball and break bricks. The goal is to destroy all bricks on the screen without letting the ball fall off the bottom.

## Features
- Java Swing-based GUI
- Collision detection for ball, paddle, and bricks
- Multiple levels with increasing difficulty
- Score tracking system
- Responsive paddle movement using keyboard input

## Requirements
- Java Development Kit (JDK) 8 or later
- IDE like IntelliJ IDEA, Eclipse, or NetBeans (optional)

## Installation
1. Clone this repository or download the source code.
   ```bash
   git clone https://github.com/your-repo/brickbreaker-java.git
   cd brickbreaker-java
   ```
2. Compile the Java source files.
   ```bash
   javac -d bin src/*.java
   ```
3. Run the game.
   ```bash
   java -cp bin Main
   ```

## How to Play
- Use the **left arrow** and **right arrow** keys to move the paddle.
- Bounce the ball off the paddle to hit and break bricks.
- If the ball falls off the bottom, you lose a life.
- Break all bricks to advance to the next level.

## File Structure
```
brickbreaker-java/
├── src/
│   ├── Main.java          # Entry point of the game
│   ├── GamePanel.java     # Main game logic and rendering
│   ├── Paddle.java        # Paddle class
│   ├── Ball.java          # Ball class
│   ├── Brick.java         # Brick class
│   ├── Collision.java     # Collision detection logic
├── bin/                   # Compiled class files
├── README.md              # This file
├── resources/             # Game assets (images,  etc.)
```

## Future Enhancements
- Power-ups (e.g., larger paddle, multiple balls)
- More sound effects and background music
- Different types of bricks with varying durability
- Multiplayer mode

## License
This project is licensed under the MIT License.

## Contributions
Contributions are welcome! Feel free to fork this repository, create a new branch, and submit a pull request.

## Contact
For any questions or feedback, contact [Shariya Tinney] at [shariyatinney@gmail.com].


