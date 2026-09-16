# Apex OOP Concepts

**Series 7: Apex Fundamentals — 9 Topics**
Properties · Methods · Constructors · Public/Private/Global/Protected · Inheritance (`extends`) · Interface (`implements`) · Polymorphism · Encapsulation · Abstraction

---

## 1. Properties

A property looks like a field, but has built-in get/set logic.

### 🌱 Simple
```apex
public Integer count { get; set; }
```
- **Normal field:** data is accessed directly
- **Property:** data goes through get/set — controlled access

A property combines a field with its accessors.

### Automatic Property
```apex
public Integer count { get; set; }
```
No need to manually write getter/setter bodies.

### Custom Setter
```apex
public String name {
    get;
    set {
        name = value?.trim();
    }
}
```

### Asymmetric Access
Getter and setter can have different visibility:
```apex
public Integer x {
    get;
    private set;
}
```
Meaning:
- Outside the class → read allowed
- Outside the class → write **not** allowed
- Inside the class → read + write allowed

### Key Patterns

**1. Asymmetric Visibility**
```apex
public X {
    get;
    private set;
}
```
External code can read the value but not modify it — the idiomatic Apex pattern for exposing externally immutable/read-only state.

**2. Lazy Loading**
Defer an expensive operation until first access:
```apex
get {
    if (cache == null) {
        cache = compute();
    }
    return cache;
}
```
Benefits: avoids unnecessary work, defers expensive calculations/queries, pay the cost only when needed.

**3. Controller Binding**
Visualforce expressions and LWC `@AuraEnabled` getters can depend on properties/accessors. Keep getters side-effect-free — a getter may be called multiple times during rendering.

**4. Public Field vs Property**
If validation/calculation/extra logic might be needed later, prefer a property over a public field. Especially in managed packages — changing a public field to a property later can break binary compatibility.

### Architect Rule
> Future logic expected? → Use Property

Properties preserve encapsulation through controlled access, lazy loading, and validation.

### Code Example
```apex
public class Cart {
    // Computed + read-only
    public Decimal total {
        get {
            return computeTotal();
        }
    }
    // Externally read-only
    public String code {
        get;
        private set;
    }
}
```
- `total` → externally readable, internally calculated
- `code` → externally readable, but not externally writable

### Limits
- Auto-implemented properties have no body
- Auto properties can't be inline-initialized in every context
- The property getter runs on every access — avoid expensive work in getters
- Properties used in Visualforce can contribute to the 170 KB view state limit, unless marked `transient`

### Best Practice
Prefer asymmetric visibility + lazy getters + setter validation over plain public fields.

**Why?** Encapsulation + deferred cost + validation + future flexibility.

---

## 2. Methods

A method is reusable, named behavior.

A method takes parameters, does some work, and optionally returns a value.

**Common components:** access modifier, `static` (optional), return type/`void`, method name, parameters.

### Passing Behavior
Apex passes:
- Primitives → **by value**
- Objects → **by reference**

### A Good Method
Ideally a method should be:
- Single-purpose
- Bulk-safe
- Clear about side effects
- Accepting/returning collections where appropriate

### Static vs Instance

**Static method** — associated with the class:
```apex
Utility.calculate();
```

**Instance method** — associated with an object:
```apex
service.calculate();
```

**Static:** stateless utility methods.
**Instance:** stateful services/handlers — instance design enables mocking/DI through interfaces.

### One Method = One Job
Small methods are easier to test, compose, and reduce cyclomatic complexity.

### Mutation Awareness
If you mutate a passed sObject/collection, the caller's object may change too.
- Mutating intentionally? → document it
- Unexpected mutation risk? → clone/copy

### Separate Calculation from I/O

**Bad:**
```
calculate → SOQL → DML → calculation
```

**Better:**
```
SOQL / DML → Pure calculation → Result
```

### Overloading vs Optional Parameters
Apex has no default parameters. For optional behavior, use:
- Overloaded methods
- Wrapper parameter (e.g., a settings object)
- Collection-based input

### Visibility
Expose minimum visibility. Default mindset: **private first**.

Architects prefer small, bulk-safe, single-purpose methods.

### Limits
- Maximum 32 parameters per method
- Recursion is bounded mainly by heap/CPU, not a fixed depth
- Apex org code-size limit is 6 MB characters (test classes excluded)
- Managed-package class methods can't be overridden unless declared `virtual`/`global` appropriately
- Every method call adds measurable CPU cost, especially in hot loops

### Best Practice
Small + single-purpose + collection-based methods, with pure logic separated from I/O.

**Why?** Bulk-safe, reusable, unit-testable, maintainable.

---

## 3. Constructors

A constructor does initial setup when a new object is created.

### 🌱 Simple
When you create an object with `new`, the constructor runs:
```apex
public Cart(Id ownerId) {
    this.ownerId = ownerId;
}
```
Purpose: `Object create → Initial state setup`

### Advanced
A constructor:
- Has the same name as the class
- Has no return type
- Can be overloaded

### Default Constructor
If you write no constructor, Apex provides a no-arg constructor. But as soon as you define any constructor yourself, the implicit default constructor disappears — add one explicitly if needed.

### Constructor Chaining
Use `this(...)` to call another constructor, avoiding duplicated initialization.

### Constructor Responsibility
A constructor's main purpose: required dependencies + initial invariants + valid object state. Avoid heavy SOQL/DML in the constructor.

### Key Patterns

**1. Dependency Injection**
A constructor can accept collaborators (Selector, Service, Repository):
```apex
public AccountService(IAccountSelector selector) {
    this.selector = selector;
}
```
Tests can pass a mock — an important pattern for unit-testable Apex.

**2. Default Constructor Rule**
If you define any constructor, the implicit no-arg constructor is gone. Some frameworks/deserialization/Visualforce scenarios need a no-arg constructor — provide it explicitly if required.

**3. Constructor Chaining**
`this(...)` centralizes initialization and avoids duplication.

**4. Keep Constructors Cheap**
Avoid SOQL, DML, and callouts in constructors — a constructor may run many times; defer heavy work to a method.

**5. Invariants**
Validate required arguments and fail fast before an invalid object is created. Goal: an object should never exist in an invalid state.

**6. Static Factory Methods**
Named factory methods can be clearer than overloaded constructors:
```apex
Payment.createForCustomer()
Payment.createFromOrder()
```
This makes creation intent explicit.

### Errors & Gotchas
- Missing no-arg constructor → can break frameworks
- Heavy SOQL/DML in constructor → governor-limit issues
- Duplicate initialization across multiple constructors → use `this()`

### Limits
- No explicit constructor → default no-arg constructor is provided
- Any explicit constructor → implicit default is removed
- Constructors can't be `@future` or `@AuraEnabled`
- Constructors don't return a value
- Queueable/Batch classes may need a no-arg constructor for certain serialization paths
- Heavy constructor work runs on every instantiation

### Best Practice
Lightweight constructors + DI + invariants + constructor chaining. Provide an explicit no-arg constructor if a framework requires it.

**Why?** Testable, always-valid objects, no hidden initialization cost.

---

## 4. Public / Private / Global / Protected

Access modifiers control the visibility of classes/members.

### Visibility Table
| Modifier | Visibility |
|---|---|
| `private` | Same class |
| `protected` | Same class + subclasses |
| `public` | Same namespace/app |
| `global` | All namespaces |

`global` is important for cross-namespace access needed by managed package subscribers.

- `private` — default visibility for Apex members
- `public` — accessible within the same namespace
- `global` — required for cross-namespace access (managed package APIs, web services, certain `@AuraEnabled`, `Batchable`, `Schedulable`)
- `protected` — for subclass access

### Golden Rule
Expose the minimum required visibility.

### Key Patterns

**1. Least Privilege**
Default to `private`; widen visibility only when a consumer genuinely requires it. Over-exposure increases coupling.

**2. `global` Is a Long-Term Contract**
Exposing a `global` API in a managed package is a serious decision — removing/narrowing it later can break subscribers. So `global` = a deliberate API contract.

**3. `public` ≠ Cross-Namespace**
Common mistake: assuming `public` is cross-namespace. Actually:
- `public` → same namespace
- `global` → cross-namespace

**4. `protected` for Extension Points**
Useful for giving subclasses hooks without making everything public — useful in the Template Method pattern.

**5. Interfaces over Global Classes**
Better managed-package architecture:
```
global interface
      ↓
hidden implementation
```
The contract stays stable while you can change the implementation.

### Errors & Gotchas
- Calling a `public` method from another namespace → "Method not visible"
- Too many `global` methods → the package API becomes permanently locked
- Tests can't directly access private members → use `@TestVisible`

### Limits
- In a managed package, `global` members are a permanent API commitment
- `@RestResource` classes and methods must be `global`
- `@TestVisible` makes private members accessible to tests
- Access modifiers are **not** a security boundary — CRUD/FLS/sharing are separate security concerns

### Best Practice
Private by default → protected for extension → global only for deliberate cross-namespace contracts → `@TestVisible` for testing.

**Why?** Minimizes coupling and avoids a permanent public surface. Expose a `global interface` with a hidden implementation, and keep the implementation public/private as needed.

---

## 5. Inheritance (`extends`)

Reusing an existing class to build a new, specialized class.

```apex
class SavingsAccount extends BankAccount {
}
```
```
BankAccount
      ↓
SavingsAccount
```

A subclass can inherit parent members and override behavior. In Apex, the base class/methods must be `virtual` or `abstract` to allow overriding.

- Subclass → uses `extends`
- Override → uses `override`
- Call the parent implementation → `super.method();`

Apex supports **single inheritance** — a class can extend only one class, but can implement multiple interfaces.

Use inheritance only for a genuine **is-a** relationship, e.g., `SavingsAccount is-a BankAccount`.

### Key Patterns

**1. `virtual` / `abstract`**
Apex methods are `final`/non-overridable by default. To allow overriding, use `virtual` or `abstract`.

**2. Template Method Pattern**
An abstract base class defines the algorithm's skeleton; subclasses implement specific steps:
```
TriggerHandler
  ├── beforeInsert()
  ├── afterUpdate()
  └── common framework
```
Subclasses override only the hooks they need.

**3. Single Inheritance**
Use interfaces for cross-cutting capabilities: one base class + many interfaces.

**4. Favor Composition over Inheritance**
If there's no true is-a relationship, composition is often better:
```
Instead of: Class A extends B
Use:        Class A → uses/injects B
```
Deep inheritance can be fragile.

**5. `super` + Constructors**
A subclass constructor can call the parent constructor via `super(...)`. Manage initialization order carefully.

**6. Liskov Substitution**
A subclass should be a valid substitute for its parent. If a subclass weakens/breaks the parent's contract, the design is problematic.

### Errors & Gotchas
- "Cannot override non-virtual method" → the base method isn't `virtual`/`abstract`
- Trying multiple inheritance → not supported
- Deep hierarchies → fragile; consider composition

### Limits
- Single inheritance only — a class can extend at most one class
- The parent must be `virtual` or `abstract` where overriding is needed
- Override behavior depends on the base's `virtual`/`abstract` design
- Extending a managed-package class requires an appropriate `global virtual` design
- Deep hierarchies increase CPU/testing complexity

### Best Practice
Abstract base classes for shared frameworks + interfaces for capabilities + composition for everything else.

**Why?** True is-a relationships get reuse without an unnecessarily brittle hierarchy.

### Example — Trigger Handler
**Problem:** Every trigger handler repeats the same before/after boilerplate.
**Solution:** Create a `virtual TriggerHandler` base class with `beforeInsert()`, `afterUpdate()`, etc. Each object handler `extends TriggerHandler` and overrides required methods — a Template Method pattern.

---

## 6. Interface (`implements`)

An interface is a contract.

### 🌱 Simple
An interface defines method **signatures**, not implementations.
```apex
interface Payable {
    void pay();
}
```
```apex
class CreditCardPayment implements Payable {
    public void pay() {
        // implementation
    }
}
```
- Interface → Contract
- Class → Actual implementation

A class can implement multiple interfaces.

**Benefits:** polymorphism, decoupling, dependency injection, mocking.

Salesforce system interfaces: `Database.Batchable`, `Queueable`, `Schedulable`, `Comparable`, `Callable` — used to plug into platform behavior.

### Key Patterns

**1. Program to Interfaces**
Don't depend on a concrete implementation — depend on `IAccountSelector` instead. A real, mock, or alternative implementation can be swapped easily.

**2. Strategy / Plugin Pattern**
```apex
Map<String, ProcessorIntf>
```
Select the correct implementation at runtime. New behavior can be added without editing the existing caller — supports the Open/Closed Principle.

**3. Platform Contracts**
- `Batchable` → async batch
- `Queueable` → queue job
- `Schedulable` → schedule
- `Comparable` → custom sorting
- `Callable` → loose invocation

**4. Multiple Interfaces**
Apex doesn't allow multiple inheritance — interfaces compensate for that:
```
Class
 ├── Payable
 ├── Loggable
 └── Comparable
```

**5. Global Interface**
In a managed package, a `global interface` can be a stable package API while the implementation stays hidden.

**6. No Default Methods**
Apex interfaces have no default methods — keep interfaces focused. A large/fat interface can violate the Interface Segregation Principle.

### Errors & Gotchas
- Missing implementation → the class must implement the method
- Fat interface → too many unrelated methods
- Concrete dependency → depending directly on a concrete class reduces testability

### Limits
- A class can implement multiple interfaces
- Interfaces can't contain implementations or fields
- Interface methods are implicitly `public`
- You can't narrow an interface method's visibility
- Adding a method to a managed package's `global interface` can break existing implementers
- Interfaces enable `Test.createStub()` mocking design

### Best Practice
Focused interfaces + Dependency Injection. Implement platform interfaces for async/sorting/integration hooks.

**Why?** Decoupling + mocking + strategy dispatch + stable package API.

---

## 7. Polymorphism

One interface, many implementations.

### 🌱 Simple
Same method call, different classes can give different behavior.
```apex
List<Shape> shapes; // Circle, Square, Triangle
shape.area();
```
Calling `shape.area()` executes the actual object's implementation.

Apex has two major forms:

**Runtime Polymorphism** — using `virtual`, `abstract`, `override`, interfaces. The caller calls through a base type/interface; the actual subclass implementation executes at runtime. This is **dynamic dispatch**.

**Compile-Time Polymorphism** — method overloading:
```apex
calculate(x)
calculate(x, y)
```
The compiler selects the correct version.

### Key Patterns

**1. Dynamic Dispatch**
```apex
TriggerHandler handler = new AccountTriggerHandler();
handler.run();
```
Reference type: `TriggerHandler`. Actual object: `AccountTriggerHandler`. `run()` executes the concrete subclass implementation — the dispatcher doesn't need to know the concrete type.

**2. Strategy Pattern**
Store implementations in a map (`Map<Key, Interface>`) and choose a strategy at runtime by key. New behavior = new class + registration, instead of editing a giant switch.

**3. Replaces Conditionals**

**Bad:**
```
if email
else if sms
else if push
else if whatsapp
```

**Polymorphism:**
```
Notifier
  ↓
EmailNotifier / SMSNotifier / PushNotifier

notifier.send()
```

**4. Interface + Override**
Both can compose:
```
Interface      → Contract
Abstract class → Shared behavior
Subclass       → Specialized behavior
```

**5. Liskov Substitutability**
A subtype must honor the base contract. If a subclass breaks what the base guarantees, polymorphism becomes unsafe.

**6. Metadata-Driven Polymorphism**
Custom Metadata (key → class name) + `Type.forName()` + `newInstance()` at runtime enables configuration-driven dispatch — a highly extensible, architect-level pattern.

### Errors & Gotchas
- Base method not `virtual` → no dynamic override
- Repeatedly casting to a concrete type → defeats the benefit of polymorphism
- Subtype breaks the base contract → Liskov violation

### Limits
- Resolution happens at runtime
- The compiler can't warn about every unimplemented override path
- Casting to the wrong subtype → runtime `TypeException`
- `instanceof` returns `false` for `null`
- Virtual dispatch in large loops can add CPU overhead

### Best Practice
Interface/abstract-based polymorphism + Strategy dispatch, optionally metadata-driven, over giant type-based conditionals.

**Why?** Extensible + testable + less need for caller modification.

---

## 8. Encapsulation

Hiding internal state and exposing a controlled interface.

### 🌱 Simple
Keep data private and expose access through methods/properties.
```
Private data → Methods / Properties → Caller
```
Callers shouldn't manipulate internals directly.

### Advanced
Encapsulation is usually achieved through private fields + properties + methods. Benefits:

**Protect Invariants** — the object stays in a valid state, e.g. `balance >= 0`.

**Reduce Coupling** — the caller doesn't depend on the internal representation.

**Enable Refactoring** — you can change internal implementation without breaking callers.

**Opposite (bad):**
```apex
public Decimal balance;
```
Any caller can assign anything.

### Key Patterns

**1. Invariant Protection**
```
private balance → validate → update
```
The object never enters an invalid configuration.

**2. Information Hiding**
Caller depends on behavior:
```apex
account.getBalance()
```
not internal representation:
```apex
account.internalList[0]
```
The internal implementation (e.g., List → Map) can change without affecting consumers — extremely useful in managed packages and long-lived systems.

**3. Reduced Coupling / Blast Radius**
Fewer public members → fewer dependencies → smaller change impact.

**4. Asymmetric Properties**
```apex
get;
private set;
```
Expose read access, hide mutation.

**5. Encapsulate Collections**

**Bad:**
```apex
public List<Item> items;
// caller: items.clear();  → internal state corrupted
```

**Better:**
```
private List + addItem() + removeItem() + getItems() → returns a copy
```
Don't expose a live internal collection directly.

**6. `@TestVisible`**
Lets tests access internals without widening production visibility.

### Errors & Gotchas
- Public mutable fields → invalid/corrupt state
- Exposing live collections → external mutation
- Leaky abstraction → callers tightly coupled to internals

### Limits
- Access modifiers are **not** a security boundary — Apex generally runs in system mode
- CRUD/FLS/sharing must be enforced separately (`WITH USER_MODE`, `stripInaccessible`)
- `@TestVisible` deliberately weakens encapsulation for testing — use sparingly

### Best Practice
Private state + validating properties/methods + minimal public API. Return copies for collections.

**Why?** Invariant protection + less coupling + internal freedom.

---

## 9. Abstraction

Expose essential behavior, hide implementation details.

### 🌱 Simple
Abstraction focuses on **what**, not **how**.
```apex
PaymentGateway.pay()
```
The caller doesn't need to know how payment is processed internally.

Apex provides abstraction via `interface` and `abstract class`. Concrete classes implement the actual details.

### Advanced

**Interface** — a pure contract: *what should be done?*

**Abstract Class** — partial implementation + abstract methods: some common behavior + some mandatory implementation.

Callers work with the abstraction, not concrete details — enabling **Policy vs Mechanism** separation. Useful in layered architecture: Service, Selector, Domain.

### Key Patterns

**1. Interface vs Abstract Class**

| | Interface | Abstract Class |
|---|---|---|
| Nature | Pure contract | Shared behavior + enforced steps |
| Mental model | "can-do" | "is-a" |
| Multiple implementation | Yes | Single inheritance only |
| Example | `Comparable`, `Payable`, `Schedulable` | Useful for Template Method |

**2. Dependency Inversion**
High-level business policy shouldn't depend on a concrete implementation:
```
High-level policy → Abstraction → Concrete implementation
```
The concrete implementation is injected. Benefits: testability, swappability, loose coupling.

**3. Hide Volatility**
Hide things likely to change behind an abstraction: external API, query logic, payment provider, database implementation. If the external API changes, the whole system doesn't need to change.

**4. Right Level of Abstraction**
Don't create an interface for everything. If there's one implementation, no variation, and no expected volatility, unnecessary abstraction adds noise.

**5. Stable Contracts**
An abstraction can become an API contract — keep it small, clear, and intention-revealing.

**6. DI + Factories**
Combine Abstraction + Dependency Injection + Factory to choose an implementation at runtime.

Architects generally abstract only where variation or volatility actually exists.

### When to Use Which

**Abstract class** when:
- Implementations share common code
- There's a genuine is-a relationship
- Template Method pattern is useful
- Shared state/behavior is needed

**Interface** when:
- You need a pure capability contract
- You need multiple implementations
- You need to compose multiple capabilities
- No shared implementation is required

> Short answer: Abstract class = shared behavior + is-a hierarchy. Interface = pure contract + capability + multiple implementations.

### Errors & Gotchas
- **Over-Abstraction** — creating an interface for a single implementation adds unnecessary complexity
- **Leaky Abstraction** — exposing implementation details to the caller
- **Concrete Dependency** — high-level code directly depends on a concrete implementation, making swapping/testing difficult

### Limits
- An abstract class can't be instantiated — it must be extended
- An abstract class can hold state; an interface can't hold state/fields
- A class can extend only one abstract/base class, but implement multiple interfaces
- Abstract methods must be implemented in the concrete subclass

### Best Practice
Interfaces for capability contracts; abstract classes for shared is-a behavior; introduce abstraction only where actual variation/volatility exists.

**Why?** Change isolation + dependency inversion + flexibility, without unnecessary over-engineering.

---

## 10. OOP Concepts — Quick Mental Model

If confused during an interview or while problem-solving, use this model:

| Concept | Ask Yourself |
|---|---|
| **Property** | Need to expose data in a controlled way? |
| **Method** | Need reusable behavior/function? |
| **Constructor** | Need to set initial state/dependencies at object creation? |
| **Access Modifier** | Who should have access? |
| **Inheritance** | Is there a true "is-a" relationship + shared behavior? |
| **Interface** | Need a contract/capability + multiple implementations + mocking? |
| **Polymorphism** | Need same call, different implementations? Need to eliminate a large if/switch? |
| **Encapsulation** | Need to protect internal state? |
| **Abstraction** | Need to hide implementation details and expose "what"? |

---

## 11. How These Concepts Compose in a Real Project

```
INTERFACE
   ↓
defines contract
   ↓
   ┌───────┴────────┐
   ↓                ↓
Implementation A   Implementation B
   │                │
   └───────┬────────┘
           ↓
     POLYMORPHISM
           ↓
      Service Layer
           ↓
     ENCAPSULATION
           ↓
  private state + methods
           ↓
      PROPERTIES
           ↓
    controlled access
           ↓
      CONSTRUCTOR
           ↓
   Dependency Injection
```

Add inheritance/abstract classes only when there's a genuine **is-a + shared behavior** relationship.

---

## 12. One-Glance Summary Table

| Concept | Key Idea | Syntax/Example | Use When |
|---|---|---|---|
| **Property** | Field with built-in get/set | `public Integer x { get; set; }` | Need controlled/lazy/validated access |
| **Asymmetric Property** | Different get/set visibility | `get; private set;` | Externally read-only state |
| **Method** | Reusable named behavior | `void calculate() { }` | Any repeatable operation |
| **Static Method** | Class-level, stateless | `Utility.calculate();` | Utility/helper logic |
| **Instance Method** | Object-level, stateful | `service.calculate();` | Services/handlers, mockable via DI |
| **Constructor** | Initial object setup | `public Cart(Id ownerId){}` | Setting required state/dependencies |
| **Constructor Chaining** | Delegate to another constructor | `this(...)` | Avoiding duplicate init logic |
| **private** | Same class only | `private Integer x;` | Default visibility |
| **protected** | Class + subclasses | `protected void hook(){}` | Extension points for subclasses |
| **public** | Same namespace | `public class Foo{}` | Shared within your own app |
| **global** | Cross-namespace | `global class Api{}` | Managed package API contracts |
| **Inheritance** | Reuse + specialize (`extends`) | `class A extends B {}` | True is-a relationship |
| **virtual/abstract** | Enables overriding | `virtual void run(){}` | Base class needs overridable methods |
| **super** | Call parent implementation | `super.method();` | Extending, not replacing, parent logic |
| **Interface** | Pure contract (`implements`) | `interface Payable { void pay(); }` | Capability contract, multiple implementations |
| **Polymorphism** | Same call, different behavior | `shape.area();` | Replacing large if/switch chains |
| **Dynamic Dispatch** | Runtime method resolution | `TriggerHandler h = new AccountHandler();` | Base-type calls resolving to subclass logic |
| **Strategy Pattern** | Map-based runtime dispatch | `Map<String, ProcessorIntf>` | Extensible behavior without editing callers |
| **Encapsulation** | Hide state, expose controlled access | `private Decimal balance;` | Protecting invariants, reducing coupling |
| **Abstraction** | Expose "what", hide "how" | `abstract class Base { abstract void step(); }` | Hiding volatility, dependency inversion |
| **Template Method** | Abstract skeleton + subclass steps | `abstract class TriggerHandler {}` | Shared workflow, varying steps |
| **@TestVisible** | Expose internals to tests only | `@TestVisible private Integer x;` | Testing without widening production access |

---

## 13. Problem Solve Karte Time Kaise Identify Karein?

**Problem 1:** "External code meri value change kar raha hai."
→ Think: **Encapsulation + Property + `private set`**

**Problem 2:** "Same logic multiple jagah repeat ho raha hai."
→ Think: **Method**

**Problem 3:** "Object ko create karte time mandatory dependencies chahiye."
→ Think: **Constructor + Dependency Injection**

**Problem 4:** "Different classes ko same contract follow karwana hai."
→ Think: **Interface**

**Problem 5:** "Email/SMS/Push ke liye same operation hai but implementation different."
→ Think: **Interface + Polymorphism + Strategy Pattern**

**Problem 6:** "Bahut bada if/else/switch hai jo type ke according behavior choose karta hai."
→ Think: **Polymorphism + Strategy Pattern**

**Problem 7:** "Common framework behavior share karna hai aur subclasses specific steps implement karein."
→ Think: **Abstract Class + Inheritance + Template Method**

**Problem 8:** "Database provider/API future mein change ho sakta hai."
→ Think: **Abstraction + Interface + Dependency Injection**

**Problem 9:** "Internal List external caller modify kar raha hai."
→ Think: **Encapsulation + private List + return copy**

---

## 14. Golden Rules

1. Private by default.
2. Public surface minimum rakho.
3. Properties use karo when controlled access needed.
4. Getters mein heavy SOQL/DML avoid karo.
5. Methods small + single-purpose rakho.
6. Methods ko bulk-safe banao.
7. Constructors lightweight rakho.
8. Dependencies constructor mein inject karo.
9. Interface = contract.
10. Abstract class = shared behavior + enforced structure.
11. Inheritance only for genuine "is-a".
12. Composition often better than deep inheritance.
13. Polymorphism se giant switch/if-else replace karo.
14. Internal state expose mat karo.
15. Collections ki live references expose karne se bacho.
16. Abstraction sirf actual variation/volatility par introduce karo.
17. `global` ko carefully use karo — especially managed packages mein.
18. CRUD/FLS/sharing ko access modifiers ke saath confuse mat karo.

---

## 15. Final Summary

| Concept | Main Purpose | Yaad Rakho |
|---|---|---|
| Property | Controlled data access | `get`/`set` |
| Method | Reusable behavior | One job |
| Constructor | Object initialization | `new` |
| Private | Hide implementation | Default |
| Protected | Subclass access | Extension |
| Public | Same namespace access | Internal API |
| Global | Cross-namespace API | Permanent contract |
| Inheritance | Reuse + specialization | is-a |
| Interface | Contract | `implements` |
| Polymorphism | One call, many behaviors | Dynamic dispatch |
| Encapsulation | Protect state | Hide internals |
| Abstraction | Hide implementation | Focus on what |

---

## 16. Kaun Concept Kaunsa Problem Solve Karta Hai — Salesforce Mein Helpful Kaise?

| Concept | Kya Problem Solve Karta Hai | Salesforce Mein Kaise Helpful Hai |
|---|---|---|
| **Property** | Field ko directly expose karne se data uncontrolled ho jaata hai | Visualforce/LWC binding mein controlled, lazy aur validated access deta hai; `private set` se accidental external overwrite rokta hai |
| **Method** | Same logic baar-baar likhna padta hai, maintenance mushkil hoti hai | Trigger handlers, service classes, batch classes mein reusable, testable, bulk-safe logic banata hai |
| **Constructor** | Object incomplete/invalid state mein create ho sakta hai | Service/Selector classes ko required dependencies ke saath hi initialize karta hai — DI aur unit testing (mocks) easy ho jaati hai |
| **Access Modifiers** | Sab kuch public hone se unintended coupling aur breakage ka risk | Managed packages mein `private`/`protected`/`public`/`global` se API surface control hoti hai; galat exposure se subscriber orgs break hone se bachte hain |
| **Inheritance** | Har object ke liye trigger handler/logic se code duplicate hota hai | Common `TriggerHandler` base class se saare SObject triggers same framework follow karte hain — consistency + less duplication |
| **Interface** | Different implementations ko ek jaisa treat nahi kar sakte | `Batchable`, `Queueable`, `Schedulable`, `Comparable` jaise platform interfaces implement karke Salesforce ke async/sorting engine mein directly plug hote ho |
| **Polymorphism** | Giant if/else ya switch se naya type add karna risky hota hai | Notification (Email/SMS/Push), Payment gateways, ya multiple record-type-specific logic ko ek hi call se handle karta hai, bina existing code chhede naya type add ho jaata hai |
| **Encapsulation** | External code internal state corrupt kar sakta hai | Selector/Service layer mein internal List/Map ko protect karta hai, taaki controller ya trigger accidentally data corrupt na kare |
| **Abstraction** | Concrete implementation (jaise ek specific callout/API) par pura system depend ho jaata hai | Integration layer (Payment Gateway, External API) ko interface ke peeche chhupa ke rakhte ho — provider change hone par sirf ek class replace karni padti hai, poora system nahi |

### Ek Line Mein Yaad Rakho
- **Property** → Controlled access ka problem solve karta hai
- **Method** → Duplication ka problem solve karta hai
- **Constructor** → Invalid object state ka problem solve karta hai
- **Access Modifiers** → Unwanted exposure ka problem solve karta hai
- **Inheritance** → Framework duplication ka problem solve karta hai
- **Interface** → Inconsistent contract ka problem solve karta hai
- **Polymorphism** → Giant conditional logic ka problem solve karta hai
- **Encapsulation** → State corruption ka problem solve karta hai
- **Abstraction** → Tight coupling/volatility ka problem solve karta hai

---

## 17. Confusing Pairs — Difference Clear Karo

### Interface vs Abstract Class
| Interface | Abstract Class |
|---|---|
| Pure contract | Partial implementation |
| Multiple interfaces implement ho sakti hain | Sirf ek base class extend ho sakti hai |
| Capability define karta hai (can-do) | is-a relationship define karta hai |
| No shared implementation | Shared state/behavior possible hai |

> **Yaad rakho:** Interface = "kya karna hai" ka contract. Abstract Class = "kuch already kiya hua hai, baaki tum karo".

### Encapsulation vs Abstraction
| Encapsulation | Abstraction |
|---|---|
| "How do I protect my internal state?" | "How do I hide implementation complexity?" |
| Data ko private rakh ke controlled access deta hai | Implementation details ko interface/abstract class ke peeche chhupata hai |
| Focus: **state protection** | Focus: **complexity hiding** |

> **Yaad rakho:** Encapsulation state ke around wall banata hai. Abstraction "how it works" ko chhupa ke sirf "what it does" dikhata hai.

### Inheritance vs Composition
| Inheritance | Composition |
|---|---|
| **IS-A** relationship | **HAS-A** / **USES-A** relationship |
| `SavingsAccount extends BankAccount` | `Class A → uses/injects Class B` |
| Tight coupling, deep hierarchy risk | Flexible, loosely coupled |

> **Yaad rakho:** Genuine is-a ho tabhi inheritance karo, warna composition safer aur flexible hoti hai.

### Property vs Field
| Field | Property |
|---|---|
| Direct state — `public Integer x;` | Controlled state access — `get`/`set` |
| Validation possible nahi | Validation add kar sakte ho |
| Computed value possible nahi | Computed values possible hain |
| Lazy loading possible nahi | Lazy loading possible hai |

> **Yaad rakho:** Field = raw data. Property = data + logic (validation, computation, lazy loading) ek saath.

### Polymorphism vs Overloading
| Polymorphism / Override | Overloading |
|---|---|
| **Runtime** par resolve hota hai | **Compile-time** par resolve hota hai |
| Actual object (subclass) behavior decide karta hai | Parameters (type/count) method decide karte hain |
| Same method signature, different class implementation | Same method name, different parameter signatures |

> **Yaad rakho:** Override = same shape, different body, runtime decides. Overload = different shape, compiler decides.

---

## 18. Examples — Confusing Pairs Code Ke Saath

### Interface vs Abstract Class
```apex
// Interface — pure contract
interface Payable {
    void pay();
}

// Abstract Class — shared behavior + enforced steps
abstract class BaseHandler {
    public void run() {
        beforeProcess();
        process();
    }
    protected void beforeProcess() {
        System.debug('Common logging...');
    }
    abstract void process(); // subclass isko implement karega
}
```

### Encapsulation vs Abstraction
```apex
// Encapsulation — internal state protect karna
public class BankAccount {
    private Decimal balance; // directly access nahi ho sakta

    public Decimal getBalance() {
        return balance;
    }
    public void deposit(Decimal amt) {
        if (amt > 0) balance += amt; // validation ke saath controlled update
    }
}

// Abstraction — "how" hide, sirf "what" expose
public interface PaymentGateway {
    void pay(Decimal amount); // caller ko pata nahi kaise process hota hai
}
```

### Inheritance vs Composition
```apex
// Inheritance — IS-A
class SavingsAccount extends BankAccount {
}

// Composition — HAS-A / USES-A
class AccountService {
    private IAccountSelector selector; // BankAccount "use" karta hai, "is" nahi

    public AccountService(IAccountSelector selector) {
        this.selector = selector;
    }
}
```

### Property vs Field
```apex
// Field — direct state
public Integer count;

// Property — controlled state access
public Integer safeCount {
    get { return safeCount; }
    set {
        if (value >= 0) safeCount = value; // validation
    }
}
```

### Polymorphism vs Overloading
```apex
// Polymorphism / Override — runtime decides
abstract class Notifier {
    abstract void send();
}
class EmailNotifier extends Notifier {
    override void send() { System.debug('Email sent'); }
}
class SMSNotifier extends Notifier {
    override void send() { System.debug('SMS sent'); }
}
Notifier n = new EmailNotifier();
n.send(); // "Email sent" — actual object type decide karta hai

// Overloading — compile-time decides
void calculate(Integer x) { }
void calculate(Integer x, Integer y) { }
calculate(5);      // pehla method call hoga
calculate(5, 10);  // doosra method call hoga
```
