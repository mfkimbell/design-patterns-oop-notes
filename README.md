# design-patterns-oop-notes

Be able to describe all of these:

<img width="1048" alt="Screenshot 2024-11-11 at 10 53 35 AM" src="https://github.com/user-attachments/assets/a3f3a9fb-2cbc-4193-ac54-7cbee8d819b6">

# Notes for interviews
* I have been learning about design patterns, but it's important to know WHEN to use them, if you use them willy nilly, you might just be increasing complexity without positive impact
* i feel like in general, as projects become larger in scale design patterns become a lot more worth it
  
# repository pattern
* Mediates between the domain and data mapping layers, acting like an **in memory collection** of domain objects

  <img width="820" alt="Screenshot 2024-08-25 at 2 55 55 PM" src="https://github.com/user-attachments/assets/c4de6e47-9fec-43fc-9f63-6b8d1cac4bad">

### First, what is an ORM?
* map data objects (object relational mappers) to table and records in a relational dataabase.
* you get to work with objects rather than database tables
* **modularization and decoupling** (these words come up all the time, keep them in vocab)
* examples: entity framework, dapper, etc...
### Benefits
* minimizes dupliate query logic
* decouples your applicatin from persistence frameworks (for instance, entity framework)
* **"architechture should be independent of framworks"**
<img width="596" alt="Screenshot 2024-08-25 at 2 41 56 PM" src="https://github.com/user-attachments/assets/152fffcf-abce-4f4a-a616-f23f3c21a230">

* (sometimes you HAVE to switch, every 2 years a new ORM comes out!)
Here is what it should look like

<img width="206" alt="Screenshot 2024-08-25 at 2 32 34 PM" src="https://github.com/user-attachments/assets/4f697e83-a287-434a-8d56-4d6aef685543">

### Unit of work
* in the context of entity framework, DbContext is the unit of work
* it keeps track of the changes to the database objects and can Save() the changes to the datbase
* Here is an example, we could call Complete(), Save() instead
<img width="502" alt="Screenshot 2024-08-25 at 3 04 46 PM" src="https://github.com/user-attachments/assets/795287f6-95b4-45a6-b3a3-1e2a7a86048a">


### how it related to graphQL 

# CQRS (Command Query Resposibility Segregation)

<img width="1253" alt="Screenshot 2024-08-26 at 4 16 02 PM" src="https://github.com/user-attachments/assets/c0679ad7-c6f0-4b69-bf5d-cee94a6f4c65">

* a method should either read or write, but never both
* **Queries** are for retrieving information and **Commands** are for changing or adding information
* "dequeue" is a good example of a violation, every time you take something off the queue, you both return it and alter the original queue. You would need to use a command and a query to achieve the same result. 
### Why is this beneficial
* Cloud-native applications benefit from event-driven architectures where services communicate asynchronously through events. CQRS supports this paradigm by allowing event sourcing for write operations and efficient query models for read operations
* with queries not having side effects we can retry without complications
### separation of concerns
* our application now has completely separate paths for queries and commands, so they can be scaled separately
* helps join data from multiple different microservices (modularization)
* better for systems that want to execute commands in a queue
* you can scale the read and writes separately 
### When is it NOT beneficial
* when you are altering data and need it back in the same format every time
* when the other pros aren't relevant

Here are some things that CQRS can build into:

<img width="1262" alt="Screenshot 2024-08-26 at 4 25 31 PM" src="https://github.com/user-attachments/assets/5087815a-a72a-416c-9a97-e9dd3d8237af">

CQRS is simlilar to GraphQL in that they separate the read and writes logic.
- again this is helpful for a lot of reasons, but one that sticks in my mind is that with less side effects you can have retries without causing problems.


# Singleton
* useful when you need a single instance of a class, such as a **logger**, or a database connection pool
* we make the **constructor private** so that we can't do `new Instance = Instance()` we instead use `getInstance()` and it makes a new one ONLY if there isn't one already
* shared state!
* need to be careful with concureency (race conditions)
* here's a good race condition exmaple, they both grab the starting amount, BEFORE thread1 has time to update the new total
* Some people say it's an **anti-pattern** because it's **rigid** and it uses **global instance** which is hard to track how it changes
```java
public static Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();  // Create the instance if it does not exist
    }
    return instance;
}
```
```python
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);

        // Simulate multiple deposits happening concurrently
        Thread thread1 = new Thread(() -> {
            account.deposit(100);
            System.out.println("Thread 1: New balance after deposit: " + account.getBalance());
        });

        Thread thread2 = new Thread(() -> {
            account.deposit(200);
            System.out.println("Thread 2: New balance after deposit: " + account.getBalance());
        });

        thread1.start();
        thread2.start();
    }
}
```
# Factory
```java 
public class MainApp {
    public static void main(String[] args) {
        VehicleFactory carFactory = new CarFactory();
        Vehicle car = carFactory.createVehicle();
        System.out.println(car.getType());  // Output: Car

        VehicleFactory bikeFactory = new BikeFactory();
        Vehicle bike = bikeFactory.createVehicle();
        System.out.println(bike.getType());  // Output: Bike

        VehicleFactory truckFactory = new TruckFactory();
        Vehicle truck = truckFactory.createVehicle();
        System.out.println(truck.getType());  // Output: Truck
    }
}
```

* The Factory Pattern is a creational design pattern used to abstract the process of creating objects, making code more flexible and reusable.
* Follows Open/Closed principle (we can make new objects without editing our vehicle factory)
* The reason we do this this way instead of passing in a type is that we can just add new factories code and don't have to change the vehicle factory

<img width="384" alt="Screenshot 2024-09-01 at 10 40 37 AM" src="https://github.com/user-attachments/assets/21b20819-ae48-404f-bb9b-d20dc87f3a21">

https://www.youtube.com/watch?v=ub0DXaeV6hA

<img width="1020" alt="Screenshot 2024-09-01 at 10 32 02 AM" src="https://github.com/user-attachments/assets/2ba37314-2049-4e92-8a58-3c7b3021b8f0">


<img width="1164" alt="Screenshot 2024-09-01 at 10 36 49 AM" src="https://github.com/user-attachments/assets/0b575503-58a0-4847-b913-b1126dae6774">

- I love this example, it's so we can generate random enemies AT RUNTIME since it can generate any of the classes (as long as they share a superclass)

#### Focus:

* Factory Pattern: Focuses on deciding which subclass to create. It's about polymorphism—choosing among many possible classes.
* Builder Pattern: Focuses on constructing complex objects step-by-step. It's about assembling an object from multiple components.
#### Examples:

* Factory Pattern: You want to create different types of Shape objects—Circle, Square, Triangle. The factory determines which to create based on input or context.
* Builder Pattern: You want to create a House that may have different configurations—Bedrooms, Bathrooms, Garden, Swimming Pool. The builder allows you to add components as needed.

# Builder

<img width="680" alt="Screenshot 2024-11-11 at 1 46 43 PM" src="https://github.com/user-attachments/assets/6fbf2256-0028-4416-b75b-90f3a424f08d">

you can also do it like this so you can actually specify starters this way, so 
* here is another way to do it I found
* In the earlier approach, we created specialized builders like VeganMealBuilder or HealthyMealBuilder because each of those meals had different rules and constraints.
* This pizza option doesn't have the strict rules
```java
public class Pizza {
    private final int size;
    private final boolean cheese;
    private final boolean pepperoni;
    private final boolean bacon;

    private Pizza(Builder builder) {
        this.size = builder.size;
        this.cheese = builder.cheese;
        this.pepperoni = builder.pepperoni;
        this.bacon = builder.bacon;
    }

    public static class Builder {
        private final int size;
        private boolean cheese = false;
        private boolean pepperoni = false;
        private boolean bacon = false;

        public Builder(int size) {
            this.size = size;
        }

        public Builder cheese(boolean value) {
            this.cheese = value;
            return this;  // Returning the builder instance (this)
        }

        public Builder pepperoni(boolean value) {
            this.pepperoni = value;
            return this;  // Returning the builder instance (this)
        }

        public Builder bacon(boolean value) {
            this.bacon = value;
            return this;  // Returning the builder instance (this)
        }

        public Pizza build() {
            return new Pizza(this);
        }
    }
}
```
```java
public class MainApp {
    public static void main(String[] args) {
        HealthyMealBuilder builder = new HealthyMealBuilder();
        
        // Customizing the meal by passing parameters
        builder.addStarter(Starter.BRUSCHETTA);
        builder.addMainCourse(Main.GRILLED_CHICKEN);
        builder.addDessert(Dessert.ICE_CREAM);
        builder.addDrink(Drink.FRUIT_JUICE);

        Meal customizedMeal = builder.build();
        System.out.println("Customized Meal: " + customizedMeal);
    }
}
```
```java
// Enums for Meal Components
enum Starter { SALAD, SOUP }
enum Main { GRILLED_CHICKEN, PASTA }
enum Dessert { FRUIT_SALAD, ICE_CREAM }
enum Drink { WATER, SODA }

// Meal Class
class Meal {
    private Starter starter;
    private Main main;
    private Dessert dessert;
    private Drink drink;

    // Setters for each component
    void setStarter(Starter starter) { this.starter = starter; }
    void setMain(Main main) { this.main = main; }
    void setDessert(Dessert dessert) { this.dessert = dessert; }
    void setDrink(Drink drink) { this.drink = drink; }

    @Override
    public String toString() {
        return "Meal: Starter=" + starter + ", Main=" + main + ", Dessert=" + dessert + ", Drink=" + drink;
    }
}

// Builder Interface
interface Builder {
    void addStarter();
    void addMainCourse();
    void addDessert();
    void addDrink();
    Meal build();
}

// VeganMealBuilder Class
class VeganMealBuilder implements Builder {
    private Meal meal = new Meal();

    @Override public void addStarter() { meal.setStarter(Starter.SALAD); }
    @Override public void addMainCourse() { meal.setMain(Main.PASTA); }
    @Override public void addDessert() { meal.setDessert(Dessert.FRUIT_SALAD); }
    @Override public void addDrink() { meal.setDrink(Drink.WATER); }
    @Override public Meal build() { return meal; }
}

// Director Class
class Director {
    void constructMeal(Builder builder) {
        builder.addStarter();
        builder.addMainCourse();
        builder.addDessert();
        builder.addDrink();
    }
}

// Client Code
public class MainApp {
    public static void main(String[] args) {
        Director director = new Director();
        Builder veganBuilder = new VeganMealBuilder();
        director.constructMeal(veganBuilder);
        Meal veganMeal = veganBuilder.build();
        System.out.println(veganMeal);
    }
}
```
* we add then return the object eventually
* The Builder Pattern is used to construct complex objects step-by-step.
* Each Concrete Builder has a single responsibility—to build a specific type of product (e.g., a specific meal configuration).
* The Open/Closed Principle states that a class should be open for extension but closed for modification.
If you need to add a new type of meal, you simply create a new Concrete Builder (e.g., GlutenFreeMealBuilder). You do not need to modify the existing builders. This makes the system more extensible and less prone to errors.

<img width="667" alt="Screenshot 2024-09-01 at 11 10 57 AM" src="https://github.com/user-attachments/assets/fe3892a7-f360-4e45-9c5d-733036413011">

<img width="656" alt="Screenshot 2024-09-01 at 11 10 32 AM" src="https://github.com/user-attachments/assets/604a8420-b95c-458b-919e-bda88eefa706">

- the functions are the same but the implementation is different, one builds the car while the other builds the manual. This is a weird example though, it's more relevant to refer to the builder pattern as something that builds a bunch of different objects that implement the interface (rather than building a sub-class for each specific object:

<img width="682" alt="Screenshot 2024-09-01 at 11 12 28 AM" src="https://github.com/user-attachments/assets/48973860-6386-49e7-b4b3-b4111d3a4e1f">

# Polymorphism 

Polymorphism is a core concept in object-oriented programming (OOP) that allows objects of different types to be treated as objects of a common super type. It enables a single function, method, or operator to perform different tasks based on the context or the input.

There are two main types of polymorphism in OOP:

**Compile-time Polymorphism** (also known as Static Polymorphism)
**Runtime Polymorphism** (also known as Dynamic Polymorphism)

### Overloading
* Compile time polymorhpism
* same function name, different parameters


### Overwriting

* Overriding is a form of runtime polymorphism where a derived class provides a specific implementation of a method that is already defined in its base (parent) class.
* The method in the base class is marked as virtual, abstract, or override, allowing the derived class to provide its implementation.

# Virtual Tables (V-tables): virtual, abstract, or override

Why Do You Need virtual, abstract, or override?

A vtable (virtual table) is a mechanism used in languages like C++ and C# to support dynamic method dispatch (runtime polymorphism). It is essentially a table of function pointers, where each entry points to the implementation of a virtual method for a given class.

Each class that has virtual methods (methods marked with virtual, abstract, or override) has its own vtable.
Instances of a class contain a hidden pointer to the vtable for their class. This pointer is used to look up method implementations at runtime.

### Virtual:

* Purpose: The virtual keyword is used to indicate that a method in a base class can be overridden in any derived class. It allows a method to be polymorphic, meaning that the exact method that gets called is determined at runtime based on the object's type.
* Usage: When you mark a method as virtual, it tells the compiler to include an entry for this method in the vtable of the class. If a derived class overrides this method, the vtable entry is updated to point to the overridden method.
Example:
```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Some generic animal sound");
    }
}
```

### Abstract:

* Purpose: The abstract keyword is used to define a method that must be implemented by any non-abstract derived class. An abstract method has no implementation in the base class; it serves as a template or contract that derived classes must fulfill.
* Usage: Abstract methods are inherently virtual because they require derived classes to provide an implementation. However, abstract methods don't provide their own implementation; they just define the method signature.
Example:
```csharp
public abstract class Animal
{
    public abstract void MakeSound(); // No implementation here
}
```


### Override:

* Purpose: The override keyword is used in a derived class to provide a new implementation for a method that is declared as virtual or abstract in its base class.
* Usage: It tells the compiler that the method is meant to override a base class method, ensuring that the method signatures match. The overridden method will replace the base class method's entry in the vtable.
Example:
```csharp
public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Bark");
    }
}
```



## Abstraction:

Definition: Simplifying complex systems by modeling classes appropriate to the problem, focusing on essential characteristics while hiding unnecessary details.

Purpose: To reduce complexity by exposing only the relevant attributes and behaviors of an object.
Key Benefit: Makes the code easier to use and understand by highlighting what an object does rather than how it does it.

# Abstraction

* Abstraction is the process of hiding the complex implementation details of a system and showing only the essential features or functionalities to the user. It focuses on exposing only the relevant parts of an object or class while keeping the rest hidden.

* ***Interfaces and Abstract Classes:*** In OOP, abstraction is often achieved using interfaces and abstract classes. These allow you to define a contract or blueprint that classes must follow without specifying the implementation details.

## Encapsulation:

Definition: Bundling data and methods that operate on the data into a single unit (class) and restricting direct access to some of an object’s components.

Purpose: To hide the internal state of an object and only expose a controlled interface to interact with it.
Key Benefit: Provides data protection and controlled access through getters and setters.

# Encapsulation

* Encapsulation is the bundling of data (variables) and methods that operate on the data (functions) into a single unit or class. It restricts direct access to some of the object's components and prevents the accidental modification of data.
* ***Private and Protected Access Modifiers and Methods: Encapsulation protects the internal state of an object by hiding its data members from outside access. This is typically achieved by declaring data members as private or protected and providing public methods (getters and setters) to access or modify them.

```java
public class BankAccount
{
    private decimal balance; // Private field to hold balance

    public decimal GetBalance() // Public getter method to access balance
    {
        return balance;
    }

    public void Deposit(decimal amount) // Public method to deposit money
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }

    public void Withdraw(decimal amount) // Public method to withdraw money
    {
        if (amount > 0 && amount <= balance)
        {
            balance -= amount;
        }
    }
}
```

* The balance field is encapsulated within the BankAccount class and marked as private, preventing direct access from outside the class.
Methods like Deposit and Withdraw provide controlled ways to interact with the balance, ensuring that it cannot be arbitrarily modified.


# Interfaces
- help with testing code and writing unit tests
- ex: our payment service relies on a banking servive we import, we can make a
Let me break it down a bit more:


An interface is like a contract that defines what methods a class should have, but it doesn't say how those methods should work. When a class implements an interface, it agrees to follow that contract by providing specific implementations of those methods.

### Why Use Interfaces?

When you write code that depends on an interface rather than a specific class, you're creating a flexible setup where your code doesn't need to know the details of how those methods work—it just knows that the methods exist. This is key for unit testing.

### How Does This Help with Unit Testing?

1. **Substituting Real Objects with Mocks**:
   - Imagine you have a class that depends on another class to do something. If your class is directly tied to that other class (without an interface), it's difficult to test your class without also involving that other class. This can make testing hard because you're not just testing one piece of code—you’re testing everything it depends on too.
   - When you use an interface, you can "swap out" the real class with a "mock" class in your test. This mock class implements the same interface but is controlled by your test code, allowing you to simulate different scenarios without relying on the real dependencies.

   **Example**: 
   Let's say you have a `PaymentService` class that relies on a `BankService` to process payments.

   ```python
   class BankService:
       def transfer(self, amount):
           # Logic to transfer money
           pass

   class PaymentService:
       def __init__(self, bank_service: BankService):
           self.bank_service = bank_service

       def make_payment(self, amount):
           return self.bank_service.transfer(amount)
   ```

   - In a test, without interfaces, you'd have to deal with the real `BankService`, which might actually try to transfer money, something you definitely don't want during testing.
   - If `BankService` was an interface instead, you could create a mock version of `BankService` in your tests:

   ```python
   class MockBankService(BankService):
       def transfer(self, amount):
           return "Mock Transfer Success"
   ```

   Now, when you test `PaymentService`, you can use `MockBankService`, which doesn't actually move money around but lets you test how `PaymentService` works when a payment is "successful."

# class types, static, protecged etc...


# Adapter pattern
-basically you use the adapater object instead of the other object, so that you can write custom code to make it work with noncompatible types.
-aka putting a circle in a square hole (instead of a square)
-our adapter object acts like an api that implements the intended client interface but with the updated functionality (new payment processor)

<img width="461" alt="Screenshot 2024-09-01 at 12 19 42 PM" src="https://github.com/user-attachments/assets/4a4e7ff2-89b0-4a0b-b33a-37270d8e9231">

<img width="1064" alt="Screenshot 2024-09-01 at 12 00 32 PM" src="https://github.com/user-attachments/assets/0b2f0acc-885d-43a1-92ee-df35ff896e02">

<img width="781" alt="Screenshot 2024-09-01 at 12 02 04 PM" src="https://github.com/user-attachments/assets/1468d280-8b48-420e-8a17-ce35b838f9bf">

<img width="957" alt="Screenshot 2024-09-01 at 12 03 14 PM" src="https://github.com/user-attachments/assets/5d7e9e25-7c0b-43e8-a858-919378fd0370">

<img width="1115" alt="Screenshot 2024-09-01 at 12 04 37 PM" src="https://github.com/user-attachments/assets/6cd94011-0605-44bc-9066-a03a23c8f2d2">

<img width="1138" alt="Screenshot 2024-09-01 at 12 05 25 PM" src="https://github.com/user-attachments/assets/c32ab4c1-5a5b-46e3-87d1-fa1b5bd95314">

<img width="767" alt="Screenshot 2024-09-01 at 12 21 53 PM" src="https://github.com/user-attachments/assets/487c8210-7c31-43c0-96c1-0960ddba3708">

<img width="776" alt="Screenshot 2024-09-01 at 12 25 42 PM" src="https://github.com/user-attachments/assets/49d39411-27fd-41eb-ae64-9bd5e323f7d0">

# Observer pattern
### pub/sub pattern

* one to many relationship with objects
* When the state of one object (the subject) changes, all of its dependents (called observers) are automatically notified and updated
* The Observer Pattern is useful in scenarios where you need to maintain consistency across related objects or want to allow multiple components to react to changes in another component's state.

# Facade pattern
* The Facade Pattern is a structural design pattern that provides a simplified interface to a complex subsystem. It hides the complexities of the subsystem by exposing a single, unified interface, making the subsystem easier to use and understand for the client.
* Many libraries have hidden logic the the user is unaware of
<img width="522" alt="Screenshot 2024-09-01 at 4 00 08 PM" src="https://github.com/user-attachments/assets/41ea2e92-0365-40ca-9a74-9793e77cc65f">

### SAM CLI for Cloudformation YAML is a good example, makes writing code a lot easier

# Proxy pattern
<img width="991" alt="Screenshot 2024-09-01 at 4 04 16 PM" src="https://github.com/user-attachments/assets/81020854-8c72-44ae-a26f-436bf7ece83b">

Purpose of the Proxy Pattern
The Proxy Pattern is used when you want to provide controlled access or add extra functionality to an object, such as:

* Lazy Initialization: Delaying the creation or loading of a resource-intensive object until it is actually needed.
* Access Control: Controlling access to the original object to ensure that only authorized clients can access certain methods or data.
* Remote Proxy: Providing a local representative for an object in a different address space (e.g., making a remote procedure call).
* Logging or Caching: Intercepting calls to the original object to log operations, cache results, or perform other pre-processing or post-processing tasks.
```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create a proxy for the image
        IImage image = new ImageProxy("example.jpg");

        // Image is loaded only when Display is called
        Console.WriteLine("First call to Display:");
        image.Display(); // Output: Loading image: example.jpg from disk...
                         //         Displaying image: example.jpg

        // Image is already loaded, so no need to load again
        Console.WriteLine("\nSecond call to Display:");
        image.Display(); // Output: Displaying image: example.jpg
    }
}
```
# Prototype pattern
* The Prototype Pattern is useful in situations where creating new instances of a class is expensive, complex, or requires significant resources. By cloning an existing object (a prototype), you can quickly produce new objects without going through the full creation process.
*  Use the Prototype pattern when your code shouldn’t depend on the concrete classes of objects that you need to copy.
*  This happens a lot when your code works with objects passed to you from 3rd-party code via some interface. The concrete classes of these objects are unknown, and you couldn’t depend on them even if you wanted to.
* allows an object to create a deep copy of itself (points to different data)
* The Prototype Pattern is about delegating object creation to the objects themselves. Instead of relying on a factory or a constructor, objects themselves define how they should be cloned.
* There’s a problem with directly cloning objects: you need to know the object's exact class to create a duplicate. This creates a dependency on that specific class in your code, which can make it less flexible.
* Also, sometimes you only know the interface an object uses, not its actual class. For example, a method might accept any object that implements a particular interface, but you won't know the concrete class of those objects.
* The Prototype pattern delegates the cloning process to the actual objects that are being cloned. The pattern declares a common interface for all objects that support cloning. This interface lets you `clone` an object without coupling your code to the class of that object. Usually, such an interface contains just a single `clone` method.
* An object that supports cloning is called a prototype.
* When your objects have dozens of fields and hundreds of possible configurations, cloning them might serve as an alternative to subclassing.

``` csharp
public class Main {
    public static void main(String[] args) {
        // Original shape with a base configuration
        Shape originalShape = new Shape("Circle", 10, 20, "Red");
        originalShape.draw(); // Output: Drawing Circle at position (10, 20) with color Red

        // Clone the shape and modify the configuration
        Shape clonedShape1 = (Shape) originalShape.clone();
        clonedShape1.setPosition(30, 40); // Change position of the cloned shape
        clonedShape1.setColor("Blue");    // Change color of the cloned shape
        clonedShape1.draw();              // Output: Drawing Circle at position (30, 40) with color Blue

        // Clone another shape and modify the configuration differently
        Shape clonedShape2 = (Shape) originalShape.clone();
        clonedShape2.setPosition(50, 60); // Change position of another cloned shape
        clonedShape2.setColor("Green");   // Change color of another cloned shape
        clonedShape2.draw();              // Output: Drawing Circle at position (50, 60) with color Green
    }
}
```

# Iterator pattern
* Benefit: The traversal logic is encapsulated in the iterator, so the client code does not need to understand how the collection is internally structured.
* The Iterator Pattern is a behavioral design pattern that provides a way to access the elements of a collection (such as a list, array, or tree) sequentially without exposing the underlying representation of the collection. 
* `for loop` is an example this
<img width="1089" alt="Screenshot 2024-09-01 at 4 24 58 PM" src="https://github.com/user-attachments/assets/5f228c1c-a15e-4f3f-bcd3-197d0ce3e933">

```java
// Concrete Iterator class
public class BookIterator implements Iterator<Book> {
    private BookCollection collection;
    private int index = 0; // Current position in the collection

    public BookIterator(BookCollection collection) {
        this.collection = collection;
    }

    @Override
    public boolean hasNext() {
        return index < collection.size(); // Checks if there are more elements
    }

    @Override
    public Book next() {
        if (!hasNext()) {
            throw new NoSuchElementException("No more books available.");
        }
        return collection.getBookAt(index++); // Returns the next book and moves the index forward
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        // Create a collection of books
        BookCollection bookCollection = new BookCollection();
        bookCollection.addBook(new Book("1984", "George Orwell"));
        bookCollection.addBook(new Book("To Kill a Mockingbird", "Harper Lee"));
        bookCollection.addBook(new Book("The Great Gatsby", "F. Scott Fitzgerald"));

        // Create an iterator for the book collection
        Iterator<Book> iterator = bookCollection.createIterator();

        // Traverse the collection using the iterator
        while (iterator.hasNext()) {
            Book book = iterator.next();
            System.out.println(book);
        }
    }
```
}
# Mediator
When to Use the Mediator Pattern:
* When a system has many objects that interact with each other, leading to a complex web of dependencies.
* When you want to encapsulate and centralize control logic for how objects interact.
* When you want to promote loose coupling and make the system easier to understand and modify.
* The Mediator Pattern is a behavioral design pattern that facilitates communication between multiple objects by centralizing their interactions through a single mediator object. This pattern helps to reduce the dependencies between the communicating objects, making the system easier to understand, maintain, and extend.
### A popular metaphor is a airport, the planes don't communciate with each other to decide who is gonna get the runway next, they get info from the control tower
<img width="1002" alt="Screenshot 2024-09-01 at 4 41 00 PM" src="https://github.com/user-attachments/assets/55a77a7d-57af-4f02-9d0d-3e26d2d0a39f">
* Imagine a simple chat room where multiple users can send messages to each other. Instead of each user having a direct reference to every other user (which would be a complex web of dependencies), the chat room acts as a mediator to handle the communication between users.

```java
// Mediator interface
public interface IChatRoomMediator
{
    void ShowMessage(User user, string message);
}
```
```java
using System;

// Concrete mediator class
public class ChatRoom : IChatRoomMediator
{
    public void ShowMessage(User user, string message)
    {
        // Display the message with the user's information
        Console.WriteLine($"{DateTime.Now.ToShortTimeString()} [{user.Name}]: {message}");
    }
}
```
* The collegue class (user) instantiates the mediator
```java
// Colleague class
public class User
{
    private string _name;
    private IChatRoomMediator _chatRoom;

    public User(string name, IChatRoomMediator chatRoom)
    {
        _name = name;
        _chatRoom = chatRoom;
    }

    public string Name => _name;

    public void SendMessage(string message)
    {
        _chatRoom.ShowMessage(this, message);
    }
}
```
```java
public class Program
{
    public static void Main(string[] args)
    {
        // Create a mediator
        IChatRoomMediator chatRoom = new ChatRoom();

        // Create users and register them with the mediator
        User user1 = new User("Alice", chatRoom);
        User user2 = new User("Bob", chatRoom);

        // Users send messages via the mediator
        user1.SendMessage("Hello, Bob!");
        user2.SendMessage("Hey, Alice! How are you?");
    }
}
```
```

The Mediator Pattern is a behavioral design pattern that facilitates communication between multiple objects by centralizing their interactions through a single mediator object. This pattern helps to reduce the dependencies between the communicating objects, making the system easier to understand, maintain, and extend.

Purpose of the Mediator Pattern:
Reduce Direct Dependencies:

Objects communicate through the mediator instead of directly with each other, which reduces the number of dependencies between them. This makes the system more loosely coupled and easier to manage.
Centralize Control:

The mediator centralizes the logic that governs how different components interact, improving code organization and reducing the complexity of each individual object.
Simplify Object Interactions:

Instead of having a web of interactions between objects, all communication is channeled through the mediator. This simplifies the relationships and dependencies between the objects.
When to Use the Mediator Pattern:
When a system has many objects that interact with each other, leading to a complex web of dependencies.
When you want to encapsulate and centralize control logic for how objects interact.
When you want to promote loose coupling and make the system easier to understand and modify.
Simple Example of the Mediator Pattern in C#: A Chat Room
Imagine a simple chat room where multiple users can send messages to each other. Instead of each user having a direct reference to every other user (which would be a complex web of dependencies), the chat room acts as a mediator to handle the communication between users.

Step 1: Define the Mediator Interface (IChatRoomMediator)
csharp
Copy code
// Mediator interface
public interface IChatRoomMediator
{
    void ShowMessage(User user, string message);
}
Step 2: Implement the Concrete Mediator (ChatRoom)
csharp
Copy code
using System;

// Concrete mediator class
public class ChatRoom : IChatRoomMediator
{
    public void ShowMessage(User user, string message)
    {
        // Display the message with the user's information
        Console.WriteLine($"{DateTime.Now.ToShortTimeString()} [{user.Name}]: {message}");
    }
}
Step 3: Define the Colleague Class (User)
csharp
Copy code
// Colleague class
public class User
{
    private string _name;
    private IChatRoomMediator _chatRoom;

    public User(string name, IChatRoomMediator chatRoom)
    {
        _name = name;
        _chatRoom = chatRoom;
    }

    public string Name => _name;

    public void SendMessage(string message)
    {
        _chatRoom.ShowMessage(this, message);
    }
}
Step 4: Use the Mediator in Client Code
csharp
Copy code
public class Program
{
    public static void Main(string[] args)
    {
        // Create a mediator
        IChatRoomMediator chatRoom = new ChatRoom();

        // Create users and register them with the mediator
        User user1 = new User("Alice", chatRoom);
        User user2 = new User("Bob", chatRoom);

        // Users send messages via the mediator
        user1.SendMessage("Hello, Bob!");
        user2.SendMessage("Hey, Alice! How are you?");
    }
}
```
```makefile
12:00 PM [Alice]: Hello, Bob!
12:00 PM [Bob]: Hey, Alice! How are you?
```

# Composition vs. Inheritance

<img width="626" alt="Screenshot 2024-09-01 at 5 00 46 PM" src="https://github.com/user-attachments/assets/58384ea0-a336-400c-b28c-b38500325585">

* GOLANG encourages composition over inheritance
* <img width="761" alt="Screenshot 2024-09-01 at 4 55 21 PM" src="https://github.com/user-attachments/assets/09ecba6a-73dc-4a2f-9ac6-ad59d81413b5">

* Composition is a fundamental concept in object-oriented programming (OOP) that refers to building complex objects by combining or "composing" other objects. Rather than inheriting behavior from a parent class (as with inheritance), composition involves creating a class that contains references to other objects, allowing it to reuse and delegate behavior
* Composition is a design principle where a class is made up of one or more objects from other classes. It is often described as a "has-a" relationship.
* For example, a Car class might be composed of several parts like an Engine, Wheel, and Transmission. The Car "has a" Engine, "has a" Wheel, etc.
* In composition, an object is created by including other objects inside it as its members. These member objects are often passed in through the constructor or are set up at runtime, allowing for more flexibility.
  
* Reusability: Allows classes to reuse code by composing objects rather than inheriting from a base class.
* Flexibility: Composed objects can change behavior at runtime by changing their member objects.
* Encapsulation: Keeps objects encapsulated within the class, promoting modular design.

* This is a good example where inheritance breaks down, line and circle add problems for subclasses, lines don't have width, so we have fields in line class that aren't used. A line might have an angle property as well, and we don't want it in the base class since it wont' be used by other shapes
* We want to avoid these complex inheritance chains for somewhat related classes, while still reusing code
<img width="756" alt="Screenshot 2024-09-01 at 4 56 59 PM" src="https://github.com/user-attachments/assets/3dac5596-e3c6-4c8a-8d24-ca9a7f9d4e9d">
<img width="656" alt="Screenshot 2024-09-01 at 5 00 14 PM" src="https://github.com/user-attachments/assets/f96212c3-7733-4740-b596-ca9285a5bb80">

### Inheritance:

* Is-a Relationship: Inheritance creates a relationship between classes where one class (child) is a subtype of another (parent). For example, a Dog class might inherit from an Animal class because a dog "is an" animal.
* Tightly Coupled: Inheritance creates a strong coupling between the child and parent classes. Changes to the parent class can affect all child classes.
* Reuse by Extending: The child class inherits fields and methods from the parent class and can override or extend them.
* Less Flexible: Because inheritance is static, the relationship between parent and child classes is fixed at compile time. It is difficult to change or extend this relationship dynamically.

<img width="600" alt="Screenshot 2024-09-01 at 4 48 00 PM" src="https://github.com/user-attachments/assets/a852f2a9-1905-4c38-8de7-e503a5b919a6">
<img width="947" alt="Screenshot 2024-09-01 at 4 48 24 PM" src="https://github.com/user-attachments/assets/4033ac57-a9c0-4a2b-8e6e-a91b9c030267">


### When to Use Composition Over Inheritance:

### Prefer Composition When:

* You want to create a flexible and dynamic relationship between objects.
* You want to avoid tight coupling between classes.
* You need to change behavior at runtime by swapping out member objects.
* You need to use multiple behaviors in a single class that cannot be easily represented in a single inheritance hierarchy.
  
### Use Inheritance When:

* There is a clear "is-a" relationship, and the child class should inherit and extend the behavior of the parent class.
* You want to use polymorphism to enable substituting objects of a derived class for objects of the base class.
* You need to provide default behavior that can be overridden by subclasses.

```java
// Composition example: Car "has an" Engine
class Engine {
    public void start() {
        System.out.println("Engine started");
    }
}

class Car {
    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine; // Car has an Engine
    }

    public void startCar() {
        engine.start(); // Delegates to the Engine's start method
    }
}

// Usage
Engine engine = new Engine();
Car car = new Car(engine);
car.startCar(); // Output: Engine started
```

```java
// Inheritance example: Dog "is an" Animal
class Animal {
    public void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    public void bark() {
        System.out.println("Dog is barking");
    }
}

// Usage
Dog dog = new Dog();
dog.eat();  // Inherited from Animal
dog.bark(); // Specific to Dog
```
