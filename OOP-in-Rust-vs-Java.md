# **Rust vs. Java: A Comparison of OOP Implementations**
This blog compares the object-oriented programming (OOP) features of Rust and Java. It explores how Rust implements OOP concepts like classes, inheritance, interfaces, access modifiers, method overriding and overloading, and common design patterns using structs, traits, and modules. Through code examples, this blog demonstrates how Java OOP patterns can be translated into Rust, highlighting Rust's strengths in safety and performance.

---

## **Introduction**
Object-oriented programming (OOP) is a paradigm centered around objects that contain both data and methods to manipulate that data. Java is a language that fully embraces OOP principles with its class-based architecture, inheritance, and interfaces. Rust, while not class-based in the traditional sense, supports OOP concepts using structs and traits, and modules, promoting memory safety and concurrency without a garbage collector. This blog compares how Rust and Java implement core OOP concepts, illustrating how familiar Java patterns can be achieved in Rust.

---

## **Classes and Structs**
### **Java's Classes**
In Java, a class is a blueprint for creating objects, encapsulating data for the object and methods to manipulate that data.
```java

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String get_name() {
        return this.name;
    }
}
```

### **Rust's Structs**
Rust doesn't have classes in the traditional sense. Instead, it uses structs to define custom data types.  While structs hold data, methods are defined separately.
```rust
struct Person {
    name: String,
    age: u32,
}
```

#### **Implementing Methods in Rust**
Methods are associated with structs using the `impl` keyword.
```rust
impl Person {
    fn new(name: String, age: u32) -> Self {
        Person { name, age }
    }
    
    fn get_name(&self) -> &str {
        &self.name
    }
}
```

### **Comparison**
- **Encapsulation**: Java encapsulates data and methods within classes. Rust separates data (structs) and behavior (`impl` blocks).
- **Instantiation**: Java uses the `new` keyword (e.g. `new Person()`), whereas Rust uses associated functions (e.g. `Person::new()`).

---

## **Access Modifiers**
### **Java's Access Modifiers**
Java provides `public`, `protected`, `private`, and package-private (default) access modifiers to control visibility.
```java
public class Person {
    private String name;
    protected int age;
    public String gender;
}
```

### **Rust's Access Control**
Rust controls access at the module level using the `pub` keyword. By default, items are private to their module. It doesn't have a protected keyword, but similar behavior can be achieved through module hierarchies.
```rust
pub struct Person {
    name: String, // private by default
    pub(crate) age: u32, // accessible within the same crate
    pub gender: String,
}
```

### **Comparison**
- Rust lacks protected, but module-level privacy can achieve similar encapsulation.
- No fine-grained access control like in Java, but the module system provides necessary encapsulation.

---

## **Inheritance and Traits**
### **Java's Inheritance**
Java supports single inheritance, allowing a class to inherit from one superclass.
```java
public class Student extends Person {
    private int studentId;

    public Student(String name, int age, int studentId) {
        super(name, age);
        this.studentId = studentId;
    }
}
```

### **Rust's Traits and Composition**
Rust doesn't support inheritance in the traditional sense. Instead, it uses traits to define shared behavior and composition to reuse code.
```rust
trait PersonTrait {
    fn get_name(&self) -> &str;
}

struct Student {
    name: String,
    age: u32,
    student_id: u32,
}

impl PersonTrait for Student {
    fn get_name(&self) -> &str {
        &self.name
    }
}
```

### **Comparison**
- **Inheritance vs. Traits**: Java's inheritance is about extending classes, whereas Rust's traits are about implementing shared behavior.
- **Composition over Inheritance**: Rust encourages composition, combining simple types to build complex ones.

---

## **Abstract Classes and Interfaces vs. Traits**
### **Java's Abstract Classes and Interfaces**
Abstract classes can have method implementations, while interfaces (pre-Java 8) cannot.
```java
public abstract class Animal {
    public abstract void makeSound();
}

public interface Pet {
    void play();
}
```

### **Rust's Traits**
Traits in Rust can provide method signatures and default implementations.
```rust
trait Animal {
    fn make_sound(&self);
}

trait Pet {
    fn play(&self) {
        println!("Playing!");
    }
}
```

### **Comparison**
- **Default Implementations**: Both Java (from Java 8) and Rust support default method implementations.
- **Multiple Trait Implementation**: Rust allows a struct to implement multiple traits, similar to Java's multiple interfaces.

---

## **Method Overriding and Overloading**
### **Method Overriding**
#### **Java's Method Overriding**
Allows a subclass to provide a specific implementation of a method already provided by its superclass.
```java
@Override
public void makeSound() {
    System.out.println("Meow");
}
```

#### **Rust's Trait Method Implementation**
Trait methods can be overridden when implemented for a struct.
```rust
impl Animal for Cat {
    fn make_sound(&self) {
        println!("Meow");
    }
}
```

### **Method Overloading**
#### **Java's Method Overloading**
Java allows methods with the same name but different parameters.
```java
public void greet() {}
public void greet(String name) {}
```

#### **Rust's Approach**
Rust doesn't support method overloading directly but can achieve similar results using traits and enums.
```rust
impl Person {
    fn greet(&self) {}
}

trait Greeter {
    fn greet(&self, name: &str);
}

impl Greeter for Person {
    fn greet(&self, name: &str) {}
}
```

### **Comparison**
- **Overriding**: Both languages support method overriding through inheritance (Java) or trait implementation (Rust).
- **Overloading**: Java supports method overloading; Rust requires different method names or argument patterns.

---
## **Design Patterns**
Rust's approach to implementing design patterns often differs from traditional object-oriented languages due to its unique features like ownership, borrowing, and traits. Many classical design patterns become either unnecessary or are naturally integrated into the language's features. For example, the Singleton pattern is seldom needed because Rust's module system and the ability to create static variables with thread-safe access patterns (using `lazy_static` or `std::sync::OnceCell`) provide safe global state management. Patterns like Strategy and Observer can be elegantly implemented using traits and closures, leveraging Rust's powerful abstraction capabilities without incurring runtime overhead.

---
## **Conclusion**
While Rust and Java differ in their approach to object-oriented programming, Rust provides powerful mechanisms to achieve similar objectives. Rust's focus on safety and performance leads to design choices like composition over inheritance and traits over classes/interfaces. Understanding these differences allows developers to write idiomatic Rust code while leveraging familiar OOP concepts from Java.

---

## **References**
- [The Rust Programming Language Book - Traits](https://doc.rust-lang.org/book/ch10-02-traits.html)
- [The Rust Programming Language Book - OOP](https://doc.rust-lang.org/stable/book/ch17-00-oop.html)
- [Rust Reference](https://doc.rust-lang.org/reference)
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/)
- [Java Tutorials](https://docs.oracle.com/javase/tutorial/java/concepts/)

