---
title: "Java - OOP Concepts"
author : Seif-Allah
layout: post
category : Java
Description : Java OOP concepts.
image : /assets/images/java/oop_concepts.png
---

### Introduction 
The main aim of object-oriented programming is to implement real-world entities, for example, object, classes, abstraction, inheritance, polymorphism, etc. 

Before you read my article, I totally **recommend** you this Youtube video which really explains Java OOP in 10 minutes ! 
<p align='center'>
<iframe width="560" height="315" src="https://www.youtube.com/embed/CWYv7xlKydw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"></iframe>
</p>


### Object Oriented Programming System
Object means a real-world entity such as a pen, chair, table, computer, watch, etc. 
Object-Oriented Programming is a methodology or paradigm to design a program using classes and objects. It simplifies software development and maintenance by providing some concepts:

- **Object** : Any entity that has state and behavior is known as an object.

An object can be defined as an instance of a class. 
An object contains an address and takes up some space in memory. Objects can communicate without knowing the details of each other's data or code. 
The only necessary thing is the type of message accepted and the type of response returned by the objects.

- **Class** : Collection of objects is called class. It is a logical entity. 
A class can also be defined as a blueprint. 
Class doesn't consume any space. 

- **Inheritance** : When one object acquires all the properties and behaviors of a parent object, it is known as inheritance. It provides code *reusability*. 
It is used to achieve runtime polymorphism. 

- **Polymorphism** : If one task is performed in different ways, it is known as polymorphism.

In Java, we use method overloading and method overriding to achieve polymorphism.

- **Abstraction** : Hiding internal details and showing functionality is known as abstraction. 

In Java, we use abstract class and interface to achieve abstraction. 

- **Encapsulation** : Wrapping code and data together into a single unit are known as encapsulation. 

A Java class is the example of encapsulation. Java bean is the fully encapsulated class because all the data members are private there. 

- **Coupling** : Coupling refers to the knowledge or information or dependency of another class. It arises when classes are aware of each other. If a class has the details information of another class, there is strong coupling. In Java, we use private, protected, and public modifiers to display the visibility level of a class, method, and field. You can use interfaces for the weaker coupling because there is no concrete implementation.

- **Cohesion** : Cohesion refers to the level of a component which performs a single well-defined task. A single well defined task is one by a highly cohesive method. The weakly cohesive method will split the task into separate parts. The java.io package is a highly cohesive package because it has I/O related classes and interface. However, the java.util package is weakly cohesive package because it has unrelated classes and interfaces.

- **Association** : Association represents the relationship between the objects. 
 1. One to One
 2. One to Many
 3. Many to One
 4. Many to May
 Association can be unidirectional or bidirectional.



- **Aggregation** : Aggregation is a way to achieve Association. Aggregation represents the relationship where one object contains other objects as a part of its state. It represents the weak relationship between objects. It is also termed as **has-a** relationship in Java. Like, inheritance represents the **is-a** relationship. It is another way to reuse objects.

- **Composition** : The composition is also a way to achieve Association. The composition represents the relationship where one object contains other objects as a part of its state.There is a strong relationship between the containing object and the dependant object. It is the state where containing objects do not have an independent existence. If you delete the parent object, all the child objects will be deleted automatically.
