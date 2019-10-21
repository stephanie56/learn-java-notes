# Chapter 1: Java Fundamentals

(Book: Java a Beginner’s Guide Sixth Edition)

## Why Java is portable, can work on any environment?

The key that allows Java to solve both the security and the portability problems is that the output of a Java compiler is not executable code, but instead is bytecode. Translate a java program into byte code makes it easier to run a program in a wide variety of environments because only the JVM needs to be implemented for each platform. Once the run-time package exists for a given system, any java program can run on it.

Java code -> Java Compiler -> Bytecode -> JVM (Java run-time system, like an interpreter) -> native code

Just-in-time (JIT) compiler: selected portions of byte code are compiled into executable code in real time on a piece-by-piece, demand basis.

Ahead-of-time (AOT) compiler: ???

## Java Development Kit (JDK) includes:

- javac: the Java compiler
- java: the standard Java interpreter (the application launcher)

## Entering the Program: Java file name convention

- The java compiler requires that a source file use the .java extension
- The name of the main class should match the name of the file that holds the program
- The capitalization of the filename should match the class name (Java is case sensitive)

## Compiling the program

- To compile a java program, execute the compiler, javac: `javac Example.java`
- The compiler creates a file called Example.class that contains the bytecode version of the program
- To actually run the program, use the Java interpreter, java: `java Example`

## Servlet VS Applet (Page 30 & 32)

They are a couple on the two different ends of the client/server equation.

- Applet is similar to javascript - it’s a small program that is downloaded on demand and executed automatically by a web browser. It is typically used to display data provided by the server, handle user input, or provide simple functions on the client side.
- A servlet, on the other hand, is a small program that executes on a server. Just as applets dynamically extend the functionality of a web browser, servlets dynamically extend the functionality of a web server.

## OOP VS Functional Programming

- FP: the program is organized around its code - “code acting on data.” Support for stand-alone functions, local variables and etc.
- OOP: the program is organized around its data - “data controlling access to code.” You define the data and the methods that are permitted to act on that data. Thus, a data type defines precisely what sort of operation can be applied to that data

## Three Traits of OOP:

- Encapsulation
- Polymorphism
- Inheritance

### Encapsulation

Encapsulation is a programming mechanism that binds together code and the data it manipulates.

1. Necessary data and code are linked together within a self-contained block box (an object)
2. Member variable and methods can be either private or public
3. Private code and data is known by and accessible by only another parts of the object; cannot be accessed by a piece of program that exists outside the object.
4. When code and data is public, they are accessible by programs exist outside of the object. Typically, the public parts of an object are used to provide a controlled interface to the private elements of the object.
5. The basic unit of encapsulation is the class. A class is essentially a set of plans that specify how to build an object. Java use a class specification to construct objects. Objects are instances of a class.

### Polymorphism ??

1. One interface, multiple methods
2. It’s possible to design a generic interface to a group of related activities
3. Real life example: the steering wheel (the interface) is the same no matter what type of actual steering mechanism is used (whether your car has manual steering or power steering, once you know how to operate the steering wheel, you can drive any type of car)

### Inheritance

1. One object can acquire the properties of another object
2. Without inheritance, each object would have to define all of its characteristics
3. Using inheritance, an object need only define those qualities that make it unique. It can inherit its general attributes from its parent.

## ABC Of `public static void main (String args[])`

- All java applications begin execution by calling the main() method within the bootstrapping class
- Public: access modifier. The main() must be declared as public, since it must be called by code outside of its class when the program is started
- Static: allows main() to be called before an object of the class has been created (it would be called before constructor() of any objects); this is necessary because main() is called by the JVM before any objects are made
- Void: return type of main(); tells the compiler that main() does not return a value.
- String args[]: declares a parameter named args; this is an array of objects of type String. In this case, args receives any command-line arguments present when the program is executed
- System.out.println: System is a predefined class that provides access to the system. Out is the output stream that is connected to the console. System.out is an object that encapsulates console output.

## Primitive Data Types

- Whole number: int (the size of a medium size coffee cup)
- Fractional number: float, double (the size of a large size coffee cup)
- If an int is assigned to a double, there is no loss on the value of the int; however, if a double is assigned to an int type, the value of the double is loss (e.g. from 1.4 to 1)
- The Why:

1. Speed: integer arithmetic is faster than floating-point calculations, so, if you don’t need fractional values, then you don’t need to incur the overhead associated with types float or double
2. Space: the amount of memory required for one type of data might be less than that required for another. By supplying different types, Java enables you to make best use of system resource
