# Design Patterns in Ruby

Summary of the design patterns explained in the book [Design Patterns in Ruby](http://designpatternsinruby.com/), where [Russ Olsen](http://russolsen.com/) explains and adapts to Ruby 14 of the original 23 GoF design patterns.

## Design Patterns

### GoF Patterns

* [Adapter](adapter.md): helps two incompatible interfaces to work together
* [Builder](builder.md): create complex objects that are hard to configure
* [Command](command.md): performs some specific task without having any information about the receiver of the request
* [Composite](composite.md): builds a hierarchy of tree objects and interacts with all them the same way
* [Decorator](decorator.md): vary the responsibilities of an object adding some features
* [Factory](factory.md): create objects without having to specify the exact class of the object that will be created
* [Interpreter](interpreter.md): provides a specialized language to solve a well defined problem of know domain
* [Iterator](iterator.md): provides a way to access a collection of sub-objects without exposing the underlaying representation
* [Observer](observer.md): helps building a highly integrated system, maintainable and avoids coupling between classes
* [Proxy](proxy.md): allows us having more control over how and when we access to a certain object
* [Singleton](singleton.md): have a single instance of certain class across the application
* [Strategy](strategy.md): varies part of an algorithm at runtime
* [Template Method](template_method.md): redefines certain steps of an algorithm without changing the algorithm's structure

### Non-GoF Patterns: Patterns For Ruby

* [Convention Over Configuration](convention_over_configuration.md): build an extensible system and not carrying the configuration burden.
* [Domain-Specific Language](dsl.md): build a convenient syntax for solving problems of a specific domain.
* [Meta-Programming](meta_programming.md): gain more flexibility when defining new classes and create custom tailored objects on the fly.




--------------

### Creational Patterns

- **Singleton**: Ensures a class only has one instance and provides a global point of access to it.
- **Factory**: Provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created.
- **Abstract Factory**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- **Builder**: Separates the construction of a complex object from its representation so that the same construction process can create different representations.
- **Prototype**: Specifies the kind of objects to create using a prototypical instance and create new objects by copying this prototype.

### Structural Patterns

- **Adapter**: Converts the interface of a class into another interface the clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- **Decorator**: Attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
- **Facade**: Provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
- **Composite**: Composes objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.
- **Proxy**: Provides a surrogate or placeholder for another object to control access to it.
- **Bridge**: Decouples an abstraction from its implementation so that the two can vary independently.
- **Flyweight**: Uses sharing to support large numbers of fine-grained objects efficiently.

### Behavioral Patterns

- **Observer**: Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
- **Command**: Encapsulates a request as an object, thereby letting you parameterize clients with queues, requests, and operations.
- **Strategy**: Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from the clients that use it.
- **State**: Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
- **Template**: Defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
- **Iterator**: Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
- **Mediator**: Defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.
- **Memento**: Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later.
- **Chain of Responsability**: Used to achieve loose coupling in software design where a request from the client is passed to a chain of objects to process them.
- **Interpreter**: Defines a grammatical representation for a language and provides an interpreter to deal with this grammar.
- **Visitor**: Used when we have to perform an operation on a group of similar kind of Objects.

## Other Patterns

- **Active Record Pattern**: Found in many modern ORM (Object Relational Mapping) libraries, the Active Record pattern ties database access directly to an object, where the object mirrors the database table. Each instance of the object corresponds to a row in the table.
- **Repository Pattern**: Acts as a kind of in-memory domain object collection. Client objects construct query specifications declaratively and submit them to Repository for satisfaction.
- **Data Access Object (DAO) Pattern**: Provides an abstract interface to a database or a persistent storage mechanism.
- **Dependency Injection (DI) Pattern**: A way to achieve Inversion of Control (IoC) in a system by breaking dependencies between higher-level and lower-level software layers.
- **Model-View-ViewModel (MVVM) Pattern**: Primarily used for designing the user interface. It divides an application into three interconnected components: Model (represents the data), View (represents the UI), and ViewModel (acts as a bridge that handles the data presentation in the View).
- **CQRS (Command Query Responsibility Segregation)**: A pattern where you separate the command (write) operations from the query (read) operations, ensuring that a method is either a command that performs an action or a query that returns data, but not both.
- **Event Sourcing**: Capturing all changes to an application state as a sequence of events, allowing for replaying these events to restore the application's state.
- **Flux/Redux Pattern**: Used mainly in frontend frameworks like React. It involves a unidirectional data flow where the view sends actions to a central dispatcher, which updates the store, and the changed state then reflects in the view.
- **Saga Pattern**: A mechanism to manage long-running, complex processes or workflows. Sagas manage failures, maintain consistency, and coordinate activities.
- **Service Locator Pattern**: Provides a centralized point from which objects can retrieve services (like dependency injection but involves the client requesting its dependencies).
- **Null Object Pattern**: A design pattern that uses polymorphism to represent the absence of an object by providing an alternative that offers default do-nothing behavior.
- **Value Object Pattern**: Small objects that represent a descriptive aspect of the domain with no conceptual identity. They are immutable, meaning that they cannot be altered once they are created.
- **Thread Pool Pattern**: Allows a limited set of threads to be reused for executing multiple tasks.

  
## Contributing

Contributions are welcome! What could you do?:
* Find typos and grammar mistakes
* Propose a better way to explain a pattern
* Add clearer examples of a pattern usage
* Add other GoF patterns that are not covered in the book

**Code examples refactoring** PR's will **not be considered**. The examples provided by Russ Olsen in his book are meant to be simple and self explanatory, not the best performing or most elegant, their purpose is just educational.
