<!-- .slide: data-state="main" data-background="../../images/master.png" data-background-size="contain" -->
# Lambda Expression <!-- .element: style="text-align: center;font-size: 2em;" -->
###  Part 2

---

### The @FunctionalInterface Annotation
###### Review: @Override

```java
public class MyCoolClass {
@Override
public String tostring() { ... }
}
```

- Correct code will work with or without @Override, but @Override still useful
- Catches errors at compile time
  - Real method is toString, not tostring
- Expresses design intent
  - Tells fellow developers this is a method that came from parent class, so API for Object will describe how it is used

---

###### New: @FunctionalInterface

```java
@FunctionalInterface
public interface Integrable {
double eval(double x);
}
```

- Catches errors at compile time
  - If developer later adds a second abstract method, interface will not compile
- Expresses design intent
  - Tells fellow developers that this is interface that you expect lambdas to be used for
- But, like @Override not technically required
  - You can use lambdas anywhere 1-abstract-method interfaces (aka functional
  interfaces, SAM interfaces) are expected, whether or not that interface used
  @FunctionalInterface

---

### Method References <!-- .element: style="text-align: center;font-size: 2em;" -->
###### Basic Method References

```java
(args) -> ClassName.staticMethodName(args)
```

Replace by

```java
ClassName::staticMethodName
```

E.g., Math::cos, Arrays::sort, String::valueOf


- Another way of saying this is that if the function you want to describe already has a name,
you don’t have to write a lambda for it, but can instead just use the method name
- The signature of the method you refer to must match signature of the method in functional
(SAM) interface to which it is assigned

---


### Basic Method References

```java
/**
 * Using Lambda
 */
MathUtilities.integrationTest(x -> Math.sin(x), 0, Math.PI);
MathUtilities.integrationTest(x -> Math.exp(x), 2, 20);

/**
 * Using Method Ref
 */
MathUtilities.integrationTest(Math::sin, 0, Math.PI);
MathUtilities.integrationTest(Math::exp, 2, 20);
```

- Other method references described later
  - variable::instanceMethod (e.g., System.out::println)
  - Class::instanceMethod (e.g., String::toUpperCase)
  - ClassOrType::new (e.g., String[]::new)

---

### The Type of Method References

- Question: what is type of Math::sin?
  - Double? Function? Math?
- Answer: can determine from context only
  - We can answer this the same way we answer any question about the type of an argument to a method: by looking at the API.

---

### The Type of Lambdas or Method References

```java
/**
 * Interfaces (like Java 7)
 */
- public interface Foo { double method1(double d); }
- public interface Bar { double method2(double d); }
- public interface Baz { double method3(double d); }
/**
 * Methods that use the interfaces (like Java 7)
 */
- public void blah1(Foo f) { … f.method1(…)… }
- public void blah2(Bar b) { … b.method2(…)… }
- public void blah3(Baz b) { … b.method3(…)… }
/**
 * Calling the methods (use lambda or method references)
 */
- blah1(Math::cos) or blah1(d -> Math.cos(d))
- blah2(Math::cos) or blah2(d -> Math.cos(d))
- blah3(Math::cos) or blah3(d -> Math.cos(d))
// All the above could also use Math::sin, Math::log, Math::sqrt, Math::abs, etc.
```

---

### Importance of Using Method References

**Low!**
- If you do not understand method references, you can always use explicit lambdas
  - Replace foo(Math::cos) with foo(d -> Math.cos(d))

**But method references are popular**
- More succinct
- Familiar to developers from several other languages, where you can refer directly to
  existing functions. E.g., in JavaScript
```javascript
function square(x) { return(x*x); }
var f = square;
f(10); // 100
```

---

### The Four Kinds of Method References

Method Ref Type | Example | Equivalent Lambda
--------------- | ------- | -----------------
SomeClass::staticMethod | Math::cos | x -> Math.cos(x)
someObject::instanceMethod | someString::toUpperCase | () -> someString.toUpperCase()
SomeClass::instanceMethod | String::toUpperCase | s -> s.toUpperCase()
SomeClass::new | Employee::new | () -> new Employee()

---

### var::instanceMethod vs. Class::instanceMethod

- someObject::instanceMethod
  - Produces a lambda that takes exactly as many arguments as the method expects.

```java
String test = "PREFIX:";
String result1 = transform(someString, test::concat);
/**
 * The concat method takes one arg
 * This lambda takes one arg, passing s as argument to test.concat
 * Equivalent lambda is s -> test.concat(s)
 */
```

---

###### var::instanceMethod vs. Class::instanceMethod

- SomeClass::instanceMethod
  - Produces a lambda that takes one more argument than the method expects. The first
argument is the object on which the method is called; the rest of the arguments are
the parameters to the method.

```java
String result2= transform(someString, String::toUpperCase);
/**
 * The toUpperCase method takes zero args
 * This lambda takes one arg, invoking toUpperCase on that argument
 * Equivalent lambda is s -> s.toUpperCase()
 */
```

---

### Method Reference Examples

Helper Interface
```java
@FunctionalInterface
public interface StringFunction {
  String applyFunction(String s);
}
```

Halper Class
```java
public class Utils {
  public static String transform(String s, StringFunction f) {
    return(f.applyFunction(s));
  }
  public static String makeExciting(String s) {
  return(s + "!!");
  }
  private Utils() {}
}
```

---

###### Method Reference Examples

Testing Code
```java
public static void main(String[] args) {
  String s = "Test";
  // SomeClass::staticMethod
  String result1 = Utils.transform(s, Utils::makeExciting);
  System.out.println(result1); // Test!!
  // someObject::instanceMethod
  String prefix = "Blah";
  String result2 = Utils.transform(s, prefix::concat);
  System.out.println(result2); // BlahTest
  // SomeClass::instanceMethod
  String result3 = Utils.transform(s, String::toUpperCase);
  System.out.println(result3); // TEST
}
```

---

### Constructor References

In Java 7, difficult to randomly choose which class to create
- Suppose you are populating an array of random shapes, and sometimes you want a
Circle, sometimes a Square, and sometimes a Rectangle
- It requires tedious code to do this, since constructors cannot be bound to variables

In Java 8, this is simple
- Make array of constructor references and choose one at random
- This will be more clear once we introduce the Supplier type, which can refer to a
constructor reference

---


### Making Random Person

```java
private final static Supplier[] peopleGenerators = {
  Person::new, Writer::new, Artist::new, Consultant::new, EmployeeSamples::randomEmployee,
  () -> {
      Writer w = new Writer();
      w.setFirstName("Ernest");
      w.setLastName("Hemingway");
      w.setBookType(Writer.BookType.FICTION);
      return(w);
  }
};

public static Person randomPerson() {
  Supplier<Person> generator = RandomUtils.randomElement(peopleGenerators);
  return(generator.get());
}

```

---

### Array Constructor References

- Will soon see how to turn Stream into array

```java
Employee[] employees = employeeStream.toArray(Employee[]::new);
```

- This is a special case of a constructor ref
  - It takes an int as an argument, so you are calling
“new Employee[n]” behind the scenes. This builds an empty Employee array, and
then toArray fills in the array with the elements of the Stream
- Most general form
  - toArray takes a lambda or method reference to anything that takes an int as an
argument and produces an array of the right type and right length
    - That array will then be filled in by toArray


---

### Main Points

- Lambdas are lexically scoped
  - They do not introduce a new level of scoping
- Implications
  - The “this” variable refers to the outer class, not to the anonymous inner class that the lambda is turned into
  - There is no “OuterClass.this” variable
- Unless lambda is inside a normal inner class
  - Lambdas cannot introduce “new” variables with same name as variables in method that creates the lambda
    - However, lambdas can refer to (but not modify) local variables from the surrounding method
  - Lambdas can still refer to (and modify) instance variables from the surrounding
class

---

### Examples

```java
// Illegal: repeated variable name
double x = 1.2;
someMethod(x -> doSomethingWith(x));
// Illegal: repeated variable name
double x = 1.2;
someMethod(y -> { double x = 3.4; … });
// Illegal: lambda modifying local var from the outside
double x = 1.2;
someMethod(y -> x = 3.4);
// Legal: modifying instance variable
private double x = 1.2;
public void foo() { someMethod(y -> x = 3.4); }
// Legal: local name matching instance variable name
private double x = 1.2;
public void bar() { someMethod(x -> x + this.x); }
```

---

### Effectively Final Local Variables

- Lambdas can refer to local variables that are not declared final
(but are never modified)
  - This is known as “effectively final” – variables where it would have been legal to
declare them final
  - You can still refer to mutable instance variables
    - “this” in a lambda refers to main class, not inner class that was created for the lambda
    - There is no OuterClass.this.

```java
// With explicit declaration (explicitly final)
final String s = "...";
doSomething(someArg -> use(s));
// Effectively final (without explicit declaration)
String s = "...";
doSomething(someArg -> use(s));
```

- Note the rule where the use of “final” is optional also applies in Java 8 to anonymous
inner classes

---

### Example

```java
for (int i = 0; i < n; i++) {
    new Thread(() -> System.out.println(i)).start();
        // Error—cannot capture i
}
```

The lambda expression tries to capture i, but this is not legal because i changes. There is no single value to capture. The rule is that a lambda expression can only access local variables from an enclosing scope that are effectively final. An effectively final variable is never modified—it either is or could be declared as final.

```java
for (String arg : args) {
    new Thread(() -> System.out.println(arg)).start();
        // OK to capture arg
}
```

A new variable arg is created in each iteration and assigned the next value from the args array. In contrast, the scope of the variable i in the preceding example was the entire loop.

---

### Summary

- @FunctionalInterface
  - Use for all interfaces that will permanently have only a single abstract method
- Method references

```java
arg -> Class.method(arg)
```

Replace by

```java
Class::method
```

- Variable scoping rules
  - Lambdas do not introduce a new scoping level
  - “this” always refers to main class

---

###### Summary

- Effectively final local variables
  - Lambdas can refer to, but not modify, local variables from the surrounding method
  - These variables need not be explicitly declared final as in Java 7
  - This rule (cannot modify the local variables but they do not need to be declared
final) applies also to anonymous inner classes in Java 8