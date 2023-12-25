# @Primary and @Qualifier
	* In previous step, when we run GameRunner bean it founds two GamingConsole class (SuperContraGame and MarioGame)
	* To solve this issue, we can add @Primary annotation to one of these class

	* If one class is tagged with primary and you want run another bean then we can use @Qualifier
	* First add @Qualifier("name") to thet class, then use @Qualifier("name") before where you pass that class




				### amingAppLauncherApplication.java

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


				### GameRunner.java

				```java
				package com.in28minutes.learnspringframework.game;

				import org.springframework.beans.factory.annotation.Qualifier;
				import org.springframework.stereotype.Component;

				@Component
				public class GameRunner {
					
					private GamingConsole game;
					
					public GameRunner(@Qualifier("SuperContraGameQualifier") GamingConsole game) {
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


				### MarioGame.java

				```java
				package com.in28minutes.learnspringframework.game;

				import org.springframework.context.annotation.Primary;
				import org.springframework.stereotype.Component;

				@Component
				@Primary
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

				import org.springframework.beans.factory.annotation.Qualifier;
				import org.springframework.stereotype.Component;

				@Component
				@Qualifier("SuperContraGameQualifier")
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


