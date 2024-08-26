# design-patterns-oop-notes

# Notes for interviews
* I have been learning about design patterns, but it's important to know WHEN to use them, if you use them willy nilly, you might just be increasing complexity without positive impact

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
* separation of concerns
* helps join data from multiple different microservices (modularization)
* better for systems that want to execute commands in a queue
* you can scale the read and writes separately 
### When is it NOT beneficial
* when you are altering data and need it back in the same format every time
* when the other pros aren't relevant

Here are some things that CQRS can build into:

<img width="1262" alt="Screenshot 2024-08-26 at 4 25 31 PM" src="https://github.com/user-attachments/assets/5087815a-a72a-416c-9a97-e9dd3d8237af">

# DRY (don't repeat yourself)

# KISS (keep it simple stupid)

# Singleton

# Factory

# builder

# polymorphism 

### overloading

### overwriting

# abstrqction

# encapsulation



# interfaces
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
