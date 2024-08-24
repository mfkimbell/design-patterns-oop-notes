# design-patterns-oop-notes

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
