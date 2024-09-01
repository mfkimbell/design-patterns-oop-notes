# design-patterns-oop-notes

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
* useful when you need a single instance of a class, such as a logger, or a database connection pool
* need to be careful with concureency (race conditions)
* here's a good race condition exmaple, they both grab the starting amount, BEFORE thread1 has time to update the new total
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

<img width="384" alt="Screenshot 2024-09-01 at 10 40 37 AM" src="https://github.com/user-attachments/assets/21b20819-ae48-404f-bb9b-d20dc87f3a21">

https://www.youtube.com/watch?v=ub0DXaeV6hA

<img width="1020" alt="Screenshot 2024-09-01 at 10 32 02 AM" src="https://github.com/user-attachments/assets/2ba37314-2049-4e92-8a58-3c7b3021b8f0">


<img width="1164" alt="Screenshot 2024-09-01 at 10 36 49 AM" src="https://github.com/user-attachments/assets/0b575503-58a0-4847-b913-b1126dae6774">

- I love this example, it's so we can generate random enemies AT RUNTIME since it can generate any of the classes (as long as they share a superclass)

# Builder

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

# Abstraction

* Abstraction is the process of hiding the complex implementation details of a system and showing only the essential features or functionalities to the user. It focuses on exposing only the relevant parts of an object or class while keeping the rest hidden.

* ***Interfaces and Abstract Classes:*** In OOP, abstraction is often achieved using interfaces and abstract classes. These allow you to define a contract or blueprint that classes must follow without specifying the implementation details.



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


<img width="1176" alt="Screenshot 2024-09-01 at 12 14 35 PM" src="https://github.com/user-attachments/assets/61f0317e-dc25-4ad0-8f3e-6a5271763c66">


<img width="1064" alt="Screenshot 2024-09-01 at 12 00 32 PM" src="https://github.com/user-attachments/assets/0b2f0acc-885d-43a1-92ee-df35ff896e02">

<img width="781" alt="Screenshot 2024-09-01 at 12 02 04 PM" src="https://github.com/user-attachments/assets/1468d280-8b48-420e-8a17-ce35b838f9bf">

<img width="957" alt="Screenshot 2024-09-01 at 12 03 14 PM" src="https://github.com/user-attachments/assets/5d7e9e25-7c0b-43e8-a858-919378fd0370">

<img width="1115" alt="Screenshot 2024-09-01 at 12 04 37 PM" src="https://github.com/user-attachments/assets/6cd94011-0605-44bc-9066-a03a23c8f2d2">

<img width="1138" alt="Screenshot 2024-09-01 at 12 05 25 PM" src="https://github.com/user-attachments/assets/c32ab4c1-5a5b-46e3-87d1-fa1b5bd95314">

<img width="767" alt="Screenshot 2024-09-01 at 12 21 53 PM" src="https://github.com/user-attachments/assets/487c8210-7c31-43c0-96c1-0960ddba3708">

<img width="776" alt="Screenshot 2024-09-01 at 12 25 42 PM" src="https://github.com/user-attachments/assets/49d39411-27fd-41eb-ae64-9bd5e323f7d0">

# Observer pattern

# Composition vs. Inheritance
