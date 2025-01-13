### 1. *HeroActions.java*
java
// Interface for Hero actions
public interface HeroActions {
    void attack();
    void magic();
}


### 2. *Hero.java*
java
// Abstract class for Hero
public abstract class Hero implements HeroActions {
    protected String name;
    protected int hp;

    public Hero(String name, int hp) {
        this.name = name;
        this.hp = hp;
    }

    public String getName() {
        return name;
    }

    public int getHp() {
        return hp;
    }

    public void setHp(int hp) {
        this.hp = hp;
    }

    public abstract void displayHeroType();
}


### 3. *Warrior.java*
java
import java.util.Random;

// Warrior class
public class Warrior extends Hero {
    public Warrior(String name) {
        super(name, new Random().nextInt(151) + 50); // HP between 50-200
    }

    @Override
    public void attack() {
        System.out.println(name + " attacks with sword! (-50 Enemy HP)");
    }

    @Override
    public void magic() {
        System.out.println(name + " uses magic! (-40 Enemy HP)");
    }

    @Override
    public void displayHeroType() {
        System.out.println("Hero Type: Warrior");
    }
}


### 4. *Archer.java*
java
import java.util.Random;

// Archer class
public class Archer extends Hero {
    public Archer(String name) {
        super(name, new Random().nextInt(101) + 50); // HP between 50-150
    }

    @Override
    public void attack() {
        System.out.println(name + " shoots an arrow! (-30 Enemy HP)");
    }

    @Override
    public void magic() {
        System.out.println(name + " uses magic! (-60 Enemy HP)");
    }

    @Override
    public void displayHeroType() {
        System.out.println("Hero Type: Archer");
    }
}


### 5. *Game.java*
java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

// Main class
public class Game {
    private static List<Hero> heroList = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("1. Create New Hero");
            System.out.println("2. View Hero List");
            System.out.println("3. Find Enemy and Battle");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    createNewHero();
                    break;
                case 2:
                    viewHeroList();
                    break;
                case 3:
                    findEnemyAndBattle();
                    break;
                case 4:
                    System.out.println("Exiting the game.");
                    break;
                default:
                    System.out.println("Invalid choice. Please select between 1-4.");
            }
        } while (choice != 4);
    }

    private static void createNewHero() {
        System.out.print("Enter Hero Type (Warrior/Archer): ");
        String type = scanner.nextLine();
        System.out.print("Enter Hero Name (5-30 characters): ");
        String name = scanner.nextLine();

        if (name.length() < 5 || name.length() > 30) {
            System.out.println("Invalid name length.");
            return;
        }

        Hero hero;
        if (type.equalsIgnoreCase("Warrior")) {
            hero = new Warrior(name);
        } else if (type.equalsIgnoreCase("Archer")) {
            hero = new Archer(name);
        } else {
            System.out.println("Invalid hero type.");
            return;
        }

        heroList.add(hero);
        System.out.println("Hero created successfully!");
    }

    private static void viewHeroList() {
        if (heroList.isEmpty()) {
            System.out.println("No heroes available.");
        } else {
            for (Hero hero : heroList) {
                System.out.println("Name: " + hero.getName() + ", HP: " + hero.getHp());
                hero.displayHeroType();
            }
        }
    }

    private static void findEnemyAndBattle() {
        if (heroList.isEmpty()) {
            System.out.println("No heroes available to battle.");
            return;
        }

        Hero hero = heroList.get(0);
        int enemyHp = new Random().nextInt(151) + 50; // Enemy HP between 50-200
        System.out.println("Battle Start! " + hero.getName() + " vs Enemy");
        System.out.println(hero.getName() + " HP: " + hero.getHp() + ", Enemy HP: " + enemyHp);

        while (hero.getHp() > 0 && enemyHp > 0) {
            System.out.print("Choose action (1: Attack, 2: Magic): ");
            int action = scanner.nextInt();

            if (action == 1) {
                hero.attack();
                enemyHp -= (hero instanceof Warrior) ? 50 : 30;
            } else if (action == 2) {
                hero.magic();
                enemyHp -= (hero instanceof Warrior) ? 40 : 60;
            } else {
                System.out.println("Invalid action.");
                continue;
            }

            if (enemyHp > 0) {
                int damage = new Random().nextInt(61) + 10; // Enemy attack between 10-70
                hero.setHp(hero.getHp() - damage);
                System.out.println("Enemy attacks! " + hero.getName() + " HP: " + hero.getHp());
            }
        }

        if (hero.getHp() <= 0) {
            System.out.println(hero.getName() + " has been defeated.");
        } else {
            System.out.println("Enemy has been defeated.");
        }
    }
}



/YourProjectDirectory
    ├── Game.java
    ├── Hero.java
    ├── HeroActions.java
    ├── Warrior.java
    └── Archer.java
