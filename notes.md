# Java OOPDA

Welcome to the markdown notes of my OOPDA class.

For now, this is the completed version of the site. I hope you like it! 
If you run into any issues using the site, email me at carrwi96@students.rowan.edu

If you were wondering about the forced refresh after clicking on a link, there is a certain html tag that is making it impossible for me to use jumping links and I have to refresh the page for the links to work properly.

---

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

# Design

## Single Responsibility Principle

Classes should only ever handle a single responsibility. It makes refactoring, debugging, and all of your life easier after the code is written and possibly modified.

## Open Closed Principle

Classes should be closed to modification but open for extension.

Take the Foxes and Rabbits lab for example.

If we wanted the Simulator class to be able to be 3d, the system should be **"open for extension"** or able to allow the addition of features.

However, the code should not require a bunch of changes in every single class if you wanted to modify a single class, which is the purpose of **"closed for modification"**.

It is always better to couple to superclasses than to couple to subclasses. 
Then, it is always optimal to work at the superclass level. 
You also want the parents to **ALWAYS** be deadbeat dads that don't know anything about their kids.
If you follow that principle, the classes can be decoupled, and you can be an amazing Object-Oriented Programmer.

Classes should be opened for extension but closed for modification. (Necessary for Foxes and Rabbits lab).

**Extension** - the addition of new functionality

**Modification** - any change that does not involve the addition of new functionality

This principle should be applied to each and every class you create.

Nouns generally will mean `classes` and `instance variables`, and verbs generally mean `methods`.
When choosing a type for a book's author, the instance variable could be a String or an Author class
that you write.

## The Black Box

What's the reason why we keep instance variables private?

We call it the **Black Box**. When we encapsulate instance variables by making them private, we make it so that the variables are controlled by us and no other classes.

Take this for example:
```java
it.setAuthor("St. King");

it.author = "Stephen King";
```

The difference here is that we don't really know what setAuthor does.
It could validate if the author is correct. It could make sure the String follows a certain format.
We don't know what it does, which is exactly how **Encapsulation** is supposed to work.

We put the setAuthor() method in a **"Black Box"** with our encapsulation.

## DRY - Don't Repeat Yourself

Use variables for hard-coded values that repeat but need to be the same value even if it changes.
Use methods to 

## Liskov Substitution Principle

"Subtypes must be substitutable for their base type."

While you would think a square is a rectangle, squares behave differently than rectangles.

## Errors

We **DON'T** want to check for every possible case in the history of cases. We will spend so much time that we don't get much done.

Now this might come as a surprise, but let's explore this topic.

There are 3 approaches:

1. One is to assume that the client objects will know what they are doing and will request services in a sensible/well-defined way.

2. The second is to assume that server objects will operate in an essentially problematic environment in which all possible steps should be taken to prevent them being used incorrectly.

3. The third is to assume an intentionally hostile client who is trying to break or find weakness in the server.

In practice, most client requests are reasonable, with the occasional invalid call.
How much error checking should we do?

What you want to do is protect your code from invalid data coming from the "*outside*", wherever you decide that "*outside*" is.

 - You want to protect your code from anything that's external.
 - Establish where you draw your "trust boundaries", where everything outside of that boundary is potentially dangerous
 - Validate all input data at that barricade/boundary
 - After checking for bad data, decide how to handle it. We want it to be robust but also correct.

Robust, meaning it keeps running, and correct, meaning it would never return inaccurate results.

Possible ways to handle bad data:

 - Return an error and stop immediately(fast fail)
 - Return a neutral value
 - Substitute data values

"Look at the context, and develop a strategy." - Myers

If you are telling a car to brake and it is pressed at a higher value than the computer can handle, you would want the car to be less "**correct**" and more "**robust**" by telling the computer to just brake as if it were the max "brake pressed" value.

Watch out, and don't use exception handling any more than you **absolutely** need to.

Also, `throws` and `throw` are not the same:

 - `throws` goes in the method header as an exception that might be thrown during the code's execution
 - `throw` puts your code in an error state, throwing the exception right there

# JavaFX

The overall window is called a **stage**. Inside of a stage is a **scene**(containing user interface controls), and inside of the scene is a **root node**, which is generally a layout pane.

Remember, for Myers, you want to keep your `start()` method short, just like the `main()` method.

## How does a GUI work?

1. You will have to use the Application.launch() method.
2. Next the `init()` method will run. (allows you to run code before the UI displays)
3. The `start()` method in Application is abstract, which forces you to override it. This is where most of your work will be done.
4. Wait for the application to end. (the application does whatever you told it to do)
5. Lastly, the `stop()` method will run. (allows you to do post-processing, such as saving files or data; very similar to init())

GUI's run on threads. There can be multiple at the same time, which is how you do parallel processing.
Using a logger periodically to state which method we are in is a good practice.

Anything you want to do with your GUI, just look it up. Someone else will have already done it. Keep in mind you can put panes inside of panes.

If you want a part of the UI to be changeable, make it into an instance variable of the class.

Modals force you to deal with the modal till it closes before you can go back to using the rest of your program.

Myers has good resources for you to play around with JavaFX.

# Streams

They're useful and can allow you to handle collections quickly.

`stream()` will handle each item sequentially.

`parallelStream()` allows for parallel processing of every item in a collection.

Streams are **never** reusable.

## Mapping and flatMapping

Mapping is how you change one type into another in a stream.
`flatMapping` is how you change the collection to the contents of the stream.
It is something akin to an assignment operator for the stream, 
where you set the contents of the stream by having a function return streams for you.

`map()` can transform the data however we want. 


We can pass a method into the map function for it to use like so:

```java
//local libraries is an ArrayList
localLibraries.parallelStream() //parallel processes each library
              .map(Library::getBookCollection) // transforms each library into a collection of their books
              .flatMap(books -> books.values().parallelStream()) // flatMap() replaces what's currently in the stream with the contents returned from this parallelStream()
              // i.e. if each library had 1000 books and there were 4 libraries, you would now have 4000 books in the stream.
              .filter(book -> book.getAuthor().equals("J.K. Rowling")) // filters out using a predicate that will only keep J.K. Rowling books
              .forEach(book -> System.out.println(book));
```

## Functions to remember

|name|quick description|
|-|-|
|stream()|returns a sequential Stream sourced from a collection|
|parallelStream()|returns a possibly parallel Stream sourced from a collection|
|count()|returns the count of elements in the stream|
|forEach(Consumer<? super T> action)|performs an action for each element of the stream|
|filter(Predicate<? super T> predicate|returns a Stream consisting of the elements of the stream that match the given predicate|
|collect(Collector<? super T,A,R> collector)|performs "mutable reduction operation" using collector(allows you to put the elements into a container)|
|toArray()|returns an array containing the elements of this stream|
|||
|||
|||
|||

## Input and Output

Input and output goes through streams to either a file, a pipe, a network connection, or a memory buffer(a place in the memory holding data to be used, think like when a video or song stops loading).

`System.out.print();` is actually using a stream as well.

Only things that can be opened with notepad are character-based. This includes `.csv` files.

When reading a `FileInputStream`, use a `try-with-resources` block. 
Any object that implements `AutoClosable` can be instantiated in the parentheses of the `try-with-resources` block. 
This is useful to automatically close a stream, especially for things like IO where some streams could be left open if they are not closed.

When writing to a file, there are two constructors.
The type with just one parameter will overwrite the original file.
The type with two allows for appending as well as overwriting.

## Latches

If we wanted to wait for threads to finish before executing code, use a `CountDownLatch`.

There is a counter that you set in the constructor of `CountDownLatch`.
The instance's `countDown()` method will decrease the count on the `CountDownLatch`.
The `await()` method of this instance is waiting for that count to be 0; it will end when `countDown()` was called enough times to decrease count to 0.
Nice and easy!

## Futures

Think of it like a promise to return an object.
If you use `get()`, the thread will wait until the Future object finishes its task.

When your task is one-and-done, use an object of type `Future`.
When you want to use your result for a second task or more, use a `CompletableFuture`.

# Whiteboard Problems

There are 140 types of ducks, but only 12 ways to quack. What inheritance structure would you use?

Duck is an abstract class that has a variable of type QuackBehavior, 
with subclasses of the different continents that the ducks originated in.
When the duck wants to quack, Duck `delegates` the job of quacking to the QuackBehavior.

```java
quackBehavior.quack(); //delegation aka the strategy pattern
```

---

We only want one object of this type to ever exist at one time. 
We have a private constructor and static object of the same type.
There is a public static method that allows us to access that object.

---

If we have data that is changing and we need to update the data:
We keep a List of the Monitor interface that we loop through and call its update() method.
The update() method would change the value to the new value.

---

Every person has a different membership state which can be swapped.
It passes itself as a parameter.

---

There is also a decorator pattern where we just hold an object and do the same things but better.

"Anything you can do, I can do better" - that one song Myers played every time we talked about this.

---

MVC Pattern:
It is Model, View, and Control. Your classes should fall into one of these categories.

Model is the objects you are actually trying to model.

The view is the GUI of application.

The controller handles the logic.

