# Structural Design Patterns

## Adapter

<figure><img src="../.gitbook/assets/adapter_design_pattern.png" alt=""><figcaption></figcaption></figure>

> The **Adapter Pattern** is a **structural design pattern** that allows **incompatible interfaces (Classes/Objects)** to work together by **converting one interface into another** that a client expects.

### When to Use

* Integrating a **legacy system** or **third-party library** that doesn’t match your current interface.
* Reusing existing functionality **without modifying source code**.
* Bridging the gap between **new and old systems** with different interfaces.

### The Problem – Payment Gateway Integration

You have a **checkout system** that expects all payment gateways to expose a `pay(amount)` method.

However, a third-party service like **OldPay** or **LegacyPayService** exposes a completely different interface:

* The method name is `make_payment()`
* Requires different arguments

#### Naive Implementation

```python
class LegacyPayService:
    def make_payment(self, value):
        print(f"Paid {value} using LegacyPay.")

# Your system expects this interface:
class Checkout:
    def __init__(self, payment_gateway):
        self.payment_gateway = payment_gateway

    def process_payment(self, amount):
        self.payment_gateway.pay(amount)  # expects 'pay'

# Trying to use LegacyPay directly
payment = LegacyPayService()
checkout = Checkout(payment)  # Will raise AttributeError: 'LegacyPayService' has no 'pay'
checkout.process_payment(100)

```

#### Why Naivety Is a Problem

* The interface (`make_payment`) doesn’t match what the client (`pay`) expects.
* You can’t change `LegacyPayService` (e.g., it’s third-party or legacy).
* No decoupling — tight integration breaks flexibility.

Challenges in Adapting: Interface mismatch, Closed source, Incompatible contracts.

### Enter: Adapter Pattern

#### Two Types of Adapters:

| Type               | Description                                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| **Object Adapter** | Uses **composition** – wraps the adaptee in a new object and translates interface calls                                    |
| **Class Adapter**  | Uses **inheritance** – adapter subclasses both adaptee and target (works in languages with multiple inheritance, like C++) |

In Python, the **Object Adapter** is most commonly used.

#### Class Diagram

<figure><img src="../.gitbook/assets/class_diagram_adapter_design_pattern.png" alt=""><figcaption></figcaption></figure>

| Component   | Description                                                             |
| ----------- | ----------------------------------------------------------------------- |
| **Client**  | Uses `Target` interface (`pay(amount)`)                                 |
| **Target**  | Interface expected by the client                                        |
| **Adapter** | Converts the interface of the Adaptee to match the Target               |
| **Adaptee** | Existing class with a different interface (e.g., `make_payment(value)`) |

#### Code

```python
# Adaptee (third-party or legacy system)
class LegacyPayService:
    def make_payment(self, value):
        print(f"Paid {value} using LegacyPay.")

# Target Interface (what your system expects)
class PaymentGateway:
    def pay(self, amount):
        raise NotImplementedError

# Adapter
class LegacyPayAdapter(PaymentGateway):
    def __init__(self, legacy_service):
        self.legacy_service = legacy_service

    def pay(self, amount):
        self.legacy_service.make_payment(amount)

# Client (uses Target Interface)
class Checkout:
    def __init__(self, payment_gateway):
        self.payment_gateway = payment_gateway

    def process_payment(self, amount):
        self.payment_gateway.pay(amount)

# Usage
legacy = LegacyPayService()
adapter = LegacyPayAdapter(legacy)
checkout = Checkout(adapter)
checkout.process_payment(100)

```

#### What We Did and Achieved

* Decoupled `Checkout` from specific payment gateways
* Reused legacy code **without modifying it**
* Bridged the mismatch between `pay()` and `make_payment()`
* Achieved **interface compatibility** with clean separation of concerns

### Pros and Cons

| Pros                                                   | Cons                                                  |
| ------------------------------------------------------ | ----------------------------------------------------- |
| Promotes **reusability** of legacy or third-party code | Can add **extra layer of indirection**                |
| No need to modify existing code (adheres to OCP)       | Requires one adapter per incompatible interface       |
| **Improves flexibility and testability**               | Adapter logic may become complex if APIs differ a lot |
| Great for **plug-and-play architecture**               |                                                       |

***

## Decorator



***

## Facade



***

## Composite



***

## Proxy



***

## Bridge



***

## Flyweight

###

***

## &#x20;References
