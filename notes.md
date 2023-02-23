# Java OOPDA

Welcome to the markdown notes of my OOPDA class.

## ArrayLists & Linked Lists

Should not be left uninitialized as an instance variable.

### Some practice from 1/23/23:

```java
import java.util.ArrayList;
import java.util.ListIterator;
import java.util.Iterator;

public static void main(String[] args) {
    ArrayList<Double> dbls = new ArrayList<Double>();
    dbls.add(15.5);
    dbls.add(84.1);
    dbls.add(22.6);
    dbls.add(48.15);
    dbls.add(64.001);
    Iterator<Double> regIt = dbls.iterator();
    ListIterator<Double> listIt = dbls.listIterator();

    while(regIt.hasNext()) 
        System.out.println(regIt.next());

    System.out.println("\n\n\n\n\n");

    for(int i = 0; i < 5; i++)
        listIt.next();

    while(listIt.hasPrevious()) 
        System.out.println(listIt.previous());
}
```

### Which is better at retrieval?

LinkedLists would have you move past all the other items in order to get to an item in the middle of the list. Accessing just one item is easier by just using the index position in an **ArrayList**.

### Which is better at insertion and removal?

ArrayLists would have to shift all the other indexes that come after when an item is added or removed. **LinkedLists** can just shift the position of two items, which is faster for insertertion and removal.

---

# Hashing

Hashing is basically just putting an object through a function. The function will produce a unique **hashcode**, which tells the computer how to order the objects in a collection.

A hash should produce the same hashcode if given the same values. In order to not run into a collision when the values are the same, we consider the memory location. That way, two objects, even with the same values in their fields, hashing will produce different results.

A perfect hashing algorithm would never result in a collision.

## HashSet

For HashSets, we don't want duplicates. In this instance, the hashcode function is based only on the values, so we know that we don't add duplicate hashcodes to the collection.

**LinkedHashSets** preserve the insertion order.

## "Red-Black" Tree

1. Every node is either black or red.
2. The root is always black.
3. If a node is red, it's children must be black.
4. Every path from the root to a leaf (or null child) must contain the same number of black nodes.

# Maps

## TreeMaps

These Maps use a `compareTo()` method to figure out where in the tree to place an item being added. It returns a negative, a positive, or a zero.

* A negative number means that the current object(the one invoking the method) goes before the object that it is compared to.
* A positive number means that the current object would go after the object it's being compared to.
* A zero means that the objects are equal

### Rewriting compareTo()

If we want to change how `compareTo()` works, we would create a class implementing the `Comparable` interface, in order to override the `compareTo()` method with our own.

## HashMaps

The keys cannot be duplicates; therefore the keys are stored in a HashSet. The values can be duplicates; however, making a List the optimal kind of collection.

Also, TreeMaps use a TreeSet to contain the keys.

Similarly, LinkedHashMaps have LinkedHashSets to contain keys.

---

#  Interfaces

Defines behaviors of subclasses. 
It's a way to spell out a connection between two groups: the supplier of a service and the classes that want their objects to be a part of the service.

It should be used by a system designer to **enforce behaviors** that we choose.
An example would look a little like this:

```java
public interface Enrollable {
    public void add(Student student);
    public void drop(Student student);
}
```

There is also a `default` keyword, which sets the default behavior for a given interface. We call this kind of method **concrete**. It would look like this:

```java
public interface Enrollable {
    public default void add(Student student) {
        logger.log(Level.INFO, student.getName() + " enrolled in " + this);
        EnrollmentManager.enroll(this, student);
    }
}
```

Interface methods don't need to necessarily have the same return type. The parameter's data types have to be the same, and the ***arity***, aka the number of parameters, must be the same. But, the return types can be different. 

# Inheritance

The return types can't be just anything though, they must be **covariant**. You can use a subclass of the original return type. This kind of overriding works with interfaces as well as subclass relationships.

There are 3 options of default methods being overridden:

1. Option 1 is to have `abstract` methods in the interface, but **concrete** methods in the class
2. Option 2 is to have `default` **concrete** methods in the interface, but no version of the  methods in the class
3. Option 3 is to have `default` **concrete** methods in the interface, as well as overrideen **concrete** methods in the class

Nice Quote from Myers:
> "Java is known for deadbeat dads, and rebellious children" - Professor Jack Myers

The parent class knows **absolutely nothing** about the child classes. It is completely unaware of it's children, ergo "a deadbeat dad."

The children don't do the behaviors that the parents want them to, by `overriding` methods.

However, child classes can use a superclass' method by using `super`. For example:

    The child class overrode the drive() method.

    If it wanted to use the parent class' method, it would use super.drive()

If you used multiple parent classes with the same name for the method, use the name of the interface or parent class first. Looks like this:

    The child class overrode the drive() method, but has two parents `Mom` and `Dad` that both have drive() methods of their own.

    If it wanted to use the Mom's method, it would use Mom.super.drive()
    If it wanted to use the Dad's method, it uses Dad.super.drive()

## Typecasting

When you want to use a child class's method, use typecasting like so:

```java
List<String> words = new ArrayList<String>();

((ArrayList<String>)words).trimToSize(); //the double parens are necessary because without them, the dot operator comes first in the order of operations
```

If you want to make sure that your object is the correct type before typecasting, you can do something like this:

```java
if(words.getClass().getSimpleName().equals("Integer")) {
    int x = (Integer)words; // just proof of concept for these lines
}
```

The `getClass()` method will tell you the dynamic type, and the `getSimpleName()` method will return a String we could use.

Something really useful is Java's `instanceof` keyword, which just checks the 'is-a' relationship. For example:

```java
List<String> list = new ArrayList<String>();
System.out.println(list instanceof ArrayList); // prints true because list 'is-a' ArrayList
System.out.println(list instanceof List); // prints true for the same reason
System.out.println(list instanceof String); // errors out because it's not the correct type
```

## Java Lists

The Java List interface has methods that have to follow behaviors. If you wanted to create a new type of list following this interface, you will have to provide all of the methods that are inside of the List interface.

## Open closed principle

Classes should be closed to modification but open for extension.

Take the Foxes and Rabbits lab for example.

If we wanted the Simulator class to be able to be 3d, the system should be **"open for extension"** or able to allow the addition of features.

However, the code should not require a bunch of changes in every single class if you wanted to modify a single class, which is the purpose of **"closed for modification"**.

It is always better to couple to superclasses than to couple to subclasses. 
Then, it is always optimal to work at the superclass level. 
You also want the parents to **ALWAYS** be deadbeat dads that don't know anything about their kids.
If you follow that principle, the classes can be decoupled, and you can be an amazing Object-Oriented Programmer.

# Static and Dynamic

The **static** type is how a variable is declared and what the compiler thinks it is.

The **dynamic** type is what the object really was created as and how it is stored in memory.

Below we have the static types on the left side of the '=' and we have the dynamic type on the right of it. These three objects are like so:

[\\]: # (First time using a markdown table, kind of odd at first but nice)
|name|oopda|oopda1|oopda2|
|-|-|-|-|
|static type| Course | Section | Enrollable |
|dynamic type| Course | Section | Section |

```java
    private static Course oopda = new Course(<parameters>);
    private static Section oopda1 = new Section(<parameters>);
    private static Enrollable oopda2 = new Section(<parameters>);
```

**Watch out!** We use the word `static` for two different uses. The static type of an object is the type of the container, even though it can contain a child class. It just means that the static type is the variable's type, but the dynamic type can be the same or a child class.

The other kind of `static` means that a method or field is usable, even without an instance of the object.

# Lambda Functions

Lambda functions are written inline, similar to passing a value into a function. They take an input, and use it for an output or an action.

    A -> B

They generally look like the above line of code.

Lamda functions treat **functional interfaces**(interfaces with just one method) differently. You can override the **functional method**(the only method in the interface) with the code in your lamda like so:

```java
public class Calculator {
    public static void main(String[] args) {
        Calculator myApp = new Calculator();
        IntegerMath addition = (a,b) -> a + b;
        IntegerMath subtraction = (a,b) -> a - b;
        System.out.println("40 + 2 = " + myApp.operateBinary(40, 2, addition));
        System.out.println("20 - 10 = " + myApp.operateBinary(20, 10, subtraction));
    }

    //this is called an inner interface
    interface IntegerMath {
        int operation(int a, int b);
    }

    public int operateBinary(int a, int b, IntegerMath op) {
        return op.operation(a, b);
    }
}
```

Functional interfaces use some generic types:
|R|T|U|
|-|-|-|
|return type|1st param type|2nd param type|