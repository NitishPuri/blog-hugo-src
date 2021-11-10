---
title: Head First Design Patterns
tags: [notes, design, programming]
categories: [books]
lastmod: 2017-05-09
---

#### Strategy Pattern
- Defines a family of algorithms, encapsulates each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.   

### Observer Pattern
- Defines one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated.

### Decorator Pattern
- Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for existing functionality.
- Java IO classes

### Factory Method Pattern
- Defines an interface for creating an object, but lets subclasses decide which class to instantiate. factory Method lets a class defer instantiation to subclass.

### Abstract Factory Method Pattern
- Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

### Singleton Pattern
- Ensure a class only has one instance and provide a global point of access to it.

### Command Pattern
- Encapsulates a request as an object, thereby letting you parameterize clients with different requests, queue or log requests and support undo-able operations.
- Transactions

### Adapter Pattern
- Converts the interface of a class into another interface clients expect. Lets classes work together that couldn't otherwise because of incompatible interface.

### Facade pattern
- Provides a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

### Template Method Pattern
- Define the skeleton of an algorithm in n operation, deferring some steps to subclasses. Template Method lets subclass redefine certain steps of an algorithm without changing the algorithm's structure.

### Iterator Pattern
- Provide a way to access the elements of an aggregate object sub sequentially without exposing its underlying representation.

### Composite Pattern
- Compose objects into tree structures to reresent part-whole heriarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

### State Pattern
- It Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

### Proxy Pattern
- Provides a surrogate or placeholder for another object to control access to it.
