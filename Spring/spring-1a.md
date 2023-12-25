# Spring 
  * It is a lightweight framework which comprises oof several modules such as IOC, AOP, DAO,MVC,etc

## Advantages
  * Loose coupling: spring app are loosely couples due to deppendency injection
  * Easy to test, it's doesn't require server to test app
  * Lightweight
  * Fast deployment (spring-boot-devtools dependency, not need to run app after any small change)

## Tighly coupled Java Code Examples
    * we have two game class mario and super contra
    * Inside gameRunner, we created a instance of these games and its constructor accepts the game
    * In main class, we are creating a instance of one game and passing it to GameRunner constructor.
    * How it is tightly coupled: If we want to change the game then we need to change main app and also variables inside GameRunner. 

### AppGamingBasicJava.java

```java
package com.in28minutes.learnspringframework;

import com.in28minutes.learnspringframework.game.GameRunner;
import com.in28minutes.learnspringframework.game.MarioGame;
import com.in28minutes.learnspringframework.game.SuperContraGame;

public class AppGamingBasicJava {

public static void main(String[] args) {

//var marioGame = new MarioGame();
var superContraGame = new SuperContraGame();
var gameRunner = new GameRunner(superContraGame);
gameRunner.run();

}

}
```
---


### GameRunner.java

```java
package com.in28minutes.learnspringframework.game;

public class GameRunner {

private SuperContraGame game;

public GameRunner(SuperContraGame game) {
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

### MarioGame.java

```java
package com.in28minutes.learnspringframework.game;

public class MarioGame {

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

### SuperContraGame.java

```java
package com.in28minutes.learnspringframework.game;

public class SuperContraGame {

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
