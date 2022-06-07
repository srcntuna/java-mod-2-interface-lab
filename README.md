# Interface Lab

## Learning Goals

- Create and use interfaces

## Instructions

Create a simple game that asks the player what type of bird it would like to
capture and tells them if their bird was able to escape the monster based on how
fast their bird can fly.

You will need the following:

1. A "Runner" class to run your game
2. A method to ask the user what type of bird they want
3. A `CanFly` interface to use as the type of the bird object you will use
4. A `fly()` method in your `CanFly` interface, but it will return an int that
   represents the speed at which the bird is flying instead of returning nothing
   like our sample code above
5. A method to ask the user how fast the monster is
6. An `if` statement that lets the bird escape if their speed is faster than the
   speed of the monster

In writing this code, you should be able to see that once you've created the
instance of the bird the user asked for, you shouldn't have to worry about its
specific type anymore.

## Walkthrough

Here is a reference implementation of the new `CanFly` interface:

```java
public interface CanFly {
    int fly();
}
```

As well as the 2 updated implementations for `Pigeon` and `Puffin`:

```java
public class Pigeon implements CanFly, Animal {
    public int fly() {
        System.out.println("I'm a pigeon and I can fly up to 90 miles an hour!");
        return 90;
    }

    public void takeBreath() {
        System.out.println("Pigeon has taken a breath");
    }

    public void makeSound() {
        System.out.println("Pigeon goes coo, coo");
    }
}
```

```java
public class Puffin implements Animal, CanSwim, CanFly {
        public int fly() {
            System.out.println("I'm puffin and I can fly up to 55 miles an hour!");
            return 55;
        }

        public void swim() {
            System.out.println("I'm a puffin and I can swim down to 200 feet deep");
        }

        public void takeBreath() {
            System.out.println("Puffin took a breath");
        }

        public void makeSound() {
            System.out.println("Puffin goes brau, brau");
        }
}
```

And a reference implementation of the `AnimalRunner`:

```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class AnimalRunner {

    private static void log(String message) {
        System.out.println(message);
    }

    public static void main(String[] args) {
        CanFly userBird = getUserBird();
        int monsterSpeed = getMonsterSpeed();

        int birdSpeed = userBird.fly();
        if (birdSpeed > monsterSpeed) {
            log("Your bird was fast enough to escape");
        } else {
            log("Your bird was not fast enough to escape :-(");
        }
    }

    private static CanFly getUserBird() {
        Scanner inputScanner = new Scanner(System.in);
        while (true) {
            log("Please enter the type of bird you would like");
            log("1. Pigeon");
            log("2. Puffin");
            try {
                int userSelection = inputScanner.nextInt();
                if (userSelection == 1) {
                    return new Pigeon();
                } else if (userSelection == 2) {
                    return new Puffin();
                } else {
                    throw new InputMismatchException("Value must be either 1 or 2");
                }
            } catch(InputMismatchException inputException) {
                inputScanner.nextLine(); // clear the invalid input
                log("Invalid input - " + inputException.getMessage());
            }
        }
    }

    private static int getMonsterSpeed() {
        Scanner inputScanner = new Scanner(System.in);
        while (true) {
            log("How fast can the monster fly (in mph)?");
            try {
                int monsterSpeed = inputScanner.nextInt();
                return monsterSpeed;
            } catch(InputMismatchException inputException) {
                inputScanner.nextLine(); // clear the invalid input
                log("Invalid input - " + inputException.getMessage());
            }
        }
    }
}
```
