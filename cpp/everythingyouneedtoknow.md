# Everything You Need to Know – Object-Oriented Programming with C++

## Module I – Introduction

### 1. Evolution of Programming Paradigms

```text
Monolithic Programming (Assembly / BASIC)
       │
       ▼
Procedural Programming (FORTRAN / COBOL)
       │
       ▼
Structured Programming (C / Pascal)
       │
       ▼
Object-Oriented Programming (C++ / Java / Smalltalk)
```

* **Monolithic Paradigm:** The program consists of a single linear block of code with global data. It relies on unconditional jumps (`goto`), which creates an unstructured flow that becomes difficult to maintain as code size grows.
* **Procedural Paradigm:** Code is modularized into reusable subroutines and functions. It concentrates on the sequence of operations (algorithms) rather than the data being manipulated.
* **Structured Paradigm:** Enforces structured control flow using subroutines, conditionals (`if-else`), and loops (`for`, `while`), eliminating unstructured branching (`goto`). Data and logic remain separate.
* **Object-Oriented Paradigm:** Integrates data and functions into cohesive units called **objects**. It prioritizes data security, code reusability, and direct real-world problem modeling.

### 2. Difference between C and C++

| Feature | C | C++ |
| :--- | :--- | :--- |
| **Programming Paradigm** | Procedural | Multi-paradigm (Procedural + Object-Oriented) |
| **Focus** | Functions and procedures | Objects and data |
| **Data Hiding** | Not supported | Supported via classes (Encapsulation) |
| **Inheritance** | Not available | Fully supported |
| **Polymorphism** | Limited (macros/emulation) | Full compile-time and runtime polymorphism |
| **Memory Management** | `malloc()` / `free()` | `new` / `delete` + RAII |
| **Standard Library** | Standard C Library | Standard Template Library (STL) + C Library |
| **Input/Output** | `printf` / `scanf` | `cin` / `cout` (streams) + `printf` / `scanf` |
| **Default Arguments** | Not supported | Supported |
| **Reference Variables** | Not available | Available |
| **Exception Handling** | Not available | Native `try` / `catch` / `throw` |
| **Templates** | Not available | Fully supported |

**Key Takeaway**: C++ is a superset of C with powerful object-oriented features, better type safety, and a richer standard library.

### 3. Procedure-Oriented vs Object-Oriented Approach

**Procedure-Oriented Programming (POP)**
* Focuses on **functions** (procedures).
* Data is global or passed around freely.
* Follows top-down design.
* Example languages: C, Pascal, FORTRAN.

**Object-Oriented Programming (OOP)**
* Focuses on **objects** that contain both data and methods.
* Data is hidden and protected from unauthorized access.
* Follows bottom-up design.
* Encourages code reuse through inheritance and polymorphism.

| Aspect | Procedure-Oriented | Object-Oriented |
| :--- | :--- | :--- |
| **Basic Unit** | Function | Object / Class |
| **Approach** | Top-down | Bottom-up |
| **Data Security** | Low (data moves freely) | High (Encapsulation & Access Specifiers) |
| **Code Reusability** | Limited | High (Inheritance, Polymorphism) |
| **Real-world Mapping** | Poor | Excellent |

### 4. Object-Based vs. Object-Oriented Languages

| Criterion | Object-Based Language | Object-Oriented Language |
| :--- | :--- | :--- |
| **Encapsulation & Abstraction** | Supported | Supported |
| **Inheritance** | **Not Supported** | Fully Supported |
| **Dynamic Binding (Runtime Polymorphism)** | **Not Supported** | Fully Supported |
| **Examples** | Ada (early versions), JavaScript (pre-ES6), Visual Basic (classic) | C++, Java, C#, Smalltalk, Python |

> **Key Distinction:** An object-based language supports data abstraction and encapsulation via objects, but lacks native support for **inheritance** and **runtime dynamic polymorphism**.

### 5. Basic Concepts of OOP

#### Objects

An **object** is an instance of a class and a real-world entity with:
* **State** (attributes / data members)
* **Behaviour** (methods / member functions)
* **Identity** (unique memory address)

```cpp
class Car {
public:
    std::string brand;
    int speed;

    void accelerate() {
        speed += 10;
    }
};

int main() {
    Car myCar;               // Object created
    myCar.brand = "Toyota";
    myCar.speed = 0;
    myCar.accelerate();
}
```

#### Classes

A **class** is a user-defined blueprint or template from which individual objects are instantiated. It defines the attributes and methods shared by all objects of that type.

```cpp
class Student {
    // Attributes (private by default)
    std::string name;
    int rollNo;

public:
    // Methods (member functions)
    void setData(std::string n, int r) {
        name = n;
        rollNo = r;
    }

    void display() {
        std::cout << "Name: " << name << ", Roll: " << rollNo << std::endl;
    }
};
```

#### Principles of OOP

**1. Abstraction**

Hiding complex underlying implementation details and exposing only the essential interface to the user.

```cpp
class BankAccount {
private:
    double balance;                 // Implementation detail hidden from outside

public:
    void deposit(double amount);    // Only public interface is visible
    void withdraw(double amount);
    double getBalance();
};
```

**2. Encapsulation**

Binding together the data and the functions that manipulate that data into a single package (class), while restricting direct access to internal components.

```cpp
class Rectangle {
private:
    int length, breadth;            // Data is private and shielded

public:
    void setDimensions(int l, int b) {
        if (l > 0 && b > 0) {
            length = l;
            breadth = b;
        }
    }

    int area() {
        return length * breadth;
    }
};
```

**3. Inheritance**

The mechanism where a derived (child) class acquires the state and behavior of an existing base (parent) class, promoting code reuse.

```cpp
class Animal {
public:
    void eat() { std::cout << "Eating..." << std::endl; }
};

class Dog : public Animal {         // Dog inherits from Animal
public:
    void bark() { std::cout << "Barking..." << std::endl; }
};
```

**4. Polymorphism**

"One name, many forms." The ability of a message, function call, or operator to execute differently based on the object invoking it.
* **Compile-time**: Function overloading, Operator overloading
* **Runtime**: Virtual functions

**5. Dynamic Binding**

The code associated with a given procedure call is not determined until runtime (late binding), implemented using virtual functions and pointers/references.

**6. Message Passing**

The communication mechanism between objects in a running system: an object invokes a method on another object to request information or actions.

```cpp
obj1.processData(obj2); // obj1 sends a message to obj2
```

### 6. Characteristics of Object-Oriented Languages

1. **Everything is an object** (or can be modeled as one)
2. **Classes** act as blueprints for instances
3. **Encapsulation** guarantees data protection
4. **Inheritance** delivers clean code reuse
5. **Polymorphism** supports flexible program behaviors
6. **Dynamic binding** resolves operations at runtime
7. **Message passing** facilitates object communication
8. **Extensibility** allows modification without altering tested legacy classes
9. **Modularity** allows independent component development and testing

### 7. Benefits and Real-World Applications of OOP

#### Key Benefits

* **Software Reusability:** Derived classes inherit tested components, eliminating duplicate code.
* **Maintainability & Modularity:** Changes to internal class implementations do not impact external client code that interacts via stable public interfaces.
* **Data Security:** Private and protected access specifiers prevent unintentional state corruption.
* **Scalability:** Bottom-up design enables systems to grow from independent, validated components.
* **Reduced Development Time:** Class libraries and component frameworks accelerate development workflows.

#### Real-World Applications

* **GUI Frameworks:** Windows, buttons, dialogs, and event-handling pipelines map directly to inheritance trees.
* **Simulations & Games:** Real-time physics engines and game entities model active state, behavior, and collision boundaries using object hierarchies.
* **Object-Oriented Databases (OODBMS):** Store and retrieve complex native domain objects directly without object-relational mapping overhead.
* **CAD/CAM Software:** Geometric shapes, engineering assemblies, and spatial transforms model naturally through composition and inheritance.
* **Compilers & Operating Systems:** Abstract Syntax Trees (ASTs), file system drivers, and protocol stacks use polymorphic interfaces.

---

## Module II – Classes & Objects

### 1. Abstract Data Types (ADT)

An ADT is a theoretical model for data structures where the type is defined entirely by its supported operations and behavior, rather than its concrete storage implementation. In C++, classes serve as the vehicle for implementing ADTs.

*Example:* A `Stack` ADT defines `push()`, `pop()`, and `isEmpty()` without exposing whether the underlying storage uses an array or a linked list.

### 2. Objects, Classes, Attributes, and Methods

* **Attributes:** Variables declared within a class representing its state (data members).
* **Methods:** Functions declared within a class representing its behavior (member functions).

```cpp
class Circle {
private:
    double radius;                                      // Attribute

public:
    void setRadius(double r) { radius = r; }             // Method
    double area() { return 3.14159 * radius * radius; }  // Method
};
```

### 3. Class vs. Structure in C++

```cpp
struct StructDemo {
    int x; // public by default
};

class ClassDemo {
    int x; // private by default
};
```

* **Default Member Access:** Members of a `class` are `private` by default; members of a `struct` are `public` by default.
* **Default Inheritance Access:** Deriving from a `class` defaults to `private` inheritance; deriving from a `struct` defaults to `public` inheritance.

### 4. C++ Class Declaration Syntax

```cpp
class ClassName {
private:
    // private members: accessible only inside this class

protected:
    // protected members: accessible inside this class and derived classes

public:
    // public members: accessible anywhere

    ClassName();  // Constructor
    ~ClassName(); // Destructor
};
```

### 5. Local Class vs. Global Class

* **Global Class:** Declared outside of all functions; accessible throughout the file or translation unit.
* **Local Class:** Declared inside a function block.
  * Cannot contain `static` data members.
  * All member functions must be defined inline within the class body.
  * Can access only local `static` variables or global variables, not regular automatic local variables from the enclosing function.

```cpp
void fun() {
    class Local {
    public:
        void display() { std::cout << "Local class" << std::endl; }
    };

    Local obj;
    obj.display();
}
```

### 6. State, Identity, and Behaviour of an Object

* **State:** The current values assigned to an object's data members.
* **Identity:** The unique memory address occupied by the object, distinguishing it from all other instances.
* **Behaviour:** The operations an object can perform, defined by its member functions.

### 7. Local Object vs. Global Object

* **Global Object:** Instantiated outside any function; constructed before `main()` executes and destroyed when the program terminates.
* **Local Object:** Instantiated on the stack inside a function or block; constructed when execution hits the definition and destroyed when leaving the scope.

```cpp
class Demo {
public:
    Demo() { std::cout << "Constructor called" << std::endl; }
    ~Demo() { std::cout << "Destructor called" << std::endl; }
};

Demo globalObj; // Global object (lives for program duration)

int main() {
    Demo localObj; // Local object (lives for main scope)
    return 0;
}
```

### 8. Memory Layout of an Object in C++

```text
+------------------------------------+
| _vptr (if virtual functions exist) | 8 bytes (on 64-bit architecture)
+------------------------------------+
| Non-static Data Member 1           |
+------------------------------------+
| Non-static Data Member 2           |
+------------------------------------+
| Padding Bytes (Memory Alignment)   |
+------------------------------------+
```

* **What Consumes Object Memory:** Only **non-static data members** and the virtual table pointer (`_vptr`, if the class contains virtual functions) occupy memory inside an instantiated object.
* **What Does NOT Consume Object Memory:** Member functions, static variables (which reside in the static data segment), and type declarations do not add to an object's instance size.
* **Size of an Empty Class:** In C++, `sizeof(EmptyClass)` evaluates to **1 byte** to ensure every distinct object instance possesses a unique memory address.

### 9. Scope Resolution Operator (`::`)

Used to:
1. Access global variables hidden by local variables.
2. Define member functions outside the class declaration.
3. Access static members of a class.
4. Resolve naming collisions in multiple inheritance.

```cpp
int x = 10; // Global variable

class Test {
    int x;
public:
    void setX(int x);
    void show() {
        std::cout << "Local x = " << x << std::endl;
        std::cout << "Global x = " << ::x << std::endl; // Accesses global x
    }
};

// Defining member function outside the class
void Test::setX(int val) {
    x = val;
}
```

### 10. Friend Functions and Friend Classes

A **friend function** is a non-member function granted explicit access to `private` and `protected` members of a class.

```cpp
class Box {
private:
    int length;
public:
    Box(int l) : length(l) {}
    friend void printLength(Box b);     // Friend function declaration
};

void printLength(Box b) {               // Non-member definition
    std::cout << "Length = " << b.length << std::endl; // Direct access to private member
}
```

A **friend class** grants all of its member functions access to the private and protected internals of the granting class:

```cpp
class Server {
private:
    int secretKey = 9921;
    friend class Admin; // Entire class Admin is granted access
};

class Admin {
public:
    void inspect(const Server& s) {
        std::cout << "Key: " << s.secretKey << std::endl;
    }
};
```

#### Properties of Friendship

* **Non-Mutual (Asymmetric):** If `A` declares `B` as a friend, `B` can access `A`'s internals, but `A` cannot access `B`'s private members.
* **Non-Transitive:** If `A` friends `B`, and `B` friends `C`, `C` does not automatically gain access to `A`.
* **Not Inherited:** Derived classes do not inherit friend status from their base classes.

### 11. Inline Functions

Instructs the compiler to substitute the function call site directly with the compiled function body to eliminate function call overhead (stack frame setup, parameter passing).

```cpp
class Sample {
public:
    void display() { // Implicitly inline (defined inside class body)
        std::cout << "Hello" << std::endl;
    }
};

// Explicit inline definition
inline int square(int x) {
    return x * x;
}
```

### 12. Constructors and Destructors

* **Constructor:** Special member function named after the class, invoked automatically upon object creation to initialize members. Has no return type.
* **Destructor:** Member function named after the class preceded by `~`, invoked automatically when an object leaves its scope to release allocated resources. Takes no parameters and returns nothing.

```cpp
class Student {
    std::string name;
    int* marks;
public:
    Student(std::string n, int m) {
        name = n;
        marks = new int(m);
        std::cout << "Constructor called" << std::endl;
    }

    ~Student() {
        delete marks;
        std::cout << "Destructor called" << std::endl;
    }
};
```

### 13. Member Initialization Lists

Initializes member variables *before* the constructor body begins execution.

```cpp
class Account {
    const int accountId;       // const member
    int& transactionRef;       // reference member
    std::string owner;

public:
    Account(int id, int& ref, std::string name)
        : accountId(id), transactionRef(ref), owner(name) {} // Initialization list
};
```

#### Scenarios Where Initialization Lists are Mandatory

1. **`const` Data Members:** Cannot be assigned values inside the constructor body; must be initialized on creation.
2. **Reference (`&`) Data Members:** References cannot exist in an uninitialized state and cannot be rebound.
3. **Base Classes Lacking Default Constructors:** The derived constructor must pass arguments directly to the parameterized base constructor.
4. **Member Objects Lacking Default Constructors:** Contained class objects that only provide parameterized constructors must be constructed via the list.
5. **Performance Optimization:** Bypasses default object construction followed by assignment.

### 14. Types of Constructors

1. **Default Constructor:** Takes no arguments (or all arguments have defaults).
2. **Parameterized Constructor:** Accepts explicit arguments to initialize state.
3. **Copy Constructor:** Initializes an object from another existing object of the same class.
4. **Dynamic Constructor:** Allocates heap memory at runtime within the constructor body using `new`.

```cpp
class Point {
    int x, y;
public:
    Point() : x(0), y(0) {}                          // Default constructor
    Point(int a, int b) : x(a), y(b) {}              // Parameterized constructor
    Point(const Point& p) : x(p.x), y(p.y) {}        // Copy constructor
};
```

> **Important:** If you define any custom constructor, the compiler **no longer** supplies an implicit default constructor.

### 15. Shallow Copy vs. Deep Copy

```text
Shallow Copy:
Object A [ ptr ] ───┐
                    ▼
Object B [ ptr ] ───► [ Dynamic Heap Memory ] (Double-free crash on destruction!)

Deep Copy:
Object A [ ptr ] ───► [ Dynamic Block A ]
Object B [ ptr ] ───► [ Dynamic Block B ] (Independent heap allocations)
```

#### Shallow Copy

* Performs a bitwise/member-wise copy of primitive values and pointers.
* If an object owns a pointer to dynamically allocated heap memory, only the memory address is copied. Both objects point to the same memory block.
* **Consequence:** When both objects go out of scope, their destructors attempt to free the same heap address twice, causing a **double-free runtime crash**.

#### Deep Copy

* Allocates a distinct block of heap memory for the destination object and copies the actual underlying data values.
* Requires defining a custom **Copy Constructor** and overloading the **Assignment Operator (`operator=`)**.

### 16. Static Class Data and Static Member Functions

* **Static Data Members:** Shared by all instances of a class; only a single copy exists in the program's static data segment. Must be defined outside the class declaration.
* **Static Member Functions:** Can be called without instantiating an object (`ClassName::function()`). They have no implicit `this` pointer and can access **only** static data members or other static member functions.

```cpp
class Counter {
public:
    static int count; // Declaration

    Counter() { count++; } // Increments shared count

    static int getCount() { // Static member function
        return count;
    }
};

int Counter::count = 0; // Definition and memory allocation (mandatory)

int main() {
    Counter c1, c2, c3;
    std::cout << Counter::getCount() << std::endl; // Output: 3
}
```

### 17. Array of Objects

```cpp
class Student {
    int roll;
public:
    void setRoll(int r) { roll = r; }
    void show() { std::cout << roll << " "; }
};

int main() {
    Student s[5]; // Allocates 5 Student objects sequentially
    for (int i = 0; i < 5; i++)
        s[i].setRoll(i + 1);

    for (int i = 0; i < 5; i++)
        s[i].show();
}
```

### 18. Constant Member Functions and Objects

* **Constant Member Function:** Guaranteed not to modify any non-static data members of the calling object.
* **Constant Object:** Cannot have its data members altered post-initialization and can **only** invoke `const` member functions.

```cpp
class Demo {
    int x;
public:
    Demo(int a) : x(a) {}
    void show() const { // const member function
        std::cout << x << std::endl;
        // x = 10;      // Compile Error: cannot modify member
    }
};

int main() {
    const Demo d(5);
    d.show();           // Allowed
}
```

### 19. Memory Management Operators

| Operator | Purpose |
| :--- | :--- |
| `new` | Dynamically allocates typed memory from the heap and calls the constructor |
| `delete` | Invokes the destructor and frees single-object memory allocated by `new` |
| `new[]` | Dynamically allocates an array of objects on the heap |
| `delete[]` | Invokes destructors for all elements and frees array memory |

```cpp
int* p = new int(10);        // Single integer allocation
int* arr = new int[5];       // Array allocation of 5 integers

delete p;                    // Free single integer
delete[] arr;                // Free array (must use delete[])
```

---

## Module III – Inheritance

### 1. Inheritance Concept & Syntax

The mechanism that allows a derived class to inherit members from a base class, establishing an "is-a" relationship.

```cpp
class DerivedClass : access-specifier BaseClass {
    // Derived members
};
```

### 2. Types of Inheritance

1. **Single Inheritance:** One base class $\to$ One derived class.
2. **Multiple Inheritance:** Multiple base classes $\to$ One derived class.
3. **Multilevel Inheritance:** Base class $\to$ Intermediate class $\to$ Derived class.
4. **Hierarchical Inheritance:** One base class $\to$ Multiple derived classes.
5. **Hybrid Inheritance:** Combination of multiple inheritance types in a single class diagram.

```cpp
// Single
class A {};
class B : public A {};

// Multiple
class C : public A, public B {};

// Multilevel
class D : public B {};

// Hierarchical
class E : public A {};
class F : public A {};
```

### 3. Access Modes – public, protected, and private

| Base Member Visibility | `public` Inheritance | `protected` Inheritance | `private` Inheritance |
| :--- | :--- | :--- | :--- |
| **`public`** | `public` in derived | `protected` in derived | `private` in derived |
| **`protected`** | `protected` in derived | `protected` in derived | `private` in derived |
| **`private`** | **Inaccessible** directly | **Inaccessible** directly | **Inaccessible** directly |

### 4. Construction and Destruction Order in Inheritance

```cpp
class A { public: A() { std::cout << "A "; } ~A() { std::cout << "~A "; } };
class B : public A { public: B() { std::cout << "B "; } ~B() { std::cout << "~B "; } };
class C : public B { public: C() { std::cout << "C "; } ~C() { std::cout << "~C "; } };

int main() {
    C obj; // Output: Construction: A B C | Destruction: ~C ~B ~A
}
```

* **Construction Order:**
  1. Virtual base classes (if any).
  2. Non-virtual base classes in the order listed in the inheritance declaration.
  3. Contained member objects in order of declaration inside the class.
  4. Derived constructor body runs last.
* **Destruction Order:** Executes in the exact reverse order of construction.

### 5. Ambiguity Resolution & The Diamond Problem

#### Multiple Inheritance Ambiguity

When two base classes define a function with the same signature, invoking it on a derived instance results in a compile-time ambiguity.

```cpp
class A { public: void show() { std::cout << "A"; } };
class B { public: void show() { std::cout << "B"; } };
class C : public A, public B {};

int main() {
    C obj;
    // obj.show();       // Compile Error: Ambiguous
    obj.A::show();       // Resolved using scope resolution operator
    obj.B::show();       // Resolved
}
```

#### The Diamond Problem and Virtual Base Classes

```text
        ┌───────────┐
        │  Device   │ (int id)
        └─────┬─────┘
       ┌──────┴──────┐
       ▼             ▼
  ┌─────────┐   ┌─────────┐
  │ Printer │   │ Scanner │
  └────┬────┘   └────┬────┘
       └──────┬──────┘
              ▼
         ┌─────────┐
         │ Copier  │ (Contains duplicate copies of 'Device' without virtual base!)
         └─────────┘
```

When classes inherit through multiple intermediate parents sharing a common root ancestor, the derived child receives duplicate sub-objects of that root class.

```cpp
class Device { public: int id; };
class Printer : virtual public Device {}; // Virtual base class
class Scanner : virtual public Device {}; // Virtual base class
class Copier  : public Printer, public Scanner {}; // Only one shared instance of Device
```

* **Underlying Mechanics:** The compiler inserts a **Virtual Base Pointer (`vbptr`)** into derived objects referencing a **Virtual Base Table (`vbtable`)**, which stores byte offsets to locate the single shared base instance in memory.

### 6. Object Slicing

Object slicing occurs when a derived class instance is assigned **by value** to a base class object variable.

```cpp
class Base { public: int b = 1; };
class Derived : public Base { public: int d = 2; };

Derived der;
Base base = der; // Object Slicing Occurs!
```

```text
[ Derived Object Memory ]            [ Base Object Memory ]
+-----------------------+            +--------------------+
| Base members (b = 1)  | ──Copy───► | Base members (b=1) |
+-----------------------+            +--------------------+
| Derived members (d=2) | (Sliced off and discarded!)
+-----------------------+
```

* The base class copy constructor only copies base members.
* Derived-specific data members and polymorphic virtual table assignments are sliced away.
* **Remedy:** Pass objects polymorphically using **pointers** (`Base*`) or **references** (`Base&`).

### 7. Overriding Base Class Methods

A derived class provides a new implementation for a function already defined in its base class.

```cpp
class Base {
public:
    void show() { std::cout << "Base class" << std::endl; }
};

class Derived : public Base {
public:
    void show() { std::cout << "Derived class" << std::endl; } // Method Override
};
```

### 8. Constructors in Derived Classes

```cpp
class Base {
public:
    Base(int x) { std::cout << "Base: " << x << std::endl; }
};

class Derived : public Base {
public:
    Derived(int x, int y) : Base(x) { // Explicitly invokes Base parameterized constructor
        std::cout << "Derived: " << y << std::endl;
    }
};
```

### 9. Abstract Classes and Pure Virtual Functions

An **abstract class** is designed to serve strictly as an interface and cannot be directly instantiated. A class is abstract if it contains at least one **pure virtual function** (`virtual void func() = 0;`).

```cpp
class Shape {
public:
    virtual void draw() = 0; // Pure virtual function makes Shape abstract
};

class Circle : public Shape {
public:
    void draw() override {
        std::cout << "Drawing Circle" << std::endl;
    }
};
```

### 10. Aggregation, Composition, and UML Class Hierarchies

```text
Dependency ───► Association ───► Aggregation ───► Composition ───► Generalization
(Weakest)                                                        (Strongest)
```

* **Generalization (Inheritance):** An "is-a" relationship (e.g., `Dog` *is-an* `Animal`).
* **Composition:** Strong "has-a" relationship where the child component's lifecycle is bound to the parent (e.g., a `Car` *owns an* `Engine`; if the car is destroyed, the engine is destroyed).
* **Aggregation:** Weak "has-a" relationship where the child component can exist independently of the container (e.g., a `Department` *has a* `Professor`).
* **Association:** Peer "uses-a" relationship between independent objects (e.g., a `Doctor` *attends to a* `Patient`).
* **Dependency:** A transient usage relationship, such as an object passed as a temporary parameter to a single function.

```cpp
// Composition example
class Engine {
public:
    void start() { std::cout << "Engine running"; }
};

class Car {
    Engine eng; // Composition: Car owns Engine instance directly
public:
    void drive() { eng.start(); }
};
```

### 11. Nesting of Classes

Declaring a class inside the scope of another enclosing class.

```cpp
class Outer {
public:
    class Inner {
    public:
        void display() { std::cout << "Inner class method" << std::endl; }
    };

    void show() {
        Inner obj;
        obj.display();
    }
};
```

---

## Module IV – Polymorphism

### 1. Polymorphism Concept & Classification

Polymorphism allows an interface to execute different underlying logic depending on the object it is applied to.

### 2. Compile-Time vs. Runtime Polymorphism

| Attribute | Compile-Time Polymorphism | Runtime Polymorphism |
| :--- | :--- | :--- |
| **Resolution Time** | Resolved by compiler during compilation | Resolved dynamically at runtime |
| **Mechanism** | Function Overloading, Operator Overloading | Virtual Functions, Pointers/References |
| **Execution Speed** | Faster (no indirect lookup overhead) | Slightly slower (due to `vtable` pointer traversal) |
| **Flexibility** | Static binding | Highly extensible, dynamic binding |

### 3. Function Overloading & Polymorphism by Parameter

Defining multiple functions in the same scope with the same identifier but differing parameter signatures (number or types of arguments).

```cpp
class Printer {
public:
    void print(int i)         { std::cout << "Int: " << i << std::endl; }
    void print(double d)      { std::cout << "Double: " << d << std::endl; }
    void print(std::string s) { std::cout << "String: " << s << std::endl; }
};
```

### 4. Operator Overloading

Redefining the behavior of C++ operators when applied to user-defined types.

#### Unary Operator Overloading

```cpp
class Counter {
    int value;
public:
    Counter(int v = 0) : value(v) {}

    // Prefix increment (++c)
    Counter operator++() {
        ++value;
        return *this;
    }

    // Postfix increment (c++)
    Counter operator++(int) {
        Counter temp = *this;
        ++value;
        return temp;
    }
};
```

#### Binary Operator Overloading

```cpp
class Complex {
    float real, imag;
public:
    Complex(float r = 0, float i = 0) : real(r), imag(i) {}

    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }
};
```

#### Rules and Restrictions of Operator Overloading

* **Operators that CANNOT be overloaded:**
  1. Scope resolution: `::`
  2. Member selection: `.`
  3. Pointer-to-member: `.*`
  4. Ternary conditional: `?:`
  5. `sizeof`
  6. `typeid`
* **Inviolable Semantics:** You cannot create brand-new operator symbols, modify precedence rules, or alter operand arity (unary remains unary, binary remains binary).
* **Type Requirement:** At least one argument must be a user-defined type.
* **Mandatory Member Operators:** The assignment (`=`), function call (`()`), subscript (`[]`), and class member access (`->`) operators **must** be implemented as member functions, never as external friend functions.

### 5. Pointers to Objects

```cpp
class Student {
public:
    void show() { std::cout << "Student instance" << std::endl; }
};

int main() {
    Student s;
    Student* ptr = &s;
    ptr->show(); // Arrow operator used to dereference and call member
}
```

### 6. The `this` Pointer

An implicit pointer passed to non-static member functions pointing to the object that invoked the function.
* Type inside class `T`: `T* const this`.
* Used to differentiate shadowed member variables from parameters and return reference to calling object for method chaining.

```cpp
class Test {
    int x;
public:
    void setX(int x) {
        this->x = x; // Disambiguates member x from parameter x
    }

    Test& chainSet(int x) {
        this->x = x;
        return *this; // Enables method cascading
    }
};
```

### 7. Virtual Functions & Dynamic Binding

A member function declared with the `virtual` keyword in a base class, overridden by derived classes to enable runtime dynamic dispatch via pointers or references.

```cpp
class Base {
public:
    virtual void show() { std::cout << "Base show" << std::endl; }
};

class Derived : public Base {
public:
    void show() override { std::cout << "Derived show" << std::endl; }
};

int main() {
    Base* ptr = new Derived();
    ptr->show(); // Dynamically binds and executes Derived::show()
    delete ptr;
}
```

### 8. Internal Mechanism: `vtable` and `vptr`

Dynamic dispatch is coordinated by data structures managed by the compiler:

```text
    Object Instance (Memory)                         Virtual Table (vtable)
+-------------------------------+                   +------------------------+
| _vptr (Hidden VPTR Pointer)   | ─────────────────►| &Derived::draw()       |
+-------------------------------+                   +------------------------+
| int data_field                |                   | &Derived::render()     |
+-------------------------------+                   +------------------------+
```

1. **Virtual Table (`vtable`):** A static array of function pointers created once per polymorphic class during compilation and stored in read-only memory.
2. **Virtual Table Pointer (`_vptr`):** A hidden pointer inserted into every object instance of a polymorphic class pointing to the appropriate `vtable`.
3. **Dispatch Step:** Executing `ptr->draw()` resolves at runtime by reading `ptr->_vptr`, looking up the index for `draw()`, and jumping to the resolved function address.

### 9. Virtual Destructors and Pure Virtual Destructors

```cpp
class Base {
public:
    virtual ~Base() { std::cout << "~Base" << std::endl; } // Virtual destructor
};

class Derived : public Base {
    int* data;
public:
    Derived() { data = new int[50]; }
    ~Derived() override {
        delete[] data;
        std::cout << "~Derived" << std::endl;
    }
};

int main() {
    Base* ptr = new Derived();
    delete ptr; // Safely executes ~Derived() followed by ~Base()
}
```

* **Hazard of Non-Virtual Destructors:** If a base class destructor is non-virtual, calling `delete` on a `Base*` pointing to a `Derived` instance uses static binding and invokes only `~Base()`. The derived cleanup is skipped, causing memory leaks.
* **Pure Virtual Destructor:** A class can be made abstract through its destructor (`virtual ~Base() = 0;`), but an out-of-line body definition **must** still be provided (`Base::~Base() {}`) because base destructors are always called during derived destruction.

---

## Module V – Strings, Files, Exception Handling & Generic Programming

### 1. Manipulating Strings

The `std::string` class manages dynamic character buffers with built-in memory management and operations.

```cpp
#include <string>

std::string s1 = "Object";
std::string s2 = "Oriented";
s1 += " " + s2;                            // Concatenation: "Object Oriented"

std::cout << s1.length() << std::endl;     // Length check
std::cout << s1.substr(0, 6) << std::endl; // Substring extraction: "Object"
size_t pos = s1.find("Oriented");          // Search position
s1.replace(pos, 8, "Design");              // Replace substring
```

### 2. Streams and File Handling

**C++ Stream Class Inheritance Hierarchy:**

```text
ios
 ├── istream  → cin, ifstream
 ├── ostream  → cout, ofstream
 └── iostream → fstream
```

```cpp
#include <fstream>
#include <iostream>

int main() {
    // Writing output to file
    std::ofstream outFile("test.txt", std::ios::out);
    outFile << "C++ File IO Data" << std::endl;
    outFile.close();

    // Reading input from file
    std::ifstream inFile("test.txt", std::ios::in);
    std::string line;
    while (std::getline(inFile, line)) {
        std::cout << line << std::endl;
    }
    inFile.close();
}
```

**File Open Modes:**
* `std::ios::in` – Open for reading
* `std::ios::out` – Open for writing (truncates existing contents)
* `std::ios::app` – Open for appending
* `std::ios::binary` – Open in binary mode
* `std::ios::ate` – Seek to the end of the file immediately after opening

### 3. Stream State Flags and Error Queries

| State Bit Flag | Condition Indicated | Diagnostic Query Method |
| :--- | :--- | :--- |
| `ios::goodbit` | Operational success; no error flags set | `stream.good()` |
| `ios::eofbit` | End of stream encountered | `stream.eof()` |
| `ios::failbit` | Logical formatting or extraction error (recoverable) | `stream.fail()` |
| `ios::badbit` | Serious I/O or device integrity failure (unrecoverable) | `stream.bad()` |

* `stream.clear()` resets error flags back to `goodbit` to allow further operations.

### 4. File Pointers and Random Access Mechanics

* **Get Pointer (`seekg`, `tellg`):** Manages the read position for input streams.
* **Put Pointer (`seekp`, `tellp`):** Manages the write position for output streams.

```cpp
// Seeking reference positions:
// ios::beg (from start), ios::cur (from current position), ios::end (from end)
file.seekg(10, std::ios::beg);     // Move get pointer to the 10th byte from start
std::streampos pos = file.tellg(); // Return current byte index of get pointer
```

### 5. Formatted and Unformatted I/O

* **Unformatted I/O:** Transfers raw bytes directly without parsing (`cin.get()`, `cin.getline()`, `cout.put()`, `cout.write()`).
* **Formatted I/O:** Uses stream manipulators from `<iomanip>` to format presentation:
  * `std::setw(int n)`: Sets character display field width.
  * `std::setprecision(int n)`: Configures floating-point output precision.
  * `std::fixed`: Enforces fixed-point decimal notation.
  * `std::hex`, `std::dec`: Controls base numbering output.

### 6. Exception Handling

Encapsulates runtime error handling within structured blocks:

```cpp
try {
    int age = -5;
    if (age < 0)
        throw "Age cannot be negative"; // Throwing an exception
}
catch (const char* msg) {               // Catching specific exception type
    std::cout << "Error: " << msg << std::endl;
}
catch (...) {                           // Catch-all handler
    std::cout << "Default catch-all handler executed" << std::endl;
}
```

### 7. Stack Unwinding and RAII

* **Stack Unwinding:** When an exception is thrown, runtime execution pauses, and the call stack unwinds frame by frame until an appropriate `catch` block is reached. Destructors are called for all fully constructed local objects in those exiting scopes.
* **RAII (Resource Acquisition Is Initialization):** Wrapping heap pointers, file descriptors, and hardware sockets inside classes ensures that stack unwinding automatically invokes their destructors, preventing resource leaks.

### 8. Standard Exception Class Hierarchy

```text
                    std::exception
                          │
       ┌──────────────────┴──────────────────┐
       ▼                                     ▼
 std::logic_error                      std::runtime_error
   ├── std::invalid_argument             ├── std::overflow_error
   ├── std::out_of_range                 ├── std::underflow_error
   ├── std::length_error                 └── std::range_error
   └── std::domain_error
```

* **`std::logic_error`:** Faults caused by program logic errors that can theoretically be avoided through validation checks (e.g., out-of-range bounds access).
* **`std::runtime_error`:** Faults caused by unpredictable external events during program execution (e.g., disk full, numeric overflow).

### 9. Generic Programming – Templates

**Function Template:**

```cpp
template <typename T>
T maximum(T a, T b) {
    return (a > b) ? a : b;
}
```

**Class Template:**

```cpp
template <class T>
class Stack {
    T arr[100];
    int top;
public:
    Stack() : top(-1) {}
    void push(T x) { arr[++top] = x; }
    T pop() { return arr[top--]; }
};

Stack<int> intStack;            // Instantiates integer stack
Stack<std::string> stringStack; // Instantiates string stack
```

### 10. Templates vs. C Preprocessor Macros

| Criterion | C++ Templates | Preprocessor Macros (`#define`) |
| :--- | :--- | :--- |
| **Processing Stage** | Evaluated by the **compiler** | Expanded by the **preprocessor** |
| **Type Safety** | Fully type-checked at compile time | No type-checking; simple text substitution |
| **Scope Sensitivity** | Respects class, function, and namespace scopes | Completely ignores scope boundaries |
| **Side Effects** | Evaluates arguments once | Can evaluate arguments multiple times (e.g., `SQR(x++)`) |
| **Debugging** | Clear compiler error output and debug symbol tracking | Hard-to-trace macro expansion errors |

### 11. Template Specialization

Provides an alternative implementation tailored to a specific data type.

```cpp
// Primary general template
template <typename T>
class Formatter {
public:
    void print(T val) { std::cout << "Value: " << val << std::endl; }
};

// Explicit full specialization for bool
template <>
class Formatter<bool> {
public:
    void print(bool val) { std::cout << (val ? "True" : "False") << std::endl; }
};
```

### 12. Standard Template Library (STL) Architecture

**The Pillars of STL:**
1. **Containers:** Data structures that hold collections of objects.
2. **Algorithms:** Built-in operations for sorting, searching, and transforming container data.
3. **Iterators:** Object-oriented abstractions for pointers used to step through containers.
4. **Functors (Function Objects):** Objects that can be invoked like functions by overloading `operator()`.
5. **Allocators:** Abstract memory managers that handle dynamic allocation for containers.

| Container Type | Containers | Characteristics |
| :--- | :--- | :--- |
| **Sequence** | `std::vector`, `std::deque`, `std::list`, `std::array` | Linear, sequential element ordering |
| **Associative** | `std::set`, `std::map`, `std::multiset`, `std::multimap` | Key-sorted tree structures (usually Red-Black trees) |
| **Unordered** | `std::unordered_set`, `std::unordered_map` | Key-hashed hash table structures |
| **Adapters** | `std::stack`, `std::queue`, `std::priority_queue` | Constrained container wrappers |

### 13. STL Iterator Hierarchy & Invalidation Rules

```text
Random Access Iterator (std::vector, std::deque, raw pointer)
         ▲
         │ (supports +n, -n, [], <, >)
Bidirectional Iterator (std::list, std::set, std::map)
         ▲
         │ (supports --)
  Forward Iterator (std::forward_list, std::unordered_set)
         ▲
         │ (supports ++)
   Input Iterator / Output Iterator
```

* **Iterator Invalidation:** Modifying a container (such as calling `push_back()` on a `std::vector` that exceeds its current capacity) triggers dynamic reallocation, invalidating existing pointers, references, and iterators to its elements.

---

## Quick Revision Checklist

- [ ] Differences between C and C++ & POP vs OOP
- [ ] Object-Based vs Object-Oriented languages
- [ ] Four pillars: Abstraction, Encapsulation, Inheritance, Polymorphism
- [ ] Memory layout of an object, empty class size (1 byte), and data alignment
- [ ] Member initialization lists (required scenarios)
- [ ] Shallow copy vs Deep copy mechanics
- [ ] Static data members, static member functions, and friend declarations
- [ ] Constructors (default, parameterized, copy, dynamic) and destructors
- [ ] Inheritance access modes (`public`, `protected`, `private`)
- [ ] Construction and destruction order across inheritance hierarchies
- [ ] Diamond problem, virtual base classes, `vbptr`, and `vbtable`
- [ ] Object slicing causes and prevention
- [ ] Virtual functions, `vtable`, `_vptr`, and dynamic binding
- [ ] Virtual destructors and memory leak prevention
- [ ] Operator overloading rules and non-overloadable operators
- [ ] Exception handling: `try`, `catch`, `throw`, and stack unwinding
- [ ] Standard exception class hierarchy (`logic_error` vs `runtime_error`)
- [ ] File handling streams, open modes, and file pointers (`seekg`/`seekp`)
- [ ] Templates vs macros, and template specialization
- [ ] STL containers, algorithms, iterator category hierarchy, and iterator invalidation

