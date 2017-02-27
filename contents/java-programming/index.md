<!-- .slide: data-state="main" data-background="../../images/master.png" data-background-size="contain" -->
# Java Programming <!-- .element: style="text-align: center;font-size: 2em;" -->

---

# Characteristics

- Simple, Object Oriented, and Familiar
- Robust and Secure
- Architecture Neutral and Portable
- High Performance
- Interpreted, Threaded, and Dynamic

![alt text](images/getStarted-compiler.gif "Java Compiler")

Note:
Simple, no need to allocate and deallocate memory, familiar for C++ devs
Extensive compile-time checking, and run-time checking for security
JVM optimization
Static compile time, Dynamic run-time

---

# Platform

> A platform is the hardware or software environment in which a program runs.

- Java Virtual Machine (JVM)
- Java Application Programming Interface (API)

![alt text](images/getStarted-jvm.gif "Java Platform")
<small>The Java platform can be a bit slower than native code due to its platform-independent environment. However Java provides just in time (JIT) compilation and optimization which bring performance close to native code.</small>

---

# "Hello World!"

- Code <!-- .element: class="fa-code" -->

```java
package com.mitrais.app;

/**
 * The HelloWorldApp class implements an application that
 * simply prints "Hello World!" to standard output.
 */
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}
```

- Compile and Run <!-- .element: class="fa-code" -->

```bash
javac HelloWorld.java
java -cp . HelloWorld
Hello World!
```

Note:
Discuss about comments syntax, class definition, main method, usage of core library System.out