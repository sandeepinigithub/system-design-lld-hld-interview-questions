# Low Level Design (LLD) Interview Questions & Answers

> **500 Most Asked LLD Interview Questions** — Basic to Advanced  
> Format: Question → Answer | **133+ code snippets**  
> **Snippet Language:** Every code block is labeled with its language (`JavaScript`, `TypeScript`, or `Python`).  
> **Multi-language examples:** Concepts not native to JavaScript include all three languages side by side.

---

## Table of Contents

| # | Section | Questions |
|---|---------|-----------|
| 1 | OOP Fundamentals | Q1–Q60 |
| 2 | SOLID Principles | Q61–Q110 |
| 3 | Creational Design Patterns | Q111–Q160 |
| 4 | Structural Design Patterns | Q161–Q210 |
| 5 | Behavioral Design Patterns | Q211–Q270 |
| 6 | UML & Class Design | Q271–Q310 |
| 7 | Classic LLD Problems | Q311–Q400 |
| 8 | Concurrency & Thread Safety | Q401–Q440 |
| 9 | API, Module & Error Design | Q441–Q470 |
| 10 | Advanced LLD Topics | Q471–Q510 |

**Total: 510 Questions** | Levels: Basic → Intermediate → Advanced

### Multi-Language Code Examples (JS · TS · Python)

| Question | Concept | JS | TS | Python |
|----------|---------|:--:|:--:|:------:|
| Q9 | Method overloading | workaround | native | `@overload` |
| Q11 | Interface | duck typing | `interface` | `Protocol` |
| Q12 | Abstract class | throw in base | `abstract class` | `ABC` |
| Q23 | Duck typing | native | structural typing | native |
| Q67 | Interface Segregation | mixin objects | split interfaces | split Protocols |
| Q106 | DIP without interfaces | duck typing | `interface` | `Protocol` |
| Q107 | OCP payment providers | duck typing | `interface` | `Protocol` |
| Q114 | Factory Method (abstract) | throw in base | `abstract` method | `ABC` |
| Q148 | Factory returns interface | duck typing | return type | `Protocol` |
| Q293 | Class visibility (`protected`) | `#` only | `protected` | `_` convention |
| Q318 | Enum (`Direction`) | frozen object | `enum` | `Enum` |
| Q497 | Generic repository | no generics | `<T>` | `TypeVar` |

---

## 1. OOP Fundamentals

### Q1. What is Object-Oriented Programming (OOP)?
**Level:** Basic

**Answer:**
OOP organizes software around **objects** — bundles of data and behavior. Four pillars: Encapsulation, Abstraction, Inheritance, Polymorphism.

**Snippet Language:** JavaScript

```javascript
class BankAccount {
  #balance = 0;
  deposit(amt) {
    this.#balance += amt;
  }
  getBalance() {
    return this.#balance;
  }
}
```

### Q2. What is Encapsulation?
**Level:** Basic

**Answer:**
Hide internal state; expose controlled access.

**Snippet Language:** JavaScript

```javascript
class User {
  #password;
  constructor(pw) {
    this.#password = pw;
  }
  auth(input) {
    return this.#password === input;
  }
}
```

### Q3. What is Abstraction?
**Level:** Basic

**Answer:**
Expose **what**, hide **how**.

**Snippet Language:** JavaScript

```javascript
class PaymentService {
  pay(amount) {
    this._charge(amount);
  }
  _charge(amount) {
    /* gateway */
  }
}
```

### Q4. What is Inheritance?
**Level:** Basic

**Answer:**
Child reuses/extends parent.

**Snippet Language:** JavaScript

```javascript
class Animal {
  speak() {
    return "sound";
  }
}
class Dog extends Animal {
  speak() {
    return "bark";
  }
}
```

### Q5. What is Polymorphism?
**Level:** Basic

**Answer:**
Same interface, different runtime behavior.

**Snippet Language:** JavaScript

```javascript
class Dog extends Animal {
  speak() {
    return "bark";
  }
}
class Cat extends Animal {
  speak() {
    return "meow";
  }
}
function makeSpeak(animal) {
  return animal.speak();
}
```

### Q6. Class vs Object?
**Level:** Basic

**Answer:**
Class = blueprint. Object = instance of class.

**Snippet Language:** JavaScript

```javascript
class Car {
  constructor(brand) {
    this.brand = brand;
  }
}
const tesla = new Car("Tesla");
```

### Q7. What is a constructor?
**Level:** Basic

**Answer:**
Initializes state on instantiation via `new`.

**Snippet Language:** JavaScript

```javascript
class Product {
  constructor(id, name, price) {
    Object.assign(this, { id, name, price });
  }
}
```

### Q8. Method overriding?
**Level:** Basic

**Answer:**
Subclass provides own implementation of parent method.

**Snippet Language:** JavaScript

```javascript
class Shape {
  area() {
    return 0;
  }
}
class Circle extends Shape {
  constructor(r) {
    super();
    this.r = r;
  }
  area() {
    return Math.PI * this.r ** 2;
  }
}
```

### Q9. Method overloading in JS?
**Level:** Basic

**Answer:**
JavaScript has **no true method overloading** — use default parameters, rest args, or runtime type checks. **TypeScript** supports compile-time overload signatures. **Python** uses `@overload` for type hints with a single implementation.

**Snippet Language:** JavaScript (workaround)
```javascript
function createUser(name, email = null, role = "user") {
  return { name, email, role };
}

// Single function handles all call patterns
createUser("Alice");
createUser("Bob", "bob@example.com");
createUser("Carol", "carol@example.com", "admin");
```

**Snippet Language:** TypeScript (native overloads)
```typescript
interface User {
  name: string;
  email: string | null;
  role: string;
}

function createUser(name: string): User;
function createUser(name: string, email: string): User;
function createUser(name: string, email?: string, role = "user"): User {
  return { name, email: email ?? null, role };
}
```

**Snippet Language:** Python
```python
from typing import overload

@overload
def create_user(name: str) -> dict: ...

@overload
def create_user(name: str, email: str, role: str = "user") -> dict: ...

def create_user(name, email=None, role="user"):
    return {"name": name, "email": email, "role": role}
```

### Q10. Composition over inheritance?
**Level:** Intermediate

**Answer:**
Combine objects/behaviors instead of deep inheritance trees.

**Snippet Language:** JavaScript

```javascript
const canFly = {
  fly() {
    return "flying";
  },
};
class Duck {
  constructor() {
    Object.assign(this, canFly);
  }
}
```

### Q11. What is an interface?
**Level:** Basic

**Answer:**
A **contract without implementation** — defines *what* a type must do, not *how*. JavaScript has no `interface` keyword; it relies on **duck typing**. **TypeScript** and **Python** (`Protocol` / `ABC`) provide explicit contracts.

**Snippet Language:** JavaScript (duck typing)
```javascript
// No interface keyword — any object with a log() method works
class ConsoleLogger {
  log(message) {
    console.log(message);
  }
}

class FileLogger {
  log(message) {
    // write to file
  }
}

function audit(logger, event) {
  logger.log(event); // works if shape matches
}
```

**Snippet Language:** TypeScript (native interface)
```typescript
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(message);
  }
}

function audit(logger: Logger, event: string): void {
  logger.log(event);
}
```

**Snippet Language:** Python (Protocol — structural typing)
```python
from typing import Protocol

class Logger(Protocol):
    def log(self, message: str) -> None: ...

class ConsoleLogger:
    def log(self, message: str) -> None:
        print(message)

def audit(logger: Logger, event: str) -> None:
    logger.log(event)
```

### Q12. Abstract class?
**Level:** Intermediate

**Answer:**
Cannot be instantiated directly; subclasses **must** implement abstract methods. JavaScript has no `abstract` keyword — simulate by throwing in base methods. **TypeScript** has `abstract class`. **Python** uses `abc.ABC`.

**Snippet Language:** JavaScript (simulated)
```javascript
class Processor {
  run() {
    throw new Error("Subclass must implement run()");
  }
}

class ImageProcessor extends Processor {
  run() {
    return "processing image";
  }
}

// new Processor();  // would throw at runtime if run() called
new ImageProcessor().run(); // "processing image"
```

**Snippet Language:** TypeScript (native)
```typescript
abstract class Processor {
  abstract run(): string;
}

class ImageProcessor extends Processor {
  run(): string {
    return "processing image";
  }
}

// const p = new Processor(); // compile error
```

**Snippet Language:** Python (ABC)
```python
from abc import ABC, abstractmethod

class Processor(ABC):
    @abstractmethod
    def run(self) -> str: ...

class ImageProcessor(Processor):
    def run(self) -> str:
        return "processing image"
```

### Q13. Coupling?
**Level:** Basic

**Answer:**
Inter-module dependency. Keep it low.

**Snippet Language:** JavaScript

```javascript
class EmailSvc {
  constructor(p) {
    this.p = p;
  }
}
```

### Q14. Cohesion?
**Level:** Basic

**Answer:**
Focus of module responsibilities. Keep it high.

**Snippet Language:** JavaScript

```javascript
class Calc {
  total(items, t) {
    return items.reduce((s, i) => s + i.p * i.q, 0) * (1 + t);
  }
}
```

### Q15. IS-A vs HAS-A?
**Level:** Basic

**Answer:**
Inheritance vs composition. Prefer HAS-A.

**Snippet Language:** JavaScript

```javascript
class Car {
  constructor() {
    this.engine = {
      start() {
        return "on";
      },
    };
  }
}
```

### Q16. Static methods?
**Level:** Basic

**Answer:**
Belong to class, not instance.

**Snippet Language:** JavaScript

```javascript
class M {
  static add(a, b) {
    return a + b;
  }
}
```

### Q17. Immutable object?
**Level:** Intermediate

**Answer:**
State cannot change after creation.

**Snippet Language:** JavaScript

```javascript
class Money {
  constructor(a, c) {
    Object.freeze(Object.assign(this, { amount: a, currency: c }));
  }
}
```

### Q18. Value object?
**Level:** Intermediate

**Answer:**
Equality by value, not identity.

**Snippet Language:** JavaScript

```javascript
class Email {
  constructor(a) {
    this.a = a.toLowerCase();
  }
  equals(o) {
    return this.a === o.a;
  }
}
```

### Q19. Aggregation vs Composition?
**Level:** Intermediate

**Answer:**
Weak vs strong ownership of contained objects.

### Q20. DTO?
**Level:** Basic

**Answer:**
Data carrier across layers without business logic.

**Snippet Language:** JavaScript

```javascript
class UserDTO {
  constructor(d) {
    Object.assign(this, d);
  }
}
```

### Q21. Namespace/module?
**Level:** Basic

**Answer:**
Group related code; JS modules.

**Snippet Language:** JavaScript

```javascript
export class User {
  constructor(n) {
    this.name = n;
  }
}
```

### Q22. Getter/setter?
**Level:** Basic

**Answer:**
Controlled property access.

**Snippet Language:** JavaScript

```javascript
class T {
  #c = 0;
  get f() {
    return (this.#c * 9) / 5 + 32;
  }
  set c(v) {
    this.#c = v;
  }
}
```

### Q23. Duck typing?
**Level:** Basic

**Answer:**
*"If it walks like a duck and quacks like a duck, it's a duck."* Type is determined by **behavior/shape**, not explicit inheritance. Native in **JavaScript** and **Python**. **TypeScript** uses **structural typing** — compatible if the shape matches, even without `implements`.

**Snippet Language:** JavaScript
```javascript
class Dog {
  speak() {
    return "woof";
  }
}

class Robot {
  speak() {
    return "beep";
  }
}

function announce(entity) {
  return entity.speak(); // no shared class or interface needed
}

announce(new Dog());   // "woof"
announce(new Robot()); // "beep"
```

**Snippet Language:** TypeScript (structural typing)
```typescript
interface Speaker {
  speak(): string;
}

function announce(entity: Speaker): string {
  return entity.speak();
}

// Dog/Robot need NOT declare "implements Speaker"
// — shape match is enough at compile time
class Dog {
  speak() { return "woof"; }
}
```

**Snippet Language:** Python
```python
class Dog:
    def speak(self):
        return "woof"

class Robot:
    def speak(self):
        return "beep"

def announce(entity):
    return entity.speak()  # duck typing — behavior matters, not type name

announce(Dog())    # "woof"
announce(Robot())  # "beep"
```

### Q24. Mixin?
**Level:** Intermediate

**Answer:**
Reusable behavior without inheritance.

**Snippet Language:** JavaScript

```javascript
const L = (B) =>
  class extends B {
    log(m) {
      console.log(m);
    }
  };
```

### Q25. Prototype chain?
**Level:** Basic

**Answer:**
JS inheritance via prototype objects.

**Snippet Language:** JavaScript

```javascript
const base = {
  greet() {
    return "hi";
  },
};
Object.create(base);
```

### Q26. Shallow vs deep clone?
**Level:** Intermediate

**Answer:**
`{...obj}` vs `structuredClone(obj)`.

**Snippet Language:** JavaScript

```javascript
structuredClone({ user: { name: "Alice" } });
```

### Q27. Law of Demeter?
**Level:** Intermediate

**Answer:**
Only talk to immediate collaborators.

**Snippet Language:** JavaScript

```javascript
class Account {
  withdraw(a) {
    if (a > this.balance) throw Error();
    this.balance -= a;
  }
}
```

### Q28. Tell, Don't Ask?
**Level:** Intermediate

**Answer:**
Tell objects what to do; don't query and decide externally.

**Snippet Language:** JavaScript

```javascript
class Order {
  addItem(i) {
    if (this.status !== "PENDING") throw Error();
    this.items.push(i);
  }
}
```

### Q29. Domain model?
**Level:** Intermediate

**Answer:**
Business entities with enforced invariants.

### Q30. Anemic domain model?
**Level:** Intermediate

**Answer:**
Entities without behavior — anti-pattern.

**Snippet Language:** JavaScript

```javascript
class Order {
  #items = [];
  addItem(i) {
    this.#items.push(i);
  }
  total() {
    return this.#items.reduce((s, i) => s + i.price, 0);
  }
}
```

### Q31. Entity vs Value Object?
**Level:** Intermediate

**Answer:**
Identity-based vs value-based objects.

**Snippet Language:** JavaScript

```javascript
class User {
  constructor(id, n) {
    this.id = id;
    this.name = n;
  }
}
```

### Q32. Aggregate Root?
**Level:** Advanced

**Answer:**
Entry point to a cluster of domain objects.

**Snippet Language:** JavaScript

```javascript
class UserRepo {
  constructor(db) {
    this.db = db;
  }
  findById(id) {
    return this.db.find(id);
  }
}
```

### Q33. Repository (concept)?
**Level:** Intermediate

**Answer:**
Abstraction over persistence.

**Snippet Language:** JavaScript

```javascript
Object.seal({ a: 1 });
Object.freeze({ a: 1 });
```

### Q34. Service layer?
**Level:** Intermediate

**Answer:**
Orchestrates business operations.

### Q35. Object.freeze/seal?
**Level:** Intermediate

**Answer:**
Immutability/sealing in JS.

### Q36. Association vs dependency?
**Level:** Intermediate

**Answer:**
Structural vs temporary relationship.

### Q37. Multiplicity in UML?
**Level:** Intermediate

**Answer:**
Cardinality of relationships.

**Snippet Language:** JavaScript

```javascript
class UserSvc {
  create() {}
}
class EmailSvc {
  send() {}
}
```

### Q38. Fat/God class?
**Level:** Basic

**Answer:**
Anti-pattern: one class does everything.

### Q39. Delegation over inheritance?
**Level:** Intermediate

**Answer:**
Forward calls to composed object.

### Q40. Unit of Work?
**Level:** Advanced

**Answer:**
Track changes; commit atomically.

### Q41. Specification pattern?
**Level:** Advanced

**Answer:**
Composable business rule objects.

### Q42. Identity map?
**Level:** Advanced

**Answer:**
Cache entities by ID in session.

### Q43. Covariance/contravariance?
**Level:** Advanced

**Answer:**
Subtype rules for return/param types.

### Q44. When NOT to use OOP?
**Level:** Intermediate

**Answer:**
Simple transforms, scripts — functional may suffice.

### Q45. OOP interview approach?
**Level:** Basic

**Answer:**
Entities → relationships → responsibilities → patterns.

### Q46. Class diagram?
**Level:** Basic

**Answer:**
UML showing structure.

### Q47. Callback vs method?
**Level:** Basic

**Answer:**
Passed function vs object-owned function.

### Q48. Late binding?
**Level:** Intermediate

**Answer:**
Method resolved at runtime in JS.

### Q49. Diamond problem?
**Level:** Advanced

**Answer:**
Multiple inheritance ambiguity.

### Q50. Data class smell?
**Level:** Intermediate

**Answer:**
Only getters/setters, no domain methods.

### Q51. Type vs class?
**Level:** Intermediate

**Answer:**
Compile-time vs runtime construct.

### Q52. Nested/inner class?
**Level:** Intermediate

**Answer:**
Simulated via closures in JS.

### Q53. Generalization/realization?
**Level:** Basic

**Answer:**
Inheritance vs interface implementation in UML.

### Q54. Encapsulation vs data hiding?
**Level:** Basic

**Answer:**
Bundling vs access restriction.

### Q55. Sealed class?
**Level:** Intermediate

**Answer:**
Cannot be subclassed.

### Q56. Primitive obsession?
**Level:** Intermediate

**Answer:**
Use value objects for domain concepts.

### Q57. Parallel inheritance?
**Level:** Advanced

**Answer:**
Coupled class hierarchies — smell.

### Q58. Feature envy?
**Level:** Intermediate

**Answer:**
Method overly uses another class's data.

### Q59. Prefer small classes?
**Level:** Basic

**Answer:**
SRP: easier test, maintain, understand.

---

## 2. SOLID Principles

### Q60. What does SOLID stand for?
**Level:** Basic

**Answer:**
SRP, OCP, LSP, ISP, DIP.

### Q61. Single Responsibility Principle?
**Level:** Basic

**Answer:**
One class, one reason to change.

**Snippet Language:** JavaScript

```javascript
class User {
  constructor(n) {
    this.name = n;
  }
}
class Repo {
  save() {}
}
class Email {
  send() {}
}
```

### Q62. SRP example violation?
**Level:** Intermediate

**Answer:**
User class doing auth + email + DB.

### Q63. Open/Closed Principle?
**Level:** Basic

**Answer:**
Open for extension, closed for modification.

**Snippet Language:** JavaScript

```javascript
class Disc {
  apply(p) {
    return p;
  }
}
class Student extends Disc {
  apply(p) {
    return p * 0.9;
  }
}
```

### Q64. OCP + Strategy?
**Level:** Intermediate

**Answer:**
New strategies extend without modifying context.

### Q65. Liskov Substitution Principle?
**Level:** Intermediate

**Answer:**
Subtypes substitutable for base types.

**Snippet Language:** JavaScript

```javascript
class Fly {
  fly() {
    return "fly";
  }
}
class NoFly {
  walk() {
    return "walk";
  }
}
```

### Q66. LSP violation example?
**Level:** Intermediate

**Answer:**
Penguin extends Bird but can't fly.

### Q67. Interface Segregation Principle?
**Level:** Intermediate

**Answer:**
No **fat interfaces** — split into small, client-specific contracts. JavaScript uses separate mixin objects; **TypeScript** splits `interface`s; **Python** splits `Protocol`s.

**Snippet Language:** JavaScript (small focused shapes)
```javascript
const canWork = { work() { return "working"; } };
const canEat = { eat() { return "eating"; } };

class Human {
  constructor() {
    Object.assign(this, canWork, canEat);
  }
}

class Robot {
  constructor() {
    Object.assign(this, canWork); // robots don't need canEat
  }
}
```

**Snippet Language:** TypeScript (segregated interfaces)
```typescript
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

class Human implements Workable, Eatable {
  work() { console.log("working"); }
  eat() { console.log("eating"); }
}

class Robot implements Workable {
  work() { console.log("working"); }
  // no eat() — not forced by a fat interface
}
```

**Snippet Language:** Python (segregated Protocols)
```python
from typing import Protocol

class Workable(Protocol):
    def work(self) -> None: ...

class Eatable(Protocol):
    def eat(self) -> None: ...

class Human:
    def work(self) -> None: print("working")
    def eat(self) -> None: print("eating")

class Robot:
    def work(self) -> None: print("working")
```

### Q68. Dependency Inversion Principle?
**Level:** Intermediate

**Answer:**
Depend on abstractions, not concretions.

**Snippet Language:** JavaScript

```javascript
class Svc {
  constructor(db) {
    this.db = db;
  }
}
```

### Q69. Dependency Injection?
**Level:** Intermediate

**Answer:**
Pass dependencies in from outside.

**Snippet Language:** JavaScript

```javascript
class NotificationService {
  constructor(sender) {
    this.sender = sender;
  }

  notify(message) {
    this.sender.send(message);
  }
}

const emailNotifier = new NotificationService(new EmailSender());
emailNotifier.notify("Order confirmed!");
```

### Q70. Constructor vs Setter injection?
**Level:** Intermediate

**Answer:**
Required vs optional dependencies.

### Q71. DI Container?
**Level:** Advanced

**Answer:**
Framework wiring dependency graph.

**Snippet Language:** JavaScript

```javascript
class Container {
  constructor() {
    this.services = new Map();
  }

  register(name, factory) {
    this.services.set(name, factory);
  }

  resolve(name) {
    const factory = this.services.get(name);
    return factory(this);
  }
}
```

### Q72. DIP vs DI?
**Level:** Intermediate

**Answer:**
Principle vs technique.

### Q73. Hollywood Principle / IoC?
**Level:** Intermediate

**Answer:**
Framework calls your code.

**Snippet Language:** JavaScript

```javascript
class Framework {
  run(app) {
    app.onInit();
    app.handleRequest();
    app.onDestroy();
  }
}
```

### Q74. God Object anti-pattern?
**Level:** Basic

**Answer:**
Violates SRP — does everything.

### Q75. Shotgun Surgery?
**Level:** Intermediate

**Answer:**
One change, many files.

### Q76. Divergent Change?
**Level:** Intermediate

**Answer:**
Many reasons to change one class.

### Q77. SOLID in microservices?
**Level:** Advanced

**Answer:**
Service boundaries mirror principles.

**Snippet Language:** JavaScript

```javascript
const h = { pdf: exportPdf, csv: exportCsv };
const run = (t, d) => h[t](d);
```

### Q78. When to break SOLID?
**Level:** Advanced

**Answer:**
Prototypes, trivial scripts.

### Q79. SRP in React?
**Level:** Intermediate

**Answer:**
Container vs presentational split.

### Q80. OCP in payments?
**Level:** Intermediate

**Answer:**
New provider class, no checkout edit.

### Q81. LSP in collections?
**Level:** Advanced

**Answer:**
ReadOnly list throwing on add.

### Q82. ISP for notifications?
**Level:** Intermediate

**Answer:**
Separate channel interfaces.

### Q83. DIP in testing?
**Level:** Intermediate

**Answer:**
Inject mocks.

### Q84. SOLID + patterns?
**Level:** Advanced

**Answer:**
Strategy=OCP, Adapter=DIP, etc.

### Q85. Stable Abstractions Principle?
**Level:** Advanced

**Answer:**
Stable = abstract.

### Q86. Acyclic Dependencies?
**Level:** Advanced

**Answer:**
No circular package deps.

### Q87. SRP at module level?
**Level:** Intermediate

**Answer:**
Package by feature.

### Q88. OCP switch anti-pattern?
**Level:** Intermediate

**Answer:**
Replace with polymorphism.

### Q89. LSP formal rule?
**Level:** Advanced

**Answer:**
Weaker preconditions, stronger postconditions.

### Q90. ISP in REST APIs?
**Level:** Advanced

**Answer:**
Granular endpoints.

### Q91. Test LSP?
**Level:** Advanced

**Answer:**
Contract tests on all subtypes.

### Q92. SOLID interview tip?
**Level:** Basic

**Answer:**
Violation + fix example always.

### Q93. SRP vs SoC?
**Level:** Intermediate

**Answer:**
Class-level vs architectural.

### Q94. Service locator vs DI?
**Level:** Advanced

**Answer:**
Locator hides deps — prefer DI.

### Q95. SOLID refactoring steps?
**Level:** Advanced

**Answer:**
Identify axes → extract → inject → split.

### Q96. Over-abstraction risk?
**Level:** Intermediate

**Answer:**
YAGNI — don't SOLID tiny scripts.

### Q97. Fat interface smell?
**Level:** Intermediate

**Answer:**
Split per ISP.

### Q98. Dependency cycles?
**Level:** Advanced

**Answer:**
Break with abstraction/events.

### Q99. SOLID in functional?
**Level:** Advanced

**Answer:**
Small pure functions, HOFs.

### Q100. OCP via plugins?
**Level:** Advanced

**Answer:**
Dynamic module loading.

### Q101. Constructor injection immutability?
**Level:** Intermediate

**Answer:**
Deps set once at creation.

### Q102. Reused Abstractions?
**Level:** Advanced

**Answer:**
Both depend on shared abstraction.

### Q103. Code review SRP signals?
**Level:** Intermediate

**Answer:**
Manager/Handler doing unrelated tasks.

### Q104. Parallel inheritance fix?
**Level:** Advanced

**Answer:**
Composition or Bridge pattern.

### Q105. LSP and exceptions?
**Level:** Advanced

**Answer:**
Subtype shouldn't add unexpected throws.

### Q106. DIP without interfaces?
**Level:** Intermediate

**Answer:**
In JavaScript, **duck typing** replaces explicit interfaces for Dependency Inversion — depend on objects that expose the required methods. **TypeScript** formalizes this with interfaces. **Python** uses `Protocol` or `ABC`.

**Snippet Language:** JavaScript (duck typing)
```javascript
class EmailSender {
  send(to, body) {
    console.log(`Email to ${to}: ${body}`);
  }
}

class NotificationService {
  constructor(sender) {
    this.sender = sender; // any object with send() works
  }
  notify(user, message) {
    this.sender.send(user.email, message);
  }
}

new NotificationService(new EmailSender()).notify(
  { email: "a@b.com" },
  "Hello"
);
```

**Snippet Language:** TypeScript (interface)
```typescript
interface MessageSender {
  send(to: string, body: string): void;
}

class EmailSender implements MessageSender {
  send(to: string, body: string): void {
    console.log(`Email to ${to}: ${body}`);
  }
}

class NotificationService {
  constructor(private sender: MessageSender) {}
  notify(user: { email: string }, message: string): void {
    this.sender.send(user.email, message);
  }
}
```

**Snippet Language:** Python (Protocol)
```python
from typing import Protocol

class MessageSender(Protocol):
    def send(self, to: str, body: str) -> None: ...

class EmailSender:
    def send(self, to: str, body: str) -> None:
        print(f"Email to {to}: {body}")

class NotificationService:
    def __init__(self, sender: MessageSender):
        self.sender = sender

    def notify(self, user: dict, message: str) -> None:
        self.sender.send(user["email"], message)
```

### Q107. OCP payment providers?
**Level:** Intermediate

**Answer:**
Open for extension, closed for modification — add new payment providers without changing existing code. Use a **PaymentProvider** contract. JavaScript relies on duck typing; **TypeScript** uses `interface`; **Python** uses `Protocol`/`ABC`.

**Snippet Language:** JavaScript
```javascript
class StripeProvider {
  pay(amount) {
    return `Stripe charged $${amount}`;
  }
}

class PayPalProvider {
  pay(amount) {
    return `PayPal charged $${amount}`;
  }
}

class Checkout {
  constructor(provider) {
    this.provider = provider;
  }
  process(amount) {
    return this.provider.pay(amount);
  }
}

// Add new provider — no change to Checkout
new Checkout(new StripeProvider()).process(100);
```

**Snippet Language:** TypeScript
```typescript
interface PaymentProvider {
  pay(amount: number): string;
}

class StripeProvider implements PaymentProvider {
  pay(amount: number): string {
    return `Stripe charged $${amount}`;
  }
}

class Checkout {
  constructor(private provider: PaymentProvider) {}
  process(amount: number): string {
    return this.provider.pay(amount);
  }
}
```

**Snippet Language:** Python
```python
from typing import Protocol

class PaymentProvider(Protocol):
    def pay(self, amount: float) -> str: ...

class StripeProvider:
    def pay(self, amount: float) -> str:
        return f"Stripe charged ${amount}"

class Checkout:
    def __init__(self, provider: PaymentProvider):
        self.provider = provider

    def process(self, amount: float) -> str:
        return self.provider.pay(amount)
```

### Q108. SOLID trade-offs?
**Level:** Intermediate

**Answer:**
Indirection vs flexibility.

### Q109. Common mistake?
**Level:** Basic

**Answer:**
Definitions without code examples.

### Q110. Feature Envy fix?
**Level:** Intermediate

**Answer:**
Move method to data owner.

---

## 3. Creational Design Patterns

### Q111. Creational patterns list?
**Level:** Basic

**Answer:**
Singleton, Factory Method, Abstract Factory, Builder, Prototype.

### Q112. Singleton?
**Level:** Basic

**Answer:**
One global instance.

**Snippet Language:** JavaScript

```javascript
class DB {
  static #i;
  static get() {
    return (this.#i ??= new DB());
  }
}
```

### Q113. Singleton problems?
**Level:** Intermediate

**Answer:**
Global state, hard to test.

### Q114. Factory Method?
**Level:** Intermediate

**Answer:**
Subclass decides which concrete type to instantiate. The base **abstract method** pattern is simulated in JavaScript; native in **TypeScript** and **Python**.

**Snippet Language:** JavaScript (throw as abstract)
```javascript
class Dialog {
  createButton() {
    throw new Error("Subclass must implement createButton()");
  }
  render() {
    const btn = this.createButton();
    return btn.render();
  }
}

class WebDialog extends Dialog {
  createButton() {
    return { render: () => "<button>OK</button>" };
  }
}
```

**Snippet Language:** TypeScript (abstract method)
```typescript
abstract class Dialog {
  abstract createButton(): { render(): string };
  render(): string {
    return this.createButton().render();
  }
}

class WebDialog extends Dialog {
  createButton() {
    return { render: () => "<button>OK</button>" };
  }
}
```

**Snippet Language:** Python (ABC)
```python
from abc import ABC, abstractmethod

class Dialog(ABC):
    @abstractmethod
    def create_button(self) -> dict: ...

    def render(self) -> str:
        return self.create_button()["render"]()

class WebDialog(Dialog):
    def create_button(self) -> dict:
        return {"render": lambda: "<button>OK</button>"}
```

### Q115. Simple Factory?
**Level:** Basic

**Answer:**
Central creation function by type param.

**Snippet Language:** JavaScript

```javascript
const m = { email: EmailNotif, sms: SmsNotif };
const create = (t) => new m[t]();
```

### Q116. Abstract Factory?
**Level:** Advanced

**Answer:**
Families of related products.

**Snippet Language:** JavaScript

```javascript
class DF {
  btn() {
    return new DarkBtn();
  }
  check() {
    return new DarkCheck();
  }
}
```

### Q117. Builder?
**Level:** Intermediate

**Answer:**
Fluent step-by-step construction.

**Snippet Language:** JavaScript

```javascript
class ReqBuilder {
  url(u) {
    this.u = u;
    return this;
  }
  method(m) {
    this.m = m;
    return this;
  }
  build() {
    return { url: this.u, method: this.m };
  }
}
```

### Q118. Builder vs Factory?
**Level:** Intermediate

**Answer:**
Factory: which type. Builder: how to configure.

### Q119. Prototype?
**Level:** Intermediate

**Answer:**
Clone existing object as template.

**Snippet Language:** JavaScript

```javascript
const p = {
  greet() {
    return `Hi ${this.name}`;
  },
};
Object.create(p);
```

### Q120. Object Pool?
**Level:** Advanced

**Answer:**
Reuse expensive instances.

**Snippet Language:** JavaScript

```javascript
class Pool {
  constructor(max, f) {
    this.max = max;
    this.pool = [];
    this.f = f;
  }
  acquire() {
    return this.pool.pop() || this.f();
  }
}
```

### Q121. When Factory?
**Level:** Basic

**Answer:**
Runtime type selection, complex creation.

### Q122. When Builder?
**Level:** Intermediate

**Answer:**
Many optional constructor params.

### Q123. Telescoping constructor?
**Level:** Intermediate

**Answer:**
Too many overloads — use Builder.

**Snippet Language:** JavaScript

```javascript
class UB {
  constructor(n) {
    this.d = { name: n };
  }
  email(e) {
    this.d.email = e;
    return this;
  }
  build() {
    return this.d;
  }
}
```

### Q124. Lazy init?
**Level:** Intermediate

**Answer:**
Create on first access.

**Snippet Language:** JavaScript

```javascript
class H {
  static #i;
  static get() {
    return (this.#i ??= new H());
  }
}
```

### Q125. Multiton?
**Level:** Advanced

**Answer:**
One instance per key.

**Snippet Language:** JavaScript

```javascript
const inst = new Map();
const get = (k) => inst.get(k) || inst.set(k, {}).get(k);
```

### Q126. Static factory method?
**Level:** Intermediate

**Answer:**
`User.fromJSON(data)` named constructors.

**Snippet Language:** JavaScript

```javascript
class User {
  static fromJSON(j) {
    return new User(JSON.parse(j).name);
  }
}
```

### Q127. Fluent interface?
**Level:** Intermediate

**Answer:**
Method chaining returns `this`.

**Snippet Language:** JavaScript

```javascript
class QB {
  from(t) {
    this.t = t;
    return this;
  }
  where(c) {
    this.c = c;
    return this;
  }
  build() {
    return `SELECT * FROM ${this.t} WHERE ${this.c}`;
  }
}
```

### Q128. Director in Builder?
**Level:** Advanced

**Answer:**
Orchestrates build steps.

### Q129. Abstract Factory vs Factory Method?
**Level:** Advanced

**Answer:**
Product family vs single product.

### Q130. Prototype deep clone?
**Level:** Intermediate

**Answer:**
`structuredClone` for full copy.

### Q131. Singleton in ES modules?
**Level:** Intermediate

**Answer:**
Module evaluated once = natural singleton.

### Q132. Builder validation?
**Level:** Intermediate

**Answer:**
Validate in `build()`, throw if invalid.

### Q133. Factory + OCP?
**Level:** Intermediate

**Answer:**
Registry of creators avoids switch edits.

### Q134. Creational smell?
**Level:** Basic

**Answer:**
Scattered `new` keywords.

### Q135. Pool exhaustion?
**Level:** Advanced

**Answer:**
Block, timeout, or scale.

### Q136. Factory async?
**Level:** Advanced

**Answer:**
`await Factory.create()` for async deps.

### Q137. Bill Pugh / enum singleton?
**Level:** Advanced

**Answer:**
JVM idioms; JS uses module scope.

### Q138. Prototype registry?
**Level:** Advanced

**Answer:**
Named prototypes for cloning.

### Q139. DI as creational?
**Level:** Advanced

**Answer:**
Container is meta-factory.

### Q140. Interview Logger design?
**Level:** Advanced

**Answer:**
DI-scoped + Strategy formatters + Factory appenders.

### Q141. Anti-pattern new everywhere?
**Level:** Basic

**Answer:**
Centralize in factories.

### Q142. Registry pattern?
**Level:** Advanced

**Answer:**
Named factory lookup map.

### Q143. Flyweight relation?
**Level:** Advanced

**Answer:**
Shared instances via factory.

### Q144. Config factory?
**Level:** Intermediate

**Answer:**
`Config.for(env)` returns env object.

### Q145. Anonymous factory IIFE?
**Level:** Intermediate

**Answer:**
Closure returning configured instance.

### Q146. Modern Singleton alt?
**Level:** Intermediate

**Answer:**
DI-scoped singleton per request.

### Q147. Builder immutable result?
**Level:** Advanced

**Answer:**
Freeze object in `build()`.

### Q148. Factory returns interface?
**Level:** Intermediate

**Answer:**
Factory hides concrete class — callers depend on a **contract**, not implementation. JavaScript uses duck typing; **TypeScript** returns an `interface` type; **Python** returns a `Protocol` type.

**Snippet Language:** JavaScript
```javascript
function createNotifier(type) {
  const types = {
    email: () => ({ send(msg) { console.log(`Email: ${msg}`); } }),
    sms: () => ({ send(msg) { console.log(`SMS: ${msg}`); } }),
  };
  return types[type](); // caller only needs .send()
}

const notifier = createNotifier("email");
notifier.send("Order shipped");
```

**Snippet Language:** TypeScript
```typescript
interface Notifier {
  send(message: string): void;
}

function createNotifier(type: "email" | "sms"): Notifier {
  const types: Record<string, () => Notifier> = {
    email: () => ({ send: (msg) => console.log(`Email: ${msg}`) }),
    sms: () => ({ send: (msg) => console.log(`SMS: ${msg}`) }),
  };
  return types[type]();
}
```

**Snippet Language:** Python
```python
from typing import Protocol

class Notifier(Protocol):
    def send(self, message: str) -> None: ...

class EmailNotifier:
    def send(self, message: str) -> None:
        print(f"Email: {message}")

class SmsNotifier:
    def send(self, message: str) -> None:
        print(f"SMS: {message}")

def create_notifier(type_: str) -> Notifier:
    return {"email": EmailNotifier, "sms": SmsNotifier}[type_]()
```

### Q149. Creational in React?
**Level:** Intermediate

**Answer:**
`createContext`, `createRoot`.

### Q150. Parking lot VehicleFactory?
**Level:** Intermediate

**Answer:**
`create('car'|'bike')`.

### Q151. Simple Factory OCP issue?
**Level:** Intermediate

**Answer:**
Switch grows — use registry.

### Q152. Test factories?
**Level:** Intermediate

**Answer:**
Mock factory interface in tests.

### Q153. Creational GoF count?
**Level:** Basic

**Answer:**
Five patterns.

### Q154. Object pool use case?
**Level:** Advanced

**Answer:**
DB connections, threads.

### Q155. Builder director combo meal?
**Level:** Advanced

**Answer:**
Director calls builder steps in order.

### Q156. Factory Method dialog example?
**Level:** Intermediate

**Answer:**
WinDialog vs WebDialog create buttons.

### Q157. Lazy singleton thread-safe?
**Level:** Advanced

**Answer:**
JS single-threaded; use locks in Java.

### Q158. Creational MVVM?
**Level:** Advanced

**Answer:**
ViewModel factory per view.

### Q159. Clone vs new?
**Level:** Intermediate

**Answer:**
Clone copies existing state; new starts fresh.

### Q160. Factory pattern diagram?
**Level:** Basic

**Answer:**
Creator → Product inheritance.

### Q161. Abstract factory UI theme?
**Level:** Advanced

**Answer:**
DarkThemeFactory vs LightThemeFactory.

**Snippet Language:** JavaScript

```javascript
class Legacy {
  fetch() {
    return "old";
  }
}
class Adapter {
  constructor(l) {
    this.l = l;
  }
  get() {
    return this.l.fetch();
  }
}
```

---

## 4. Structural Design Patterns

### Q162. Structural patterns list?
**Level:** Basic

**Answer:**
Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy.

**Snippet Language:** JavaScript

```javascript
class Facade {
  order(u, i) {
    this.auth.verify(u);
    return this.pay.charge(u, i);
  }
}
```

### Q163. Adapter Pattern?
**Level:** Intermediate

**Answer:**
Convert interface to one client expects.

**Snippet Language:** JavaScript

```javascript
class OldAPI {
  fetchData() {
    return "old";
  }
}
class Adapter {
  constructor(old) {
    this.old = old;
  }
  get() {
    return this.old.fetchData();
  }
}
```

### Q164. Decorator Pattern?
**Level:** Intermediate

**Answer:**
Add behavior dynamically.

**Snippet Language:** JavaScript

```javascript
class Coffee {
  cost() {
    return 5;
  }
}
class MilkDecorator {
  constructor(c) {
    this.c = c;
  }
  cost() {
    return this.c.cost() + 2;
  }
}
```

### Q165. Facade Pattern?
**Level:** Basic

**Answer:**
Simplified interface to complex subsystem.

**Snippet Language:** JavaScript

```javascript
class N {
  constructor(s) {
    this.s = s;
  }
  notify(m) {
    return this.s.send(m);
  }
}
```

### Q166. Proxy Pattern?
**Level:** Intermediate

**Answer:**
Surrogate controlling access.

**Snippet Language:** JavaScript

```javascript
class RealImage {
  constructor(p) {
    this.p = p;
    this.load();
  }
  load() {
    /* heavy */
  }
  display() {}
}
class ImageProxy {
  constructor(p) {
    this.p = p;
    this.real = null;
  }
  display() {
    this.real ??= new RealImage(this.p);
    this.real.display();
  }
}
```

### Q167. Bridge Pattern?
**Level:** Advanced

**Answer:**
Separate abstraction from implementation.

### Q168. Composite Pattern?
**Level:** Intermediate

**Answer:**
Tree structure treated uniformly.

**Snippet Language:** JavaScript

```javascript
class File {
  constructor(n) {
    this.n = n;
  }
}
class Folder {
  constructor(n) {
    this.n = n;
    this.children = [];
  }
  add(c) {
    this.children.push(c);
  }
}
```

### Q169. Flyweight Pattern?
**Level:** Advanced

**Answer:**
Share intrinsic state across many objects.

### Q170. Adapter class vs object?
**Level:** Intermediate

**Answer:**
Class adapter: inheritance. Object adapter: composition (preferred).

### Q171. Decorator vs inheritance?
**Level:** Intermediate

**Answer:**
Decorator composes at runtime; inheritance is compile-time.

### Q172. Proxy types?
**Level:** Intermediate

**Answer:**
Virtual (lazy), protection (auth), remote (RPC), caching.

### Q173. Facade vs Mediator?
**Level:** Intermediate

**Answer:**
Facade simplifies API. Mediator coordinates peers.

### Q174. Composite file system?
**Level:** Intermediate

**Answer:**
File and Folder both implement `getSize()`.

### Q175. Bridge remote control?
**Level:** Advanced

**Answer:**
Remote abstraction + Device implementation.

### Q176. Flyweight text editor?
**Level:** Advanced

**Answer:**
Share character glyph objects.

### Q177. Decorator streams?
**Level:** Intermediate

**Answer:**
Node.js `zlib.createGzip()` wraps streams.

### Q178. Proxy caching?
**Level:** Intermediate

**Answer:**
Cache expensive computation results.

### Q179. Adapter legacy integration?
**Level:** Intermediate

**Answer:**
Wrap old SOAP service for REST client.

### Q180. Structural in React HOC?
**Level:** Intermediate

**Answer:**
HOC is Decorator pattern.

### Q181. Facade payment subsystem?
**Level:** Intermediate

**Answer:**
`PaymentFacade.charge()` hides gateway, fraud, ledger.

### Q182. Composite menu UI?
**Level:** Intermediate

**Answer:**
Menu contains MenuItems and sub-Menus.

### Q183. Bridge vs Strategy?
**Level:** Advanced

**Answer:**
Bridge separates dimensions; Strategy swaps algorithm.

### Q184. Decorator pizza toppings?
**Level:** Basic

**Answer:**
Each topping wraps base pizza.

### Q185. Proxy auth check?
**Level:** Intermediate

**Answer:**
Proxy validates token before real object.

### Q186. Flyweight factory?
**Level:** Advanced

**Answer:**
Manages shared object pool.

### Q187. Adapter two interfaces?
**Level:** Intermediate

**Answer:**
Translate method names and params.

### Q188. Facade microservices?
**Level:** Advanced

**Answer:**
API Gateway as Facade to backend services.

### Q189. Composite transparency?
**Level:** Intermediate

**Answer:**
Leaf and composite share same interface.

### Q190. Decorator logging?
**Level:** Intermediate

**Answer:**
Wrap service with logging decorator.

### Q191. Proxy lazy loading ORM?
**Level:** Advanced

**Answer:**
Lazy-load related entities via proxy.

### Q192. Bridge notification?
**Level:** Advanced

**Answer:**
Notification × DeliveryChannel (email/sms).

### Q193. Structural vs behavioral?
**Level:** Basic

**Answer:**
Structural: composition. Behavioral: communication.

### Q194. Wrapper vs Adapter?
**Level:** Intermediate

**Answer:**
Often synonymous; adapter emphasizes interface conversion.

### Q195. Decorator stack?
**Level:** Intermediate

**Answer:**
Multiple decorators chain behavior.

### Q196. Facade testing?
**Level:** Intermediate

**Answer:**
Mock facade's subsystems individually.

### Q197. Composite iterator?
**Level:** Intermediate

**Answer:**
Traverse tree uniformly.

### Q198. Protection proxy?
**Level:** Intermediate

**Answer:**
Role-based access control.

### Q199. Virtual proxy image?
**Level:** Intermediate

**Answer:**
Load full-res only when displayed.

### Q200. Remote proxy?
**Level:** Advanced

**Answer:**
Local stub for remote service.

### Q201. Flyweight extrinsic state?
**Level:** Advanced

**Answer:**
Context-specific state passed in, not stored.

### Q202. Bridge JDBC?
**Level:** Advanced

**Answer:**
Driver abstraction + DB implementation.

### Q203. Decorator middleware Express?
**Level:** Intermediate

**Answer:**
Each middleware decorates request handling.

### Q204. Adapter third-party SDK?
**Level:** Intermediate

**Answer:**
Wrap SDK to match your interface.

### Q205. Composite organizational chart?
**Level:** Intermediate

**Answer:**
Employee and Department both report structure.

### Q206. Facade home theater?
**Level:** Basic

**Answer:**
One `watchMovie()` powers on all devices.

### Q207. Structural interview tip?
**Level:** Basic

**Answer:**
Draw before/after class diagram.

### Q208. Proxy vs Decorator?
**Level:** Intermediate

**Answer:**
Proxy controls access; Decorator adds behavior.

### Q209. Adapter in LLD?
**Level:** Intermediate

**Answer:**
Integrate external payment gateway.

### Q210. Bridge extensibility?
**Level:** Advanced

**Answer:**
Add new implementation without changing abstraction.

### Q211. Composite pricing?
**Level:** Intermediate

**Answer:**
Package price = sum of component prices.

### Q212. Flyweight memory savings?
**Level:** Advanced

**Answer:**
Thousands of objects share few shared instances.

**Snippet Language:** JavaScript

```javascript
class File {
  constructor(n, s) {
    this.n = n;
    this.s = s;
  }
  getSize() {
    return this.s;
  }
}
class Dir {
  constructor() {
    this.c = [];
  }
  getSize() {
    return this.c.reduce((a, f) => a + f.getSize(), 0);
  }
}
```

---

## 5. Behavioral Design Patterns

### Q213. Behavioral patterns list?
**Level:** Basic

**Answer:**
Observer, Strategy, Command, State, Template Method, Iterator, Chain of Responsibility, Mediator, Memento, Visitor, Interpreter.

**Snippet Language:** JavaScript

```javascript
class ChatRoom {
  send(m, from, to) {
    to.receive(m, from);
  }
}
```

### Q214. Observer Pattern?
**Level:** Basic

**Answer:**
Pub-sub: subjects notify observers.

**Snippet Language:** JavaScript

```javascript
class EventEmitter {
  constructor() {
    this.listeners = {};
  }
  on(e, fn) {
    (this.listeners[e] ??= []).push(fn);
  }
  emit(e, data) {
    (this.listeners[e] || []).forEach((fn) => fn(data));
  }
}
```

### Q215. Strategy Pattern?
**Level:** Basic

**Answer:**
Interchangeable algorithms.

**Snippet Language:** JavaScript

```javascript
const strategies = {
  road: (a, b) => a + b,
  highway: (a, b) => a * 0.8 + b,
};
function route(type, a, b) {
  return strategies[type](a, b);
}
```

### Q216. Command Pattern?
**Level:** Intermediate

**Answer:**
Encapsulate request as object.

**Snippet Language:** JavaScript

```javascript
class Command {
  constructor(exec, undo) {
    this.exec = exec;
    this.undo = undo;
  }
}
class Invoker {
  constructor() {
    this.history = [];
  }
  run(cmd) {
    cmd.exec();
    this.history.push(cmd);
  }
  undo() {
    this.history.pop()?.undo();
  }
}
```

### Q217. State Pattern?
**Level:** Intermediate

**Answer:**
Object behavior changes with internal state.

**Snippet Language:** JavaScript

```javascript
class Locked {
  unlock(d) {
    d.state = new Unlocked();
  }
}
class Unlocked {
  lock(d) {
    d.state = new Locked();
  }
}
class Door {
  constructor() {
    this.state = new Locked();
  }
  lock() {
    this.state.lock(this);
  }
}
```

### Q218. Template Method?
**Level:** Intermediate

**Answer:**
Skeleton algorithm; subclasses fill steps.

**Snippet Language:** JavaScript

```javascript
class DataParser {
  parse(raw) {
    const d = this.extract(raw);
    return this.transform(d);
  }
  extract(raw) {
    throw new Error("implement");
  }
  transform(d) {
    return d;
  }
}
```

### Q219. Chain of Responsibility?
**Level:** Intermediate

**Answer:**
Pass request along handler chain.

**Snippet Language:** JavaScript

```javascript
class Handler {
  setNext(h) {
    this.next = h;
    return h;
  }
  handle(req) {
    return this.next?.handle(req);
  }
}
```

### Q220. Iterator Pattern?
**Level:** Basic

**Answer:**
Traverse collection without exposing internals.

**Snippet Language:** JavaScript

```javascript
class Range {
  constructor(max) {
    this.max = max;
  }
  *[Symbol.iterator]() {
    for (let i = 0; i < this.max; i++) yield i;
  }
}
```

### Q221. Mediator Pattern?
**Level:** Intermediate

**Answer:**
Central coordinator reduces peer coupling.

**Snippet Language:** JavaScript

```javascript
class ChatRoom {
  send(msg, from, to) {
    to.receive(msg, from);
  }
}
```

### Q222. Memento Pattern?
**Level:** Advanced

**Answer:**
Save/restore object state.

**Snippet Language:** JavaScript

```javascript
class EditorMemento {
  constructor(content) {
    this.content = content;
  }
}
class Editor {
  save() {
    return new EditorMemento(this.content);
  }
  restore(m) {
    this.content = m.content;
  }
}
```

### Q223. Visitor Pattern?
**Level:** Advanced

**Answer:**
Separate algorithm from object structure.

### Q224. Strategy vs State?
**Level:** Intermediate

**Answer:**
Strategy: client chooses algorithm. State: object changes behavior internally.

### Q225. Observer vs Pub/Sub?
**Level:** Intermediate

**Answer:**
Observer: direct coupling. Pub/Sub: message broker decouples.

### Q226. Command undo/redo?
**Level:** Intermediate

**Answer:**
Store command history; undo reverses.

### Q227. State vending machine?
**Level:** Intermediate

**Answer:**
Classic LLD: states handle transitions.

### Q228. Template Method HTTP client?
**Level:** Intermediate

**Answer:**
Base defines request flow; subclass sets auth/parse.

### Q229. Chain middleware?
**Level:** Intermediate

**Answer:**
Express/Koa middleware chain.

### Q230. Iterator custom collection?
**Level:** Intermediate

**Answer:**
Implement `[Symbol.iterator]`.

### Q231. Mediator chat?
**Level:** Intermediate

**Answer:**
Users don't message directly — via ChatRoom.

### Q232. Memento undo stack?
**Level:** Advanced

**Answer:**
Editor undo restores previous memento.

### Q233. Observer DOM events?
**Level:** Basic

**Answer:**
addEventListener is Observer.

### Q234. Strategy sorting?
**Level:** Basic

**Answer:**
Pluggable compare functions.

### Q235. Command macro?
**Level:** Advanced

**Answer:**
Composite command runs many commands.

### Q236. State pattern vs if-else?
**Level:** Intermediate

**Answer:**
State: each state is a class; cleaner transitions.

### Q237. Template Method test?
**Level:** Intermediate

**Answer:**
Test base flow with mock subclass steps.

### Q238. Chain auth handlers?
**Level:** Intermediate

**Answer:**
Auth → RateLimit → Validation → Handler.

### Q239. Iterator fail-fast?
**Level:** Advanced

**Answer:**
Detect concurrent modification during iteration.

### Q240. Mediator air traffic?
**Level:** Advanced

**Answer:**
Tower mediates plane communication.

### Q241. Visitor AST traversal?
**Level:** Advanced

**Answer:**
Lint rules visit AST nodes.

### Q242. Interpreter pattern?
**Level:** Advanced

**Answer:**
Evaluate grammar/sentences. Regex, SQL parsers.

### Q243. Null Object pattern?
**Level:** Intermediate

**Answer:**
No-op object instead of null checks.

### Q244. Specification + Strategy?
**Level:** Advanced

**Answer:**
Rules as composable strategy objects.

### Q245. Observer memory leak?
**Level:** Intermediate

**Answer:**
Unsubscribe listeners on destroy.

### Q246. Strategy payment?
**Level:** Intermediate

**Answer:**
CreditCard, PayPal, UPI strategies.

### Q247. Command queue?
**Level:** Intermediate

**Answer:**
Job queue executes commands async.

### Q248. State game character?
**Level:** Intermediate

**Answer:**
Idle, Running, Jumping states.

### Q249. Template Method in frameworks?
**Level:** Intermediate

**Answer:**
React lifecycle hooks pattern.

### Q250. Chain servlet filters?
**Level:** Intermediate

**Answer:**
Java filter chain = CoR.

### Q251. Behavioral vs creational?
**Level:** Basic

**Answer:**
Creational: birth. Behavioral: collaboration.

### Q252. Observer RxJS?
**Level:** Advanced

**Answer:**
Reactive streams extend Observer.

### Q253. Strategy compression?
**Level:** Intermediate

**Answer:**
Zip, Gzip, Brotli strategies.

### Q254. Command transaction?
**Level:** Advanced

**Answer:**
All commands succeed or rollback.

### Q255. State TCP connection?
**Level:** Advanced

**Answer:**
Established, Listen, Closed states.

### Q256. Mediator Redux?
**Level:** Advanced

**Answer:**
Store mediates components.

**Snippet Language:** JavaScript

```javascript
class TL {
  constructor() {
    this.s = "RED";
  }
  next() {
    const n = { RED: "GREEN", GREEN: "YELLOW", YELLOW: "RED" };
    return (this.s = n[this.s]);
  }
}
```

### Q257. Memento caretaker?
**Level:** Advanced

**Answer:**
Caretaker stores mementos, not originator.

### Q258. Visitor double dispatch?
**Level:** Advanced

**Answer:**
Accept(visitor) + visit(element).

### Q259. Behavioral interview tip?
**Level:** Basic

**Answer:**
Explain who talks to whom.

### Q260. Observer stock ticker?
**Level:** Intermediate

**Answer:**
Investors subscribe to price updates.

### Q261. Strategy route navigation?
**Level:** Intermediate

**Answer:**
Fastest vs shortest vs toll-free.

### Q262. Command smart home?
**Level:** Intermediate

**Answer:**
TurnOnLightCommand with undo.

### Q263. State document workflow?
**Level:** Intermediate

**Answer:**
Draft → Review → Published.

### Q264. Chain of approval?
**Level:** Intermediate

**Answer:**
Manager → Director → VP approval chain.

### Q265. Iterator graph BFS?
**Level:** Advanced

**Answer:**
Custom iterator for graph traversal.

### Q266. Mediator UI components?
**Level:** Intermediate

**Answer:**
Form mediator coordinates inputs.

### Q267. Null Object logger?
**Level:** Intermediate

**Answer:**
NoOpLogger when logging disabled.

### Q268. Interpreter calc?
**Level:** Advanced

**Answer:**
Parse and eval `1 + 2 * 3`.

### Q269. Event-driven architecture?
**Level:** Advanced

**Answer:**
Observer pattern at system level.

### Q270. Strategy validation?
**Level:** Intermediate

**Answer:**
Different validation per country.

### Q271. Command batch?
**Level:** Intermediate

**Answer:**
Execute all or none.

### Q272. State lift/elevator?
**Level:** Intermediate

**Answer:**
MovingUp, MovingDown, Stopped.

### Q273. Behavioral patterns count?
**Level:** Basic

**Answer:**
GoF: 11 behavioral patterns.

---

## 6. UML & Class Design

### Q274. What is UML?
**Level:** Basic

**Answer:**
Unified Modeling Language — visual software design notation.

### Q275. Class diagram elements?
**Level:** Basic

**Answer:**
Classes, attributes, methods, relationships.

### Q276. Association?
**Level:** Basic

**Answer:**
Structural relationship between classes.

### Q277. Aggregation?
**Level:** Intermediate

**Answer:**
Hollow diamond — weak whole-part.

### Q278. Composition?
**Level:** Intermediate

**Answer:**
Filled diamond — strong whole-part.

### Q279. Generalization?
**Level:** Basic

**Answer:**
Inheritance — hollow triangle arrow.

### Q280. Realization?
**Level:** Basic

**Answer:**
Interface implementation — dashed arrow.

### Q281. Dependency?
**Level:** Basic

**Answer:**
Dashed arrow — temporary use.

### Q282. Multiplicity notation?
**Level:** Intermediate

**Answer:**
1, *, 0..1, 1..* on association ends.

### Q283. Sequence diagram?
**Level:** Intermediate

**Answer:**
Shows message flow over time between objects.

### Q284. Activity diagram?
**Level:** Intermediate

**Answer:**
Workflow/flowchart of activities.

### Q285. State diagram?
**Level:** Intermediate

**Answer:**
Object state transitions.

### Q286. Use case diagram?
**Level:** Basic

**Answer:**
Actors and system interactions.

### Q287. Component diagram?
**Level:** Intermediate

**Answer:**
High-level system components.

### Q288. Deployment diagram?
**Level:** Advanced

**Answer:**
Physical deployment of artifacts.

### Q289. Sequence diagram lifeline?
**Level:** Intermediate

**Answer:**
Vertical dashed line per participant.

### Q290. Sequence sync vs async?
**Level:** Advanced

**Answer:**
Filled arrow = sync. Open arrow = async.

### Q291. Activity fork/join?
**Level:** Advanced

**Answer:**
Parallel activities in workflow.

### Q292. State initial/final?
**Level:** Intermediate

**Answer:**
Black dot = initial. Bullseye = final.

### Q293. Class visibility?
**Level:** Basic

**Answer:**
UML: `+` public, `-` private, `#` protected. JavaScript only has `#` private fields (no `protected`). **TypeScript** adds `public`/`private`/`protected` (compile-time). **Python** uses naming conventions (`_`, `__`).

**Snippet Language:** JavaScript (`#` private only)
```javascript
class BankAccount {
  #balance = 0; // truly private field

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}
```

**Snippet Language:** TypeScript (public / private / protected)
```typescript
class BankAccount {
  public accountId: string;
  protected balance: number = 0; // visible to subclasses
  private pin: string;

  constructor(id: string, pin: string) {
    this.accountId = id;
    this.pin = pin;
  }

  deposit(amount: number): void {
    this.balance += amount;
  }
}
```

**Snippet Language:** Python (convention-based)
```python
class BankAccount:
    def __init__(self, account_id: str, pin: str):
        self.account_id = account_id   # public
        self._balance = 0              # protected (convention)
        self.__pin = pin               # name-mangled private

    def deposit(self, amount: float) -> None:
        self._balance += amount
```

### Q294. Interface in UML?
**Level:** Basic

**Answer:**
`<<interface>>` stereotype — contract with method signatures, no implementation. See **Q11** for JavaScript (duck typing), TypeScript (`interface`), and Python (`Protocol`) examples.

### Q295. Abstract class notation?
**Level:** Basic

**Answer:**
Italic name or `{abstract}` in UML. See **Q12** for JavaScript (simulated), TypeScript (`abstract class`), and Python (`ABC`) examples.

### Q296. Navigability?
**Level:** Intermediate

**Answer:**
Arrow showing which direction association is traversed.

### Q297. Self-message in sequence?
**Level:** Intermediate

**Answer:**
Arrow looping on same lifeline.

### Q298. Alt frame sequence?
**Level:** Intermediate

**Answer:**
Conditional branches [if]/[else].

### Q299. Loop frame sequence?
**Level:** Intermediate

**Answer:**
Repeated messages.

### Q300. Object diagram?
**Level:** Advanced

**Answer:**
Snapshot of object instances at a moment.

### Q301. Package diagram?
**Level:** Intermediate

**Answer:**
Groups classes into packages.

### Q302. ER diagram vs class?
**Level:** Intermediate

**Answer:**
ER: data model. Class: OO structure.

### Q303. CRC cards?
**Level:** Intermediate

**Answer:**
Class-Responsibility-Collaborator brainstorming tool.

### Q304. UML in LLD interview?
**Level:** Basic

**Answer:**
Draw class + sequence for key flows.

### Q305. When to skip UML?
**Level:** Basic

**Answer:**
Quick whiteboard — boxes and arrows suffice.

### Q306. Tool for UML?
**Level:** Basic

**Answer:**
draw.io, Lucidchart, PlantUML.

### Q307. Sequence for login?
**Level:** Intermediate

**Answer:**
User → Controller → Service → DB → back.

### Q308. Class diagram parking lot?
**Level:** Intermediate

**Answer:**
ParkingLot, Spot, Vehicle, Ticket.

### Q309. Composition example diagram?
**Level:** Basic

**Answer:**
House ◆— Room.

### Q310. Dependency injection in UML?
**Level:** Intermediate

**Answer:**
Dashed arrow to interface.

### Q311. Bounded context?
**Level:** Advanced

**Answer:**
DDD: diagram per domain context.

### Q312. UML aggregation example?
**Level:** Intermediate

**Answer:**
Department ◇— Employee.

### Q313. Notes in UML?
**Level:** Basic

**Answer:**
Folded corner comment box.

---

## 7. Classic LLD Problems

### Q314. Design Parking Lot — entities?
**Level:** Intermediate

**Answer:**
ParkingLot, Floor, ParkingSpot, Vehicle (Car/Bike/Truck), Ticket, Payment, EntranceGate, ExitGate.

**Snippet Language:** JavaScript

```javascript
class ParkingLot {
  constructor(spots) {
    this.spots = spots;
    this.tickets = new Map();
  }

  park(vehicle) {
    const spot = this.spots.find((s) => !s.occupied);
    if (!spot) return null;

    spot.occupied = true;
    const ticket = { id: Date.now(), spot, entryTime: Date.now() };
    this.tickets.set(ticket.id, ticket);
    return ticket;
  }
}
```

### Q315. Parking Lot — spot assignment?
**Level:** Intermediate

**Answer:**
Strategy: nearest, largest-fit, handicapped. SpotFactory by vehicle type.

### Q316. Parking Lot — concurrency?
**Level:** Intermediate

**Answer:**
Lock spot on entry; release on exit. Queue if full.

### Q317. Parking Lot JS skeleton?
**Level:** Intermediate

**Answer:**
**Snippet Language:** JavaScript

```javascript
class ParkingLot {
  constructor(floors) {
    this.floors = floors;
    this.tickets = new Map();
  }
  park(vehicle) {
    const spot = this.findSpot(vehicle);
    if (!spot) return null;
    spot.park(vehicle);
    const ticket = new Ticket(spot, Date.now());
    this.tickets.set(ticket.id, ticket);
    return ticket;
  }
  exit(ticketId) {
    const t = this.tickets.get(ticketId);
    t.spot.unpark();
    return this.calcFee(t);
  }
}
```

### Q318. Design Elevator System?
**Level:** Intermediate

**Answer:**
Elevator, Floor, Request, Controller, **Direction enum**, scheduling algorithm. JavaScript has no native `enum` — use frozen objects. **TypeScript** and **Python** have native enums.

**Snippet Language:** JavaScript (const object)
```javascript
const Direction = Object.freeze({
  UP: "UP",
  DOWN: "DOWN",
  IDLE: "IDLE",
});

class Elevator {
  constructor(id) {
    this.id = id;
    this.floor = 0;
    this.direction = Direction.IDLE;
    this.requests = [];
  }

  addRequest(floor, direction) {
    this.requests.push({ floor, direction });
  }
}
```

**Snippet Language:** TypeScript (native enum)
```typescript
enum Direction {
  UP = "UP",
  DOWN = "DOWN",
  IDLE = "IDLE",
}

class Elevator {
  constructor(
    public id: number,
    public floor = 0,
    public direction: Direction = Direction.IDLE,
    public requests: { floor: number; direction: Direction }[] = []
  ) {}

  addRequest(floor: number, direction: Direction): void {
    this.requests.push({ floor, direction });
  }
}
```

**Snippet Language:** Python (Enum)
```python
from enum import Enum

class Direction(Enum):
    UP = "UP"
    DOWN = "DOWN"
    IDLE = "IDLE"

class Elevator:
    def __init__(self, elevator_id: int):
        self.id = elevator_id
        self.floor = 0
        self.direction = Direction.IDLE
        self.requests: list[dict] = []

    def add_request(self, floor: int, direction: Direction) -> None:
        self.requests.append({"floor": floor, "direction": direction})
```

### Q319. Elevator scheduling?
**Level:** Advanced

**Answer:**
SCAN/LOOK algorithm, priority for emergencies.

### Q320. Elevator JS state?
**Level:** Intermediate

**Answer:**
**Snippet Language:** JavaScript

```javascript
class Elevator {
  constructor(id, floors) {
    this.id = id;
    this.floor = 0;
    this.direction = "IDLE";
    this.requests = [];
  }
  addRequest(floor, dir) {
    this.requests.push({ floor, dir });
  }
  step() {
    /* move one floor toward target */
  }
}
```

### Q321. Design Library Management?
**Level:** Intermediate

**Answer:**
Book, Member, Librarian, Catalog, Loan, Reservation, Fine.

**Snippet Language:** JavaScript

```javascript
class Book {
  constructor(i, t) {
    this.isbn = i;
    this.title = t;
  }
}
```

### Q322. Library — search?
**Level:** Intermediate

**Answer:**
Search by ISBN, author, title via Catalog index.

### Q323. Library — loan rules?
**Level:** Intermediate

**Answer:**
Max books, due date, fine calculation in LoanService.

### Q324. Design ATM?
**Level:** Intermediate

**Answer:**
ATM, Card, Account, Transaction, CashDispenser, PinValidator, BankService.

### Q325. ATM state machine?
**Level:** Intermediate

**Answer:**
Idle → CardInserted → PinEntered → Transaction → EjectCard.

### Q326. ATM JS?
**Level:** Intermediate

**Answer:**
**Snippet Language:** JavaScript

```javascript
class ATM {
  constructor(bank) {
    this.bank = bank;
    this.state = "IDLE";
  }
  insertCard(card) {
    this.card = card;
    this.state = "PIN";
  }
  withdraw(amount) {
    if (!this.bank.validate(this.card, amount)) throw Error("fail");
    return this.bank.debit(this.card.account, amount);
  }
}
```

### Q327. Design Chess Game?
**Level:** Advanced

**Answer:**
Board, Piece (King/Queen/...), Move, Player, Game, MoveValidator.

### Q328. Chess — move validation?
**Level:** Advanced

**Answer:**
Strategy per piece type. Check/checkmate detection.

### Q329. Design Tic-Tac-Toe?
**Level:** Basic

**Answer:**
Board 3x3, Player X/O, Game checks win/draw after each move.

### Q330. Tic-Tac-Toe JS?
**Level:** Basic

**Answer:**
**Snippet Language:** JavaScript

```javascript
class Board {
  constructor() {
    this.cells = Array(9).fill(null);
  }
  move(i, p) {
    if (this.cells[i]) return false;
    this.cells[i] = p;
    return true;
  }
  winner() {
    const wins = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];
    return wins
      .find(
        ([a, b, c]) =>
          this.cells[a] &&
          this.cells[a] === this.cells[b] &&
          this.cells[b] === this.cells[c],
      )
      ?.map((i) => this.cells[i])[0];
  }
}
```

### Q331. Design Snake & Ladder?
**Level:** Basic

**Answer:**
Board, Player, Dice, Snake, Ladder, Game loop.

### Q332. Design Vending Machine?
**Level:** Intermediate

**Answer:**
Product, Inventory, Coin inventory, State pattern for coin insertion.

### Q333. Vending Machine states?
**Level:** Intermediate

**Answer:**
Idle, HasMoney, Dispensing, ReturnChange.

**Snippet Language:** JavaScript

```javascript
class VM {
  constructor() {
    this.bal = 0;
  }
  coin(a) {
    this.bal += a;
  }
}
```

### Q334. Design Hotel Booking?
**Level:** Intermediate

**Answer:**
Room, Guest, Reservation, Payment, AvailabilityCalendar.

### Q335. Hotel — double booking?
**Level:** Advanced

**Answer:**
Optimistic lock on reservation slot; transaction on book.

### Q336. Design Movie Ticket Booking?
**Level:** Intermediate

**Answer:**
Cinema, Screen, Show, Seat, Booking, Payment — seat lock during checkout.

**Snippet Language:** JavaScript

```javascript
class LRU {
  constructor(c) {
    this.c = c;
    this.m = new Map();
  }
  get(k) {
    if (!this.m.has(k)) return -1;
    const v = this.m.get(k);
    this.m.delete(k);
    this.m.set(k, v);
    return v;
  }
  put(k, v) {
    if (this.m.has(k)) this.m.delete(k);
    this.m.set(k, v);
    if (this.m.size > this.c) this.m.delete(this.m.keys().next().value);
  }
}
```

### Q337. Seat locking strategy?
**Level:** Intermediate

**Answer:**
Temporary hold (5 min TTL) then release.

**Snippet Language:** JavaScript

```javascript
class TokenBucket {
  constructor(rate, capacity) {
    this.rate = rate;
    this.capacity = capacity;
    this.tokens = capacity;
    this.lastRefill = Date.now();
  }

  allow() {
    const now = Date.now();
    this.tokens = Math.min(
      this.capacity,
      this.tokens + ((now - this.lastRefill) / 1000) * this.rate,
    );
    this.lastRefill = now;

    if (this.tokens >= 1) {
      this.tokens--;
      return true;
    }
    return false;
  }
}
```

### Q338. Design Restaurant Order System?
**Level:** Intermediate

**Answer:**
Menu, Order, OrderItem, KitchenQueue, Bill, Table.

**Snippet Language:** JavaScript

```javascript
class Logger {
  constructor(a) {
    this.a = a;
  }
  log(l, m) {
    this.a.forEach((x) => x.write(l, m));
  }
}
```

### Q339. Design Food Delivery (LLD)?
**Level:** Intermediate

**Answer:**
Customer, Restaurant, Order, DeliveryAgent, Tracker, Rating.

**Snippet Language:** JavaScript

```javascript
class Short {
  constructor() {
    this.m = new Map();
    this.n = 1;
  }
  shorten(u) {
    const c = (this.n++).toString(36);
    this.m.set(c, u);
    return c;
  }
}
```

### Q340. Design Splitwise?
**Level:** Intermediate

**Answer:**
User, Group, Expense, Split (equal/exact/%), BalanceSheet.

**Snippet Language:** JavaScript

```javascript
class Broker {
  constructor() {
    this.t = new Map();
  }
  sub(topic, cb) {
    (this.t.get(topic) ?? this.t.set(topic, []).get(topic)).push(cb);
  }
  pub(topic, m) {
    (this.t.get(topic) || []).forEach((cb) => cb(m));
  }
}
```

### Q341. Splitwise balance?
**Level:** Advanced

**Answer:**
Minimize transactions via debt simplification graph.

**Snippet Language:** JavaScript

```javascript
class KV {
  constructor() {
    this.s = new Map();
  }
  set(k, v) {
    this.s.set(k, v);
  }
  get(k) {
    return this.s.get(k) ?? null;
  }
}
```

### Q342. Design Stack Overflow (LLD)?
**Level:** Intermediate

**Answer:**
User, Question, Answer, Vote, Comment, Tag, Reputation.

### Q343. Design LinkedIn (LLD)?
**Level:** Advanced

**Answer:**
Profile, Connection, Job, Message, FeedPost, Notification.

### Q344. Design Amazon Cart (LLD)?
**Level:** Intermediate

**Answer:**
Cart, CartItem, Product, Inventory, PricingService, Checkout.

### Q345. Design LRU Cache?
**Level:** Intermediate

**Answer:**
HashMap + Doubly Linked List for O(1) get/put.

**Snippet Language:** JavaScript

```javascript
class LRUCache {
  constructor(cap) {
    this.cap = cap;
    this.map = new Map();
  }
  get(key) {
    if (!this.map.has(key)) return -1;
    const v = this.map.get(key);
    this.map.delete(key);
    this.map.set(key, v);
    return v;
  }
  put(key, val) {
    if (this.map.has(key)) this.map.delete(key);
    this.map.set(key, val);
    if (this.map.size > this.cap) {
      const first = this.map.keys().next().value;
      this.map.delete(first);
    }
  }
}
```

### Q346. Design Rate Limiter (LLD)?
**Level:** Intermediate

**Answer:**
Token bucket or sliding window.

**Snippet Language:** JavaScript

```javascript
class TokenBucket {
  constructor(rate, cap) {
    this.rate = rate;
    this.cap = cap;
    this.tokens = cap;
    this.last = Date.now();
  }
  allow() {
    const now = Date.now();
    this.tokens = Math.min(
      this.cap,
      this.tokens + ((now - this.last) / 1000) * this.rate,
    );
    this.last = now;
    if (this.tokens >= 1) {
      this.tokens--;
      return true;
    }
    return false;
  }
}
```

### Q347. Design Logger (LLD)?
**Level:** Intermediate

**Answer:**
Logger, Appender (File/Console), Formatter, LogLevel, singleton or DI.

**Snippet Language:** JavaScript

```javascript
class MinStack {
  constructor() {
    this.s = [];
    this.m = [];
  }
  push(v) {
    this.s.push(v);
    this.m.push(Math.min(this.m.at(-1) ?? v, v));
  }
}
```

### Q348. Design Task Scheduler?
**Level:** Intermediate

**Answer:**
Task, PriorityQueue, Worker, Cron expression parser.

**Snippet Language:** JavaScript

```javascript
class CmdMgr {
  constructor() {
    this.u = [];
  }
  run(c) {
    c.exec();
    this.u.push(c);
  }
  undo() {
    this.u.pop()?.undo();
  }
}
```

### Q349. Design URL Shortener (LLD)?
**Level:** Intermediate

**Answer:**
UrlMap, Base62Encoder, Counter/Hash, RedirectService, Analytics.

### Q350. Design Pub-Sub System (LLD)?
**Level:** Intermediate

**Answer:**
Topic, Publisher, Subscriber, Message, Broker, Subscription.

**Snippet Language:** JavaScript

```javascript
class Wallet {
  constructor() {
    this.b = 0;
    this.t = [];
  }
  credit(a, k) {
    if (this.t.find((x) => x.k === k)) return;
    this.b += a;
    this.t.push({ a, k });
  }
}
```

### Q351. Design Key-Value Store (LLD)?
**Level:** Intermediate

**Answer:**
Store interface, InMemoryStore, Eviction policy, TTL support.

### Q352. Design File System?
**Level:** Intermediate

**Answer:**
Node (File/Directory), Path resolver, Size calculation via Composite.

### Q353. Design Meeting Scheduler?
**Level:** Intermediate

**Answer:**
User, Meeting, Room, Calendar, ConflictDetector.

### Q354. Design Car Rental?
**Level:** Intermediate

**Answer:**
Vehicle, Reservation, Customer, Pricing, Availability.

**Snippet Language:** JavaScript

```javascript
class CB {
  constructor(t = 5) {
    this.f = 0;
    this.t = t;
    this.s = "CLOSED";
  }
  call(fn) {
    if (this.s === "OPEN") throw Error();
    try {
      const r = fn();
      this.f = 0;
      return r;
    } catch (e) {
      if (++this.f >= this.t) this.s = "OPEN";
      throw e;
    }
  }
}
```

### Q355. Design Course Registration?
**Level:** Intermediate

**Answer:**
Student, Course, Section, Waitlist, Prerequisite check.

### Q356. Design Stack with getMin O(1)?
**Level:** Intermediate

**Answer:**
Two stacks or store pair (val, minSoFar).

### Q357. Design Deck of Cards?
**Level:** Basic

**Answer:**
Card, Deck, shuffle (Fisher-Yates), Hand for players.

### Q358. Design Parking Lot fees?
**Level:** Intermediate

**Answer:**
Strategy: hourly, flat, weekend rates. Factory for pricing.

### Q359. Design Book My Show concurrency?
**Level:** Advanced

**Answer:**
Pessimistic seat lock during payment window.

### Q360. Design Cache with TTL?
**Level:** Intermediate

**Answer:**
Store expiry timestamp; lazy delete on access or background sweep.

### Q361. Design Undo/Redo?
**Level:** Intermediate

**Answer:**
Command pattern with two stacks.

### Q362. Design Traffic Light?
**Level:** Basic

**Answer:**
State pattern: Red, Yellow, Green with timers.

### Q363. Design Digital Wallet (LLD)?
**Level:** Intermediate

**Answer:**
Wallet, Transaction, Idempotency key, Balance invariant.

### Q364. Design Notification Service (LLD)?
**Level:** Intermediate

**Answer:**
Notification, Channel strategy, Template, User preferences.

### Q365. Design Inventory Management?
**Level:** Intermediate

**Answer:**
SKU, Warehouse, StockMovement, ReorderPoint alert.

### Q366. Design Ride-sharing matching (LLD)?
**Level:** Advanced

**Answer:**
Rider, Driver, Trip, Matcher by geo proximity.

### Q367. Design Chat App (LLD)?
**Level:** Intermediate

**Answer:**
User, Conversation, Message, ReadReceipt, TypingIndicator.

### Q368. Design Forum (LLD)?
**Level:** Intermediate

**Answer:**
Thread, Post, Moderator, Flag, Karma.

### Q369. Design Calendar (LLD)?
**Level:** Intermediate

**Answer:**
Event, Recurrence rule, Reminder, Timezone handling.

### Q370. Design Shopping Discount engine?
**Level:** Advanced

**Answer:**
Rule chain: coupon, bulk, member tier. Strategy per rule.

### Q371. Design Multi-level cache?
**Level:** Advanced

**Answer:**
L1 in-memory, L2 Redis — read-through write-through.

### Q372. Design Connection Pool?
**Level:** Advanced

**Answer:**
Pool with min/max, acquire timeout, idle eviction.

### Q373. Design Event Bus (in-process)?
**Level:** Intermediate

**Answer:**
Sync/async event dispatch with typed handlers.

### Q374. Design Plugin system?
**Level:** Advanced

**Answer:**
Load plugins dynamically; define Plugin interface.

### Q375. Design Form Builder?
**Level:** Advanced

**Answer:**
Field types, validation rules, conditional visibility.

### Q376. Design Workflow Engine?
**Level:** Advanced

**Answer:**
State machine with transitions, guards, actions.

### Q377. Design Report Generator?
**Level:** Intermediate

**Answer:**
Template Method: fetch → transform → format (PDF/CSV).

### Q378. Design Permission System (LLD)?
**Level:** Intermediate

**Answer:**
User, Role, Permission, RBAC check via RoleResolver.

### Q379. Design Audit Log?
**Level:** Intermediate

**Answer:**
Append-only log; Observer on domain events.

### Q380. Design Idempotent API handler?
**Level:** Advanced

**Answer:**
Store request ID; return cached response on duplicate.

### Q381. Design Retry with backoff?
**Level:** Intermediate

**Answer:**
Exponential backoff + jitter wrapper.

### Q382. Design Circuit Breaker (LLD)?
**Level:** Advanced

**Answer:**
Closed → Open → Half-Open states.

### Q383. Design Object Pool for DB?
**Level:** Advanced

**Answer:**
Acquire/release with health check.

### Q384. Design Thread Pool (concept)?
**Level:** Advanced

**Answer:**
Worker queue + fixed workers (Node: worker_threads).

### Q385. Design Batch Processor?
**Level:** Intermediate

**Answer:**
Read chunk → process → commit offset.

### Q386. Design Feature Flags (LLD)?
**Level:** Intermediate

**Answer:**
FlagService evaluates rules per user/context.

### Q387. Design Search Autocomplete (LLD)?
**Level:** Intermediate

**Answer:**
Trie or prefix index with ranking.

### Q388. Design Leaderboard?
**Level:** Intermediate

**Answer:**
Sorted set; score updates; tie-breaking.

### Q389. Design Coupon System?
**Level:** Intermediate

**Answer:**
Coupon, constraints (min cart, expiry), redemption tracking.

### Q390. Design Subscription billing?
**Level:** Advanced

**Answer:**
Plan, Invoice, Renewal job, Proration.

### Q391. Design Multi-tenant SaaS (LLD)?
**Level:** Advanced

**Answer:**
TenantId on all entities; TenantContext middleware.

### Q392. Design API versioning (LLD)?
**Level:** Intermediate

**Answer:**
URL path /v1, header, or content negotiation.

### Q393. Design Validation framework?
**Level:** Intermediate

**Answer:**
Chain of validators per field.

### Q394. Design ORM (simplified)?
**Level:** Advanced

**Answer:**
Entity mapping, query builder, lazy relations.

### Q395. Design Rule Engine?
**Level:** Advanced

**Answer:**
Facts + Rules → inference. Forward chaining.

### Q396. Design Stock exchange order matching?
**Level:** Advanced

**Answer:**
OrderBook with buy/sell priority queues.

### Q397. Design Parking Lot display board?
**Level:** Intermediate

**Answer:**
Observer on spot changes updates board.

### Q398. Design Cache stampede prevention?
**Level:** Advanced

**Answer:**
Mutex per key, early expiration jitter.

### Q399. Design Serializable transaction isolation?
**Level:** Advanced

**Answer:**
Lock rows on read/write.

### Q400. Design Gift card system?
**Level:** Intermediate

**Answer:**
Card, balance, partial redemption, expiry.

### Q401. Design Queue with priority?
**Level:** Intermediate

**Answer:**
Heap-based priority queue.

**Snippet Language:** JavaScript

```javascript
class Mutex {
  constructor() {
    this.c = Promise.resolve();
  }
  run(fn) {
    this.c = this.c.then(fn);
    return this.c;
  }
}
```

### Q402. Design Barcode/QR ticket?
**Level:** Intermediate

**Answer:**
Ticket encodes signed payload; verify on scan.

### Q403. Design Hand of Cards game?
**Level:** Intermediate

**Answer:**
Deal, play valid card, turn rotation.

### Q404. Design Minesweeper?
**Level:** Intermediate

**Answer:**
Board, Cell, reveal, flag, mine placement.

### Q405. Design Sudoku validator?
**Level:** Basic

**Answer:**
Check row, col, box constraints.

### Q406. LLD interview structure?
**Level:** Basic

**Answer:**
1) Requirements 2) Entities 3) Class diagram 4) Key flows 5) Patterns 6) Edge cases.

### Q407. LLD — functional vs non-functional?
**Level:** Basic

**Answer:**
Functional: features. Non-functional: scale, latency — mention but focus LLD on structure.

### Q408. LLD edge cases?
**Level:** Intermediate

**Answer:**
Null input, concurrency, invalid state transitions, overflow.

### Q409. LLD extensibility?
**Level:** Intermediate

**Answer:**
Show OCP: where new types plug in without edits.

### Q410. LLD testing approach?
**Level:** Intermediate

**Answer:**
Unit test entities, integration test flows.

---

## 8. Concurrency & Thread Safety

### Q411. Thread vs Process?
**Level:** Basic

**Answer:**
Process: isolated memory. Thread: shared memory within process.

### Q412. Race condition?
**Level:** Intermediate

**Answer:**
Outcome depends on timing of concurrent access.

### Q413. Mutex/Mutual Exclusion?
**Level:** Intermediate

**Answer:**
Only one thread accesses critical section.

### Q414. Semaphore?
**Level:** Intermediate

**Answer:**
Counting lock; allow N concurrent accessors.

### Q415. Deadlock?
**Level:** Advanced

**Answer:**
Circular wait for resources. Prevention: ordering, timeouts.

### Q416. Livelock?
**Level:** Advanced

**Answer:**
Threads actively respond but make no progress.

### Q417. Thread safety?
**Level:** Intermediate

**Answer:**
Correct behavior with concurrent access.

### Q418. Immutable objects thread-safe?
**Level:** Basic

**Answer:**
Yes — no mutation means no races on state.

### Q419. JS concurrency model?
**Level:** Intermediate

**Answer:**
Single-threaded event loop + worker_threads for parallelism.

### Q420. async/await role?
**Level:** Intermediate

**Answer:**
Non-blocking I/O without thread-per-request.

### Q421. Promise vs callback?
**Level:** Basic

**Answer:**
Promise: chainable, composable async.

### Q422. Event loop?
**Level:** Intermediate

**Answer:**
Microtasks (promises) before macrotasks (setTimeout).

### Q423. Worker threads Node?
**Level:** Advanced

**Answer:**
**Snippet Language:** JavaScript

```javascript
const { Worker } = require("worker_threads");
new Worker("./task.js");
```

### Q424. Optimistic vs pessimistic locking?
**Level:** Advanced

**Answer:**
Optimistic: version check on commit. Pessimistic: lock on read.

### Q425. CAS (Compare-And-Swap)?
**Level:** Advanced

**Answer:**
Atomic update if value unchanged.

### Q426. Read-write lock?
**Level:** Advanced

**Answer:**
Multiple readers OR one writer.

### Q427. Concurrent HashMap?
**Level:** Advanced

**Answer:**
Segmented locks or lock-free buckets.

### Q428. Thread pool benefits?
**Level:** Intermediate

**Answer:**
Avoid thread creation cost; bound concurrency.

### Q429. Producer-Consumer?
**Level:** Intermediate

**Answer:**
Queue between producers and consumers.

### Q430. Blocking queue?
**Level:** Intermediate

**Answer:**
Consumer blocks when empty; producer blocks when full.

### Q431. Atomic operations?
**Level:** Intermediate

**Answer:**
Indivisible operations — no partial state visible.

### Q432. volatile keyword (Java)?
**Level:** Advanced

**Answer:**
Visibility guarantee across threads. JS: Atomics for SharedArrayBuffer.

### Q433. Memory model?
**Level:** Advanced

**Answer:**
Rules for when writes visible to other threads.

### Q434. Lock ordering deadlock fix?
**Level:** Intermediate

**Answer:**
Always acquire locks in same global order.

### Q435. Try-lock?
**Level:** Intermediate

**Answer:**
Attempt lock without blocking; fallback strategy.

### Q436. Spinlock?
**Level:** Advanced

**Answer:**
Busy-wait for short critical sections.

### Q437. Condition variable?
**Level:** Advanced

**Answer:**
Wait for signal when condition met.

### Q438. CountDownLatch?
**Level:** Advanced

**Answer:**
Threads wait until count reaches zero.

### Q439. CyclicBarrier?
**Level:** Advanced

**Answer:**
Threads wait at barrier point.

### Q440. Future/Promise concurrency?
**Level:** Intermediate

**Answer:**
Represents async result.

### Q441. async mutex in JS?
**Level:** Intermediate

**Answer:**
Use async-mutex npm or queue serial execution.

**Snippet Language:** JavaScript

```javascript
function errHandler(e, req, res, n) {
  res.status(e.status || 500).json({ error: e.message });
}
```

### Q442. Concurrent LRU?
**Level:** Advanced

**Answer:**
Per-segment locks or single lock — trade throughput.

**Snippet Language:** JavaScript

```javascript
app.use("/api/v1", v1Router);
app.use("/api/v2", v2Router);
```

### Q443. Double-checked locking?
**Level:** Advanced

**Answer:**
Lazy init with reduced locking. Careful with memory ordering.

**Snippet Language:** JavaScript

```javascript
function validate(s) {
  return (req, res, n) => {
    const { error } = s.validate(req.body);
    if (error) return res.status(400).json({ error: error.message });
    n();
  };
}
```

### Q444. Thread-local storage?
**Level:** Advanced

**Answer:**
Per-thread data copy.

**Snippet Language:** JavaScript

```javascript
const ok = (d) => ({ success: true, data: d });
const fail = (c, m) => ({
  success: false,
  error: { code: c, message: m },
});
```

### Q445. Green threads vs OS threads?
**Level:** Advanced

**Answer:**
Green: user-space scheduled (goroutines). OS: kernel scheduled.

**Snippet Language:** JavaScript

```javascript
class RBAC {
  has(u, p) {
    return (this.roles[u.role] || []).includes(p);
  }
}
```

### Q446. Actor model?
**Level:** Advanced

**Answer:**
Isolated actors message-pass; no shared state.

### Q447. CSP (Communicating Sequential Processes)?
**Level:** Advanced

**Answer:**
Go channels — don't share memory, communicate.

### Q448. Lock-free data structures?
**Level:** Advanced

**Answer:**
CAS-based; no mutex.

### Q449. ABA problem?
**Level:** Advanced

**Answer:**
CAS sees same value but object changed. Fix: versioned pointers.

### Q450. Priority inversion?
**Level:** Advanced

**Answer:**
Low-priority holds lock high-priority needs.

---

## 9. API, Module & Error Design

### Q451. RESTful API design principles?
**Level:** Basic

**Answer:**
Resources, HTTP verbs, stateless, proper status codes.

### Q452. Idempotent HTTP methods?
**Level:** Intermediate

**Answer:**
GET, PUT, DELETE idempotent. POST is not.

### Q453. API error response format?
**Level:** Intermediate

**Answer:**
`{ error: { code, message, details } }` consistent schema.

### Q454. Versioning strategies?
**Level:** Intermediate

**Answer:**
URL /v1, header Accept-Version, query param.

### Q455. Pagination?
**Level:** Intermediate

**Answer:**
Offset/limit or cursor-based for large datasets.

### Q456. Rate limiting headers?
**Level:** Intermediate

**Answer:**
X-RateLimit-Limit, Remaining, Reset.

### Q457. HATEOAS?
**Level:** Advanced

**Answer:**
Hypermedia links in responses for discoverability.

### Q458. DTO vs Entity in API?
**Level:** Intermediate

**Answer:**
Never expose internal entities; map to DTOs.

### Q459. Input validation layer?
**Level:** Basic

**Answer:**
Validate at API boundary before domain logic.

### Q460. Error handling strategy?
**Level:** Intermediate

**Answer:**
Central error middleware; typed errors.

### Q461. Module cohesion?
**Level:** Intermediate

**Answer:**
One module = one domain capability.

### Q462. Barrel exports?
**Level:** Basic

**Answer:**
index.js re-exports module public API.

### Q463. Circular dependency fix?
**Level:** Intermediate

**Answer:**
Extract shared interface; lazy require.

### Q464. Facade module?
**Level:** Intermediate

**Answer:**
Public API hides internal module complexity.

### Q465. 12-factor app config?
**Level:** Intermediate

**Answer:**
Config in environment variables.

### Q466. Health check endpoint?
**Level:** Basic

**Answer:**
GET /health returns status of dependencies.

### Q467. Graceful shutdown?
**Level:** Intermediate

**Answer:**
Stop accepting requests; drain in-flight; close connections.

### Q468. Structured logging?
**Level:** Intermediate

**Answer:**
JSON logs with traceId, level, timestamp.

### Q469. API authentication (LLD)?
**Level:** Intermediate

**Answer:**
Middleware validates JWT/API key before handler.

### Q470. Request validation middleware?
**Level:** Intermediate

**Answer:**
Schema validate body/params (Zod/Joi).

### Q471. Response wrapper?
**Level:** Intermediate

**Answer:**
Consistent `{ data, meta, errors }` envelope.

**Snippet Language:** JavaScript

```javascript
class OrderSvc {
  constructor(r, p, n) {
    this.r = r;
    this.p = p;
    this.n = n;
  }
  place(o) {
    this.r.save(o);
    this.p.charge(o.total);
    this.n.send(o.uid);
  }
}
```

### Q472. Content negotiation?
**Level:** Advanced

**Answer:**
Accept header for JSON vs XML.

**Snippet Language:** JavaScript

```javascript
class OrderPlaced {
  constructor(id) {
    this.id = id;
  }
}
eventBus.pub(new OrderPlaced(1));
```

### Q473. Webhook design?
**Level:** Advanced

**Answer:**
Signed payload, retry with backoff, idempotency.

**Snippet Language:** JavaScript

```javascript
class CmdSvc {
  create() {}
}
class QrySvc {
  get() {}
}
```

### Q474. OpenAPI/Swagger role?
**Level:** Intermediate

**Answer:**
Contract documentation and client generation.

**Snippet Language:** JavaScript

```javascript
class ES {
  append(id, e) {
    this.e.push({ id, e });
  }
  rebuild(id) {
    return this.e.filter((x) => x.id === id);
  }
}
```

### Q475. API backward compatibility?
**Level:** Intermediate

**Answer:**
Additive changes only; deprecate old fields.

**Snippet Language:** JavaScript

```javascript
async function save(o, db) {
  await db.tx(async (t) => {
    await t.insert("orders", o);
    await t.insert("outbox", { type: "Created", o });
  });
}
```

### Q476. Bulk operations API?
**Level:** Advanced

**Answer:**
POST /items/bulk with partial success reporting.

### Q477. Long polling vs WebSocket?
**Level:** Intermediate

**Answer:**
Long poll: repeated hold. WS: persistent bidirectional.

### Q478. CORS handling?
**Level:** Intermediate

**Answer:**
Middleware sets Access-Control headers.

### Q479. API gateway (LLD view)?
**Level:** Advanced

**Answer:**
Router, auth, rate limit, aggregation in one module.

### Q480. Error codes taxonomy?
**Level:** Intermediate

**Answer:**
4xx client, 5xx server, custom app codes in body.

---

## 10. Advanced LLD Topics

### Q481. DDD tactical patterns?
**Level:** Advanced

**Answer:**
Entity, Value Object, Aggregate, Repository, Domain Service, Domain Event.

### Q482. Bounded context?
**Level:** Advanced

**Answer:**
Explicit domain boundary with own model.

### Q483. Anti-corruption layer?
**Level:** Advanced

**Answer:**
Adapter translating external model to your domain.

### Q484. Domain events?
**Level:** Advanced

**Answer:**
`OrderPlaced` event triggers downstream handlers.

### Q485. CQRS (LLD view)?
**Level:** Advanced

**Answer:**
Separate Command and Query models in application layer.

### Q486. Event Sourcing (LLD)?
**Level:** Advanced

**Answer:**
Store events not state; replay to rebuild.

### Q487. Hexagonal architecture?
**Level:** Advanced

**Answer:**
Ports (interfaces) and adapters (implementations).

### Q488. Clean architecture layers?
**Level:** Advanced

**Answer:**
Entities → Use Cases → Adapters → Frameworks.

### Q489. Onion architecture?
**Level:** Advanced

**Answer:**
Domain center; infrastructure outer.

### Q490. Repository + Unit of Work?
**Level:** Advanced

**Answer:**
Repository per aggregate; UoW commits transaction.

### Q491. Saga (LLD orchestration)?
**Level:** Advanced

**Answer:**
Coordinate multi-step distributed transaction with compensations.

### Q492. Outbox pattern (LLD)?
**Level:** Advanced

**Answer:**
Write event to outbox table in same DB transaction.

### Q493. Inbox pattern?
**Level:** Advanced

**Answer:**
Idempotent consumer deduplicates by message ID.

### Q494. Polymorphic association?
**Level:** Advanced

**Answer:**
Entity references multiple types — use type discriminator.

### Q495. Metadata-driven design?
**Level:** Advanced

**Answer:**
Behavior configured by metadata not code.

### Q496. Reflection in design?
**Level:** Advanced

**Answer:**
Runtime class inspection — use sparingly; breaks types.

### Q497. Generic repository trade-off?
**Level:** Advanced

**Answer:**
A generic `Repository<T>` is convenient but can leak persistence into the domain. JavaScript has **no generics** — use conventions or JSDoc. **TypeScript** and **Python** support type-parameterized repositories.

**Snippet Language:** JavaScript (no generics — duck typing)
```javascript
class UserRepository {
  constructor(store) {
    this.store = store;
  }

  async findById(id) {
    return this.store.get("users", id);
  }

  async save(user) {
    return this.store.put("users", user.id, user);
  }
}
```

**Snippet Language:** TypeScript (generic repository)
```typescript
interface Repository<T, ID = string> {
  findById(id: ID): Promise<T | null>;
  save(entity: T): Promise<T>;
}

class UserRepository implements Repository<User> {
  constructor(private store: DataStore) {}

  async findById(id: string): Promise<User | null> {
    return this.store.get<User>("users", id);
  }

  async save(user: User): Promise<User> {
    return this.store.put("users", user.id, user);
  }
}
```

**Snippet Language:** Python (Generic)
```python
from typing import Generic, TypeVar, Protocol

T = TypeVar("T")
ID = TypeVar("ID")

class Repository(Protocol, Generic[T, ID]):
    def find_by_id(self, id: ID) -> T | None: ...
    def save(self, entity: T) -> T: ...

class UserRepository:
    def __init__(self, store):
        self.store = store

    def find_by_id(self, id: str) -> dict | None:
        return self.store.get("users", id)

    def save(self, user: dict) -> dict:
        return self.store.put("users", user["id"], user)
```

### Q498. Rich domain model benefits?
**Level:** Advanced

**Answer:**
Business rules colocated with data; fewer bugs.

### Q499. Transaction script vs domain model?
**Level:** Advanced

**Answer:**
Script: procedural. Domain: OOP rich model.

### Q500. Table module pattern?
**Level:** Intermediate

**Answer:**
One class per DB table — simple CRUD apps.

### Q501. Active Record pattern?
**Level:** Intermediate

**Answer:**
Object carries persistence methods — Rails style.

### Q502. Data Mapper pattern?
**Level:** Advanced

**Answer:**
Separate mapper between domain and DB.

### Q503. Identity field pattern?
**Level:** Basic

**Answer:**
DB-generated surrogate key.

### Q504. Foreign key mapping?
**Level:** Intermediate

**Answer:**
Association via FK column or link table.

### Q505. Lazy load pattern?
**Level:** Intermediate

**Answer:**
Load related data on first access.

### Q506. Plugin architecture LLD?
**Level:** Advanced

**Answer:**
SPI interface + dynamic plugin loading.

### Q507. Feature toggle service?
**Level:** Intermediate

**Answer:**
Runtime flags without deployment.

### Q508. Multi-tenancy row-level?
**Level:** Advanced

**Answer:**
tenant_id column on every row.

### Q509. Schema migration strategy?
**Level:** Advanced

**Answer:**
Backward-compatible migrations; expand-contract.

### Q510. LLD for microservice single domain?
**Level:** Advanced

**Answer:**
One bounded context per service; clear aggregate roots.
