# SOLID Principles in Java

The **SOLID principles** are five design principles in Object-Oriented Programming (OOP) that help create maintainable, scalable, and robust software. In Java, these principles guide developers to design classes and interfaces that are easier to understand and modify.

## 1. **Single Responsibility Principle (SRP)**

- **Definition:** A class should have only one reason to change, meaning it should have only one responsibility.

### Java Example:

```java
// Violates SRP: Handles both user data and database operations
class User {
    public void saveToDatabase() {
        // Code to save user to database
    }
}

// Correct: Separates responsibilities
class User {
    private String name;
    private String email;
    // Getters and setters
}

class UserRepository {
    public void save(User user) {
        // Code to save user to database
    }
}
```

## 2. **Open/Closed Principle (OCP)**

- **Definition:** Software entities (classes, modules, functions) should be open for extension but closed for modification.

### Java Example:

```java
// Violates OCP: Must modify this class to add new shapes
class AreaCalculator {
    public double calculate(Object shape) {
        if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return Math.PI * c.radius * c.radius;
        }
        return 0;
    }
}

// Correct: Open for extension, closed for modification
interface Shape {
    double area();
}

class Circle implements Shape {
    double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    public double area() {
        return Math.PI * radius * radius;
    }
}

class AreaCalculator {
    public double calculate(Shape shape) {
        return shape.area();
    }
}
```

## 3. **Liskov Substitution Principle (LSP)**

- **Definition:** Subtypes must be substitutable for their base types without altering program behavior.

### Java Example:

```java
// Violates LSP: Penguin can't fly but extends Bird
class Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}

// Correct: Redesign hierarchy
interface Bird {}

interface FlyingBird extends Bird {
    void fly();
}

class Sparrow implements FlyingBird {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Penguin implements Bird {
    // No fly method, avoids LSP violation
}
```

## 4. **Interface Segregation Principle (ISP)**

- **Definition:** Clients should not be forced to depend on interfaces they do not use.

### Java Example:

```java
// Violates ISP: Not all workers can fly
interface Worker {
    void work();
    void fly();
}

// Correct: Split interfaces
interface Worker {
    void work();
}

interface Flyer {
    void fly();
}

class HumanWorker implements Worker {
    public void work() {
        System.out.println("Working...");
    }
}

class SuperheroWorker implements Worker, Flyer {
    public void work() {
        System.out.println("Saving the world...");
    }

    public void fly() {
        System.out.println("Flying...");
    }
}
```

## 5. **Dependency Inversion Principle (DIP)**

- **Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions.

### Java Example:

```java
// Violates DIP: High-level depends on low-level
class LightBulb {
    public void turnOn() {
        System.out.println("Light On");
    }

    public void turnOff() {
        System.out.println("Light Off");
    }
}

class Switch {
    private LightBulb bulb;

    public Switch(LightBulb bulb) {
        this.bulb = bulb;
    }

    public void operate() {
        bulb.turnOn();
    }
}

// Correct: Depend on abstraction
interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    public void turnOn() {
        System.out.println("Light On");
    }

    public void turnOff() {
        System.out.println("Light Off");
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}
```

