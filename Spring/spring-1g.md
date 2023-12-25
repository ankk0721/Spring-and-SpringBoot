# How spring manages beans
	* Spring create and manage beans(java objects)
	* For this, first combine Configuration class and Application class
	* Add @Component annotation to Bean class (GameRunner, MariGame, SuperContraGAme)
	* Then add @ComponentScan("package") on Application class
	* component scan first scan all the class present in that package and creates beans 
	  for those class which have component scan annotation
	* pass same application.class to context
   
    * @Component: instance of class will be managed by spring framework
	* Dependency: GameRunner need impl of GamingConsole, so GamingConsole is a dependency of GameRunner
	* @ComponentScan(""): In this we passed packages where spring search for beans

				### GamingAppLauncherApplication.java
				```java

				package com.in28minutes.learnspringframework;

				import org.springframework.context.annotation.AnnotationConfigApplicationContext;
				import org.springframework.context.annotation.ComponentScan;
				import org.springframework.context.annotation.Configuration;

				import com.in28minutes.learnspringframework.game.GameRunner;
				import com.in28minutes.learnspringframework.game.GamingConsole;

				@Configuration
				@ComponentScan("com.in28minutes.learnspringframework.game")
				public class GamingAppLauncherApplication {
					
					public static void main(String[] args) {

						try (var context = 
								new AnnotationConfigApplicationContext
									(GamingAppLauncherApplication.class)) {

							context.getBean(GamingConsole.class).up();
							
							context.getBean(GameRunner.class).run();

						}
					}
				}

				```

				```java
				package com.in28minutes.learnspringframework;

				import org.springframework.context.annotation.Bean;
				import org.springframework.context.annotation.Configuration;

				import com.in28minutes.learnspringframework.game.GameRunner;
				import com.in28minutes.learnspringframework.game.GamingConsole;
				import com.in28minutes.learnspringframework.game.PacmanGame;

				@Configuration
				public class GamingConfiguration {

					@Bean
					public GamingConsole game() {
						var game = new PacmanGame();
						return game;
					}

					@Bean
					public GameRunner gameRunner(GamingConsole game) {
						var gameRunner = new GameRunner(game);
						return gameRunner;
					}

				}

				### GameRunner.java
				```java

				package com.in28minutes.learnspringframework.game;

				import org.springframework.stereotype.Component;

				@Component
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



