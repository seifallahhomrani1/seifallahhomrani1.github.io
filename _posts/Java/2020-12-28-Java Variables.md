---
title: "Java - Variables"
author : Seif-Allah
layout: post
category : Java
Description : "Java variables: Local, Instance and Static".
image : /assets/images/java/oop_concepts.png
---

### Java Variables

A variable is a container which holds the value while the Java Program is executed. A variable is assigned with a data type. 

Variable is a name of memory location. There are three types of variables in java: local, instance and static. 


There are two types of data types in Java: primitive and non-primitive.

1. **Local Variable**
A variable declared inside the body of the method is called local variable. You can use this variable only within that method and the other methods in the class aren't even aware that the variable exists.

A local variable cannot be defined with static keyword.

2. **Instance Variable**
A variable declared inside the class but outside the body of the method, is called instance variable. 
It is not declared as static. 

It is called instance variable because its value is instance specific and is not shared among instances.

3. **Static Variable**

A variable which is declared as static is called static variable. It cannot be local. You can create a single copy of static variable and share among all the instances of the class . Memory allocation for static variable happens only once when the class is loaded in the memory. 

#### Example to understand the types of variables in Java 

```java
class A {
    int data=50; //instance variable
    static int m=100; // static variable


    void method(){
        int n=90; //local variable
    }
} //end of class
```



##### Java Variable Example : Add Two Numbers

```java
class Simple{
    public static void main(String[] args){
        int a=10; //a is an instance variable
        int b=10; //b is an instance variable
        int c=a+b; 
        System.out.println(c);
    }   
}

Output : 20
```

##### Java Variable Example : Widening

```java
class Simple{
    public static void main(String[] args){
        int a=10; 
        float f=a;
        System.out.println(a);
        System.out.println(f);
        
    }
}

Output : 
10
10.0
```

#### Java Variable Example: Narrowing (Typecasting)

```java
class simple{
    public static void main(String[] args){
        float f=10.5f; //f is placed after value because by default value is double, so to tell the compiler to treat is as float explicitly we use f or F
        int a=f; // This will give a runtime error
        int a = (int)f; // This is typecasting
        System.out.println(f); 
        System.out.println(a); 

    }
}

Output : 
10.5
10
```

#### Java Variable Example : Overflow

Overflow occurs when we assign such a value to a variable which is more than the maximum permissible value.

```java
class Simple{
    public static void main(String[] args){
        //Overflow
        int a=130; 
        byte b = (byte)a; //Max Value of byte is 126  
        System.out.println(a);
        System.out.println(b);
    }
}

Output : 
130
-126
```
#### Java Variable Example : Adding Lower Type

```java
class Simple{
    public static void main(String[] args){
        byte a = 10; 
        byte b = 10; 
        byte c = a+b; // Compile Time Error : because a+b will be int 
        byte c = (byte)(a+b);
        System.out.println(c);
    }
}

Output :
20