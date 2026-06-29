# Creational Design Patterns in JavaScript
## A Complete Learning Roadmap — Beginner to Advanced

> **Who this is for:** Complete beginners to design patterns who want to understand, implement, and confidently discuss Creational Design Patterns in real-world JavaScript projects and interviews.

---

## Table of Contents

1. [What Are Design Patterns?](#1-what-are-design-patterns)
2. [What Are Creational Design Patterns?](#2-what-are-creational-design-patterns)
3. [Constructor Pattern](#3-constructor-pattern)
4. [Factory Method Pattern](#4-factory-method-pattern)
5. [Abstract Factory Pattern](#5-abstract-factory-pattern)
6. [Singleton Pattern](#6-singleton-pattern)
7. [Builder Pattern](#7-builder-pattern)
8. [Prototype Pattern](#8-prototype-pattern)
9. [Object Pool Pattern (Bonus)](#9-object-pool-pattern-bonus)
10. [Comparison Table — All Creational Patterns](#10-comparison-table--all-creational-patterns)
11. [Final Interview Cheat Sheet](#11-final-interview-cheat-sheet)

---

## 1. What Are Design Patterns?

### The Simple Explanation

Imagine you are a chef. Every day, customers ask for different dishes. Over years of cooking, you develop **proven recipes** — templates that always work, that other chefs in your kitchen can follow without reinventing the wheel.

**Design patterns are exactly that for software.** They are proven, reusable solutions to common problems that developers encounter repeatedly when building software.

### The Technical Definition

> A design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. — Gang of Four (GoF)

### A Brief History

In 1994, four computer scientists — Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides (collectively known as the **"Gang of Four" or GoF**) — published a groundbreaking book: *Design Patterns: Elements of Reusable Object-Oriented Software*.

They documented **23 classic design patterns** grouped into three categories:

| Category | Purpose | Examples |
|---|---|---|
| **Creational** | How objects are created | Singleton, Factory, Builder |
| **Structural** | How objects are composed | Adapter, Decorator, Facade |
| **Behavioral** | How objects communicate | Observer, Strategy, Command |

This roadmap covers **Creational Patterns** in full depth.

---

## 2. What Are Creational Design Patterns?

### The Core Idea

Every program creates objects. Naively, you just write `new SomeClass()` everywhere. But as your application grows, this naive approach causes serious problems:

- Your code becomes **tightly coupled** to specific classes
- You end up with **duplicated object creation logic** scattered everywhere
- **Changing how objects are created** requires touching dozens of files
- **Testing becomes painful** because you can't easily swap implementations
- **Complexity grows** as objects need complex initialization sequences

**Creational patterns solve all of these problems.** They give you controlled, flexible, and maintainable ways to create objects.

### The Six Creational Patterns

```
CREATIONAL DESIGN PATTERNS
│
├── Constructor Pattern    → Simplest form: using classes/constructors directly
├── Factory Method         → Let subclasses decide which object to create
├── Abstract Factory       → Create families of related objects
├── Singleton              → Guarantee only one instance of a class exists
├── Builder                → Build complex objects step by step
└── Prototype              → Clone existing objects instead of creating new ones
```

### Key Vocabulary (Before We Begin)

| Term | Plain English |
|---|---|
| **Instance** | A specific object created from a class (like a specific car made from a car blueprint) |
| **Instantiation** | The act of creating an instance using `new` |
| **Coupling** | How much one piece of code depends on another |
| **Abstraction** | Hiding implementation details behind a clean interface |
| **Encapsulation** | Bundling data and behavior together, hiding internal details |

---

## 3. Constructor Pattern

### 3.1 The Problem It Solves

Before JavaScript had classes (pre-ES6), creating multiple objects with the same shape was messy. You'd either create plain objects manually for each one (tedious and error-prone) or use function-based "constructor functions" in an inconsistent way.

The Constructor Pattern provides **a standardized, clean way to create multiple objects of the same "type"** with their own properties and shared methods.

### 3.2 Real-World Analogy

Think of a **car manufacturing mold**. A factory has a single mold (the class/constructor). Every time they pour metal into the mold, they get a new car frame (an instance). Each frame is independent — scratching one doesn't scratch the others — but they all share the same shape.

### 3.3 When to Use It

✅ **Use when:**
- You need to create multiple objects with the same structure but different data
- You want to share methods across all instances (via prototype)
- You need a clean, readable blueprint for objects

❌ **Avoid when:**
- You only need one object — use a plain object literal `{}`
- Object creation logic is complex — use Builder instead
- You need flexible creation based on conditions — use Factory instead

### 3.4 How It Works Internally

When you call `new MyClass()` in JavaScript, four things happen behind the scenes:

```
1. A brand new empty object is created: {}
2. The prototype of that object is linked to MyClass.prototype
3. `this` inside the constructor refers to that new object
4. The constructor runs and attaches properties to `this`
5. The new object is returned automatically
```

### 3.5 UML / Structure

```
┌─────────────────────────────┐
│         Vehicle              │
├─────────────────────────────┤
│ + make: string               │
│ + model: string              │
│ + year: number               │
│ + color: string              │
├─────────────────────────────┤
│ + describe(): string         │
│ + start(): void              │
└─────────────────────────────┘
         ↑           ↑
    [car1]       [car2]      ← Individual instances
```

### 3.6 JavaScript Implementation

```javascript
// ============================================================
// CONSTRUCTOR PATTERN — ES6 Class Version (Modern Approach)
// ============================================================

class Vehicle {
  // The constructor runs automatically when `new Vehicle(...)` is called
  constructor(make, model, year, color) {
    // `this` refers to the newly created object
    this.make = make;
    this.model = model;
    this.year = year;
    this.color = color;
    this.isRunning = false; // default state
  }

  // Methods defined here live on Vehicle.prototype
  // They are SHARED across all instances (memory efficient)
  describe() {
    return `${this.year} ${this.make} ${this.model} in ${this.color}`;
  }

  start() {
    if (this.isRunning) {
      console.log(`${this.model} is already running.`);
      return;
    }
    this.isRunning = true;
    console.log(`${this.model} started. Vroom!`);
  }

  stop() {
    this.isRunning = false;
    console.log(`${this.model} stopped.`);
  }
}

// --- Creating Instances ---
const car1 = new Vehicle('Toyota', 'Camry', 2022, 'Silver');
const car2 = new Vehicle('Honda', 'Civic', 2023, 'Blue');

console.log(car1.describe()); // "2022 Toyota Camry in Silver"
console.log(car2.describe()); // "2023 Honda Civic in Blue"

car1.start(); // "Camry started. Vroom!"
car2.start(); // "Civic started. Vroom!"

// Each instance has its OWN data
car1.color = 'Red'; // Only changes car1
console.log(car1.color); // "Red"
console.log(car2.color); // "Blue" — unaffected

// Methods are SHARED on the prototype (same function reference)
console.log(car1.describe === car2.describe); // true — memory efficient!
```

### 3.7 Old-Style Constructor Functions (ES5) — Good to Know

```javascript
// ============================================================
// ES5 Constructor Function — You'll see this in legacy codebases
// ============================================================

function Vehicle(make, model, year) {
  // When called with `new`, `this` is the new object
  this.make = make;
  this.model = model;
  this.year = year;
}

// Methods added to prototype manually
Vehicle.prototype.describe = function() {
  return `${this.year} ${this.make} ${this.model}`;
};

const car = new Vehicle('Ford', 'Mustang', 2020);
console.log(car.describe()); // "2020 Ford Mustang"

// ⚠️ WARNING: What happens without `new`?
const badCar = Vehicle('Ford', 'Mustang', 2020); // No `new`!
// In non-strict mode: `this` is the global object — bugs!
// In strict mode: TypeError (recommended)
```

### 3.8 Dry Run of the Code

```javascript
const car1 = new Vehicle('Toyota', 'Camry', 2022, 'Silver');
```

**Step-by-step execution:**

```
Step 1: JavaScript creates an empty object: {}
Step 2: Its prototype is set to Vehicle.prototype
        (so car1 can access describe, start, stop)
Step 3: `this` inside constructor = that empty object
Step 4: Constructor runs:
        this.make  = 'Toyota'  → car1.make  = 'Toyota'
        this.model = 'Camry'   → car1.model = 'Camry'
        this.year  = 2022      → car1.year  = 2022
        this.color = 'Silver'  → car1.color = 'Silver'
        this.isRunning = false → car1.isRunning = false
Step 5: The new object is returned and assigned to `car1`

Memory Layout:
car1 object:          Vehicle.prototype:
┌──────────────┐     ┌──────────────────────┐
│ make: Toyota │     │ describe: function() │
│ model: Camry │ ──→ │ start: function()    │
│ year: 2022   │     │ stop: function()     │
│ color: Silver│     └──────────────────────┘
│ isRunning: ✗ │
└──────────────┘
```

### 3.9 Real-World Practical Example

```javascript
// ============================================================
// Real-World Example: User Account System
// ============================================================

class User {
  #passwordHash; // Private field (ES2022) — not accessible outside

  constructor(username, email, password, role = 'user') {
    this.id = User.generateId();
    this.username = username;
    this.email = email;
    this.role = role;
    this.createdAt = new Date();
    this.isActive = true;
    this.#passwordHash = this.#hashPassword(password);
  }

  // Private method — only accessible inside this class
  #hashPassword(password) {
    // In production use bcrypt — simplified here
    return btoa(password + '_salt_12345');
  }

  // Static method — belongs to the class, not instances
  static generateId() {
    return `usr_${Date.now()}_${Math.random().toString(36).slice(2, 7)}`;
  }

  verifyPassword(password) {
    return this.#passwordHash === this.#hashPassword(password);
  }

  toJSON() {
    // Never expose password hash in serialization!
    return {
      id: this.id,
      username: this.username,
      email: this.email,
      role: this.role,
      createdAt: this.createdAt,
      isActive: this.isActive,
    };
  }

  toString() {
    return `User(${this.username}, ${this.role})`;
  }
}

// Usage
const admin = new User('alice', 'alice@example.com', 'secret123', 'admin');
const member = new User('bob', 'bob@example.com', 'pass456');

console.log(admin.toString());             // "User(alice, admin)"
console.log(member.verifyPassword('pass456')); // true
console.log(JSON.stringify(admin.toJSON())); // No password hash!
```

### 3.10 Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| Clean, readable syntax (ES6 classes) | Can cause issues if `new` is forgotten (ES5 functions) |
| Memory-efficient: methods live on prototype | Can lead to tight coupling if overused |
| Supports inheritance naturally | Not flexible when object creation needs vary |
| Familiar to developers from other OOP languages | Deep inheritance hierarchies get complex |

### 3.11 Common Mistakes and Misconceptions

**Mistake 1: Forgetting `new`**
```javascript
const user = User('alice', 'test@test.com', 'pass'); // ❌ Missing `new`
// ES6 classes throw a TypeError — that's actually good protection!
```

**Mistake 2: Defining methods inside the constructor**
```javascript
class BadClass {
  constructor(name) {
    this.name = name;
    // ❌ This creates a NEW function for EVERY instance — memory waste!
    this.greet = function() { return `Hi ${this.name}`; };
  }
}

class GoodClass {
  constructor(name) { this.name = name; }
  // ✅ One function shared across all instances via prototype
  greet() { return `Hi ${this.name}`; }
}
```

**Mistake 3: Mutating shared prototype properties**
```javascript
class Counter {
  constructor() {}
}
Counter.prototype.count = 0; // ⚠️ Shared mutable state on prototype!

const c1 = new Counter();
const c2 = new Counter();
c1.count++; // This creates count on c1 itself — not shared
// But an array/object on prototype WOULD be shared — dangerous!
```

### 3.12 Interview Questions

**Q1: What is the difference between a class and an instance?**
> A **class** is the blueprint/template (like a car mold). An **instance** is a specific object created from that blueprint (like a specific car). You can have one class and thousands of instances.

**Q2: What does `new` do in JavaScript?**
> `new` creates a fresh empty object, sets its prototype to the constructor's `.prototype`, runs the constructor function with `this` pointing to the new object, then returns that object.

**Q3: How are methods shared across instances in JavaScript?**
> Methods defined in a class body (or on `ClassName.prototype`) are placed on the **prototype object**. All instances share a reference to this prototype via the prototype chain (`[[Prototype]]`). This is memory-efficient: 1,000 instances share one copy of each method.

**Q4: What are private class fields and why should you use them?**
> Private fields (prefixed with `#`) are truly encapsulated — they cannot be accessed or modified from outside the class. Use them to protect sensitive data (passwords, internal state) from accidental modification.

**Q5: When would you choose the Constructor Pattern over a plain object literal?**
> Use a constructor/class when you need **multiple instances** of the same structure, **shared methods**, **inheritance**, or **encapsulation**. Use a plain object literal for a single, simple data container.

### 3.13 Best Practices

```javascript
// ✅ 1. Always use ES6 classes over ES5 constructor functions
// ✅ 2. Use private fields (#) for sensitive data
// ✅ 3. Keep constructors simple — avoid complex logic in them
// ✅ 4. Use static methods for utilities that don't need instance state
// ✅ 5. Implement toString() and toJSON() for debuggability
// ✅ 6. Use default parameter values for optional constructor arguments
// ✅ 7. Follow PascalCase for class names (Vehicle, UserAccount)
```

### 3.14 Summary & Key Takeaways

> The Constructor Pattern is the foundation of OOP in JavaScript. It lets you create multiple objects with the same structure using `new`. ES6 classes are syntactic sugar over JavaScript's prototype-based inheritance but provide clean, readable syntax.

**Key Takeaways:**
- `new` creates a new object, links its prototype, and runs the constructor
- Methods on the prototype are **shared** — put them there, not in the constructor
- Use private fields `#` for encapsulation
- ES6 `class` is the modern, preferred approach

**Practice Exercise:**
> Create a `BankAccount` class with `owner`, `balance` (default 0), and `accountNumber` (auto-generated). Add methods: `deposit(amount)`, `withdraw(amount)` (with balance check), and `getStatement()` that returns a formatted string. Make balance a private field.

---

## 4. Factory Method Pattern

### 4.1 The Problem It Solves

Imagine you're building a notification system. At first, you only send email notifications. You write:

```javascript
function sendNotification(type, message) {
  if (type === 'email') {
    const notif = new EmailNotification(message);
    notif.send();
  }
}
```

Then you add SMS. Then push notifications. Then Slack. Now your `sendNotification` function is a monster full of `if/else` branches, and **every time you add a new type, you touch this core function** — violating the Open/Closed Principle (open for extension, closed for modification).

**The Factory Method Pattern solves this by delegating the decision of which object to create to a method (the "factory method") that can be overridden.**

### 4.2 Real-World Analogy

Think of a **coffee shop franchise**.

The head office (abstract class) says: "We serve beverages. Make a beverage and serve it." But **each franchise location decides what "beverage" means**. The downtown location makes espresso. The drive-through makes cold brew. The airport location makes instant coffee.

The core process ("make beverage → serve it") is the same. But **which specific beverage** is created differs per location. That's the factory method.

### 4.3 When to Use It

✅ **Use when:**
- A class can't anticipate the type of objects it needs to create
- You want subclasses to control what gets created
- You want to encapsulate object creation to reduce coupling
- You have a group of related classes and want a unified creation interface

❌ **Avoid when:**
- You only have one type of object to create — it's overkill
- Object creation doesn't vary — just use `new` directly
- The hierarchy would become too complex for the benefit gained

### 4.4 How It Works Internally

```
The Factory Method pattern defines:
1. A "Creator" (base class/abstract class) with a `createProduct()` factory method
2. Concrete Creators override the factory method to return specific product types
3. The Creator uses the factory method in its own business logic
4. The client code works with the Creator, never directly instantiating Products
```

### 4.5 UML / Structure

```
┌──────────────────────────────┐
│       NotificationFactory    │  ← Abstract Creator
│    (abstract)                │
├──────────────────────────────┤
│ + createNotification()       │  ← Factory Method (abstract)
│ + send(message): void        │  ← Uses the factory method
└──────────────────┬───────────┘
                   │ (inherits)
        ┌──────────┼──────────┐
        │          │          │
┌───────┴──┐ ┌─────┴────┐ ┌──┴──────┐
│  Email   │ │   SMS    │ │  Push   │
│ Factory  │ │ Factory  │ │ Factory │   ← Concrete Creators
└───────┬──┘ └─────┬────┘ └──┬──────┘
        │          │          │
        ▼          ▼          ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│  Email   │ │   SMS    │ │  Push   │
│ Notif.   │ │  Notif.  │ │  Notif. │   ← Concrete Products
└──────────┘ └──────────┘ └──────────┘
        All implement: Notification interface
        (with a .send(message) method)
```

### 4.6 JavaScript Implementation

```javascript
// ============================================================
// FACTORY METHOD PATTERN
// Notification System Example
// ============================================================

// ---------- PRODUCT INTERFACE (what all products must implement) ----------
// JavaScript doesn't have formal interfaces, so we document the contract:
// Every Notification must have: send(message), getType()

// ---------- CONCRETE PRODUCTS ----------

class EmailNotification {
  constructor(recipient) {
    this.recipient = recipient;
    this.type = 'email';
  }

  send(message) {
    console.log(`[EMAIL] To: ${this.recipient} | Message: ${message}`);
    // In production: call an email API (SendGrid, SES, etc.)
  }

  getType() { return this.type; }
}

class SMSNotification {
  constructor(phoneNumber) {
    this.phoneNumber = phoneNumber;
    this.type = 'sms';
  }

  send(message) {
    console.log(`[SMS] To: ${this.phoneNumber} | Message: ${message}`);
    // In production: call Twilio or similar SMS API
  }

  getType() { return this.type; }
}

class PushNotification {
  constructor(deviceToken) {
    this.deviceToken = deviceToken;
    this.type = 'push';
  }

  send(message) {
    console.log(`[PUSH] Device: ${this.deviceToken} | Message: ${message}`);
    // In production: call Firebase FCM or APNs
  }

  getType() { return this.type; }
}

// ---------- ABSTRACT CREATOR ----------

class NotificationFactory {
  // The factory method — MUST be overridden by subclasses
  createNotification(contact) {
    throw new Error(`${this.constructor.name} must implement createNotification()`);
  }

  // Template method: uses the factory method internally
  // The creator doesn't know WHAT type it creates — just that it has .send()
  notify(contact, message) {
    const notification = this.createNotification(contact); // Factory method call
    console.log(`Sending ${notification.getType()} notification...`);
    notification.send(message);
    console.log(`✓ Notification sent via ${notification.getType()}`);
    return notification;
  }
}

// ---------- CONCRETE CREATORS ----------

class EmailNotificationFactory extends NotificationFactory {
  // Override the factory method to return the specific product type
  createNotification(contact) {
    return new EmailNotification(contact);
  }
}

class SMSNotificationFactory extends NotificationFactory {
  createNotification(contact) {
    return new SMSNotification(contact);
  }
}

class PushNotificationFactory extends NotificationFactory {
  createNotification(contact) {
    return new PushNotification(contact);
  }
}

// ---------- CLIENT CODE ----------

function alertUser(factory, contact, message) {
  // Client only knows about the NotificationFactory interface
  // It doesn't know (or care) if it's Email, SMS, or Push
  factory.notify(contact, message);
}

// Usage
const emailFactory = new EmailNotificationFactory();
const smsFactory = new SMSNotificationFactory();
const pushFactory = new PushNotificationFactory();

alertUser(emailFactory, 'user@example.com', 'Your order shipped!');
// Sending email notification...
// [EMAIL] To: user@example.com | Message: Your order shipped!
// ✓ Notification sent via email

alertUser(smsFactory, '+1-555-0100', 'Your order shipped!');
// [SMS] To: +1-555-0100 | Message: Your order shipped!

alertUser(pushFactory, 'device_token_abc123', 'Your order shipped!');
// [PUSH] Device: device_token_abc123 | Message: Your order shipped!
```

### 4.7 Simpler "Factory Function" Variant (Very Common in JS)

The classic GoF Factory Method uses inheritance. In JavaScript, a simpler and very popular approach uses a **factory function** or a **static factory method**:

```javascript
// ============================================================
// SIMPLE FACTORY (Static Method Variant) — Very common in JS
// ============================================================

class Notification {
  constructor(type, contact) {
    this.type = type;
    this.contact = contact;
  }

  // Static factory method on the class itself
  static create(type, contact) {
    switch (type) {
      case 'email': return new EmailNotification(contact);
      case 'sms':   return new SMSNotification(contact);
      case 'push':  return new PushNotification(contact);
      default:
        throw new Error(`Unknown notification type: "${type}"`);
    }
  }
}

// Clean, expressive usage
const notif1 = Notification.create('email', 'user@example.com');
const notif2 = Notification.create('sms', '+1-555-0100');

notif1.send('Package delivered!');
notif2.send('Package delivered!');

// ✅ Adding a new type only requires:
// 1. Creating a new SlackNotification class
// 2. Adding `case 'slack': return new SlackNotification(contact);`
// The client code stays unchanged.
```

### 4.8 Dry Run of the Code

```javascript
alertUser(emailFactory, 'user@example.com', 'Your order shipped!');
```

**Execution trace:**

```
1. alertUser receives emailFactory (an EmailNotificationFactory instance)
2. Calls: emailFactory.notify('user@example.com', 'Your order shipped!')
3. notify() is defined on the parent NotificationFactory
4. Inside notify(), calls: this.createNotification('user@example.com')
   └── `this` is emailFactory, which overrides createNotification()
   └── Returns: new EmailNotification('user@example.com')
5. notification = { recipient: 'user@example.com', type: 'email' }
6. Logs: "Sending email notification..."
7. Calls: notification.send('Your order shipped!')
   └── Logs: "[EMAIL] To: user@example.com | Message: Your order shipped!"
8. Logs: "✓ Notification sent via email"
```

**Key insight:** `notify()` in the base class never says `new EmailNotification()`. It just calls `this.createNotification()`. The subclass decides what to create. This is the essence of the pattern.

### 4.9 Real-World Practical Example

```javascript
// ============================================================
// Real-World Example: Cross-Platform UI Components
// (Think React Native — same logic, different platform rendering)
// ============================================================

// Product interface: every Button must have render() and onClick()
class Button {
  render() { throw new Error('render() must be implemented'); }
  onClick(handler) { throw new Error('onClick() must be implemented'); }
}

// Concrete Products
class WebButton extends Button {
  constructor(label) {
    super();
    this.label = label;
  }
  render() {
    return `<button class="web-btn">${this.label}</button>`;
  }
  onClick(handler) {
    console.log(`[WEB] Attaching click handler to "${this.label}" button`);
    // document.querySelector('.web-btn').addEventListener('click', handler);
  }
}

class MobileButton extends Button {
  constructor(label) {
    super();
    this.label = label;
  }
  render() {
    return `<TouchableOpacity><Text>${this.label}</Text></TouchableOpacity>`;
  }
  onClick(handler) {
    console.log(`[MOBILE] Attaching press handler to "${this.label}" button`);
  }
}

// Abstract Creator
class UIFactory {
  createButton(label) {
    throw new Error('createButton() must be implemented');
  }

  // Template method: builds a complete button with event handling
  buildSubmitButton() {
    const btn = this.createButton('Submit');
    btn.onClick(() => console.log('Form submitted!'));
    return btn;
  }
}

// Concrete Creators
class WebUIFactory extends UIFactory {
  createButton(label) { return new WebButton(label); }
}

class MobileUIFactory extends UIFactory {
  createButton(label) { return new MobileButton(label); }
}

// Client code — platform-agnostic
function renderForm(uiFactory) {
  const submitBtn = uiFactory.buildSubmitButton();
  console.log('Rendered:', submitBtn.render());
}

const platform = 'mobile'; // Could come from environment detection
const factory = platform === 'mobile' ? new MobileUIFactory() : new WebUIFactory();
renderForm(factory);
```

### 4.10 Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| Decouples object creation from usage | More classes = more complexity |
| Adding new products doesn't break existing code (Open/Closed) | Can be overkill for simple object creation |
| Centralizes object creation logic | Factory hierarchy can get deep and complex |
| Makes testing easier (swap factories) | Requires understanding of inheritance |

### 4.11 Common Mistakes and Misconceptions

**Mistake 1: Confusing "Simple Factory" with Factory Method**
> A Simple Factory is just a static method or function that creates objects — it's NOT a GoF pattern. The Factory Method pattern specifically uses **inheritance** and **overriding** to let subclasses decide what to create. Both are useful; know the difference.

**Mistake 2: Creating the factory method as static**
```javascript
// ❌ If the factory method is static, subclasses can't override it via polymorphism
class Creator {
  static createProduct() { /* ... */ } // Can't be overridden via `this.createProduct()`
}

// ✅ Instance method allows polymorphic behavior
class Creator {
  createProduct() { throw new Error('Override me!'); }
}
```

**Mistake 3: Putting all logic in the factory instead of keeping it in the products**
```javascript
// ❌ Factory is doing too much
class Factory {
  create(type) {
    if (type === 'email') {
      // 50 lines of email setup logic here
    }
  }
}

// ✅ Factory just creates; products handle their own logic
class Factory {
  create(type) {
    if (type === 'email') return new EmailNotification(); // Clean!
  }
}
```

### 4.12 Interview Questions

**Q1: What is the difference between Factory Method and Simple Factory?**
> **Simple Factory** is a static/standalone function or class that creates objects using conditions — it's not a GoF pattern. **Factory Method** is a GoF design pattern where a creator class defines a `createProduct()` method that subclasses override to return different types.

**Q2: What principle does Factory Method support?**
> **Open/Closed Principle**: classes should be open for extension (add new product type) but closed for modification (don't change existing creator code). With Factory Method, you add a new product by creating a new ConcreteCreator subclass — no existing code changes.

**Q3: How does Factory Method differ from Abstract Factory?**
> Factory Method creates **one type** of product. Abstract Factory creates **families of related products**. See the Abstract Factory section for full comparison.

**Q4: How would you test code that uses Factory Method?**
> In tests, pass a mock factory that returns mock/stub products. Since client code only depends on the factory interface, not concrete implementations, you can easily inject test doubles.

**Q5: Real-world JavaScript example of Factory Method?**
> `Promise.resolve()` and `Promise.reject()` are factory methods. `Array.from()` and `Object.create()` are factory functions. React's `createElement` is a factory function for JSX elements.

### 4.13 Comparison: Factory Method vs Constructor Pattern

| Aspect | Constructor Pattern | Factory Method |
|---|---|---|
| How to create | Direct `new MyClass()` | Call factory method |
| Flexibility | Fixed type | Determined at runtime |
| Coupling | High (client knows exact class) | Low (client knows interface only) |
| Adding new types | Modify client code | Add new subclass |
| Complexity | Simple | More classes |

### 4.14 Summary & Key Takeaways

> Factory Method separates the **decision of what to create** from the **code that uses what's created**. The creator class knows what to do with a product but delegates the creation responsibility to its subclasses.

**Key Takeaways:**
- Factory Method = a method that subclasses override to create specific objects
- Client code depends on abstractions, not concrete classes
- Adding new types requires adding code, not changing existing code
- Very common in JavaScript as static factory methods or factory functions

**Practice Exercise:**
> Build a `LoggerFactory` system. Create a `Logger` base class with a `log(message)` method. Create `ConsoleLogger`, `FileLogger`, and `RemoteLogger` concrete classes. Create corresponding factory classes. Write a function `debugApp(factory)` that only knows about `LoggerFactory` and calls `factory.createLogger()`.

---

## 5. Abstract Factory Pattern

### 5.1 The Problem It Solves

The Factory Method creates one type of product. But what if you need to create **families of related products** that must be used together?

Imagine you're building a UI library that supports multiple themes: **Light Theme** and **Dark Theme**. Each theme needs a consistent set of components: Button, Input, Modal, Tooltip. If you mix a Light Button with a Dark Modal, your UI looks inconsistent.

You need a way to **guarantee that all components created belong to the same theme family**. That's the Abstract Factory.

### 5.2 Real-World Analogy

Think of **furniture collections** from IKEA.

IKEA has two style collections: **Modern** and **Classic**. Each collection includes a sofa, coffee table, and bookshelf. If you're decorating in Modern style, you want the Modern sofa, Modern table, and Modern bookshelf — they all match because they came from the same collection.

You walk into IKEA and say "give me the Modern collection" — IKEA (the Abstract Factory) ensures every piece you get is from the same consistent family.

### 5.3 When to Use It

✅ **Use when:**
- Your system needs to work with families of related/dependent objects
- You want to enforce consistency across an entire product family
- You want to easily switch between families (e.g., Dark → Light theme)
- The system should be independent of how its products are created

❌ **Avoid when:**
- You only need one type of product (use Factory Method instead)
- Your product families rarely change or are simple
- The number of products in each family is very large (gets complex)

### 5.4 How It Works Internally

```
1. Define an "Abstract Factory" interface with a creation method per product type
2. Implement "Concrete Factories" — one per product family
3. Each Concrete Factory produces products from its own family
4. Client code only works with the Abstract Factory interface
5. Swap the entire family by swapping the factory
```

### 5.5 UML / Structure

```
┌────────────────────────────────────────────────────────┐
│            AbstractUIFactory (interface)                │
├────────────────────────────────────────────────────────┤
│ + createButton(): Button                               │
│ + createInput(): Input                                 │
│ + createModal(): Modal                                 │
└────────────────┬──────────────────┬───────────────────┘
                 │                  │
    ┌────────────┴──────┐  ┌────────┴──────────┐
    │  LightThemeFactory │  │  DarkThemeFactory  │
    ├───────────────────┤  ├───────────────────┤
    │ createButton()    │  │ createButton()    │
    │ createInput()     │  │ createInput()     │
    │ createModal()     │  │ createModal()     │
    └──────┬────────────┘  └────────┬──────────┘
           │                        │
   ┌───────┴──────┐         ┌───────┴──────┐
   │ LightButton  │         │  DarkButton  │  ← Both implement Button
   │ LightInput   │         │  DarkInput   │  ← Both implement Input
   │ LightModal   │         │  DarkModal   │  ← Both implement Modal
   └──────────────┘         └──────────────┘
```

### 5.6 JavaScript Implementation

```javascript
// ============================================================
// ABSTRACT FACTORY PATTERN
// Cross-Theme UI Component Library
// ============================================================

// ==================== ABSTRACT PRODUCTS ====================
// These define the CONTRACT (interface) each product must follow

class Button {
  render() { throw new Error('render() must be implemented'); }
  disable() { throw new Error('disable() must be implemented'); }
}

class Input {
  render() { throw new Error('render() must be implemented'); }
  getValue() { throw new Error('getValue() must be implemented'); }
}

class Modal {
  open(content) { throw new Error('open() must be implemented'); }
  close() { throw new Error('close() must be implemented'); }
}

// ==================== CONCRETE PRODUCTS — LIGHT THEME ====================

class LightButton extends Button {
  constructor(label) {
    super();
    this.label = label;
    this.theme = 'light';
  }
  render() {
    return `<button style="bg:white;color:black;border:1px solid #ccc">${this.label}</button>`;
  }
  disable() {
    return `<button style="bg:#f5f5f5;color:#ccc" disabled>${this.label}</button>`;
  }
}

class LightInput extends Input {
  constructor(placeholder) {
    super();
    this.placeholder = placeholder;
    this.value = '';
  }
  render() {
    return `<input style="bg:white;border:1px solid #ddd" placeholder="${this.placeholder}">`;
  }
  getValue() { return this.value; }
}

class LightModal extends Modal {
  open(content) {
    console.log(`[LIGHT MODAL] Opening: <div style="bg:white;shadow:0 2px 10px rgba(0,0,0,.1)">${content}</div>`);
  }
  close() {
    console.log('[LIGHT MODAL] Closing with fade-out animation');
  }
}

// ==================== CONCRETE PRODUCTS — DARK THEME ====================

class DarkButton extends Button {
  constructor(label) {
    super();
    this.label = label;
    this.theme = 'dark';
  }
  render() {
    return `<button style="bg:#333;color:#fff;border:1px solid #555">${this.label}</button>`;
  }
  disable() {
    return `<button style="bg:#222;color:#666" disabled>${this.label}</button>`;
  }
}

class DarkInput extends Input {
  constructor(placeholder) {
    super();
    this.placeholder = placeholder;
    this.value = '';
  }
  render() {
    return `<input style="bg:#2d2d2d;color:#fff;border:1px solid #555" placeholder="${this.placeholder}">`;
  }
  getValue() { return this.value; }
}

class DarkModal extends Modal {
  open(content) {
    console.log(`[DARK MODAL] Opening: <div style="bg:#1a1a1a;color:#eee;shadow:0 4px 20px rgba(0,0,0,.5)">${content}</div>`);
  }
  close() {
    console.log('[DARK MODAL] Closing with slide-out animation');
  }
}

// ==================== ABSTRACT FACTORY ====================

class UIFactory {
  createButton(label) { throw new Error('createButton() must be implemented'); }
  createInput(placeholder) { throw new Error('createInput() must be implemented'); }
  createModal() { throw new Error('createModal() must be implemented'); }
}

// ==================== CONCRETE FACTORIES ====================

class LightThemeFactory extends UIFactory {
  createButton(label) { return new LightButton(label); }
  createInput(placeholder) { return new LightInput(placeholder); }
  createModal() { return new LightModal(); }
}

class DarkThemeFactory extends UIFactory {
  createButton(label) { return new DarkButton(label); }
  createInput(placeholder) { return new DarkInput(placeholder); }
  createModal() { return new DarkModal(); }
}

// ==================== CLIENT CODE ====================
// Note: renderLoginForm knows NOTHING about Light or Dark themes!
// It just uses the UIFactory interface.

function renderLoginForm(factory) {
  // All components come from the SAME factory → guaranteed consistency
  const emailInput = factory.createInput('Enter your email');
  const passwordInput = factory.createInput('Enter your password');
  const submitButton = factory.createButton('Log In');
  const errorModal = factory.createModal();

  console.log('\n--- Login Form ---');
  console.log('Email:   ', emailInput.render());
  console.log('Password:', passwordInput.render());
  console.log('Button:  ', submitButton.render());

  // Simulate an error
  errorModal.open('Invalid credentials. Please try again.');
  errorModal.close();
  console.log('---\n');
}

// Switching the entire theme = swapping one line
const userPreference = 'dark'; // Could come from localStorage

const factory = userPreference === 'dark'
  ? new DarkThemeFactory()
  : new LightThemeFactory();

renderLoginForm(factory);
// --- Login Form ---
// Email:    <input style="bg:#2d2d2d;..." placeholder="Enter your email">
// Password: <input style="bg:#2d2d2d;..." placeholder="Enter your password">
// Button:   <button style="bg:#333;...">Log In</button>
// [DARK MODAL] Opening: ...
// [DARK MODAL] Closing with slide-out animation
```

### 5.7 Dry Run of the Code

```javascript
const factory = new DarkThemeFactory();
renderLoginForm(factory);
```

**Execution trace:**

```
renderLoginForm receives factory = DarkThemeFactory instance

Line: factory.createInput('Enter your email')
  └── DarkThemeFactory.createInput() runs
  └── Returns: new DarkInput('Enter your email')
  └── emailInput = DarkInput instance

Line: factory.createButton('Log In')
  └── DarkThemeFactory.createButton() runs
  └── Returns: new DarkButton('Log In')
  └── submitButton = DarkButton instance

Line: factory.createModal()
  └── DarkThemeFactory.createModal() runs
  └── Returns: new DarkModal()
  └── errorModal = DarkModal instance

Key observation: ALL objects are from the Dark family.
No LightButton accidentally mixed in — guaranteed consistency!
```

### 5.8 Real-World Practical Example

```javascript
// ============================================================
// Real-World Example: Database Abstraction Layer
// (PostgreSQL vs. MySQL vs. SQLite)
// ============================================================

// Abstract products
class Connection {
  connect() { throw new Error('Must implement connect()'); }
  disconnect() { throw new Error('Must implement disconnect()'); }
  execute(query) { throw new Error('Must implement execute()'); }
}

class QueryBuilder {
  select(table, fields) { throw new Error('Must implement select()'); }
  insert(table, data) { throw new Error('Must implement insert()'); }
  where(conditions) { throw new Error('Must implement where()'); }
  build() { throw new Error('Must implement build()'); }
}

// PostgreSQL family
class PostgresConnection extends Connection {
  connect() { console.log('Connecting to PostgreSQL via pg driver...'); }
  disconnect() { console.log('Closing PostgreSQL connection pool.'); }
  execute(query) { console.log(`[PostgreSQL] Executing: ${query}`); return []; }
}

class PostgresQueryBuilder extends QueryBuilder {
  constructor() { super(); this.query = ''; }
  select(table, fields = ['*']) {
    this.query = `SELECT ${fields.join(', ')} FROM "${table}"`;
    return this; // Enable chaining
  }
  where(conditions) {
    this.query += ` WHERE ${conditions}`;
    return this;
  }
  insert(table, data) {
    const cols = Object.keys(data).map(k => `"${k}"`).join(', ');
    const vals = Object.values(data).map(v => `'${v}'`).join(', ');
    this.query = `INSERT INTO "${table}" (${cols}) VALUES (${vals}) RETURNING *`;
    return this;
  }
  build() { return this.query; }
}

// SQLite family
class SQLiteConnection extends Connection {
  connect() { console.log('Opening SQLite file database...'); }
  disconnect() { console.log('Closing SQLite file.'); }
  execute(query) { console.log(`[SQLite] Executing: ${query}`); return []; }
}

class SQLiteQueryBuilder extends QueryBuilder {
  constructor() { super(); this.query = ''; }
  select(table, fields = ['*']) {
    this.query = `SELECT ${fields.join(', ')} FROM \`${table}\``;
    return this;
  }
  where(conditions) {
    this.query += ` WHERE ${conditions}`;
    return this;
  }
  insert(table, data) {
    const cols = Object.keys(data).map(k => `\`${k}\``).join(', ');
    const vals = Object.values(data).map(v => `'${v}'`).join(', ');
    this.query = `INSERT INTO \`${table}\` (${cols}) VALUES (${vals})`;
    return this;
  }
  build() { return this.query; }
}

// Abstract Factory
class DatabaseFactory {
  createConnection() { throw new Error('Must implement createConnection()'); }
  createQueryBuilder() { throw new Error('Must implement createQueryBuilder()'); }
}

// Concrete Factories
class PostgresFactory extends DatabaseFactory {
  createConnection() { return new PostgresConnection(); }
  createQueryBuilder() { return new PostgresQueryBuilder(); }
}

class SQLiteFactory extends DatabaseFactory {
  createConnection() { return new SQLiteConnection(); }
  createQueryBuilder() { return new SQLiteQueryBuilder(); }
}

// Application — database-agnostic!
class UserRepository {
  constructor(dbFactory) {
    this.connection = dbFactory.createConnection();
    this.qb = dbFactory.createQueryBuilder();
  }

  initialize() {
    this.connection.connect();
  }

  findById(id) {
    const query = this.qb
      .select('users', ['id', 'username', 'email'])
      .where(`id = '${id}'`)
      .build();
    return this.connection.execute(query);
  }

  create(userData) {
    const query = this.qb.insert('users', userData).build();
    return this.connection.execute(query);
  }
}

// Switch between databases with ONE line
const dbFactory = process.env.DB === 'postgres'
  ? new PostgresFactory()
  : new SQLiteFactory();

const userRepo = new UserRepository(dbFactory);
userRepo.initialize();
userRepo.findById('123');
userRepo.create({ username: 'alice', email: 'alice@example.com' });
```

### 5.9 Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| Guarantees product family consistency | High number of classes |
| Swapping families is trivial (one line) | Hard to add NEW product types to an existing family (must update ALL factories) |
| Isolates concrete class names from client | Can be over-engineered for simple systems |
| Supports Open/Closed Principle | Adds cognitive overhead |

### 5.10 Common Mistakes and Misconceptions

**Mistake 1: Using Abstract Factory when Factory Method is enough**
> If you only have one type of product, you don't need Abstract Factory. Only use it when you have **multiple related products** that must be used together.

**Mistake 2: Adding product types to the abstract factory frequently**
> This is the Achilles' heel. If you add a new product type (e.g., `createTooltip()`), you must update the abstract factory AND every concrete factory. Design your product family carefully upfront.

**Mistake 3: Mixing product families in client code**
```javascript
// ❌ Mixing families defeats the whole purpose
const darkFactory = new DarkThemeFactory();
const lightFactory = new LightThemeFactory();

const btn = darkFactory.createButton('Submit'); // Dark button
const input = lightFactory.createInput('Email'); // Light input — MISMATCH!
```

### 5.11 Interview Questions

**Q1: What is the main difference between Factory Method and Abstract Factory?**
> **Factory Method** creates ONE type of product, using inheritance/subclassing. **Abstract Factory** creates MULTIPLE related products (a family), using object composition. Factory Method is "one factory, one product type." Abstract Factory is "one factory, multiple product types, all from the same family."

**Q2: When would you choose Abstract Factory over simple if/else creation?**
> When you need to ensure multiple objects are from the same "family" and work together consistently, and when you expect to switch between families as a unit. The factory enforces the constraint that all products are compatible.

**Q3: How does Abstract Factory support the Dependency Inversion Principle?**
> High-level modules (client code) depend on the abstract `UIFactory` interface, not on concrete `LightThemeFactory` or `DarkThemeFactory`. The concrete factories are injected, so both the client and the concrete classes depend on the abstraction — exactly what DIP prescribes.

**Q4: What's the biggest downside of Abstract Factory?**
> Adding a new product type to the family requires modifying the abstract factory interface AND updating every concrete factory. This violates Open/Closed Principle for the factory hierarchy (though it supports OCP for the client code).

### 5.12 Summary & Key Takeaways

> Abstract Factory is "Factory of Factories." It creates entire families of related objects, guaranteeing that products within a family are compatible. It's perfect for theming systems, cross-platform UI libraries, and database abstraction layers.

**Key Takeaways:**
- Abstract Factory = multiple creation methods grouped under one interface
- All products from one factory are guaranteed to be from the same family
- Switching families requires only swapping the factory instance
- More classes, but much cleaner client code

**Practice Exercise:**
> Create a cross-platform audio player using Abstract Factory. Define two families: `DesktopAudio` and `MobileAudio`. Each family has three products: `AudioPlayer`, `VolumeControl`, and `ProgressBar`. The `DesktopAudio` family uses keyboard shortcuts; `MobileAudio` uses touch gestures. Write a `playMedia(factory)` function that works with both families.

---

## 6. Singleton Pattern

### 6.1 The Problem It Solves

Some resources in your application should only exist once:
- A connection pool to a database
- An application configuration object
- A logging service
- A global state store (like Redux store)
- A file system manager

If you create multiple instances of these, you get:
- Conflicting state (two configs with different values)
- Resource waste (two database connection pools)
- Bugs that are nearly impossible to reproduce

**The Singleton Pattern guarantees a class has only ONE instance, and provides a global access point to it.**

### 6.2 Real-World Analogy

Think of the **President of a country**. At any given time, there can only be one president. Everyone who needs to talk to "the president" talks to the same person. You don't create a new president each time — you access the single existing one.

Another analogy: your device's **Bluetooth Manager**. Your phone has exactly one Bluetooth radio and one manager controlling it. Every app that uses Bluetooth uses the SAME Bluetooth manager — not their own separate copies.

### 6.3 When to Use It

✅ **Use when:**
- Exactly one instance is needed (config, logger, connection pool)
- Global access to a shared resource is required
- You need to control access to a shared resource

❌ **Avoid when:**
- The pattern introduces global state that makes testing hard
- Multiple instances might actually be needed in the future
- You're using it as a replacement for thoughtful dependency injection

⚠️ **Controversy:** Singleton is the most debated GoF pattern. It can become an anti-pattern when overused because it introduces hidden global state and makes unit testing difficult.

### 6.4 How It Works Internally

```
1. Make the constructor private (or protect it from direct `new` calls)
2. Store the single instance in a static property on the class
3. Provide a static `getInstance()` method:
   - If instance exists: return it
   - If not: create it, store it, return it
4. All callers use getInstance(), never new DirectlyClass()
```

### 6.5 UML / Structure

```
┌─────────────────────────────────────────┐
│              Logger (Singleton)          │
├─────────────────────────────────────────┤
│ - static instance: Logger | null        │  ← The single stored instance
│ - logs: string[]                        │
├─────────────────────────────────────────┤
│ - constructor()                         │  ← Private (protected)
│ + static getInstance(): Logger          │  ← Global access point
│ + log(level, message): void             │
│ + getLogs(): string[]                   │
│ + clear(): void                         │
└─────────────────────────────────────────┘

Client A ──→ Logger.getInstance() ──→ [same Logger object]
Client B ──→ Logger.getInstance() ──→ [same Logger object]
Client C ──→ Logger.getInstance() ──→ [same Logger object]
```

### 6.6 JavaScript Implementation

```javascript
// ============================================================
// SINGLETON PATTERN — Multiple Approaches in JavaScript
// ============================================================

// ==================== APPROACH 1: Class with static instance ====================

class Logger {
  // Static private field — stores the one and only instance
  static #instance = null;

  // Private constructor-like protection
  #logs = [];
  #level;

  constructor(level = 'info') {
    // Guard: if called with `new` directly, this would be a problem
    // We handle this via getInstance()
    this.#level = level;
    this.startTime = new Date();
    console.log(`Logger initialized at ${this.startTime.toISOString()}`);
  }

  // The Global Access Point — the ONLY way to get the instance
  static getInstance(level = 'info') {
    if (!Logger.#instance) {
      // First call: create and store the instance
      Logger.#instance = new Logger(level);
    }
    // Every subsequent call: return the SAME instance
    return Logger.#instance;
  }

  // Prevent cloning via Object.assign or spread
  static resetInstance() {
    // Only for testing purposes!
    Logger.#instance = null;
  }

  log(level, message) {
    const entry = {
      timestamp: new Date().toISOString(),
      level,
      message,
    };
    this.#logs.push(entry);
    const prefix = { error: '❌', warn: '⚠️', info: 'ℹ️', debug: '🔍' }[level] || '📝';
    console.log(`${prefix} [${level.toUpperCase()}] ${message}`);
    return this; // Enable method chaining
  }

  info(message)  { return this.log('info', message); }
  warn(message)  { return this.log('warn', message); }
  error(message) { return this.log('error', message); }
  debug(message) { return this.log('debug', message); }

  getLogs() { return [...this.#logs]; } // Return copy, not the original array
  
  getStats() {
    const counts = this.#logs.reduce((acc, log) => {
      acc[log.level] = (acc[log.level] || 0) + 1;
      return acc;
    }, {});
    return { total: this.#logs.length, ...counts };
  }

  clear() { this.#logs = []; }
}

// ---- Usage ----
const logger1 = Logger.getInstance();
const logger2 = Logger.getInstance(); // Same instance!

console.log(logger1 === logger2); // true — provably the same object

logger1.info('Application started');
logger1.warn('Config file not found, using defaults');
logger2.error('Database connection failed'); // Same instance as logger1!

// All logs are in one place
console.log(logger1.getLogs().length); // 3 — including the error logged via logger2
console.log(logger1.getStats()); // { total: 3, info: 1, warn: 1, error: 1 }
```

### 6.7 Module-Based Singleton (Modern JS Approach)

```javascript
// ============================================================
// APPROACH 2: ES Module Singleton
// This is the most natural and idiomatic way in modern JavaScript!
// ES modules are cached after the first import — they ARE singletons.
// ============================================================

// --- config.js ---
// This module is loaded once and cached by the JS module system
// Every file that imports it gets the SAME object

class AppConfig {
  #settings;

  constructor() {
    this.#settings = {
      appName: 'MyApp',
      version: '1.0.0',
      environment: process.env.NODE_ENV || 'development',
      apiBaseUrl: process.env.API_URL || 'http://localhost:3000',
      maxRetries: 3,
      debugMode: false,
    };
  }

  get(key) {
    if (!(key in this.#settings)) {
      throw new Error(`Unknown config key: "${key}"`);
    }
    return this.#settings[key];
  }

  set(key, value) {
    if (!(key in this.#settings)) {
      throw new Error(`Unknown config key: "${key}"`);
    }
    this.#settings[key] = value;
  }

  getAll() { return { ...this.#settings }; } // Shallow copy — safe
}

// The module creates ONE instance and exports it
// Every importer gets the same instance
const config = new AppConfig();
export default config; // or: module.exports = config;

// --- In any other file ---
// import config from './config.js';
// config.get('appName'); // 'MyApp' — same instance everywhere
```

### 6.8 Object Literal Singleton (Simplest Form)

```javascript
// ============================================================
// APPROACH 3: Object Literal Singleton
// Perfect for simple cases with no need for instantiation logic
// ============================================================

const EventBus = {
  _listeners: {},

  on(event, callback) {
    if (!this._listeners[event]) {
      this._listeners[event] = [];
    }
    this._listeners[event].push(callback);
    // Return unsubscribe function
    return () => this.off(event, callback);
  },

  off(event, callback) {
    if (this._listeners[event]) {
      this._listeners[event] = this._listeners[event].filter(cb => cb !== callback);
    }
  },

  emit(event, data) {
    if (this._listeners[event]) {
      this._listeners[event].forEach(cb => cb(data));
    }
  },

  once(event, callback) {
    const wrapper = (data) => {
      callback(data);
      this.off(event, wrapper);
    };
    this.on(event, wrapper);
  }
};

// Usage — the object IS the singleton, no getInstance() needed
const unsub = EventBus.on('userLogin', (user) => {
  console.log(`User logged in: ${user.name}`);
});

EventBus.emit('userLogin', { name: 'Alice', id: 123 });
// "User logged in: Alice"

unsub(); // Removes the listener
```

### 6.9 Thread-Safety Note (Node.js)

```javascript
// ============================================================
// ADVANCED: Lazy vs Eager Initialization
// ============================================================

// LAZY (create on first use) — common approach
class DatabasePool {
  static #instance = null;

  static getInstance() {
    // In single-threaded JS (browser/Node.js main thread), this is fine
    // In worker threads or if creation is async, you need more care
    if (!DatabasePool.#instance) {
      DatabasePool.#instance = new DatabasePool();
    }
    return DatabasePool.#instance;
  }
}

// EAGER (create immediately when module loads) — simpler and safe
class EagerSingleton {
  // Instance created when the class definition is evaluated
  static #instance = new EagerSingleton();
  
  static getInstance() { return EagerSingleton.#instance; }
}

// ASYNC SINGLETON — for async initialization
class AsyncService {
  static #instance = null;
  static #initPromise = null;

  static async getInstance() {
    if (AsyncService.#instance) return AsyncService.#instance;

    // If already initializing, wait for that to finish
    if (!AsyncService.#initPromise) {
      AsyncService.#initPromise = AsyncService.#initialize();
    }

    AsyncService.#instance = await AsyncService.#initPromise;
    return AsyncService.#instance;
  }

  static async #initialize() {
    const service = new AsyncService();
    await service.#connect(); // Simulate async setup
    return service;
  }

  async #connect() {
    await new Promise(resolve => setTimeout(resolve, 100)); // Simulate delay
    console.log('Async service connected!');
  }
}
```

### 6.10 Dry Run

```javascript
const logger1 = Logger.getInstance();
// → Logger.#instance is null
// → Calls new Logger() → creates instance, stores in Logger.#instance
// → Returns the instance
// → logger1 = Logger instance

const logger2 = Logger.getInstance();
// → Logger.#instance is NOT null (set in previous call)
// → Returns the EXISTING instance — no new object created!
// → logger2 = SAME Logger instance as logger1

console.log(logger1 === logger2); // true — referential equality
```

### 6.11 Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| Guarantees single instance | Global state makes testing difficult |
| Controlled access to shared resource | Hard to mock/replace in unit tests |
| Lazy initialization (resource savings) | Violates Single Responsibility (manages own lifecycle AND provides service) |
| Consistent state across the application | Can hide dependencies (code depends on a global) |

### 6.12 Common Mistakes and Misconceptions

**Mistake 1: Using Singleton everywhere for "convenience"**
```javascript
// ❌ This is just lazy global state, not a true need for Singleton
const userSingleton = UserService.getInstance(); // User data should NOT be global!
// Singleton is for truly global, single-resource things like loggers and config
```

**Mistake 2: Singletons are untestable without a reset mechanism**
```javascript
// Always provide a way to reset in tests
class MyService {
  static #instance = null;
  static getInstance() { /* ... */ }
  static _reset() { MyService.#instance = null; } // For tests only!
}

// In test file:
beforeEach(() => MyService._reset());
```

**Mistake 3: Not handling async initialization**
> If your singleton needs async setup (DB connection, loading a config file), you MUST handle the async initialization pattern. A sync `getInstance()` that returns an uninitialized service causes subtle bugs.

### 6.13 Interview Questions

**Q1: What problem does Singleton solve?**
> It ensures a class has only one instance and provides a global access point to that instance. It's used for shared resources that must be consistent across the application.

**Q2: Why is Singleton controversial?**
> It introduces hidden global state, making code harder to test (you can't inject a mock), harder to reason about (state can change from anywhere), and violates Dependency Inversion (code secretly depends on a global).

**Q3: How does JavaScript's module system relate to Singleton?**
> ES modules are inherently singleton-like: a module is evaluated once, cached by the runtime, and every `import` gets the same cached module. Exporting an instance from a module is the most idiomatic "singleton" in modern JavaScript.

**Q4: How would you make a Singleton testable?**
> Provide a `_reset()` static method (marked for test use only). Better yet, use dependency injection: pass the logger/config as a constructor argument rather than calling `Logger.getInstance()` internally — this makes the dependency explicit and injectable.

**Q5: What's the difference between Singleton and a global variable?**
> Both provide global access, but Singleton is lazily initialized (created on first use), controls creation (can run setup logic), can be subclassed, and can enforce the single-instance constraint. A global variable is just a reference that any code can accidentally replace.

### 6.14 Summary & Key Takeaways

> Singleton ensures one and only one instance of a class. In modern JavaScript, ES module caching often makes explicit Singleton implementation unnecessary. Use it for genuinely shared resources; avoid it for general "global state."

**Key Takeaways:**
- Use `static #instance` and `static getInstance()` for the classic pattern
- ES module exports are naturally singleton
- Provide a reset mechanism for testing
- Prefer dependency injection over direct `getInstance()` calls for testability

**Practice Exercise:**
> Implement a `ConnectionPool` singleton that manages up to 5 database connections. It should have `acquire()` (get a connection), `release(connection)` (return it), and `getStats()` (show available/used counts). Make it testable with a static `_reset()` method.

---

## 7. Builder Pattern

### 7.1 The Problem It Solves

Imagine creating a complex `User` object:

```javascript
// This is the "telescoping constructor" anti-pattern
new User('Alice', 'alice@example.com', 30, 'admin', true, false, 'en-US', 
         'USD', new Date(), null, [], 'profile.jpg', true, 'New York');
```

Problems:
- What does `true` at position 5 mean? What does `false` at 6 mean?
- What if you want a User with only some of these fields set?
- What if field order matters and you can't skip fields?
- How do you validate the object during construction?

**The Builder Pattern separates the construction of a complex object from its representation, allowing the same construction process to create different representations.** It provides a step-by-step construction API.

### 7.2 Real-World Analogy

Think of **building a custom computer**.

You go to a PC builder website (like PCPartPicker). Instead of calling `new Computer(cpu, gpu, ram, ...)` with 20 arguments, you build it step by step:

```
Select CPU  → Select GPU → Select RAM → Select Storage → Add cooling → Build
```

Each step is optional. You can skip GPU if building a server. You can add cooling later. When you're satisfied, you click "Build" and get the final configured PC. That's the Builder pattern.

### 7.3 When to Use It

✅ **Use when:**
- An object requires many parameters and some are optional
- The construction process has multiple steps that can vary
- You want to produce different representations of the same construction process
- You want to validate the object at build time

❌ **Avoid when:**
- The object is simple with few parameters — just use a constructor
- There's only one way to construct the object — Builder adds unnecessary complexity
- All parameters are always required — no benefit over a constructor

### 7.4 How It Works Internally

```
1. The Builder has setter methods for each property
2. Each setter method returns `this` (for method chaining)
3. A final `build()` method creates and returns the complete object
4. The Director (optional) orchestrates the building sequence
5. The client only sees the final built product
```

### 7.5 UML / Structure

```
┌───────────────────┐      ┌──────────────────────────────────┐
│    Director        │      │         QueryBuilder              │
├───────────────────┤      ├──────────────────────────────────┤
│ + construct()     │─────▶│ + select(fields): QueryBuilder   │
└───────────────────┘      │ + from(table): QueryBuilder      │
                           │ + where(cond): QueryBuilder      │
                           │ + orderBy(col): QueryBuilder     │
                           │ + limit(n): QueryBuilder         │
                           │ + build(): Query                 │
                           └──────────────────────────────────┘
                                          │ builds
                                          ▼
                           ┌──────────────────────────────────┐
                           │            Query                  │
                           ├──────────────────────────────────┤
                           │ sql: string                      │
                           │ params: any[]                    │
                           └──────────────────────────────────┘
```

### 7.6 JavaScript Implementation

```javascript
// ============================================================
// BUILDER PATTERN — SQL Query Builder Example
// ============================================================

// The product that the Builder constructs
class Query {
  constructor(config) {
    this.sql = config.sql;
    this.params = config.params;
    this.type = config.type;
  }

  toString() { return this.sql; }
  
  execute() {
    console.log(`Executing: ${this.sql}`);
    console.log(`Params: ${JSON.stringify(this.params)}`);
    // In production: send to actual database driver
    return Promise.resolve([]);
  }
}

// The Builder — provides a fluent API for constructing Query objects
class QueryBuilder {
  #table = '';
  #fields = ['*'];
  #conditions = [];
  #orderByClause = '';
  #limitValue = null;
  #offsetValue = null;
  #joins = [];
  #params = [];
  #type = 'SELECT';

  // Each method returns `this` for method chaining (fluent interface)
  from(table) {
    this.#table = table;
    return this;
  }

  select(...fields) {
    this.#fields = fields.length > 0 ? fields : ['*'];
    return this;
  }

  where(condition, ...params) {
    this.#conditions.push(condition);
    this.#params.push(...params);
    return this;
  }

  orderBy(column, direction = 'ASC') {
    this.#orderByClause = `ORDER BY ${column} ${direction}`;
    return this;
  }

  limit(value) {
    this.#limitValue = value;
    return this;
  }

  offset(value) {
    this.#offsetValue = value;
    return this;
  }

  join(type, table, condition) {
    this.#joins.push(`${type} JOIN ${table} ON ${condition}`);
    return this;
  }

  innerJoin(table, condition) { return this.join('INNER', table, condition); }
  leftJoin(table, condition)  { return this.join('LEFT', table, condition); }

  // The terminal method — validates and constructs the final Query
  build() {
    // Validation step — catch errors at build time, not runtime
    if (!this.#table) {
      throw new Error('QueryBuilder: table is required. Call .from("tableName")');
    }

    // Assemble the SQL string
    const parts = [
      `SELECT ${this.#fields.join(', ')}`,
      `FROM ${this.#table}`,
      ...this.#joins,
      this.#conditions.length > 0
        ? `WHERE ${this.#conditions.join(' AND ')}`
        : '',
      this.#orderByClause,
      this.#limitValue  !== null ? `LIMIT ${this.#limitValue}`  : '',
      this.#offsetValue !== null ? `OFFSET ${this.#offsetValue}` : '',
    ].filter(Boolean); // Remove empty strings

    return new Query({
      sql: parts.join(' '),
      params: [...this.#params],
      type: this.#type,
    });
  }

  // Reset builder state (useful for reuse)
  reset() {
    this.#table = '';
    this.#fields = ['*'];
    this.#conditions = [];
    this.#orderByClause = '';
    this.#limitValue = null;
    this.#offsetValue = null;
    this.#joins = [];
    this.#params = [];
    return this;
  }
}

// ---- Usage ----

// Simple query
const simpleQuery = new QueryBuilder()
  .from('users')
  .select('id', 'username', 'email')
  .where("status = 'active'")
  .orderBy('username')
  .limit(10)
  .build();

console.log(simpleQuery.sql);
// SELECT id, username, email FROM users WHERE status = 'active' ORDER BY username ASC LIMIT 10

// Complex query with join
const complexQuery = new QueryBuilder()
  .from('orders')
  .select('orders.id', 'users.username', 'orders.total', 'orders.status')
  .innerJoin('users', 'orders.user_id = users.id')
  .where("orders.status = 'pending'")
  .where("orders.total > 100")
  .orderBy('orders.created_at', 'DESC')
  .limit(20)
  .offset(40)
  .build();

console.log(complexQuery.sql);
// SELECT orders.id, users.username, orders.total, orders.status
// FROM orders INNER JOIN users ON orders.user_id = users.id
// WHERE orders.status = 'pending' AND orders.total > 100
// ORDER BY orders.created_at DESC LIMIT 20 OFFSET 40
```

### 7.7 Builder for Complex Object Construction

```javascript
// ============================================================
// Builder Pattern — User Profile Builder
// Shows validation and optional fields
// ============================================================

class UserProfile {
  constructor(data) {
    // Freeze the object so it can't be accidentally modified
    Object.assign(this, data);
    Object.freeze(this);
  }
}

class UserProfileBuilder {
  // Required fields tracked separately
  #requiredFields = ['username', 'email'];
  #data = {};

  setUsername(username) {
    if (!username || username.length < 3) {
      throw new Error('Username must be at least 3 characters');
    }
    this.#data.username = username.toLowerCase().trim();
    return this;
  }

  setEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      throw new Error(`Invalid email: "${email}"`);
    }
    this.#data.email = email.toLowerCase().trim();
    return this;
  }

  setAge(age) {
    if (age < 0 || age > 150) throw new Error('Invalid age');
    this.#data.age = age;
    return this;
  }

  setRole(role) {
    const validRoles = ['user', 'admin', 'moderator', 'guest'];
    if (!validRoles.includes(role)) {
      throw new Error(`Invalid role. Must be one of: ${validRoles.join(', ')}`);
    }
    this.#data.role = role;
    return this;
  }

  setAvatar(url) {
    this.#data.avatar = url;
    return this;
  }

  setBio(bio) {
    if (bio.length > 500) throw new Error('Bio cannot exceed 500 characters');
    this.#data.bio = bio;
    return this;
  }

  setPreferences(prefs) {
    this.#data.preferences = {
      language: 'en',
      theme: 'light',
      notifications: true,
      ...prefs, // Override with provided values
    };
    return this;
  }

  // The terminal method: validates and builds
  build() {
    // Check that all required fields are present
    const missing = this.#requiredFields.filter(field => !this.#data[field]);
    if (missing.length > 0) {
      throw new Error(`Missing required fields: ${missing.join(', ')}`);
    }

    // Apply defaults for optional fields
    const finalData = {
      id: `usr_${Date.now()}`,
      createdAt: new Date().toISOString(),
      isActive: true,
      role: 'user',
      preferences: { language: 'en', theme: 'light', notifications: true },
      ...this.#data, // Override defaults with set values
    };

    return new UserProfile(finalData);
  }
}

// Usage — natural, readable, no positional confusion
const user = new UserProfileBuilder()
  .setUsername('alice')
  .setEmail('alice@example.com')
  .setAge(28)
  .setRole('admin')
  .setBio('Full-stack developer and open source enthusiast.')
  .setPreferences({ theme: 'dark', notifications: false })
  .build();

console.log(user);
// UserProfile {
//   id: 'usr_...',
//   username: 'alice',
//   email: 'alice@example.com',
//   age: 28,
//   role: 'admin',
//   bio: '...',
//   preferences: { language: 'en', theme: 'dark', notifications: false },
//   isActive: true,
//   createdAt: '...'
// }

// ❌ This throws immediately — fail fast!
try {
  const invalid = new UserProfileBuilder()
    .setUsername('x') // Too short
    .build();
} catch (e) {
  console.error(e.message); // "Username must be at least 3 characters"
}
```

### 7.8 Director Pattern (Optional Extension)

```javascript
// ============================================================
// DIRECTOR — Orchestrates common build sequences
// Useful when you have specific "presets" or standard configurations
// ============================================================

class UserProfileDirector {
  #builder;

  constructor(builder) {
    this.#builder = builder;
  }

  // Builds a standard guest user profile
  buildGuestProfile(sessionId) {
    return this.#builder
      .setUsername(`guest_${sessionId.slice(0, 8)}`)
      .setEmail(`guest_${sessionId}@temp.example.com`)
      .setRole('guest')
      .build();
  }

  // Builds a standard admin profile
  buildAdminProfile(username, email) {
    return this.#builder
      .setUsername(username)
      .setEmail(email)
      .setRole('admin')
      .setPreferences({ theme: 'dark', notifications: true })
      .build();
  }
}

const director = new UserProfileDirector(new UserProfileBuilder());

const guest = director.buildGuestProfile('abc123xyz');
const admin = director.buildAdminProfile('superuser', 'admin@company.com');
```

### 7.9 Dry Run of the Builder

```javascript
const query = new QueryBuilder()
  .from('users')        // 1. Sets #table = 'users', returns `this`
  .select('id', 'name') // 2. Sets #fields = ['id', 'name'], returns `this`
  .where("age > 18")    // 3. Pushes "age > 18" to #conditions, returns `this`
  .limit(5)             // 4. Sets #limitValue = 5, returns `this`
  .build();             // 5. Terminal: assembles and returns Query object
```

**Memory state after each step:**

```
After .from('users'):    { #table: 'users', #fields: ['*'], ... }
After .select('id','name'): { #table: 'users', #fields: ['id', 'name'], ... }
After .where("age > 18"): { ..., #conditions: ['age > 18'], ... }
After .limit(5):          { ..., #limitValue: 5 }
After .build():
  parts = ['SELECT id, name', 'FROM users', 'WHERE age > 18', 'LIMIT 5']
  sql   = 'SELECT id, name FROM users WHERE age > 18 LIMIT 5'
  → returns new Query({ sql: ..., params: [], type: 'SELECT' })
```

### 7.10 Real-World Practical Examples

Many popular libraries use Builder:
- **Knex.js** — SQL query builder for Node.js
- **Mongoose** — `Model.find().where().select().limit().exec()`
- **Axios** — request configuration object
- **Jest** — test configuration via `config` object building
- **Puppeteer** — page interaction chains

### 7.11 Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| Handles optional parameters naturally | More code/boilerplate than simple constructor |
| Expressive, readable fluent API | Builder class must mirror product class properties |
| Validates at build time (fail-fast) | Overkill for simple objects |
| Separates construction from representation | Mutable intermediate state in the builder |
| Can create different representations | |

### 7.12 Common Mistakes and Misconceptions

**Mistake 1: Not returning `this` from setter methods**
```javascript
// ❌ Forgetting `return this` breaks method chaining
setUsername(name) {
  this.username = name;
  // Missing: return this;
}
builder.setUsername('alice').setEmail('...'); // TypeError: setEmail is not a function
```

**Mistake 2: Mutating the product after building**
```javascript
const profile = builder.build();
profile.role = 'admin'; // ❌ If you didn't freeze the object, this silently mutates it!
// Use Object.freeze() in the product constructor to prevent this
```

**Mistake 3: Using Builder for simple objects**
```javascript
// ❌ Over-engineering — just use a constructor or object literal
const point = new PointBuilder().setX(10).setY(20).build();
// ✅ This is fine
const point = { x: 10, y: 20 };
```

### 7.13 Interview Questions

**Q1: What is the "telescoping constructor" problem that Builder solves?**
> When a class has many optional parameters, you end up with many constructor overloads or a single constructor with many parameters where most are null/undefined. This is hard to read and error-prone. Builder solves it by providing a step-by-step API where you only set what you need.

**Q2: What is a fluent interface and how does Builder use it?**
> A fluent interface is an API design where each method returns the same object (`this`), enabling method chaining: `builder.setA(1).setB(2).setC(3).build()`. It's more readable than `builder.setA(1); builder.setB(2); const result = builder.build();`.

**Q3: What is the Director in the Builder pattern?**
> The Director is an optional class that knows how to use the builder to create common/predefined configurations. It takes a builder, calls its methods in a specific order, and returns the product. It removes the need for clients to remember the construction sequence.

**Q4: When would you use Builder over a factory?**
> Use Builder when object construction is complex (many optional parameters, validation steps, multi-step process). Use Factory when the key concern is which type to create. Builder focuses on "how to build"; Factory focuses on "which type to create."

### 7.14 Summary & Key Takeaways

> Builder separates complex object construction into discrete, chainable steps. It's perfect for objects with many optional parameters, complex initialization logic, or construction that requires validation.

**Key Takeaways:**
- Each builder method returns `this` for method chaining (fluent interface)
- `build()` is the terminal method that validates and returns the final object
- Director is optional but useful for common construction presets
- Use `Object.freeze()` on the product to make it immutable after construction

**Practice Exercise:**
> Create an `HttpRequestBuilder` for a fetch API wrapper. Include: `.setUrl(url)`, `.setMethod(method)`, `.addHeader(key, value)`, `.setBody(data)`, `.setTimeout(ms)`, `.setRetries(n)`. The `build()` method should validate the URL is set and method is valid (GET, POST, PUT, DELETE, PATCH), then return a frozen `HttpRequest` object with an `execute()` method.

---

## 8. Prototype Pattern

### 8.1 The Problem It Solves

Consider this scenario: you have a complex `Configuration` object that took 20+ operations to set up (database queries, file reads, API calls). Now you need a slightly different copy of it for a specific user. You don't want to redo all 20 operations — you want to **copy the existing object and modify only what's different**.

Or imagine in a game you have 100 enemy soldiers. They're all identical except for their position. Creating each one from scratch is wasteful. You want to **clone a prototype soldier and just change the position**.

**The Prototype Pattern lets you create new objects by cloning an existing object (the prototype), rather than constructing from scratch.**

### 8.2 Real-World Analogy

Think of **photocopying a document**.

You have an original report (the prototype). To create 10 slightly different versions for 10 departments, you photocopy the original and stamp each copy with the department name. You didn't write the entire report 10 times — you copied and modified.

Another analogy: **cell division in biology**. A cell (the prototype) creates new cells by cloning itself. The copy starts as an exact replica of the parent.

### 8.3 When to Use It

✅ **Use when:**
- Creating a new object is more expensive than cloning an existing one
- Objects have only slight differences from each other
- You want to avoid complex initialization by cloning an already-initialized object
- The class to instantiate is specified at runtime (dynamic typing)

❌ **Avoid when:**
- Objects are simple and cheap to create
- Deep cloning causes unexpected behavior with circular references or complex objects
- Inheritance hierarchy or object structure changes frequently

### 8.4 How It Works Internally

```
1. Define a `clone()` method on the object to be prototyped
2. The `clone()` method creates a copy of the current object
3. SHALLOW copy: top-level properties are copied; nested objects are shared
4. DEEP copy: all properties, including nested objects, are recursively copied
5. Client receives a copy and can modify it without affecting the original
```

### 8.5 Shallow vs. Deep Copy — Critical Distinction

```
Original Object:
┌──────────────────────────────────────────┐
│ name: "Config A"                         │
│ maxConnections: 10                       │
│ settings: ──────────────────────────────┼──→ { theme: 'dark', timeout: 30 }
└──────────────────────────────────────────┘

SHALLOW COPY (Object.assign / spread):
┌──────────────────────────────────────────┐
│ name: "Config B"      ← own copy         │
│ maxConnections: 10    ← own copy         │
│ settings: ─────────────────────────────┐│
└────────────────────────────────────────┼┘
                                         │
    Both original AND copy share the ────┘→ { theme: 'dark', timeout: 30 }
    SAME settings object!
    Changing copy.settings.theme ALSO changes original.settings.theme!

DEEP COPY:
┌──────────────────────────────────────────┐
│ name: "Config B"      ← own copy         │
│ maxConnections: 10    ← own copy         │
│ settings: ──────────────────────────────┼──→ { theme: 'dark', timeout: 30 }
└──────────────────────────────────────────┘   ← SEPARATE copy of settings!
```

### 8.6 JavaScript Implementation

```javascript
// ============================================================
// PROTOTYPE PATTERN — Game Character Example
// ============================================================

class Character {
  constructor(config) {
    this.name = config.name;
    this.class = config.class;
    this.level = config.level || 1;
    this.position = config.position || { x: 0, y: 0 }; // Nested object!
    this.stats = config.stats || {
      health: 100,
      maxHealth: 100,
      attack: 10,
      defense: 5,
      speed: 3,
    };
    this.inventory = config.inventory || [];
    this.abilities = config.abilities || [];
    this.isAlive = true;
  }

  // ==================== SHALLOW CLONE ====================
  shallowClone() {
    // Object.create sets the clone's prototype to the original's prototype
    const clone = Object.create(Object.getPrototypeOf(this));
    // Object.assign copies own enumerable properties (top-level only)
    Object.assign(clone, this);
    return clone;
    // ⚠️ clone.position and clone.stats still reference the SAME objects!
    // Modifying clone.position.x also modifies this.position.x
  }

  // ==================== DEEP CLONE ====================
  clone() {
    // Creates a truly independent copy — safe to modify nested objects
    const cloneData = {
      name: this.name,
      class: this.class,
      level: this.level,
      position: { ...this.position },    // Copy the nested object
      stats: { ...this.stats },          // Copy the stats object
      inventory: [...this.inventory],    // Copy the array
      abilities: [...this.abilities],    // Copy the array
    };

    const clone = new Character(cloneData);
    clone.isAlive = this.isAlive;
    return clone;
  }

  moveTo(x, y) {
    this.position = { x, y };
    return this;
  }

  levelUp() {
    this.level++;
    this.stats.maxHealth += 20;
    this.stats.health = this.stats.maxHealth;
    this.stats.attack += 3;
    this.stats.defense += 2;
    return this;
  }

  takeDamage(amount) {
    this.stats.health = Math.max(0, this.stats.health - amount);
    if (this.stats.health === 0) this.isAlive = false;
    return this;
  }

  toString() {
    return `${this.name} (${this.class} Lv.${this.level}) @ (${this.position.x},${this.position.y}) HP:${this.stats.health}/${this.stats.maxHealth}`;
  }
}

// ---- Prototype (the "template" character) ----
const archerPrototype = new Character({
  name: 'Archer',
  class: 'Ranger',
  level: 1,
  stats: { health: 80, maxHealth: 80, attack: 15, defense: 3, speed: 5 },
  abilities: ['quick_shot', 'arrow_rain', 'evasion'],
  inventory: ['bow', 'quiver_of_arrows', 'leather_armor'],
});

// ---- Clone the prototype to create many archers ----
const archer1 = archerPrototype.clone().moveTo(10, 5);
const archer2 = archerPrototype.clone().moveTo(20, 8);
const archer3 = archerPrototype.clone().moveTo(15, 12);

// Give each a unique name
archer1.name = 'Robin';
archer2.name = 'Katniss';
archer3.name = 'Legolas';

// Modify one — others unaffected (deep clone protection)
archer1.takeDamage(30);
archer1.levelUp();

console.log(archer1.toString()); // Robin (Ranger Lv.2) @ (10,5) HP:70/100
console.log(archer2.toString()); // Katniss (Ranger Lv.1) @ (20,8) HP:80/80 — unaffected!
console.log(archer3.toString()); // Legolas (Ranger Lv.1) @ (15,12) HP:80/80 — unaffected!

// SHALLOW CLONE BUG DEMONSTRATION:
const badClone = archerPrototype.shallowClone();
badClone.name = 'CopyArcher';
badClone.stats.attack = 999; // ⚠️ This modifies the SHARED stats object!

console.log(archerPrototype.stats.attack); // 999 — BUG! Prototype is corrupted!
```

### 8.7 JavaScript's Built-in Prototype Support

```javascript
// ============================================================
// JavaScript's NATIVE prototype system
// (Different from the Prototype Design Pattern, but related)
// ============================================================

// Object.create() is the built-in prototype-based cloning
const baseConfig = {
  debug: false,
  maxRetries: 3,
  timeout: 5000,
  log(msg) { console.log(`[${this.env}] ${msg}`); },
};

// Create an object with baseConfig as its prototype
const devConfig = Object.create(baseConfig);
devConfig.env = 'development';
devConfig.debug = true; // Override only this field

const prodConfig = Object.create(baseConfig);
prodConfig.env = 'production';
prodConfig.timeout = 10000; // Override only this field

devConfig.log('Starting up...');  // [development] Starting up...
prodConfig.log('Starting up...'); // [production] Starting up...

// Property lookup:
// devConfig.maxRetries → not on devConfig → check prototype → found: 3
// devConfig.debug → found on devConfig → true (overrides prototype's false)

console.log(devConfig.maxRetries);   // 3 (from prototype)
console.log(prodConfig.maxRetries);  // 3 (from prototype)
console.log(devConfig.debug);        // true (own property, overrides prototype)
console.log(prodConfig.debug);       // false (from prototype — not overridden)
```

### 8.8 Deep Clone Utilities

```javascript
// ============================================================
// Modern Deep Clone approaches
// ============================================================

// Approach 1: structuredClone (Node.js 17+, modern browsers)
// Handles: nested objects, arrays, Date, Map, Set, RegExp, circular refs
const original = {
  name: 'Config',
  date: new Date(),
  map: new Map([['key', 'value']]),
  nested: { deeply: { nested: true } },
};

const deepCopy = structuredClone(original);
deepCopy.nested.deeply.nested = false;
console.log(original.nested.deeply.nested); // true — unaffected!

// Approach 2: JSON roundtrip (simple but lossy)
// ⚠️ Loses: undefined, functions, Date (becomes string), Map/Set, circular refs
const jsonClone = JSON.parse(JSON.stringify(original));
// original.date becomes a string in jsonClone — bad for Dates!

// Approach 3: Custom recursive deep clone
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (obj instanceof Date) return new Date(obj.getTime());
  if (obj instanceof RegExp) return new RegExp(obj.source, obj.flags);
  if (obj instanceof Map) return new Map([...obj].map(([k, v]) => [deepClone(k), deepClone(v)]));
  if (obj instanceof Set) return new Set([...obj].map(deepClone));
  if (Array.isArray(obj)) return obj.map(deepClone);

  // Plain object
  return Object.fromEntries(
    Object.entries(obj).map(([k, v]) => [k, deepClone(v)])
  );
}
```

### 8.9 Prototype Registry (Advanced)

```javascript
// ============================================================
// PROTOTYPE REGISTRY — Managing multiple prototypes
// ============================================================

class CharacterRegistry {
  #prototypes = new Map();

  register(key, prototype) {
    this.#prototypes.set(key, prototype);
    return this; // Enable chaining
  }

  clone(key, overrides = {}) {
    const prototype = this.#prototypes.get(key);
    if (!prototype) {
      throw new Error(`No prototype registered for key: "${key}"`);
    }
    // Clone the prototype and apply overrides
    const cloned = prototype.clone();
    Object.assign(cloned, overrides);
    return cloned;
  }

  list() {
    return [...this.#prototypes.keys()];
  }
}

// Setup the registry with prototypes
const registry = new CharacterRegistry();

registry
  .register('archer', new Character({
    name: 'Archer',
    class: 'Ranger',
    stats: { health: 80, maxHealth: 80, attack: 15, defense: 3, speed: 5 },
    abilities: ['quick_shot'],
  }))
  .register('warrior', new Character({
    name: 'Warrior',
    class: 'Fighter',
    stats: { health: 150, maxHealth: 150, attack: 12, defense: 10, speed: 2 },
    abilities: ['shield_bash'],
  }))
  .register('mage', new Character({
    name: 'Mage',
    class: 'Wizard',
    stats: { health: 60, maxHealth: 60, attack: 20, defense: 2, speed: 3 },
    abilities: ['fireball', 'ice_lance'],
  }));

// Spawn enemies in a game loop
function spawnWave(count) {
  const types = ['archer', 'warrior', 'mage'];
  return Array.from({ length: count }, (_, i) => {
    const type = types[i % types.length];
    return registry.clone(type, {
      name: `${type}_${i + 1}`,
      position: { x: Math.random() * 100, y: Math.random() * 100 },
    });
  });
}

const wave = spawnWave(6);
wave.forEach(enemy => console.log(enemy.toString()));
```

### 8.10 Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| Avoids expensive initialization for similar objects | Deep cloning complex objects is tricky |
| Simple cloning API | Shallow clone bugs are easy to introduce |
| Reduces subclassing for variation | Circular references can break naive deep clone |
| Efficient for creating many similar objects | `clone()` method must be maintained as class evolves |

### 8.11 Common Mistakes and Misconceptions

**Mistake 1: Confusing JavaScript's prototype chain with the Prototype Design Pattern**
> JavaScript's `[[Prototype]]` chain is a language feature (prototypal inheritance). The Prototype *Design Pattern* is a way to create objects by cloning. They're related conceptually but are distinct things.

**Mistake 2: Shallow cloning when deep clone is needed**
> The most common bug. If your object has nested objects or arrays, always deep clone unless you intentionally want shared references.

**Mistake 3: Not cloning when you should**
```javascript
const config = registry.clone('base'); // Gets a clone — safe to modify
const config2 = registry.getPrototype('base'); // ❌ Gets the ORIGINAL — don't modify!
```

### 8.12 Interview Questions

**Q1: What is the difference between the Prototype pattern and JavaScript's prototype chain?**
> JavaScript's prototype chain is a language mechanism for prototypal inheritance (property lookup delegation). The Prototype design pattern is a creational pattern for object cloning — creating new instances by copying existing ones. The design pattern can use JS's prototype chain internally, but they address different concerns.

**Q2: When should you use `structuredClone()` vs. manual deep clone?**
> Use `structuredClone()` when available (Node.js 17+, modern browsers) — it handles Dates, Maps, Sets, and circular references. Use a manual deep clone when you need custom cloning behavior (e.g., skipping certain fields, handling custom classes specially) or need to support older environments.

**Q3: What's the time complexity trade-off of cloning vs. fresh construction?**
> Cloning is O(n) where n is the object's size. Fresh construction may be O(n) too, but if it involves I/O (database reads, API calls, file system), cloning is dramatically faster. The pattern's benefit is maximized when fresh construction is expensive.

**Q4: What is a Prototype Registry?**
> A Prototype Registry is a centralized store (usually a map) that holds named prototype objects. Clients request clones by name (e.g., `registry.clone('enemy_archer')`), and the registry deep-clones the stored prototype. It decouples the client from knowing about specific prototype classes.

### 8.13 Summary & Key Takeaways

> The Prototype Pattern creates new objects by cloning an existing one. It's especially powerful when object creation is expensive and many objects share the same base configuration with minor variations.

**Key Takeaways:**
- Always deep clone unless shared references are intentional
- `structuredClone()` is the modern built-in deep clone
- A Prototype Registry centralizes prototype management
- JavaScript's module-based singleton and prototype chain are closely related concepts

**Practice Exercise:**
> Build a `DocumentTemplate` system. Create a `Document` class with title, content (array of paragraphs), metadata (object with author, date, tags), and formatting settings. Implement `clone()` and a `TemplateRegistry`. Register templates for 'invoice', 'report', and 'letter'. Each `clone()` should produce a fully independent copy, and you should demonstrate that modifying a clone's nested data doesn't affect the original template.

---

## 9. Object Pool Pattern (Bonus)

### 9.1 The Problem It Solves

Some objects are **expensive to create and destroy** (database connections, thread instances, large binary objects). If you create and destroy them constantly, your application becomes slow and resource-hungry.

**The Object Pool Pattern maintains a collection of reusable objects.** Instead of creating a new object, you "borrow" one from the pool. When done, you "return" it to the pool for the next user.

### 9.2 Real-World Analogy

Think of a **car rental company**. Instead of manufacturing a new car for every customer and scrapping it when they return, the company maintains a fleet (pool). A customer borrows a car, uses it, returns it. The next customer borrows the same (cleaned) car.

### 9.3 When to Use It

✅ **Use when:**
- Object creation is expensive (DB connections, network sockets, threads)
- You need many short-lived objects of the same type
- You can predict the maximum number of objects needed

❌ **Avoid when:**
- Objects hold state that's hard to reset between uses
- Objects are cheap to create
- You need unlimited instances

### 9.4 JavaScript Implementation

```javascript
// ============================================================
// OBJECT POOL PATTERN
// Database Connection Pool Example
// ============================================================

class DatabaseConnection {
  constructor(id) {
    this.id = id;
    this.isConnected = false;
    this.queryCount = 0;
    this.lastUsed = null;
  }

  connect() {
    this.isConnected = true;
    console.log(`Connection #${this.id} opened.`);
  }

  disconnect() {
    this.isConnected = false;
    console.log(`Connection #${this.id} closed.`);
  }

  query(sql) {
    if (!this.isConnected) throw new Error('Not connected!');
    this.queryCount++;
    this.lastUsed = new Date();
    console.log(`[Conn #${this.id}] Query #${this.queryCount}: ${sql}`);
    return []; // Simulated result
  }

  // Reset the connection to a clean state before returning to pool
  reset() {
    // Don't disconnect — keep the connection alive for reuse
    // Just clear request-specific state
    this.lastUsed = null;
    // In a real connection, you'd rollback any uncommitted transactions
  }
}

class ConnectionPool {
  #pool = [];         // Available (idle) connections
  #inUse = new Set(); // Currently borrowed connections
  #maxSize;
  #currentId = 0;

  constructor(maxSize = 5) {
    this.#maxSize = maxSize;
    // Eagerly create all connections
    this.#initialize();
  }

  #initialize() {
    for (let i = 0; i < this.#maxSize; i++) {
      const conn = new DatabaseConnection(++this.#currentId);
      conn.connect();
      this.#pool.push(conn);
    }
    console.log(`Pool initialized with ${this.#maxSize} connections.`);
  }

  // Borrow a connection from the pool
  acquire() {
    if (this.#pool.length === 0) {
      throw new Error(
        `Connection pool exhausted! All ${this.#maxSize} connections are in use.`
      );
    }

    const connection = this.#pool.pop(); // Take from the available pool
    this.#inUse.add(connection);         // Mark as in-use
    console.log(`Connection #${connection.id} acquired. Pool: ${this.#pool.length} idle, ${this.#inUse.size} in use.`);
    return connection;
  }

  // Return a connection to the pool
  release(connection) {
    if (!this.#inUse.has(connection)) {
      console.warn(`Warning: Releasing a connection that wasn't acquired from this pool.`);
      return;
    }

    connection.reset();          // Reset to clean state
    this.#inUse.delete(connection); // Remove from in-use set
    this.#pool.push(connection);    // Return to available pool
    console.log(`Connection #${connection.id} released. Pool: ${this.#pool.length} idle, ${this.#inUse.size} in use.`);
  }

  // Utility: execute a query and auto-release the connection
  async withConnection(callback) {
    const connection = this.acquire();
    try {
      const result = await callback(connection);
      return result;
    } finally {
      // Always release, even if callback throws
      this.release(connection);
    }
  }

  getStats() {
    return {
      total: this.#maxSize,
      idle: this.#pool.length,
      inUse: this.#inUse.size,
    };
  }

  // Gracefully shut down all connections
  async shutdown() {
    console.log('Shutting down connection pool...');
    // Release all in-use connections first
    [...this.#inUse].forEach(conn => this.release(conn));
    // Disconnect all connections
    this.#pool.forEach(conn => conn.disconnect());
    this.#pool.length = 0;
    console.log('Pool shut down.');
  }
}

// ---- Usage ----
const pool = new ConnectionPool(3); // Max 3 simultaneous connections
console.log(pool.getStats()); // { total: 3, idle: 3, inUse: 0 }

// Simulate concurrent requests
const conn1 = pool.acquire(); // Pool: 2 idle, 1 in use
const conn2 = pool.acquire(); // Pool: 1 idle, 2 in use
const conn3 = pool.acquire(); // Pool: 0 idle, 3 in use

conn1.query('SELECT * FROM users');
conn2.query('SELECT * FROM orders WHERE status = "pending"');

pool.release(conn1); // Pool: 1 idle, 2 in use — conn1 back and ready!
pool.release(conn2); // Pool: 2 idle, 1 in use

// Using the safe withConnection helper
pool.withConnection(async (conn) => {
  const users = conn.query('SELECT * FROM users LIMIT 10');
  return users;
  // Connection auto-released even if this throws
});

pool.shutdown();
```

### 9.5 Interview Questions

**Q1: When would you use Object Pool over just creating new instances?**
> When object creation/destruction is expensive (DB connections, large buffers, threads). The pool avoids the overhead by reusing objects. Database connection pools are the canonical real-world example — establishing a TCP connection + authentication overhead is significant.

**Q2: What is the main risk of Object Pool?**
> **Stale state**: if an object isn't properly reset before returning to the pool, the next borrower sees leftover state (uncommitted transactions, residual data). Always implement and call `reset()` before releasing.

---

## 10. Comparison Table — All Creational Patterns

| Pattern | Purpose | Key Mechanism | Use When |
|---|---|---|---|
| **Constructor** | Create instances of a class | `new ClassName()` | Multiple objects with same structure |
| **Factory Method** | Delegate creation to subclasses | Override `createProduct()` | Creating one type of object, type varies by subclass |
| **Abstract Factory** | Create families of related objects | Multiple factory methods grouped | Related objects must be consistent as a family |
| **Singleton** | Ensure single instance | `static #instance` + `getInstance()` | Shared global resource (config, logger) |
| **Builder** | Construct complex objects step-by-step | Chained setters + `build()` | Complex objects with many optional params |
| **Prototype** | Create by cloning an existing object | `clone()` / `structuredClone()` | Expensive construction, many similar objects |
| **Object Pool** | Reuse expensive objects | acquire/release pool | Expensive-to-create short-lived objects |

### Decision Tree

```
Do you need to create objects?
│
├─ Just one single instance ever?
│   └─ SINGLETON
│
├─ Complex object with many optional parameters?
│   └─ BUILDER
│
├─ Clone an existing object?
│   └─ PROTOTYPE
│
├─ Reuse expensive objects?
│   └─ OBJECT POOL
│
├─ Create a family of related objects?
│   └─ ABSTRACT FACTORY
│
├─ Let subclasses decide which type to create?
│   └─ FACTORY METHOD
│
└─ Just create instances of a known class?
    └─ CONSTRUCTOR PATTERN
```

### Pattern Relationships

```
CREATIONAL PATTERN RELATIONSHIPS

Constructor ──extends──→ Factory Method
                              │
                              │ generalizes to
                              ▼
                       Abstract Factory ←── uses ── Builder
                              
Prototype ←── can be managed by ── Object Pool

Singleton ──── can be implemented using ──── Module Pattern
```

---

## 11. Final Interview Cheat Sheet

### Quick Definitions

| Pattern | One-Line Definition |
|---|---|
| Constructor | Blueprint for creating multiple objects of the same type |
| Factory Method | A method that subclasses override to create specific object types |
| Abstract Factory | A factory of factories; creates entire families of related objects |
| Singleton | Guarantees exactly one instance with a global access point |
| Builder | Step-by-step construction of complex objects via a fluent API |
| Prototype | Create new objects by cloning existing ones |
| Object Pool | Reuse expensive objects by maintaining a pool |

### SOLID Principles Supported

| Pattern | Supported Principles |
|---|---|
| Factory Method | Open/Closed, Dependency Inversion |
| Abstract Factory | Open/Closed, Dependency Inversion, Interface Segregation |
| Singleton | (Controversial — can violate DI and SRP) |
| Builder | Single Responsibility, Open/Closed |
| Prototype | Open/Closed |

### Top 10 Interview Questions Across All Patterns

1. **What are Creational Design Patterns and why do they matter?**
   > They abstract the process of object creation, making systems more flexible, reusable, and decoupled from concrete implementations.

2. **Factory Method vs. Abstract Factory — key difference?**
   > Factory Method: one product type, uses inheritance. Abstract Factory: multiple product types (a family), uses composition.

3. **Why is Singleton controversial?**
   > It introduces hidden global state, making testing hard, hiding dependencies, and potentially violating SOLID principles.

4. **What is a fluent interface?**
   > An API design where each method returns `this`, enabling readable method chaining: `builder.setA(1).setB(2).build()`.

5. **Shallow vs. deep clone in Prototype?**
   > Shallow: copies top-level properties only, nested objects are shared. Deep: recursively copies everything, fully independent.

6. **When is `structuredClone()` the right choice?**
   > When you need a deep copy in modern JavaScript (Node.js 17+, modern browsers) that handles Date, Map, Set, and circular references.

7. **How does ES module caching relate to Singleton?**
   > ES modules are evaluated once and cached. Exporting an instance from a module is idiomatic JavaScript Singleton — no `getInstance()` needed.

8. **What problem does Builder solve that a constructor can't?**
   > Builder handles optional parameters cleanly without "telescoping constructor" issues, provides validation at build time, and gives a readable fluent API.

9. **What's the Object Pool pattern and when is it needed?**
   > It maintains a pool of reusable objects to avoid the cost of repeated creation/destruction. Used for DB connections, threads, and large buffers.

10. **How do you make a Singleton testable?**
    > Provide a static `_reset()` method for test cleanup, or better: use dependency injection instead of direct `getInstance()` calls to make dependencies explicit and swappable.

### Code Pattern Recognition

When you see this in code, recognize the pattern:

```javascript
SomeClass.getInstance()          // → Singleton
new SomeBuilder().setA().build() // → Builder
registry.clone('key')            // → Prototype + Registry
new LightFactory().createButton()// → Abstract Factory
SomeClass.create('type')         // → Factory Method (simple variant)
pool.acquire() / pool.release()  // → Object Pool
```

---

## Congratulations! 🎉

You've completed the full Creational Design Patterns learning roadmap. Here's your learning path summary:

```
✅ Constructor Pattern    — Foundation of OOP in JavaScript
✅ Factory Method         — Flexible, decoupled object creation
✅ Abstract Factory       — Consistent product families
✅ Singleton              — Single shared instances
✅ Builder                — Complex object construction
✅ Prototype              — Clone-based creation
✅ Object Pool            — Reusable expensive objects
```

### Next Steps

1. **Implement** every practice exercise from this guide
2. **Read** the original GoF book: *Design Patterns* by Gamma, Helm, Johnson, Vlissides
3. **Study** Structural Patterns next: Adapter, Decorator, Facade, Proxy, Composite
4. **Then** Behavioral Patterns: Observer, Strategy, Command, Iterator, State
5. **Practice** identifying patterns in popular libraries: Express.js, React, Lodash, Mongoose

---

*This guide was designed to take you from zero to interview-ready on all Creational Design Patterns in JavaScript. Every pattern, every concept, every nuance — no skipping.*
