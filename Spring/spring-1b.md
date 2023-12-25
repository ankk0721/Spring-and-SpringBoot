## Loosely coupled using interfaces
    * make an interface which declares all methods required by games
    * each game implements this interface
    * Then we can use this interface name instead of class inside GameRunner for instance variable
    * In this, JVM is handling game objects. Next we have to see how spring will handle this object

### AppGamingBasicJava.java

```java
package com.in28minutes.learnspringframework;

import com.in28minutes.learnspringframework.game.GameRunner;
import com.in28minutes.learnspringframework.game.PacmanGame;

public class AppGamingBasicJava {

public static void main(String[] args) {

//var game = new MarioGame();
//var game = new SuperContraGame();
var game = new PacmanGame();

var gameRunner = new GameRunner(game);
gameRunner.run();

}

}
```
---


### GameRunner.java

```java
package com.in28minutes.learnspringframework.game;

//PacmanGame
public class GameRunner {

private GamingConsole game;

public GameRunner(GamingConsole game) {
this.game = game;
}

public void run() {

System.out.println("Running game: " + game);
game.up();
game.down();
game.left();
game.right();

}

}
```
---

### GamingConsole.java

```java
package com.in28minutes.learnspringframework.game;

public interface GamingConsole {
void up();
void down();
void left();
void right();
}
```
---

### MarioGame.java

```java
package com.in28minutes.learnspringframework.game;

public class MarioGame implements GamingConsole{

public void up() {
System.out.println("Jump");
}

public void down() {
System.out.println("Go into a hole");
}

public void left() {
System.out.println("Go back");
}

public void right() {
System.out.println("Accelerate");
}


}
```
---

### PacmanGame.java

```java
package com.in28minutes.learnspringframework.game;

public class PacmanGame implements GamingConsole{

public void up() {
System.out.println("up");
}

public void down() {
System.out.println("down");
}

public void left() {
System.out.println("left");
}

public void right() {
System.out.println("right");
}


}
```
---

### SuperContraGame.java

```java
package com.in28minutes.learnspringframework.game;

public class SuperContraGame implements GamingConsole{

public void up() {
System.out.println("up");
}

public void down() {
System.out.println("Sit down");
}

public void left() {
System.out.println("Go back");
}

public void right() {
System.out.println("Shoot a bullet");
}

}
```
