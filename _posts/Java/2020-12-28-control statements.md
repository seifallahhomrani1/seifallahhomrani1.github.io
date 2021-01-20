---
title: "Java - OOP Concepts"
author : Seif-Allah
layout: post
category : Java
description : Java OOP concepts.
image : /assets/images/java/oop_concepts.png
toc : true 
---
Basically, the Java control statements are the same as in C language.

### if-else statement

```java
public class Java_Nested_if{
    public static void main(String[] args){
        int age = 25; 
        int weight = 48; 


        if (age>=18){
            if (weight>50){
                System.out.println("You can donate blood");
            }
            else{
                System.out.println("Go eat some fucking food");
            }
        else{
            System.out.println("Sorry lil boy, go play with your toys");
        }
        }
    }
}

```

### Switch statement

The switch statement tests the equality of a variable against multiple values.

Rules : 
- There can be one or N number of case values for a switch expression.
- The case value must be of switch expression type only. The case value must be literal or constant. It doesn"t allow variables.
- The case values must be unique. In case of duplicate value, it renders compile-time error.


> Switch statement example: 
```java

public class SwitchExample2{
    public static void main(String[] args){

        int number = 20; 

        switch(number){
            case 10 : System.out.println("10");
            case 20: System.out.println("20");
            case 30 : System.out.println("30");
            default : System.out.println("Not in 10, 20,30");
        }


    }
}

```
