---
layout: post
title: "My 7 Most Used Software Design Patterns"
author: "Lasse Schultebraucks"
categories:  [Software Development]
comments: true
---

As a software developer, I have encountered various challenges over time and found certain software design patterns to be particularly useful and versatile.

First of all, design patterns are divided into three different categories.

# 1. Creational Patterns
These patterns deal with the creation of objects. They allow instantiation of classes and objects in a flexible and loosely coupled manner. Creational patterns isolate the code from the specific class being created and allow the creation of objects to be delegated to subclasses or configurations. The main goals are decoupling and flexibility in object creation.

# 2. Structural Patterns:
Structural Patterns deal with the organization of classes and objects in larger structures. They emphasize how classes and objects are put together to create new structures that are easier to use and manage. These patterns help improve relationships between classes, increase reusability, and make the structure more flexible.

# 3. Behavioral Patterns:
Behavioral patterns deal with the interaction and communication between classes and objects. They define the way different objects cooperate and communicate with each other to accomplish specific tasks. These patterns define how the behavior of classes is organized and how responsibilities are distributed.

Here are my seven most frequently used design patterns, which I have applied in different projects time and again.

# 1. Builder Pattern (Creational):
The Builder Pattern is a creational pattern that simplifies the construction of complex objects by using a step-by-step approach. It separates the object construction from its representation, resulting in cleaner and more readable code.

## Example in Java: 
```java
public class Computer {
    private String cpu;
    private String gpu;
    private String ram;

    public Computer(ComputerBuilder builder) {
        this.cpu = builder.cpu;
        this.gpu = builder.gpu;
        this.ram = builder.ram;
    }

    // Getters and other methods...
}

public class ComputerBuilder {
    public String cpu;
    public String gpu;
    public String ram;

    public ComputerBuilder setCPU(String cpu) {
        this.cpu = cpu;
        return this;
    }

    public ComputerBuilder setGPU(String gpu) {
        this.gpu = gpu;
        return this;
    }

    public ComputerBuilder setRAM(String ram) {
        this.ram = ram;
        return this;
    }

    public Computer build() {
        return new Computer(this);
    }
}

// Usage:
Computer computer = new ComputerBuilder()
                        .setCPU("Intel Core i7")
                        .setGPU("Nvidia GeForce RTX 3080")
                        .setRAM("16GB DDR4")
                        .build();
```

# 2. Factory Pattern (Creational):
The Factory Pattern is another creational pattern that provides an interface for creating objects without revealing the specific implementations. It allows for a flexible and loosely coupled way of object creation.

## Example in Java:
```java
interface Animal {
    void makeSound();
}

class Dog implements Animal {
    public void makeSound() {
        System.out.println("Woof!");
    }
}

class Cat implements Animal {
    public void makeSound() {
        System.out.println("Meow!");
    }
}

class AnimalFactory {
    public static Animal createAnimal(String type) {
        if (type.equalsIgnoreCase("dog")) {
            return new Dog();
        } else if (type.equalsIgnoreCase("cat")) {
            return new Cat();
        }
        return null;
    }
}

// Usage:
Animal dog = AnimalFactory.createAnimal("dog");
dog.makeSound(); // Output: Woof!

Animal cat = AnimalFactory.createAnimal("cat");
cat.makeSound(); // Output: Meow!
```

 # 3. Adapter Pattern (Structural):
The Adapter Pattern is a structural pattern that allows two incompatible interfaces to work together. It acts as a mediator to facilitate communication between the classes and promotes reusability of existing code.

## Example in Java:
```java
// Legacy code with incompatible interface
interface OldPrinter {
    void print(String text);
}

class LegacyPrinter implements OldPrinter {
    public void print(String text) {
        System.out.println(text);
    }
}

// New code with the expected interface
interface NewPrinter {
    void printFormattedText(String text);
}

class ModernPrinter implements NewPrinter {
    public void printFormattedText(String text) {
        System.out.println("Formatted: " + text);
    }
}

// Adapter to make OldPrinter compatible with NewPrinter
class PrinterAdapter implements NewPrinter {
    private OldPrinter oldPrinter;

    public PrinterAdapter(OldPrinter oldPrinter) {
        this.oldPrinter = oldPrinter;
    }

    public void printFormattedText(String text) {
        // Converting to the old format before delegating
        oldPrinter.print(text);
    }
}

// Usage:
NewPrinter modernPrinter = new ModernPrinter();
modernPrinter.printFormattedText("Hello, World!"); // Output: Formatted: Hello, World!

OldPrinter legacyPrinter = new LegacyPrinter();
NewPrinter adaptedPrinter = new PrinterAdapter(legacyPrinter);
adaptedPrinter.printFormattedText("Hello, World!"); // Output: Hello, World!
```

# 4. Facade Pattern (Structural):
The Facade Pattern is another structural pattern that provides a simplified interface for a complex subsystem. It acts as a kind of "facade" for the underlying components, enabling easier usage and management.

## Example in Java:
```java
// Complex subsystems
class CPU {
    public void processData() {
        System.out.println("Processing data...");
    }
}

class Memory {
    public void load() {
        System.out.println("Loading data into memory...");
    }
}

class HardDrive {
    public void readData() {
        System.out.println("Reading data from the hard drive...");
    }
}

// Facade class providing a simplified interface
class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;

    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void start() {
        hardDrive.readData();
        memory.load();
        cpu.processData();
    }
}

// Usage:
ComputerFacade computerFacade = new ComputerFacade();
computerFacade.start();
// Output:
// Reading data from the hard drive...
// Loading data into memory...
// Processing data...
```

# 5. Decorator Pattern (Structural):**
The Decorator Pattern is a structural pattern that allows for flexible object extension at runtime. It enables adding additional functionality and responsibilities to an object without modifying its structure.

## Example in Java:
```java
// Component interface
interface Coffee {
    double getCost();
    String getDescription();
}

// Concrete component
class SimpleCoffee implements Coffee {
    public double getCost() {
        return 2.0;
    }

    public String getDescription() {
        return "Simple Coffee";
    }
}

// Decorator class
abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    public double getCost() {
        return decoratedCoffee.getCost();
    }

    public String getDescription() {
        return decoratedCoffee.getDescription();
    }
}

// Concrete decorators
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    public double getCost() {
        return super.getCost() + 0.5;
    }

    public String getDescription() {
        return super.getDescription() + ", Milk";
    }
}

class WhipDecorator extends CoffeeDecorator {
    public WhipDecorator(Coffee coffee) {
        super(coffee);
    }

    public double getCost() {
        return super.getCost() + 1.0;
    }

    public String getDescription() {
        return super.getDescription() + ", Whip";
    }
}

// Usage:
Coffee simpleCoffee = new SimpleCoffee();
System.out.println("Cost: " + simpleCoffee.getCost() + ", Description: " + simpleCoffee.getDescription());

Coffee coffeeWithMilk = new MilkDecorator(simpleCoffee);
System.out.println("Cost: " + coffeeWithMilk.getCost() + ", Description: " + coffeeWithMilk.getDescription());

Coffee coffeeWithMilkAndWhip = new WhipDecorator(coffeeWithMilk);
System.out.println("Cost: " + coffeeWithMilkAndWhip.getCost() + ", Description: " + coffeeWithMilkAndWhip.getDescription());
// Output:
// Cost: 2.0, Description: Simple Coffee
// Cost: 2.5, Description: Simple Coffee, Milk
// Cost: 3.5, Description: Simple Coffee, Milk, Whip
```

# 6. Composite Pattern (Structural):
The Composite Pattern is another structural pattern that allows objects to be treated as individual objects or object hierarchies uniformly. It enables working with objects as part-whole hierarchies, making the treatment of individual and group objects more straightforward.

## Example in Java:
```java
// Component interface
interface Graphic {
    void draw();
}

// Leaf class
class Circle implements Graphic {
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

class Square implements Graphic {
    public void draw() {
        System.out.println("Drawing a square");
    }
}

// Composite class
class CompositeGraphic implements Graphic {
    private List<Graphic> graphics = new ArrayList<>();

    public void add(Graphic graphic) {
        graphics.add(graphic);
    }

    public void draw() {
        for (Graphic graphic : graphics) {
            graphic.draw();
        }
    }
}

// Usage:
CompositeGraphic graphicGroup = new CompositeGraphic();
graphicGroup.add(new Circle());
graphicGroup.add(new Square());

graphicGroup.draw();
// Output:
// Drawing a circle
// Drawing a square
```

# 7. Delegation Pattern (Behavioral):
The Delegation Pattern is a behavioral pattern that delegates responsibilities to another object instead of handling them itself. It promotes loose coupling and efficient distribution of tasks.

## Example in Java:
```java
// Service interface
interface Printer {
    void print(String message);
}

// Delegate class
class RealPrinter implements Printer {
    public void print(String message) {
        System.out.println("Printing: " + message);
    }
}

// Client class
class PrintClient {
    private Printer printer;

    public PrintClient(Printer printer) {
        this.printer = printer;
    }

    public void printMessage(String message) {
        printer.print(message);
    }
}

// Usage:
Printer realPrinter = new RealPrinter();
PrintClient client = new PrintClient(realPrinter);
client.printMessage("Hello, Delegation Pattern!");
// Output: Printing: Hello, Delegation Pattern!
```

These seven design patterns have proven to be extremely valuable in my day-to-day work as a software developer. By applying them, I could make code more efficient, improve reusability, and enhance the maintainability of my projects. I hope these examples provide you with insight into the versatility and benefits of design patterns in software development in general.