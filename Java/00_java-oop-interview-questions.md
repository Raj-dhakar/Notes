# Java OOP Interview Questions and Answers

Based on the C-DAC PG-DAC syllabus "Object Oriented Programming with Java" (Sessions 1–30).

**How to use this file**

- Questions follow the syllabus order, one section per session block.
- Each section is split into **Basics**, **Deep dive** (internals, why/how, trade-offs) and **Code and scenario** (predict the output, spot the bug, write the code).
- Every question is in bold, followed by an answer short enough to say aloud in an interview. Add your own project example where you can.
- A ⭐ marks questions that come up very often.
- Version notes (for example "Java 21+") tell you where behaviour changed; mention them only if the interviewer goes there.

---

## Table of contents

1. [Introduction to Java (Session 1)](#1-introduction-to-java-session-1)
2. [Basic programming concepts (Sessions 2–3)](#2-basic-programming-concepts-sessions-23)
3. [OOP concepts (Session 4)](#3-oop-concepts-session-4)
4. [Static members and reference types (Session 5)](#4-static-members-and-reference-types-session-5)
5. [Constructors, parameter passing, stack and heap (Session 6)](#5-constructors-parameter-passing-stack-and-heap-session-6)
6. [Inheritance, association and polymorphism (Session 7)](#6-inheritance-association-and-polymorphism-session-7)
7. [Casting, abstract classes and interfaces (Session 8)](#7-casting-abstract-classes-and-interfaces-session-8)
8. [final, functional interfaces, lambdas, inner classes, enums (Sessions 9–10)](#8-final-functional-interfaces-lambdas-inner-classes-enums-sessions-910)
9. [Access modifiers and packages (Session 11)](#9-access-modifiers-and-packages-session-11)
10. [Garbage collection (Session 12)](#10-garbage-collection-session-12)
11. [Wrapper classes and Strings (Sessions 13–14)](#11-wrapper-classes-and-strings-sessions-1314)
12. [Exception handling (Sessions 15–16)](#12-exception-handling-sessions-1516)
13. [java.io, java.nio, serialization, cloning (Session 17)](#13-javaio-javanio-serialization-cloning-session-17)
14. [Object class and date/time APIs (Session 18)](#14-object-class-and-datetime-apis-session-18)
15. [Collections: List, Queue, Comparable, Comparator (Sessions 19–22)](#15-collections-list-queue-comparable-comparator-sessions-1922)
16. [Collections: Set, Map, concurrent collections, Streams (Sessions 23–26)](#16-collections-set-map-concurrent-collections-streams-sessions-2326)
17. [Multithreading basics (Session 27)](#17-multithreading-basics-session-27)
18. [Synchronization and inter-thread communication (Sessions 28–29)](#18-synchronization-and-inter-thread-communication-sessions-2829)
19. [Generics and Reflection (Session 30)](#19-generics-and-reflection-session-30)
20. [Cross-topic and design questions](#20-cross-topic-and-design-questions)

---

## 1. Introduction to Java (Session 1)

### Basics

- ⭐ **What is Java, and what are its main features?** Java is a high-level, class-based, object-oriented language that compiles to bytecode and runs on the JVM. Its main features are platform independence, object orientation, automatic memory management (garbage collection), strong static typing, built-in multithreading, robustness (exception handling, no pointer arithmetic), security (bytecode verification, class loaders) and a large standard library.
- ⭐ **What is the difference between JDK, JRE and JVM?** The JVM executes bytecode. The JRE is the JVM plus the standard class libraries, enough to run programs. The JDK is the JRE plus development tools such as `javac`, `jar`, `javadoc` and `jdb`, needed to write and compile programs. So JDK ⊃ JRE ⊃ JVM. Since Java 11 there is no separate JRE download.
- ⭐ **Why is Java called platform independent? Is the JVM platform independent?** `javac` compiles source to bytecode, which is the same on every OS. Each platform has its own JVM that translates that bytecode to native instructions. So the bytecode is platform independent, but the JVM itself is platform dependent.
- **What is bytecode? Who produces it and who consumes it?** Bytecode is the intermediate instruction set stored in `.class` files. `javac` produces it; the JVM (interpreter and JIT compiler) consumes it.
- **Is Java a pure object-oriented language? Justify your answer.** No. It has eight primitive types that are not objects, and static members that belong to classes rather than objects. Wrapper classes exist, but primitives are still used directly.
- **What does "write once, run anywhere" mean?** Code compiled once to bytecode runs unchanged on any machine that has a compatible JVM, with no recompilation.
- ⭐ **Explain each word in `public static void main(String[] args)`.** `public`: the JVM can call it from outside the class. `static`: it can be called without creating an object. `void`: it returns nothing to the JVM. `main`: the name the launcher looks for. `String[] args`: command-line arguments.
- **Can `main` be overloaded? Can it be declared `final`, `synchronized` or `strictfp`?** Yes to all. You can overload `main`, but the JVM only calls the `String[]` version. `final`, `synchronized` and `strictfp` are all legal on `main`.
- **What happens if `main` is not `static`, or not `public`?** Traditionally the code compiles but fails at launch with an error that the main method was not found or is not static. From Java 21 (preview) and Java 25 (final), instance main methods are allowed: `main` may be non-static and non-private, and the launcher creates an instance to call it.
- **Can a Java source file have more than one class? More than one public class?** It can have many classes, but at most one public top-level class.
- **What must the file name be if the file has a public class? What if it has none?** The file name must match the public class name. With no public class, any valid name works; the compiler still produces one `.class` file per class.
- **What are the eight primitive data types, with their sizes and default values?** `byte` 1 byte (0), `short` 2 (0), `int` 4 (0), `long` 8 (0L), `float` 4 (0.0f), `double` 8 (0.0d), `char` 2 (`'\u0000'`), `boolean` (false). Defaults apply to fields and array elements, not local variables.
- **Why is the size of `boolean` not precisely defined?** The JVM specification only says it represents one bit of information. In practice HotSpot uses 1 byte per element in a `boolean[]` and treats a standalone boolean as an `int` on the operand stack.
- **Why is `char` 2 bytes in Java but 1 byte in C/C++?** Java uses Unicode (UTF-16 code units) so it can represent characters of all languages; C/C++ `char` was designed around ASCII.
- **What is the range of `byte`, `short`, `int` and `long`?** `byte`: −128 to 127. `short`: −32,768 to 32,767. `int`: −2³¹ to 2³¹−1 (about ±2.1 billion). `long`: −2⁶³ to 2⁶³−1.
- **What tools ship with the JDK? What does each do?** `javac` compiles source to bytecode; `java` launches the JVM; `javap` disassembles class files; `jar` packages classes into archives; `javadoc` generates API docs; `jdb` is a command-line debugger; `jshell` is the interactive REPL (Java 9+).
- **What is the difference between `PATH` and `CLASSPATH`?** `PATH` is an OS variable that tells the shell where to find executables such as `java` and `javac`. `CLASSPATH` tells the JVM and compiler where to find user classes and JARs.

### Deep dive

- ⭐ **Explain the JVM architecture.** Three parts. The class loader subsystem loads, links and initializes classes. The runtime data areas hold the method area, heap, per-thread stacks, PC registers and native method stacks. The execution engine has the interpreter, the JIT compiler and the garbage collector. Native code is reached through JNI.
- ⭐ **What are the runtime memory areas of the JVM?** Method area (Metaspace): class metadata, static variables, the constant pool; shared. Heap: all objects and arrays; shared and garbage collected. Stack: one per thread, holding frames with local variables and partial results. PC register: per thread, the address of the current instruction. Native method stack: per thread, for native calls.
- **What are the phases of class loading?** Loading reads the `.class` bytes and creates a `Class` object. Linking has three steps: verify (bytecode is checked for safety), prepare (static fields get default values), resolve (symbolic references become direct references). Initialization runs static initializers and static blocks in textual order.
- **What are the bootstrap, platform (extension) and application class loaders?** Bootstrap loads core classes (`java.base`), is written in native code and appears as `null`. The platform loader (called extension before Java 9) loads other JDK modules. The application loader loads classes from the classpath.
- **What is the parent delegation model, and why does it exist?** A class loader asks its parent to load a class first and only tries itself if the parent fails. This prevents core classes from being replaced by user classes of the same name and avoids loading a class twice.
- **What is the difference between `ClassNotFoundException` and `NoClassDefFoundError`?** `ClassNotFoundException` is a checked exception thrown when code loads a class by name at runtime (`Class.forName`) and it is not on the classpath. `NoClassDefFoundError` is an error thrown when a class was present at compile time but is missing at runtime, or its static initialization failed earlier.
- ⭐ **What is the JIT compiler? How does it differ from the interpreter?** The interpreter executes bytecode one instruction at a time, which starts quickly but runs slowly. The JIT detects frequently executed ("hot") methods and compiles them to optimized native code that is cached and reused, so long-running code gets much faster.
- **What is HotSpot? What are C1 and C2 compilers and tiered compilation?** HotSpot is the standard OpenJDK JVM. C1 (client) compiles quickly with light optimization; C2 (server) compiles slowly with aggressive optimization. Tiered compilation starts in the interpreter, moves hot code to C1 with profiling, then to C2.
- **Is Java compiled or interpreted?** Both. Source is compiled to bytecode by `javac`, and the JVM interprets bytecode and JIT-compiles hot parts to native code.
- **What replaced PermGen in Java 8, and why?** Metaspace. PermGen was a fixed-size region inside the heap that often caused `OutOfMemoryError: PermGen space`. Metaspace lives in native memory and grows automatically by default.
- **What does the bytecode verifier check?** That the class file is well formed, types are used consistently, the operand stack does not overflow or underflow, no illegal casts or jumps occur, and final classes and methods are not overridden.
- **How does Java achieve security and robustness compared with C++?** No pointer arithmetic, array bounds checking, automatic garbage collection, strong type checking, mandatory handling of checked exceptions, bytecode verification and class loader isolation.
- **Why does Java not support pointers? Does it have them internally?** Explicit pointers allow arbitrary memory access, which is unsafe and makes garbage collection hard. Internally references are pointers (or compressed pointers), but you cannot do arithmetic on them or see the address.
- **Why are floating-point comparisons such as `0.1 + 0.2 == 0.3` false? What should you use for money?** `float` and `double` use binary IEEE 754, which cannot represent most decimal fractions exactly, so small rounding errors appear. Use `BigDecimal` (constructed from a `String`) or store the smallest currency unit in a `long`.
- **What happens on integer overflow in Java?** It wraps around silently using two's complement; no exception is thrown. Use `Math.addExact` and similar methods to get an `ArithmeticException` instead.
- **What is `var` (Java 10)? Where can it not be used?** Local variable type inference: the compiler infers the type from the initializer. It cannot be used for fields, method parameters, return types, variables without an initializer, or variables initialized to `null`.
- **What changed about `main` in recent Java versions?** Java 21 previewed, and Java 25 finalized, instance main methods and compact source files: you can write `void main() { }` without `public static`, without `String[] args`, and even without declaring a class.

### Code and scenario

- **What is printed? `System.out.println(Integer.MAX_VALUE + 1);`** `-2147483648`. The addition overflows and wraps to `Integer.MIN_VALUE`.
- **What is printed? `System.out.println('a' + 'b');` and `System.out.println("" + 'a' + 'b');`** `195` and `ab`. Adding two chars promotes them to `int` (97 + 98). Starting with a `String` makes it concatenation.
- **What is printed? `10 / 3`, `10 / 3.0`, `10 % -3`** `3`, `3.3333333333333335`, `1`. Integer division truncates; a double operand makes it floating-point division; the sign of `%` follows the left operand.
- **Does `float f = 3.14;` compile? Does `long l = 12345678901;` compile?** Neither. `3.14` is a `double` literal, so it needs `3.14f`. `12345678901` is too large for an `int` literal, so it needs the `L` suffix.
- **You run `java Hello` and get "Could not find or load main class". List the possible causes.** Wrong class name or case, passing `Hello.class` instead of `Hello`, the class is in a package and you did not use the fully qualified name from the root directory, the classpath does not include the directory or JAR, or the file was never compiled.
- **A class has no `main` but has a static block. What happens when you run it on Java 6 versus Java 8+?** On Java 6 the class is loaded first, so the static block runs and then a `NoSuchMethodError` for `main` appears. From Java 7 the launcher checks for `main` before initializing the class, so you get an error and the static block never runs.

---

## 2. Basic programming concepts (Sessions 2–3)

### Basics

- **What are Java tokens? Name the categories.** Tokens are the smallest meaningful units of a program: keywords, identifiers, literals, operators and separators (such as `;`, `{}`, `()`).
- **What are the rules for identifiers? Is `$` or `_` allowed?** An identifier may contain letters, digits, `$` and `_`, cannot start with a digit, cannot be a keyword, and is case sensitive. `$` and `_` are allowed, but a single `_` alone has been a keyword since Java 9.
- **How many keywords does Java have? Are `true`, `false` and `null` keywords?** About 50 reserved keywords, plus contextual ones such as `var`, `record`, `sealed` and `yield` in newer versions. `true`, `false` and `null` are literals, not keywords, but you still cannot use them as identifiers.
- **Are `goto` and `const` keywords? Can you use them?** They are reserved keywords with no function. You cannot use them as identifiers or for any purpose.
- **What kinds of variables exist in Java? What are their default values and scopes?** Local variables live inside a method or block, have no default value and exist on the stack. Instance variables belong to each object, get default values and live on the heap. Static variables belong to the class, get default values and exist once per class.
- ⭐ **Why must local variables be initialized before use while instance variables need not be?** The compiler can track every path to a local variable and rejects reading an unassigned one, which catches bugs. Fields can be assigned from many methods in any order, so the compiler cannot track them, and the JVM zeroes object memory on allocation.
- ⭐ **What is the difference between widening and narrowing conversion?** Widening goes from a smaller type to a larger one (`int` to `long`) and happens automatically. Narrowing goes from larger to smaller (`double` to `int`), may lose data and needs an explicit cast.
- **What is type promotion in expressions?** In arithmetic, `byte`, `short` and `char` are promoted to `int`. If either operand is `long`, `float` or `double`, the other is promoted to that type. So `byte + byte` gives an `int`.
- **What is the difference between `==` and `equals()`?** `==` compares primitive values, or for objects compares references (same object). `equals()` compares content when a class overrides it; the default in `Object` behaves like `==`.
- **What is the difference between `&` and `&&`, `|` and `||`?** `&&` and `||` are short-circuit logical operators that skip the right operand when the result is already known. `&` and `|` always evaluate both sides and also work as bitwise operators on integers.
- **What is the difference between `>>` and `>>>`? Why is there no `<<<`?** `>>` is a signed right shift that fills with the sign bit. `>>>` is an unsigned right shift that fills with zeros. Left shift always fills with zeros on the right, so signed and unsigned would be identical.
- **What is the difference between `i++` and `++i`?** Both increment `i`. `i++` evaluates to the old value; `++i` evaluates to the new value.
- **What is the ternary operator? What is the type of its result when operand types differ?** `condition ? a : b` returns `a` if the condition is true, otherwise `b`. With mixed numeric operands the result is promoted, so `true ? 1 : 2.0` gives `1.0`. Mixing a wrapper and a primitive causes unboxing, which can throw a `NullPointerException`.
- **What does `instanceof` do? What does it return for `null`?** It tests whether an object is an instance of a class or interface (or a subtype). It returns `false` for `null`.
- **What types are allowed in a `switch` expression? Can you switch on `long`, `float` or `boolean`?** `byte`, `short`, `char`, `int`, their wrappers, enums and `String` (Java 7+); Java 21 adds type patterns. Classic `switch` does not accept `long`, `float`, `double` or `boolean`.
- **What is fall-through in `switch`? How do switch expressions (Java 14) change this?** Without `break`, execution continues into the next case. Switch expressions with `case X ->` have no fall-through, can return a value, and must be exhaustive.
- **What is the difference between `break`, `continue` and `return`? What are labeled breaks?** `break` exits the nearest loop or switch; `continue` skips to the next iteration; `return` exits the method. A labeled `break outer;` exits the loop marked with that label, which is useful for nested loops.
- **What is the difference between `while` and `do-while`?** `while` checks the condition first and may run zero times. `do-while` checks after the body and always runs at least once.
- **What is the enhanced `for` loop? What are its limitations?** `for (T x : collection)` iterates over arrays and `Iterable` objects. You get no index, cannot iterate backwards or over two collections together, cannot replace array elements through the loop variable, and cannot remove from a collection without an exception.
- ⭐ **How do you declare, create and initialize a 1-D array? What are the default values of its elements?** `int[] a = new int[5];` or `int[] a = {1, 2, 3};`. Elements default to 0 for numeric types, `false` for boolean, `'\u0000'` for char and `null` for references.
- **What is the difference between `length`, `length()` and `size()`?** `length` is a field of arrays. `length()` is a method of `String`. `size()` is a method of collections.
- **What is a jagged array?** A 2-D array whose rows have different lengths, for example `int[][] a = new int[3][]; a[0] = new int[2]; a[1] = new int[5];`.
- **Are arrays objects in Java? What is the superclass of an array?** Yes. Arrays are objects created on the heap, and their superclass is `Object`. They also implement `Cloneable` and `Serializable`.
- **What are varargs? What rules apply to them?** `void m(int... nums)` accepts zero or more arguments, received as an array. A method can have only one varargs parameter and it must be the last parameter.

### Deep dive

- **Why does `byte b = 10; b = b + 1;` fail to compile while `b += 1;` and `b++` compile?** `b + 1` is promoted to `int`, and assigning an `int` to a `byte` needs a cast. Compound assignment and increment operators include an implicit cast back to the variable's type.
- **What is operator precedence and associativity? Evaluate `a = b = c` and `a + b * c - d / e`.** Precedence decides which operator binds first; associativity decides order among equal precedence. Assignment is right-associative, so `a = (b = c)`. The second is `(a + (b * c)) - (d / e)`, with operands evaluated left to right.
- **What is short-circuit evaluation, and where can it cause bugs or prevent them?** `&&` and `||` skip the right side when the left decides the result. It prevents errors in checks such as `obj != null && obj.isValid()`, but it causes bugs if the skipped side has a needed side effect such as `i++` or a method call.
- **How is a 2-D array stored in memory in Java versus C?** In C a 2-D array is one contiguous block. In Java it is an array of references to separate 1-D array objects, which is why rows can have different lengths.
- **What does array covariance mean? What is `ArrayStoreException`?** A `String[]` can be assigned to an `Object[]` variable. If you then store a non-String through that reference, the JVM throws `ArrayStoreException` at runtime because arrays check their element type.
- **What is the difference between `int[] a, b;` and `int a[], b;`?** In the first, both `a` and `b` are arrays. In the second, `a` is an array and `b` is a plain `int`.
- **How do you copy an array? Are these shallow or deep?** `clone()`, `System.arraycopy`, `Arrays.copyOf` and `Arrays.copyOfRange`. All are shallow: for object arrays the references are copied, not the objects.
- **What is the difference between `Arrays.equals` and `Arrays.deepEquals`?** `Arrays.equals` compares elements one level deep using `equals`. `Arrays.deepEquals` compares nested arrays recursively, so use it for multidimensional arrays.
- **Why does `strictfp` exist, and is it still relevant after Java 17?** It forced IEEE 754 strict floating-point results on every platform. Since Java 17 all floating-point arithmetic is strict by default, so the keyword has no effect.
- **What is the result type of `char + int`, `short + short`, `int + long`, `float + double`?** `int`, `int`, `long`, `double`.
- **What are integer, octal, hex and binary literals? What does the underscore in `1_000_000` do?** Decimal `10`, octal `012` (leading zero), hex `0xA`, binary `0b1010`. Underscores are ignored by the compiler and only improve readability.
- **How does overload resolution pick between widening, boxing and varargs?** In three phases: first exact match or widening, then boxing/unboxing, then varargs. The first phase that finds a match wins, so widening beats boxing and boxing beats varargs.
- **Compare Java and C++ syntax.** Java has references but no pointer arithmetic; garbage collection instead of destructors; no operator overloading; no multiple class inheritance (interfaces instead); generics by type erasure instead of templates; no default arguments, `goto` or header files; everything lives inside a class.

### Code and scenario

- ⭐ **What is printed? `int i = 5; i = i++ + ++i; System.out.println(i);`** `12`. `i++` yields 5 and makes `i` 6; `++i` makes `i` 7 and yields 7; 5 + 7 = 12.
- **What is the value? `int x = 10; x += x++ + ++x;`** `32`. The left `x` is read first as 10; `x++` yields 10 (x becomes 11); `++x` yields 12; 10 + 10 + 12 = 32.
- **What is printed? `System.out.println(1 + 2 + "3" + 4 + 5);`** `3345`. Evaluation is left to right: 1 + 2 = 3, then string concatenation from there on.
- **What is printed? `char c = 'A'; c += 1; System.out.println(c);` What about `c = c + 1;`?** `B`. The compound operator casts back to `char`. `c = c + 1` does not compile because `c + 1` is an `int`.
- **What is printed? `(int) 3.99`, `(byte) 130`, `(int) 'A'`** `3` (truncates), `-126` (130 − 256), `65`.
- **What is printed? `int[] a = {1, 2, 3}; int[] b = a; b[0] = 99; System.out.println(a[0]);`** `99`. `a` and `b` refer to the same array object.
- **What happens with `int[] a = new int[-1];` and with `new int[0]`?** The first compiles but throws `NegativeArraySizeException` at runtime. `new int[0]` is a valid empty array.
- **Write code to print patterns with `*` or numbers.** Use an outer loop for rows and inner loops for spaces and symbols.
  ```java
  // right-angled triangle
  for (int i = 1; i <= n; i++) System.out.println("*".repeat(i));
  // pyramid (reverse the loop for inverted; pyramid + inverted from n-1 = diamond)
  for (int i = 1; i <= n; i++) System.out.println(" ".repeat(n - i) + "*".repeat(2 * i - 1));
  // Floyd's triangle
  for (int i = 1, k = 1; i <= n; i++) { for (int j = 1; j <= i; j++) System.out.print(k++ + " "); System.out.println(); }
  // Pascal's triangle
  for (int i = 0; i < n; i++) { long c = 1; for (int j = 0; j <= i; j++) { System.out.print(c + " "); c = c * (i - j) / (j + 1); } System.out.println(); }
  ```
- **Write code to reverse an array in place, find the second largest element, and rotate an array by k positions.**
  ```java
  static void reverse(int[] a, int l, int r) { while (l < r) { int t = a[l]; a[l++] = a[r]; a[r--] = t; } }
  static int secondLargest(int[] a) {
      int first = Integer.MIN_VALUE, second = Integer.MIN_VALUE;
      for (int x : a) { if (x > first) { second = first; first = x; } else if (x > second && x != first) second = x; }
      return second;
  }
  static void rotateRight(int[] a, int k) { k %= a.length; reverse(a, 0, a.length - 1); reverse(a, 0, k - 1); reverse(a, k, a.length - 1); }
  ```
- **Write code to swap two numbers without a third variable.** `a = a + b; b = a - b; a = a - b;` or with XOR: `a ^= b; b ^= a; a ^= b;`. The XOR version avoids overflow.
- **Write code to check for a prime, a palindrome, an Armstrong number, and to print the Fibonacci series.**
  ```java
  static boolean isPrime(int n) { if (n < 2) return false; for (int i = 2; (long) i * i <= n; i++) if (n % i == 0) return false; return true; }
  static boolean isPalindrome(int n) { int r = 0, t = n; while (t > 0) { r = r * 10 + t % 10; t /= 10; } return r == n; }
  static boolean isArmstrong(int n) { int d = String.valueOf(n).length(), s = 0, t = n; while (t > 0) { s += Math.pow(t % 10, d); t /= 10; } return s == n; }
  static void fib(int n) { int a = 0, b = 1; for (int i = 0; i < n; i++) { System.out.print(a + " "); int c = a + b; a = b; b = c; } }
  ```
- **Write code to multiply two matrices and to transpose a matrix.**
  ```java
  int[][] c = new int[r1][c2];               // requires c1 == r2
  for (int i = 0; i < r1; i++) for (int j = 0; j < c2; j++) for (int k = 0; k < c1; k++) c[i][j] += a[i][k] * b[k][j];
  int[][] t = new int[cols][rows];
  for (int i = 0; i < rows; i++) for (int j = 0; j < cols; j++) t[j][i] = m[i][j];
  ```
- **What is printed by a `switch` on a `String` when the case is missing a `break`? When the value is `null`?** Without `break`, the matched case and every following case run until a `break` or the end. Switching on a `null` String throws `NullPointerException`.
- **Will `for (;;) {}` compile? Will code after `while (true) {}` compile?** `for (;;) {}` compiles and loops forever. A statement after `while (true) {}` with no `break` is a compile error: unreachable code.

---

## 3. OOP concepts (Session 4)

### Basics

- ⭐ **What is object-oriented programming? How does it differ from procedural programming?** OOP organizes a program as objects that bundle data and the behaviour that works on it. Procedural programming organizes it as functions operating on separate, often shared, data. OOP gives better modularity, reuse through inheritance, and data protection through encapsulation.
- ⭐ **What is a class? What is an object? How do they differ?** A class is a blueprint that defines fields and methods; it is a logical entity. An object is an instance of a class created at runtime with its own state in heap memory. One class can produce many objects.
- **What are state, behaviour and identity of an object?** State is the values of its fields. Behaviour is what its methods do. Identity is what makes it distinct from every other object, even one with the same state; in Java that is its reference.
- ⭐ **What are the four pillars of OOP? Explain each with a real-world example.** Encapsulation: a capsule hides medicine; a class hides data behind methods. Abstraction: you drive a car using the steering wheel and pedals without knowing the engine. Inheritance: a child inherits traits from a parent; `Dog extends Animal`. Polymorphism: one person behaves as an employee, parent or customer depending on context; one method call behaves differently depending on the object.
- ⭐ **What is encapsulation? How do you achieve it in Java?** Wrapping data and the methods that operate on it into one unit and restricting direct access to the data. Make fields `private` and expose controlled access through public getters and setters that can validate input.
- ⭐ **What is abstraction? How do you achieve it in Java?** Showing only essential features and hiding implementation details. It is achieved with abstract classes (partial abstraction) and interfaces (full abstraction).
- ⭐ **What is the difference between abstraction and encapsulation?** Abstraction hides complexity at the design level: what an object does, not how. Encapsulation hides data at the implementation level: it protects internal state with access modifiers. Abstraction is about the interface; encapsulation is about information hiding.
- **What is inheritance? What problem does it solve?** A class acquires the fields and methods of another class using `extends`. It gives code reuse, models "is-a" relationships and enables runtime polymorphism.
- **What is polymorphism? What are its types?** One name with many forms. Compile-time polymorphism is method overloading, resolved by the compiler. Runtime polymorphism is method overriding, resolved by the JVM based on the actual object.
- **What is data hiding? Is it the same as encapsulation?** Data hiding means making data inaccessible from outside using `private`. Encapsulation is broader: it bundles data with methods. Data hiding is one result of good encapsulation.
- **What are getters and setters? Why use them instead of public fields?** Methods that read and write a private field. They let you validate input, make a field read-only or write-only, change the internal representation later without breaking callers, and add logging or lazy loading.
- **How many ways can you create an object in Java?** With `new`; with reflection (`Constructor.newInstance()`); with `clone()`; by deserialization; and through factory methods, which use one of these internally.
- **What does the `new` keyword do, step by step?** It loads the class if needed, allocates heap memory for the object, sets fields to default values, runs field initializers and instance blocks, runs the constructor, and returns the reference.
- **What is an anonymous object?** An object created without storing its reference, used once, for example `new Calculator().add(2, 3);`. It becomes eligible for garbage collection right after the statement.
- **What is the difference between an instance variable and a local variable?** An instance variable is declared in a class, lives on the heap with the object, has a default value and can have access modifiers. A local variable is declared in a method, lives on the stack, has no default value and can only be `final`.

### Deep dive

- **What are the advantages and drawbacks of OOP?** Advantages: modularity, reuse, easier maintenance, data security, and it maps well to real-world problems. Drawbacks: more design effort, larger programs, some runtime overhead, and deep inheritance trees can make code rigid.
- **What is the difference between object-oriented and object-based languages?** Object-oriented languages support all OOP features including inheritance and polymorphism (Java, C++). Object-based languages have objects and encapsulation but no class inheritance (early JavaScript, VBScript).
- **What are coupling and cohesion? Which do you want high and which low?** Coupling is how much one class depends on another; cohesion is how closely the responsibilities inside one class belong together. Aim for low coupling and high cohesion.
- **What are the SOLID principles? Give an example of each.** Single responsibility: a class has one reason to change (separate `Invoice` from `InvoicePrinter`). Open/closed: open to extension, closed to modification (add a new `Shape` subclass instead of editing a switch). Liskov substitution: a subclass must be usable wherever the parent is. Interface segregation: many small interfaces rather than one large one. Dependency inversion: depend on abstractions (`List`, not `ArrayList`; inject a `Repository` interface).
- **What is the "is-a" versus "has-a" relationship?** "Is-a" is inheritance: a `Dog` is an `Animal`. "Has-a" is composition or aggregation: a `Car` has an `Engine`.
- **What is a POJO? What is a JavaBean? How do they differ?** A POJO is any plain class with no framework restrictions. A JavaBean is a POJO that follows conventions: a public no-arg constructor, private fields with getters and setters, and usually `Serializable`.
- **What is an immutable object? What does it give you?** An object whose state cannot change after construction, such as `String`. It is thread-safe without synchronization, safe as a map key, safe to share and cache, and easier to reason about.
- **What is the order of initialization when an object is created?** Parent static fields and blocks, then child static fields and blocks (once per class). Then for each object: parent instance fields and blocks, parent constructor, child instance fields and blocks, child constructor.
- **Can a class be empty? What is the size of an object of an empty class?** Yes. On a 64-bit HotSpot JVM with compressed pointers the object is 16 bytes: a 12-byte header padded to an 8-byte boundary.
- **What is the layout of an object in memory?** A header with a mark word (hash code, GC age, lock state) and a class pointer; arrays also store their length. Then the instance fields, then padding to align the size to 8 bytes.
- **How does encapsulation help with maintainability and thread safety?** All access goes through methods, so you can change the implementation in one place. For thread safety, private state can only be touched by the class's own methods, so synchronizing those methods is enough to protect it.
- **Is it good practice to generate getters and setters for every field? Why or why not?** No. Setters for everything expose internal state and break invariants, making the class a data bag. Expose only what callers need, prefer immutability, and offer behaviour methods such as `deposit()` rather than `setBalance()`.

### Code and scenario

- **Design a `BankAccount` class that is properly encapsulated. What validation goes in the setters?**
  ```java
  public class BankAccount {
      private final String accountNo;
      private double balance;
      public BankAccount(String accountNo, double opening) {
          if (opening < 0) throw new IllegalArgumentException("Negative opening balance");
          this.accountNo = accountNo; this.balance = opening;
      }
      public double getBalance() { return balance; }
      public void deposit(double amt) { if (amt <= 0) throw new IllegalArgumentException("Invalid amount"); balance += amt; }
      public void withdraw(double amt) {
          if (amt <= 0 || amt > balance) throw new IllegalArgumentException("Invalid or insufficient amount");
          balance -= amt;
      }
  }
  ```
  There is no `setBalance`; balance changes only through validated `deposit` and `withdraw`.
- **Model a library, parking lot or vending machine: identify classes, attributes, methods and relationships.** For a library: `Book` (isbn, title, author, status), `Member` (id, name, list of loans), `Loan` (book, member, issue and due dates), `Library` (catalogue, members; `issueBook`, `returnBook`, `search`). `Library` aggregates `Book` and `Member`; `Loan` associates a `Book` with a `Member`. Start from the nouns for classes and the verbs for methods.
- **Given a class with public fields, refactor it for encapsulation and explain each change.** Make fields `private` to block direct access; add getters only where reading is needed; add setters only where change is allowed, with validation; make fields that never change `final` and set them in the constructor; return copies of mutable fields.
- **A setter stores a reference to a mutable object passed in by the caller. What can go wrong, and how do you fix it?** The caller keeps the reference and can change the object later, altering your internal state without going through your class. Store a defensive copy (`this.date = new Date(date.getTime())`, `this.list = new ArrayList<>(list)`) and return copies from getters too.
- **Give one code example each for compile-time and runtime polymorphism.**
  ```java
  class Calc { int add(int a, int b) { return a + b; } double add(double a, double b) { return a + b; } } // overloading
  class Animal { void sound() { System.out.println("..."); } }
  class Dog extends Animal { @Override void sound() { System.out.println("Bark"); } }
  Animal a = new Dog(); a.sound(); // prints Bark: overriding, resolved at runtime
  ```

---

## 4. Static members and reference types (Session 5)

### Basics

- ⭐ **What is a static variable? When is it created, and where is it stored?** A variable that belongs to the class, with one copy shared by all objects. It is created when the class is loaded and initialized. It is stored with the class data: PermGen before Java 8, the heap (as part of the `Class` object) from Java 8, with class metadata in Metaspace.
- ⭐ **What is a static method? What restrictions apply to it?** A method that belongs to the class and is called with the class name. It cannot access instance variables or instance methods directly, cannot use `this` or `super`, and cannot be overridden.
- ⭐ **Why can a static method not access instance members directly?** A static method runs without any object, so there is no `this` to say whose instance members to use. It can access them through an object reference it creates or receives.
- **Why is `main` static?** So the JVM can call it without creating an object of the class, which avoids the question of which constructor to call.
- **What is a static block? When does it run? Can there be more than one?** A `static { }` block that initializes static data. It runs once when the class is initialized, before any object is created or static member used. A class can have several; they run in the order written.
- **What is an instance initializer block? When does it run?** A `{ }` block in the class body. It runs every time an object is created, after `super()` and before the rest of the constructor. It is useful for code shared by all constructors.
- **How do you access a static member of another class?** With the class name: `ClassName.member`, provided the access modifier allows it. A static import lets you use the bare name.
- **Can you call a static method through an object reference? Through a `null` reference?** Yes to both. The compiler replaces the reference with its declared type, so no `NullPointerException` occurs. It is bad style and compilers warn about it.
- **Can you use `this` or `super` in a static context?** No. Both refer to the current object, and a static context has none. It is a compile error.
- ⭐ **What is a reference data type? How does it differ from a primitive type?** A reference type (class, interface, array, enum) variable holds a reference to an object on the heap. A primitive variable holds the value itself. Primitives have fixed sizes and no methods; references can be `null` and are compared by identity with `==`.
- **What is a reference variable? What does it hold?** A variable of a class, interface or array type. It holds the reference (conceptually the address) of an object, or `null`.
- **What is the default value of a reference variable?** `null` for fields and array elements. Local reference variables have no default and must be assigned before use.
- **What is `null`? What is `NullPointerException`?** `null` is a literal meaning "refers to no object". `NullPointerException` is an unchecked exception thrown when you call a method, access a field, index an array or unbox through a `null` reference.
- **What is the difference between a reference variable and a static variable?** They are different concepts. "Reference" describes what a variable holds (a pointer to an object). "Static" describes who owns the variable (the class, not an object). A variable can be both: `static Employee manager;`.
- **What is the difference between a static variable and an instance variable?** A static variable has one copy per class, is created at class loading and is accessed with the class name. An instance variable has one copy per object, is created with the object and is accessed through a reference.

### Deep dive

- ⭐ **Can static methods be overridden? What is method hiding?** No. Static methods are bound at compile time using the reference type. If a child declares a static method with the same signature, it hides the parent's method: which one runs depends on the declared type of the reference, not the object.
- **Can static methods be overloaded?** Yes. Overloading only needs different parameter lists.
- **Can a constructor be static? Can a top-level class be static? Can a local variable be static?** No to all three. Constructors initialize objects, so static makes no sense. Only nested classes can be static. Local variables belong to a method call.
- **Can an interface have static methods? Are they inherited?** Yes, since Java 8. They are not inherited by implementing classes or sub-interfaces and must be called as `InterfaceName.method()`.
- **Can an abstract class have static methods? Can a static method be abstract?** An abstract class can have static methods. A method cannot be both static and abstract, because abstract methods need overriding and static methods cannot be overridden.
- **What is the order of execution of static blocks across a parent and child class?** The parent's static blocks run first, then the child's, each once. Then for each object: parent instance blocks and constructor, then child instance blocks and constructor.
- **When exactly is a class initialized? Which actions trigger it and which do not?** On first creating an instance, calling a static method, reading or writing a non-constant static field, `Class.forName`, or initializing a subclass. Accessing a compile-time constant (`static final int X = 10`) does not, because the value is inlined; nor does declaring a reference or creating an array of the type.
- **Where were static variables stored before Java 8 and where are they stored now?** Before Java 8 in PermGen. From Java 8 on the heap alongside the `Class` object, while class metadata sits in Metaspace.
- **Are static variables garbage collected? When can a class be unloaded?** The objects they refer to stay reachable as long as the class is loaded. A class is unloaded only when its class loader becomes unreachable, which in practice happens with custom class loaders, not the application loader.
- **Why are static variables dangerous in multithreaded code? How can they cause memory leaks?** They are shared by all threads, so unsynchronized updates cause race conditions. They live as long as the class, so a static collection that keeps growing holds its objects forever.
- **What is a static import? When is it a bad idea?** `import static java.lang.Math.*;` lets you use static members without the class name. Overuse hurts readability because the reader cannot tell where a name comes from, and it can cause name clashes.
- **How do you use static to implement a counter of created objects? A singleton? A utility class?** Counter: a `static int count` incremented in the constructor. Singleton: a `private static` instance, a private constructor and a `public static getInstance()`. Utility class: only static methods and a private constructor, like `Math`.
- **Why should a utility class have a private constructor?** To prevent instantiation and subclassing, since an instance of a class with only static methods is meaningless.
- **How is `==` evaluated for primitives versus references?** For primitives it compares values (after numeric promotion). For references it compares whether both point to the same object.

### Code and scenario

- **What is printed? A class with a static block, an instance block and a constructor, when two objects are created.** The static block prints once. Then for each object the instance block prints, followed by the constructor. So: static, instance, constructor, instance, constructor.
- **What is printed? `A obj = new B(); obj.show();` where both classes declare `static void show()`.** `A`. Static methods are resolved by the reference type at compile time; this is method hiding, not overriding.
- **What is printed? `Test t = null; t.staticMethod();`** The static method runs normally with no `NullPointerException`, because the call is compiled as `Test.staticMethod()`.
- **What is printed? A static counter incremented in the constructor after creating three objects, read through each object.** `3` each time. All objects share the single static variable.
- **Does `static int x = y; static int y = 5;` compile? What about a static block reading a variable declared later?** No: "illegal forward reference". The same error applies to reading the later variable by simple name in a static block. Assigning to it is allowed, and reading through the qualified name `Test.y` compiles and gives the default value 0.
- **What are the default values printed for static and instance fields of every primitive type and of `String`?** `byte`, `short`, `int`, `long`: 0. `float`, `double`: 0.0. `char`: `'\u0000'` (prints as a blank). `boolean`: false. `String` and any reference: null. Static and instance fields get the same defaults.
- **Two reference variables point to the same object and one is set to `null`. What happens to the object?** Nothing. The other variable still refers to it, so it stays reachable and is not eligible for garbage collection.

---

## 5. Constructors, parameter passing, stack and heap (Session 6)

### Basics

- ⭐ **What is a constructor? How does it differ from a method?** A special block that initializes a new object. It has the same name as the class, has no return type, is called automatically by `new`, and is not inherited. A method has any name, a return type, is called explicitly and can be inherited.
- **What types of constructors are there?** The default constructor (generated by the compiler), a no-arg constructor (written by you with no parameters) and parameterized constructors.
- ⭐ **What is the default constructor? When does the compiler stop providing it?** A no-arg constructor the compiler adds when a class declares no constructor. It has the same access as the class and just calls `super()`. As soon as you write any constructor, the compiler stops providing it.
- **Can a constructor have a return type? What happens if you add `void`?** No. If you add `void` it compiles, but it becomes an ordinary method that happens to share the class name, and it is not called by `new`.
- ⭐ **What is constructor overloading?** Having several constructors in one class with different parameter lists, so objects can be created in different ways.
- **Can a constructor be `private`? When would you do that?** Yes. Use it for singletons, utility classes, factory-method patterns and classes with only static constants, to control or prevent instantiation.
- **Can a constructor be `final`, `static`, `abstract` or `synchronized`?** No. Constructors are not inherited, so `final` and `abstract` are meaningless; they work on an instance, so not `static`; and the object is not shared during construction, so `synchronized` is not allowed (a synchronized block inside is fine).
- **Are constructors inherited? Can they be overridden?** No to both. A subclass calls a parent constructor with `super(...)` but does not inherit it.
- **What is a copy constructor? Does Java provide one by default?** A constructor that takes an object of the same class and copies its fields: `Employee(Employee other)`. Java does not generate one; you write it yourself.
- ⭐ **Is Java pass by value or pass by reference? Explain with an example.** Java is always pass by value. For primitives the value is copied. For objects the reference is copied, so the method can change the object's state through that copy, but reassigning the parameter does not affect the caller's variable.
- **What happens when you pass a primitive to a method? An object reference?** A primitive is copied, so changes inside the method are invisible to the caller. For an object, the reference is copied: both point to the same object, so field changes are visible, but pointing the parameter at a new object is not.
- **What happens when you reassign a reference variable?** The variable now points to a different object. The old object is unchanged, and if nothing else refers to it, it becomes eligible for garbage collection.
- ⭐ **What is the difference between stack memory and heap memory?** The stack is per thread, stores method frames with local variables and references, works in LIFO order, is freed automatically when a method returns, and is small and fast. The heap is shared by all threads, stores all objects and arrays, is managed by the garbage collector, and is larger and slower.
- **What is stored on the stack and what on the heap?** Stack: local primitives, local reference variables, method call frames and return addresses. Heap: objects, their instance fields, arrays, and the string pool.
- **What are `StackOverflowError` and `OutOfMemoryError`? What causes each?** `StackOverflowError` occurs when a thread's stack is exhausted, usually by deep or infinite recursion. `OutOfMemoryError` occurs when the heap (or Metaspace) cannot satisfy an allocation even after garbage collection, often due to a leak or too small a heap.

### Deep dive

- ⭐ **What is constructor chaining? How do `this()` and `super()` work?** Calling one constructor from another. `this(...)` calls another constructor in the same class; `super(...)` calls a parent constructor. If you write neither, the compiler inserts `super()`. Construction therefore always runs from `Object` downward.
- **Why must `this()` or `super()` be the first statement in a constructor? What has changed in recent Java versions?** So the parent part of the object is fully initialized before the child uses it. Java 25 finalized flexible constructor bodies: statements that do not use the object under construction (such as argument validation) may now appear before the call.
- **Can you use both `this()` and `super()` in the same constructor?** No. Only one explicit constructor call is allowed, since each would have to be the first.
- **What happens if the parent class has only a parameterized constructor and the child has none?** Compile error. The compiler-generated child constructor calls `super()`, which does not exist. Fix it by calling `super(args)` explicitly or adding a no-arg constructor to the parent.
- **Why is calling an overridable method from a constructor dangerous?** The child's override runs before the child's fields are initialized, so it sees default values such as `null` or 0 and may fail or behave wrongly.
- **Can a constructor throw an exception? What happens to the partially constructed object?** Yes. The `new` expression fails, no reference is returned and the half-built object becomes garbage, unless the constructor leaked `this` somewhere first.
- **What does "the reference escapes during construction" mean?** `this` is published to other code (stored in a static field, registered as a listener, passed to a new thread) before the constructor finishes, so others can see a half-initialized object. It is a thread-safety hazard.
- **What is a stack frame? What does it contain?** The record created for each method call. It has a local variable array (parameters and locals), an operand stack for intermediate values, and frame data such as the reference to the constant pool and the return address.
- **Is each thread's stack separate? Is the heap shared?** Yes and yes. Each thread has a private stack, which is why local variables are thread-safe. All threads share one heap, which is why shared objects need synchronization.
- **What is escape analysis? Can an object be allocated on the stack?** A JIT analysis that checks whether an object is reachable outside the method or thread. If it does not escape, the JIT can remove the heap allocation by breaking the object into scalar local values (scalar replacement) and can remove locks on it.
- **What are the heap generations?** The young generation has eden, where new objects are created, and two survivor spaces. Objects that survive several minor collections move to the old (tenured) generation, which is collected less often.
- **How do you set stack and heap sizes?** `-Xss` sets the thread stack size, `-Xms` the initial heap and `-Xmx` the maximum heap, for example `java -Xms512m -Xmx2g -Xss1m App`.
- **Why can't a swap method for two objects work in Java the way it does with C++ references?** The method receives copies of the references. Swapping the copies does not change the caller's variables. You can only swap the contents of the objects or return the swapped values.
- **How do you initialize a reference variable of another class inside a constructor?** Either create it there (`this.engine = new Engine();`, composition) or accept it as a parameter (`this.engine = engine;`, dependency injection). Injection is more flexible and testable.
- **What is the difference between initializing a field at declaration, in an initializer block, and in the constructor?** Declaration initializers and instance blocks run first, in textual order, for every constructor. Constructor code runs after them and can use parameters. Use declaration for fixed defaults and the constructor for values that depend on arguments.

### Code and scenario

- ⭐ **What is printed? `change(e)` sets `e.name = "B"` and then `e = new Employee("C")`; the caller prints `e.name`.** `B`. The field change goes through the copied reference to the caller's object. The reassignment only changes the method's local copy.
- **What is printed? A method that modifies an `int` parameter, an `int[]` parameter and a `String` parameter.** The caller's `int` is unchanged. Element changes to the array are visible. The `String` is unchanged, because strings are immutable and "modifying" it only reassigns the local parameter.
- **What is printed? A three-level inheritance chain where each constructor prints a line.** Grandparent, then parent, then child. Each constructor calls `super()` first, so the top of the chain finishes first.
- **Does this compile? A class with only `Employee(int id)` and the call `new Employee()`.** No. There is no no-arg constructor because the compiler does not add a default one once you declare a constructor.
- **Does this compile? A constructor that calls `this()` recursively.** No. The compiler reports "recursive constructor invocation".
- **Build an `Employee` class with overloaded constructors that chain to one master constructor.**
  ```java
  class Employee {
      private final int id; private final String name; private final double salary;
      Employee(int id) { this(id, "Unknown"); }
      Employee(int id, String name) { this(id, name, 0.0); }
      Employee(int id, String name, double salary) { this.id = id; this.name = name; this.salary = salary; }
  }
  ```
- **Build a class `Company` that holds references to `Address` and `Employee`. Draw the stack and heap after `main` creates one `Company`.** The stack frame of `main` holds one reference variable `c`. The heap holds the `Company` object, whose fields hold references to a separate `Address` object and `Employee` object, each of which may reference `String` objects. Only the reference `c` is on the stack.
- **Trace stack and heap for a recursive factorial call with n = 3.** Frames are pushed for `fact(3)`, `fact(2)`, `fact(1)` (and `fact(0)` if that is the base case), each with its own `n`. They return in reverse order: 1, 2, 6. Nothing is on the heap because only primitives are used.
- **How many objects are created by `Employee[] arr = new Employee[5];`?** One: the array object holding five `null` references. No `Employee` object is created.

---

## 6. Inheritance, association and polymorphism (Session 7)

### Basics

- ⭐ **What is inheritance? Which keyword is used?** A mechanism where a child class acquires the fields and methods of a parent class. Classes use `extends`; classes implement interfaces with `implements`.
- ⭐ **What types of inheritance does Java support? Which does it not support with classes?** Single (B extends A), multilevel (C extends B extends A) and hierarchical (B and C both extend A). Multiple and hybrid inheritance are not supported with classes, only through interfaces.
- ⭐ **Why does Java not support multiple inheritance with classes? What is the diamond problem?** If B and C both override a method from A and D extends both, it is ambiguous which version D inherits. That is the diamond problem. Java avoids the ambiguity and complexity by allowing one superclass.
- **What is the superclass of every class?** `java.lang.Object`.
- **Which members of a parent are not inherited?** Constructors, static and instance initializer blocks, and private members (they exist in the object but are not accessible). Default-access members are not accessible from a subclass in another package.
- **Are private members inherited? Are they accessible?** They are part of the child object's memory but are not accessible by name in the child. The child can use them only through the parent's public or protected methods.
- ⭐ **What is method overloading? What are its rules?** Several methods in one class with the same name and different parameter lists (number, type or order). Return type and access modifier can differ but are not enough on their own.
- ⭐ **What is method overriding? What are its rules?** A subclass gives its own implementation of an inherited method. The name and parameters must be the same; the return type must be the same or covariant; access cannot be more restrictive; it cannot throw new or broader checked exceptions; and `private`, `static` and `final` methods cannot be overridden.
- ⭐ **What is the difference between overloading and overriding?** Overloading: same class, different parameters, resolved at compile time, any return type. Overriding: parent and child, same signature, resolved at runtime by the object's actual type, needs inheritance.
- ⭐ **What is compile-time polymorphism versus runtime polymorphism?** Compile-time (static) polymorphism is overloading: the compiler picks the method from the argument types. Runtime (dynamic) polymorphism is overriding: the JVM picks the method from the actual object when a parent reference points to a child object.
- ⭐ **What is the `super` keyword? List its uses.** A reference to the parent part of the current object. It calls a parent constructor (`super(...)`), calls a parent method that was overridden (`super.method()`), and accesses a hidden parent field (`super.field`).
- ⭐ **What is the `this` keyword? List its uses.** A reference to the current object. It distinguishes fields from parameters (`this.name = name`), calls another constructor (`this(...)`), passes the current object as an argument, and returns the current object for method chaining.
- **What is the `@Override` annotation for? Is it mandatory?** It tells the compiler the method is meant to override one from a supertype, so a typo or wrong signature becomes a compile error. It is optional but strongly recommended.
- ⭐ **What is the difference between association, aggregation and composition? Give an example of each.** Association is a general "uses" relationship between independent objects (Teacher–Student). Aggregation is a weak "has-a" where the part can exist without the whole (Department–Professor). Composition is a strong "has-a" where the part cannot exist without the whole (House–Room, Car–Engine).
- **What is the difference between "is-a" and "has-a"?** "Is-a" is inheritance, implemented with `extends` or `implements`. "Has-a" is composition, implemented by holding a reference to another object as a field.

### Deep dive

- ⭐ **What is dynamic method dispatch? How does the JVM implement it?** The mechanism by which a call to an overridden method is resolved at runtime from the object's actual class. Each class has a virtual method table; `invokevirtual` looks up the method in the table of the object's class and jumps to it.
- **Can you overload by changing only the return type? Why not?** No. The caller may ignore the return value, so `m();` would be ambiguous. The compiler requires different parameter lists.
- **Can you overload `main`? Can you override it?** You can overload it; the JVM only calls `main(String[])`. You cannot override it because it is static; a child can only hide it.
- **Can you override a `private`, `static` or `final` method?** No. Private methods are not visible to the child (a same-named method is a new method). Static methods are hidden, not overridden. Final methods cause a compile error if you try.
- **What is a covariant return type?** An overriding method may return a subtype of the parent method's return type, for example the parent returns `Animal` and the child returns `Dog`. Allowed since Java 5.
- **Can an overriding method reduce visibility? Increase it?** It cannot reduce it (public to protected is an error) because that would break substitutability. It can increase it (protected to public).
- **What are the rules for exceptions in an overriding method?** It can throw the same checked exceptions, narrower ones, or none. It cannot throw new or broader checked exceptions. Unchecked exceptions are unrestricted.
- **Can an overriding method be `synchronized` or `strictfp` when the parent's is not?** Yes. These modifiers are not part of the method signature and do not affect overriding.
- **Are fields polymorphic? What is variable hiding?** No. Field access is resolved at compile time from the reference type. If a child declares a field with the same name, it hides the parent's field, and `parentRef.field` gives the parent's value.
- **What is the difference between static binding and dynamic binding?** Static binding happens at compile time based on the reference type; it applies to overloaded, private, static and final methods and to fields. Dynamic binding happens at runtime based on the object type; it applies to overridden instance methods.
- **How does the compiler resolve overloads with `null`, boxing, widening and varargs?** It chooses the most specific applicable method. `null` matches any reference type, so the most specific wins, or it is ambiguous if two are unrelated. Order of preference is exact match, widening, boxing, then varargs.
- ⭐ **Why prefer composition over inheritance?** Inheritance couples the child tightly to the parent's implementation, is fixed at compile time and exposes the whole parent API. Composition is more flexible: you can swap the delegate at runtime, expose only what you want, and avoid breaking when the parent changes.
- **What is the fragile base class problem?** A seemingly safe change in a base class breaks subclasses that depended on its internal behaviour, for example a subclass overriding `add` when the parent's `addAll` calls `add` internally.
- **What is the Liskov substitution principle? Give a violation.** Objects of a subclass must be usable in place of the parent without breaking the program. `Square extends Rectangle` violates it: setting width on a square also changes height, breaking code that expects a rectangle's width and height to be independent.
- **How can you stop a class from being inherited? A method from being overridden?** Declare the class `final` (or give it only private constructors). Declare the method `final`, `private` or `static`.
- **What are sealed classes (Java 17) and how do they restrict inheritance?** A `sealed` class lists the classes allowed to extend it with `permits`. Each permitted subclass must be `final`, `sealed` or `non-sealed`. This gives a closed hierarchy that the compiler can check exhaustively in `switch`.
- **Can you call `super.super.method()`? Why not?** No. It would bypass the parent's behaviour and break encapsulation. You can only reach the immediate parent.
- **Can `this` be assigned a new value? Can it be passed as an argument or returned?** It cannot be assigned; it is effectively final. It can be passed as an argument and returned from a method.
- **How do you call the parent's version of an overridden method from the child? From outside the child?** Inside the child, use `super.method()`. From outside it is not possible on a child object; dynamic dispatch always calls the child's version.

### Code and scenario

- ⭐ **What is printed? `A a = new B(); System.out.println(a.x); a.show();` where A has `x = 10` and B has `x = 20`, and both define `show()`.** `10` then `B`. Fields are resolved by reference type; instance methods by object type.
- **What is printed? Overloads `m(Object o)` and `m(String s)` called with `m(null)`. What if you add `m(Integer i)`?** `m(String)` runs because `String` is more specific than `Object`. Adding `m(Integer)` makes the call ambiguous and it fails to compile, since `String` and `Integer` are unrelated.
- **What is printed? Overloads `m(int)`, `m(long)`, `m(Integer)`, `m(int...)` called with a `byte`, a `char` and an `int`.** `m(int)` in all three cases. `byte` and `char` widen to `int`, and widening is preferred over boxing and varargs.
- **What is printed? A parent constructor calls an overridden method that reads a child field initialized at declaration.** The default value (0 or `null`). The child's override runs, but the child's field initializers have not yet run because the parent constructor is still executing.
- **Does this compile? Child overrides `void m() throws IOException` with `void m() throws Exception`.** No. The override declares a broader checked exception than the parent.
- **Does this compile? Child overrides `protected Object get()` with `public String get()`.** Yes. Visibility is widened and the return type is covariant.
- **Write demo classes for single, multilevel and hierarchical inheritance.**
  ```java
  class Animal { void eat() { System.out.println("eat"); } }
  class Dog extends Animal { void bark() { System.out.println("bark"); } }      // single
  class Puppy extends Dog { void weep() { System.out.println("weep"); } }      // multilevel: Animal -> Dog -> Puppy
  class Cat extends Animal { void meow() { System.out.println("meow"); } }      // hierarchical: Dog and Cat extend Animal
  ```
- **Model `Car`–`Engine`, `Department`–`Professor` and `Teacher`–`Student` and state which is composition, aggregation or association.** `Car` creates its own `Engine` in its constructor: composition. `Department` receives a list of `Professor` objects created elsewhere: aggregation. `Teacher` and `Student` only refer to each other, for example as method parameters: association.
- **Design `Shape` with `Circle` and `Rectangle` and a method that computes total area of a `Shape[]`.**
  ```java
  abstract class Shape { abstract double area(); }
  class Circle extends Shape { double r; Circle(double r) { this.r = r; } double area() { return Math.PI * r * r; } }
  class Rectangle extends Shape { double w, h; Rectangle(double w, double h) { this.w = w; this.h = h; } double area() { return w * h; } }
  static double total(Shape[] shapes) { double sum = 0; for (Shape s : shapes) sum += s.area(); return sum; }
  ```
- **Encapsulate an `Employee` class, then extend it with `Manager` and override a salary method.**
  ```java
  class Employee {
      private final String name; private final double base;
      Employee(String name, double base) { this.name = name; this.base = base; }
      public String getName() { return name; }
      protected double getBase() { return base; }
      public double salary() { return base; }
  }
  class Manager extends Employee {
      private final double bonus;
      Manager(String name, double base, double bonus) { super(name, base); this.bonus = bonus; }
      @Override public double salary() { return super.salary() + bonus; }
  }
  ```

---

## 7. Casting, abstract classes and interfaces (Session 8)

### Basics

- ⭐ **What is upcasting? Is it implicit?** Assigning a child object to a parent-type reference: `Animal a = new Dog();`. It is implicit and always safe, because a child is a parent.
- ⭐ **What is downcasting? Why does it need an explicit cast?** Converting a parent-type reference back to a child type: `Dog d = (Dog) a;`. It needs a cast because the compiler cannot be sure the object is really a `Dog`; the JVM checks at runtime.
- **What is `ClassCastException`? How do you avoid it?** A runtime exception thrown when you cast an object to a type it is not an instance of. Check with `instanceof` before casting, or use generics so casts are not needed.
- **What is `instanceof`? What is pattern matching for `instanceof` (Java 16)?** It tests an object's type. With pattern matching you test and bind in one step: `if (obj instanceof Dog d) { d.bark(); }`, with no separate cast.
- ⭐ **What is an abstract class? Can you instantiate it?** A class declared `abstract` that may contain abstract and concrete methods. It cannot be instantiated; it is meant to be extended.
- **What is an abstract method? Can it have a body?** A method declared with `abstract` and no body, ending in a semicolon. Subclasses must implement it. It cannot have a body.
- **Can an abstract class have a constructor? Why, if it cannot be instantiated?** Yes. It runs when a subclass object is created, through `super()`, to initialize the fields declared in the abstract class.
- **Can an abstract class have no abstract methods?** Yes. Declaring it abstract simply prevents direct instantiation.
- **Can an abstract class have concrete methods, static methods, final methods, instance variables?** Yes to all. It is a normal class in every way except that it cannot be instantiated and may contain abstract methods.
- ⭐ **What is an interface? Why do we need it?** A contract that declares what a class must do without saying how. It gives full abstraction, lets unrelated classes share a type, allows multiple inheritance of type, and supports loose coupling.
- **What are the default modifiers of interface fields and methods?** Fields are `public static final`. Methods without a body are `public abstract`. Default and static methods are `public` unless declared `private` (Java 9+).
- ⭐ **What is the difference between an abstract class and an interface?** An abstract class can have instance state, constructors, and methods with any access modifier; a class can extend only one. An interface has only constants, no constructors, public methods (plus default, static and private ones), and a class can implement many. Use an abstract class for shared state and code among related classes; use an interface for a capability or contract.
- ⭐ **How does Java achieve multiple inheritance?** Through interfaces. A class can implement any number of interfaces, and an interface can extend several interfaces.
- **Can an interface extend another interface? More than one? Can it extend a class?** It can extend one or more interfaces. It cannot extend a class.
- **Can a class implement two interfaces with the same method signature?** Yes. One implementation satisfies both. If both provide a default method with that signature, the class must override it to resolve the conflict.
- **What is a marker interface? Name some.** An interface with no methods that tags a class with a capability the JVM or a library checks. Examples: `Serializable`, `Cloneable`, `RandomAccess`, `Remote`.

### Deep dive

- ⭐ **When would you choose an abstract class over an interface, and the reverse?** Abstract class: closely related classes that share fields, constructors or non-public helper methods, or a template method. Interface: a contract for unrelated classes, when you need multiple inheritance, or to define a type for dependency injection. In practice, start with an interface and add an abstract base class for shared code.
- **After Java 8, what real differences remain between abstract classes and interfaces?** Interfaces still cannot hold instance state or constructors, cannot have protected or package-private methods, and cannot override `Object` methods with defaults. Classes still allow only single inheritance.
- **Can an abstract method be `private`, `static`, `final` or `synchronized`? Why not?** No. Private and final methods cannot be overridden, and static methods are not overridden, yet an abstract method must be. `synchronized` is an implementation detail, and an abstract method has no implementation.
- **Can an abstract class be `final`?** No. `abstract` requires subclassing and `final` forbids it; the combination is a compile error.
- **What happens if a class does not implement all methods of an interface?** It must be declared `abstract`, otherwise it does not compile.
- **Why are interface variables `public static final`?** An interface cannot be instantiated, so it has no instance state; the fields must be static. They are final so that all implementers share one fixed constant, and public because the interface is a public contract.
- **Can an interface have a constructor, an instance block or a static block?** No to all three. It has no instances to initialize, and static fields must be initialized at declaration.
- **What happens when two interfaces declare the same default method and a class implements both?** Compile error unless the class overrides the method. Inside the override it can pick one with `InterfaceA.super.method()`.
- **What happens when two interfaces declare the same constant and a class implements both?** It compiles until you refer to the constant by its simple name, which is ambiguous. Qualify it: `InterfaceA.CONST`.
- **What happens when a class inherits a method from a superclass and a default method with the same signature from an interface?** The class method wins. The rule is "classes win over interfaces".
- **What does "program to an interface, not an implementation" mean?** Declare variables, parameters and return types as the interface (`List<String> list = new ArrayList<>()`), so you can change the implementation without touching the calling code. It reduces coupling and helps testing with mocks.
- **How do marker interfaces work internally? Why are annotations often preferred now?** Code checks `obj instanceof Serializable` and acts on it. Annotations can carry parameters and apply to methods and fields too. Marker interfaces still have one advantage: they define a type that the compiler can check.
- **What is the template method pattern, and how do abstract classes support it?** A base class method defines the fixed steps of an algorithm and calls abstract methods for the steps that vary. Subclasses fill in those steps without changing the overall flow. The template method is usually `final`.
- **After upcasting, which members can you access through the reference?** Only members declared in the reference type. Overridden methods still run the child's version, but child-only methods need a downcast.
- **Is casting checked at compile time, at runtime, or both?** Both. The compiler rejects casts between unrelated class types. The JVM checks the actual object type at runtime and throws `ClassCastException` if it does not match.
- **What are nested interfaces?** An interface declared inside a class or another interface, such as `Map.Entry`. It is implicitly static, and referred to as `Outer.Inner`.

### Code and scenario

- **Does this compile, and does it run? `Animal a = new Animal(); Dog d = (Dog) a;`** It compiles because `Dog` is a subtype of `Animal`. At runtime it throws `ClassCastException` because the object is an `Animal`, not a `Dog`.
- **Does this compile? `Dog d = new Dog(); Cat c = (Cat) d;` Why is the answer different if `Cat` is an interface?** With two unrelated classes it is a compile error: inconvertible types. If `Cat` is an interface it compiles, because some subclass of `Dog` might implement `Cat` (unless `Dog` is final); it then fails at runtime with `ClassCastException`.
- **What is printed? `Object o = "hello"; Integer i = (Integer) o;`** Nothing; it compiles but throws `ClassCastException` at runtime.
- **What is printed? An abstract class constructor prints a line and a concrete subclass is instantiated.** The abstract class's line prints first, then the subclass constructor's output, because the subclass constructor calls `super()` first.
- **Does this compile? `abstract class A { abstract void m() {} }`** No. An abstract method cannot have a body.
- **Does this compile? `new A() { ... }` for an abstract class `A`. Is that instantiating the abstract class?** It compiles if the body implements all abstract methods. It creates an instance of an anonymous subclass of `A`, not of `A` itself.
- **Write an interface `Payable` implemented by `Employee` and `Invoice`, and process a mixed `Payable[]`.**
  ```java
  interface Payable { double getPaymentAmount(); }
  class Employee implements Payable { double salary; Employee(double s) { salary = s; } public double getPaymentAmount() { return salary; } }
  class Invoice implements Payable { double price; int qty; Invoice(double p, int q) { price = p; qty = q; } public double getPaymentAmount() { return price * qty; } }
  Payable[] items = { new Employee(50000), new Invoice(200, 3) };
  for (Payable p : items) System.out.println(p.getPaymentAmount());
  ```
- **Given a `List<Shape>`, safely call a method that only `Circle` has.** `for (Shape s : shapes) if (s instanceof Circle c) c.getRadius();`. Before Java 16, check `instanceof` and then cast. If this is needed often, the design probably needs a method on `Shape` instead.
- **Design a payment system with `Payment` as an abstract class or interface. Defend your choice.** Define an interface `PaymentMethod { void pay(double amount); }` implemented by `CardPayment`, `UpiPayment` and `WalletPayment`, so the checkout code depends only on the contract and new methods can be added without changes. If the implementations share state such as a transaction id and common steps such as validation and logging, add an abstract base class with a template method `process()`.

---

## 8. final, functional interfaces, lambdas, inner classes, enums (Sessions 9–10)

### final

- ⭐ **What does `final` mean for a variable, a method and a class?** A final variable can be assigned only once. A final method cannot be overridden. A final class cannot be extended.
- **What is a blank final variable? Where can it be initialized?** A final field declared without a value. It must be assigned exactly once in every constructor or in an instance initializer block.
- **What is a static blank final variable? Where can it be initialized?** A `static final` field without an initial value. It must be assigned in a static block.
- ⭐ **What is the difference between `final`, `finally` and `finalize`?** `final` is a modifier that prevents reassignment, overriding or inheritance. `finally` is a block that runs after try/catch whether or not an exception occurred. `finalize()` is a deprecated `Object` method the GC may call before reclaiming an object.
- **If a reference is `final`, can the object it refers to be modified?** Yes. `final` fixes the reference, not the object. You can change the object's fields but cannot point the variable to another object.
- **Can a final method be overloaded? Inherited?** Yes to both. It just cannot be overridden.
- **Can a constructor be final? Can an interface method be final?** No. Constructors are never inherited, so `final` is meaningless. Interface methods are meant to be implemented, so they cannot be final.
- **Name some final classes in the JDK. Why is `String` final?** `String`, the wrapper classes (`Integer`, `Double` and so on), `Math`, `System`, `LocalDate`. `String` is final so that no subclass can break its immutability, which the string pool, hash caching, security and thread safety all rely on.
- **What is a compile-time constant? How does the compiler inline it, and what problem can that cause across JARs?** A `static final` primitive or `String` initialized with a constant expression. The compiler copies its value into every class that uses it. If the constant changes in one JAR and dependents are not recompiled, they keep the old value.
- **What is "effectively final"? Where is it required?** A local variable that is never reassigned after initialization, even without the `final` keyword. Lambdas, anonymous classes and local classes can only capture local variables that are final or effectively final.
- ⭐ **How do you make a class immutable? What role does `final` play?** Declare the class `final`; make all fields `private final`; provide no setters; initialize everything in the constructor; make defensive copies of mutable inputs and return copies (or unmodifiable views) from getters. `final` on the class stops subclasses adding mutability, and on fields stops reassignment.
- **Does `final` help with thread safety? What are the final field semantics in the Java Memory Model?** Yes. The JMM guarantees that once a constructor finishes, any thread sees the correctly initialized values of final fields without synchronization, as long as `this` did not escape during construction.
- **Can method parameters be final? What does it buy you?** Yes. The parameter cannot be reassigned inside the method, which prevents accidental reassignment and documents intent. It has no effect on the caller.

### Interface features (Java 8, 9, 11)

- ⭐ **What are default methods? Why were they added in Java 8?** Interface methods with a body, marked `default`. They let new methods be added to existing interfaces without breaking every implementing class, which was needed to add `forEach` and `stream` to the collection interfaces.
- **What are static methods in interfaces? How are they called? Are they inherited?** Utility methods with a body declared `static` in an interface. They are called as `InterfaceName.method()` only, and are not inherited by implementing classes.
- **What are private interface methods (Java 9)? Why were they added?** Private (and private static) methods with a body inside an interface. They let default and static methods share helper code without exposing it to implementers.
- **How do you resolve a conflict between two default methods?** Override the method in the class. To reuse one, call `InterfaceName.super.method()` inside the override.
- **Can a default method override a method from `Object` such as `equals`?** No, it is a compile error. A class always inherits these from `Object`, and class methods win over interface defaults, so such a default could never be used.
- **What interface-related changes arrived by Java 11?** Java 9 added private interface methods. Java 11 itself did not add new interface syntax; related changes were `var` in lambda parameters and nest-based access control, which affects how nested types access each other's private members.

### Functional interfaces and lambdas

- ⭐ **What is a functional interface?** An interface with exactly one abstract method. It can be the target of a lambda expression or method reference.
- **What is `@FunctionalInterface`? Is it mandatory?** An annotation that makes the compiler verify the interface has exactly one abstract method. It is optional but protects against someone adding a second abstract method.
- **Can a functional interface have default and static methods? Methods from `Object`?** Yes. Default, static and private methods do not count, and neither do abstract methods that match public methods of `Object` (such as `equals`).
- ⭐ **Name the main built-in functional interfaces and their methods.** `Predicate<T>`: `boolean test(T)`. `Function<T,R>`: `R apply(T)`. `Consumer<T>`: `void accept(T)`. `Supplier<T>`: `T get()`. `BiFunction<T,U,R>`: `R apply(T,U)`. `UnaryOperator<T>`: `T apply(T)`. `BinaryOperator<T>`: `T apply(T,T)`.
- **Is `Runnable` a functional interface? `Comparator`? `Comparable`? `Callable`?** All four have exactly one abstract method, so all can be used with lambdas. `Runnable`, `Comparator` and `Callable` are annotated `@FunctionalInterface`; `Comparable` is not annotated and is not normally used as a lambda.
- ⭐ **What is a lambda expression? What is its syntax?** An anonymous function that implements a functional interface: `(parameters) -> expression` or `(parameters) -> { statements }`. For example `(a, b) -> a + b`. Parameter types and parentheses around a single parameter are optional.
- ⭐ **What is the difference between a lambda and an anonymous inner class?** A lambda works only with functional interfaces; an anonymous class can implement any interface or extend a class, and can have state. `this` in a lambda refers to the enclosing object; in an anonymous class it refers to the anonymous object. A lambda does not produce a separate class file; it uses `invokedynamic`.
- **What does `this` refer to inside a lambda versus an anonymous class?** In a lambda, `this` is the enclosing class instance, since a lambda does not introduce a new scope. In an anonymous class, `this` is the anonymous class instance.
- **Why must local variables captured by a lambda be effectively final?** The lambda captures a copy of the value, and it may run later or on another thread after the method has returned. Allowing changes would make the copy and the original disagree and cause concurrency problems.
- **Can a lambda modify an instance or static variable?** Yes. Those live on the heap, not on the stack, so the restriction does not apply. Be careful about thread safety.
- **What is a method reference? What are its four kinds?** A shorthand for a lambda that only calls an existing method. Static: `Integer::parseInt`. Bound instance: `System.out::println`. Unbound instance of a type: `String::toUpperCase`. Constructor: `ArrayList::new`.
- **What is a constructor reference?** `ClassName::new`, a method reference that creates objects. For example `Supplier<List<String>> s = ArrayList::new;`.
- **How are lambdas implemented internally? Do they create a `.class` file?** The compiler turns the lambda body into a private method and emits an `invokedynamic` instruction. At first execution, `LambdaMetafactory` generates the implementing class at runtime. No extra `.class` file is produced at compile time.
- **What is target typing? What is the type of a lambda?** A lambda has no type of its own; the compiler infers it from the context that expects a functional interface, such as an assignment or a method parameter. The same lambda can be a `Runnable` in one place and another interface elsewhere.
- **Can a lambda throw a checked exception?** Only if the functional interface's abstract method declares it. `Callable` allows it; `Runnable`, `Function` and the other `java.util.function` interfaces do not, so you must catch it inside the lambda or wrap it in an unchecked exception.
- **What are `Predicate.and/or/negate`, `Function.andThen/compose`?** Default methods for combining functions. `and`, `or` and `negate` combine predicates logically. `f.andThen(g)` applies `f` then `g`; `f.compose(g)` applies `g` then `f`.
- **What is a closure? Is a Java lambda a closure?** A closure is a function that captures variables from its enclosing scope. Java lambdas are closures in a restricted form: they capture values of effectively final locals, not the variables themselves.

### Inner classes

- ⭐ **What are the types of nested classes in Java?** Static nested classes, and three kinds of inner (non-static) classes: member inner classes, method-local inner classes and anonymous inner classes.
- **What is a regular (member) inner class? How do you instantiate it from outside?** A non-static class declared inside another class. It is tied to an outer instance: `Outer o = new Outer(); Outer.Inner i = o.new Inner();`.
- **What is a static nested class? How does it differ from an inner class?** A nested class declared `static`. It does not need an outer instance (`new Outer.Nested()`) and can access only the outer class's static members directly. An inner class holds a hidden reference to the outer object.
- **What is a method-local inner class? What can it access?** A class declared inside a method. It is visible only within that method and can access the outer class's members and the method's final or effectively final local variables.
- **What is an anonymous inner class? Can it have a constructor? Can it implement more than one interface?** A class without a name, declared and instantiated in one expression. It cannot declare a constructor (it can use an instance initializer block), and it can extend one class or implement one interface, not both and not several.
- **Can an inner class access private members of the outer class? How is that implemented?** Yes. Before Java 11 the compiler generated synthetic accessor methods. Since Java 11, nest-based access control lets nest members access each other's private members directly.
- **Can an inner class have static members? What changed in Java 16?** Before Java 16 an inner class could only have `static final` compile-time constants. From Java 16 inner classes can declare static members.
- **How do you refer to the outer instance from an inner class?** With `Outer.this`, for example `Outer.this.x`.
- **What `.class` files are generated for inner and anonymous classes?** `Outer$Inner.class` for member and static nested classes, `Outer$1.class`, `Outer$2.class` for anonymous classes, and `Outer$1Local.class` for local classes.
- **How can a non-static inner class cause a memory leak?** Each inner object holds a hidden reference to its outer object. If the inner object outlives the outer (stored in a static field, a listener list or a long-running thread), the outer object cannot be garbage collected.
- **Why does `HashMap` use a static nested class for its nodes?** A node does not need access to the map instance. Making it static avoids storing an extra outer reference in every node, which saves memory.
- **Which access modifiers are allowed on nested classes versus top-level classes?** Top-level classes can be `public` or package-private only. Member nested classes can also be `private` or `protected`. Local and anonymous classes take no access modifier.
- **What is shadowing between inner and outer class fields?** If the inner class declares a field with the same name as an outer field, the inner one hides it. Use `this.x` for the inner field and `Outer.this.x` for the outer field.

### Enum

- ⭐ **What is an enum? Why use it instead of `int` constants?** A special class that defines a fixed set of named constants. It is type-safe (you cannot pass an arbitrary int), readable when printed, usable in `switch`, and can carry fields and behaviour.
- **Can an enum have fields, constructors and methods? What modifier does its constructor have?** Yes. The constructor is implicitly `private` (it cannot be public or protected), and it runs once per constant when the enum class is loaded.
- **Can an enum extend a class? Implement an interface? Be extended?** It cannot extend a class because it already extends `java.lang.Enum`. It can implement interfaces. It cannot be extended by another class.
- **What do `values()`, `valueOf()`, `ordinal()`, `name()` and `compareTo()` do?** `values()` returns all constants in an array. `valueOf(String)` returns the constant with that exact name or throws `IllegalArgumentException`. `ordinal()` returns the zero-based position. `name()` returns the declared name. `compareTo()` compares by ordinal.
- **Can you use an enum in a `switch`? Compare enums with `==`?** Yes to both. `==` is safe and preferred because each constant is a single instance, and it never throws a `NullPointerException`.
- **How is an enum compiled internally?** Into a final class extending `Enum`, with each constant as a `public static final` instance created in a static initializer, plus synthetic `values()` and `valueOf()` methods.
- **Can enum constants have their own method bodies?** Yes. Each constant can override a method, usually an abstract one declared in the enum: `PLUS { int apply(int a, int b) { return a + b; } }`. The constant is then an anonymous subclass.
- ⭐ **Why is an enum the best way to write a singleton?** The JVM guarantees one instance per constant, creation is thread-safe, serialization preserves the single instance automatically, and reflection cannot create another instance.
- **What are `EnumSet` and `EnumMap`? Why are they efficient?** Collections specialized for enum keys. `EnumSet` is backed by a bit vector and `EnumMap` by an array indexed by ordinal, so both are compact and very fast.
- **How do enums behave under serialization and reflection?** Only the constant's name is serialized, and deserialization resolves it back to the existing instance. Reflection refuses to instantiate an enum: `Constructor.newInstance` throws `IllegalArgumentException`.
- **Why should you not rely on `ordinal()` for persistence?** The ordinal changes if constants are reordered or a new one is inserted, which silently corrupts stored data. Persist the name or an explicit code field.
- **Can you create an enum instance with `new`?** No. Enum constructors are private and can be invoked only by the constant declarations.

### Code and scenario

- **Does this compile? `final int x; x = 10; x = 20;`** No. The second assignment is an error because a final variable can be assigned only once.
- **Does this compile? `final List<String> list = new ArrayList<>(); list.add("a"); list = new ArrayList<>();`** The `add` is fine because the object is mutable. The reassignment is a compile error.
- **What is printed? A lambda inside a loop capturing the loop variable `i` (classic `for` versus enhanced `for`).** With a classic `for (int i = 0; ...; i++)` it does not compile, because `i` is modified and so not effectively final. With an enhanced `for`, each iteration has a fresh variable, so it compiles and each lambda sees its own value.
- **Write a functional interface `Calculator` and implement it with an anonymous class and with a lambda.**
  ```java
  @FunctionalInterface interface Calculator { int calc(int a, int b); }
  Calculator add1 = new Calculator() { public int calc(int a, int b) { return a + b; } };
  Calculator add2 = (a, b) -> a + b;
  System.out.println(add1.calc(2, 3) + " " + add2.calc(2, 3)); // 5 5
  ```
- **Sort a list of employees by salary, then by name, using lambdas and method references.**
  ```java
  list.sort((a, b) -> Double.compare(a.getSalary(), b.getSalary()));                      // lambda
  list.sort(Comparator.comparingDouble(Employee::getSalary).thenComparing(Employee::getName)); // method references
  ```
- **Write an immutable `Employee` class with a `Date` field and a `List` field.**
  ```java
  public final class Employee {
      private final String name; private final Date joined; private final List<String> skills;
      public Employee(String name, Date joined, List<String> skills) {
          this.name = name; this.joined = new Date(joined.getTime()); this.skills = List.copyOf(skills);
      }
      public String getName() { return name; }
      public Date getJoined() { return new Date(joined.getTime()); }
      public List<String> getSkills() { return skills; } // already unmodifiable
  }
  ```
- **Write an enum `Day` with a method `isWeekend()`, and an enum `Operation` where each constant implements `apply`.**
  ```java
  enum Day { MON, TUE, WED, THU, FRI, SAT, SUN; boolean isWeekend() { return this == SAT || this == SUN; } }
  enum Operation {
      PLUS { int apply(int a, int b) { return a + b; } },
      TIMES { int apply(int a, int b) { return a * b; } };
      abstract int apply(int a, int b);
  }
  ```
- **Write an enum-based singleton.**
  ```java
  public enum Config { INSTANCE; private int timeout = 30; public int getTimeout() { return timeout; } }
  // usage: Config.INSTANCE.getTimeout()
  ```
- **Create an array of `Employee` and fill it. How many objects are on the heap after `new Employee[3]`, and after filling all three slots?** One object (the array) after `new Employee[3]`. Four after filling: the array plus three `Employee` objects, not counting any `String` objects they refer to.
- **Instantiate a member inner class and a static nested class from another class.** `Outer.Inner in = new Outer().new Inner();` and `Outer.Nested n = new Outer.Nested();`.
- **What is printed? An inner class field `x`, an outer field `x` and a local `x`, printed via `x`, `this.x` and `Outer.this.x`.** `x` prints the local variable, `this.x` the inner class field and `Outer.this.x` the outer class field.

---

## 9. Access modifiers and packages (Session 11)

### Basics

- ⭐ **What are the four access levels in Java? Draw the visibility table.**

  | Modifier | Same class | Same package | Subclass in another package | Everywhere |
  |---|---|---|---|---|
  | `private` | Yes | No | No | No |
  | default (no keyword) | Yes | Yes | No | No |
  | `protected` | Yes | Yes | Yes (through inheritance) | No |
  | `public` | Yes | Yes | Yes | Yes |

- ⭐ **What is the difference between `protected` and default (package-private) access?** Both are visible inside the same package. `protected` is additionally visible to subclasses in other packages, through inheritance. Default access stops at the package boundary.
- **Which access modifiers can a top-level class have?** Only `public` or default (package-private). `private` and `protected` are not allowed on top-level classes.
- **What is a package? Why use packages?** A namespace that groups related classes and interfaces. Packages avoid name clashes, organize code, control access (package-private and protected) and make code easier to find and reuse.
- **What is the naming convention for packages?** All lowercase, starting with the reversed domain name of the organization, for example `com.oxane.reports.util`.
- **What is the difference between `import pkg.*` and `import pkg.ClassName`? Does `*` include subpackages?** The single-type import brings in one class; the on-demand import makes all public types of that package available. `*` does not include subpackages: `import java.util.*` does not import `java.util.concurrent`.
- **Which package is imported by default?** `java.lang`, which contains `String`, `Object`, `System`, `Math` and the wrapper classes. Classes in the same package are also visible without import.
- **What is a static import? Give an example.** An import of static members so you can use them without the class name: `import static java.lang.Math.PI;` then `double a = PI * r * r;`.
- **What is a fully qualified class name? When do you have to use it?** The class name with its package, such as `java.util.Date`. You must use it when two imported packages contain classes with the same name, or when you choose not to import.
- **Where must the `package` statement appear? What is the order of `package`, `import` and class declarations?** `package` must be the first statement (only comments may precede it), then `import` statements, then type declarations. A file has at most one `package` statement.
- **What are non-access modifiers? List them.** `static`, `final`, `abstract`, `synchronized`, `volatile`, `transient`, `native` and `strictfp`. Newer ones for classes are `sealed` and `non-sealed`.

### Deep dive

- ⭐ **Explain exactly how a protected member is accessed outside its package. Can a subclass access it through a parent-type reference?** Outside the package, a subclass can access a protected member only through inheritance: on `this`, or on a reference whose type is the subclass itself or its own subclasses. It cannot access it through a reference of the parent type, so `new Parent().protectedMethod()` in a subclass in another package does not compile.
- **Can a private constructor be invoked from outside the class in any way?** Not through normal code. Reflection can do it with `setAccessible(true)`, unless the module system or a guard inside the constructor prevents it.
- **Can you reduce or increase the access level when overriding?** You can keep it or widen it (protected to public). You cannot narrow it, because a caller using the parent type must still be able to call the method.
- **Why are private methods not polymorphic?** They are not visible to subclasses, so they cannot be overridden. Calls to them are bound at compile time. A subclass method with the same name is an unrelated new method.
- **What happens when two imported packages contain a class with the same name (`java.util.Date` and `java.sql.Date`)?** With two wildcard imports, using `Date` is ambiguous and fails to compile. Import one explicitly (the explicit import wins over the wildcard) and use the fully qualified name for the other.
- **Does importing a class increase the size of the compiled class or affect runtime performance?** No. Imports are a compile-time convenience for resolving names. The class file stores fully qualified names only for the classes actually used.
- **What is the unnamed (default) package? Why avoid it?** Classes with no `package` statement go into the unnamed package. Classes in named packages cannot import them, name clashes are likely, and it does not work with modules, so it is only for small experiments.
- **How does the directory structure relate to the package name? How do you compile and run with `-d` and `-cp`?** Each package segment is a directory: `com.app.Main` lives in `com/app/Main.class`. `javac -d out src/com/app/Main.java` puts class files in the right structure under `out`, and `java -cp out com.app.Main` runs it using the fully qualified name.
- **What is a JAR file? What is the manifest?** A JAR is a ZIP archive of class files and resources. `META-INF/MANIFEST.MF` holds metadata; its `Main-Class` entry makes the JAR runnable with `java -jar app.jar`, and `Class-Path` lists dependencies.
- **What is the Java Platform Module System (Java 9)? How does `module-info.java` add a level above packages?** A module is a named group of packages. `module-info.java` declares which packages it `exports` and which modules it `requires`. A public class in a package that is not exported is not accessible outside the module, which gives strong encapsulation above the package level.
- **How does constructor chaining work across packages when the parent constructor is protected or default?** A subclass in another package can call a `protected` parent constructor with `super(...)`, but cannot use it with `new Parent()`. If the parent constructor has default access, the subclass in another package cannot call it at all, so it does not compile.
- **What is the principle of least privilege as it applies to access modifiers?** Give each member the most restrictive access that still works: start with `private` and widen only when needed. A smaller public surface means fewer dependencies and safer refactoring.
- **Which modifiers are legal on local variables?** Only `final`. Access modifiers and `static` are not allowed.
- **Can an interface member be protected or private?** Never `protected`. Fields and abstract methods are always public. Methods with a body can be `private` since Java 9.

### Code and scenario

- **Class `A` in package `p1` has a protected method. Class `B extends A` in `p2` does `new A().m()` and `new B().m()`. Which lines compile?** `new B().m()` compiles because the access is through the subclass type. `new A().m()` does not compile, because outside the package a protected member cannot be reached through a parent-type reference.
- **A class in `p2` (not a subclass) calls a default-access method of a class in `p1`. What happens?** Compile error. Default-access members are visible only inside their own package.
- **Does this compile? A subclass in another package where the parent has only a default-access constructor.** No. The subclass constructor must call `super()`, and the parent's package-private constructor is not accessible from another package.
- **Implement multilevel inheritance across three packages and chain constructors.**
  ```java
  package p1; public class Person { protected String name; protected Person(String name) { this.name = name; } }
  package p2; import p1.Person;
  public class Employee extends Person { protected int id; protected Employee(String name, int id) { super(name); this.id = id; } }
  package p3; import p2.Employee;
  public class Manager extends Employee { public Manager(String name, int id) { super(name, id); } public String info() { return name + ":" + id; } }
  ```
- **Write a demo showing all four access levels from the same class, same package, a subclass and an unrelated class.** Declare one class with a `private`, a default, a `protected` and a `public` field. Inside the class all four are accessible. Another class in the same package can use three (not private). A subclass in another package can use `protected` (through inheritance) and `public`. An unrelated class in another package can use only `public`. Each illegal access is a compile error, which is what the demo should show in comments.
- **Use a static import for `Math.PI` and `Math.sqrt`. What happens if a local method has the same name?** `import static java.lang.Math.PI; import static java.lang.Math.sqrt;` then `sqrt(PI)`. If the class declares its own `sqrt` method, the class's method shadows the statically imported one and is called instead.
- **Two top-level public classes in one file. What is the error?** "class X is public, should be declared in a file named X.java". Only one public top-level class is allowed, and it must match the file name.

---

## 10. Garbage collection (Session 12)

### Basics

- ⭐ **What is garbage collection? Why does Java have it?** The automatic process by which the JVM finds objects that are no longer reachable and frees their heap memory. It removes the need for manual `free`/`delete`, preventing dangling pointers, double frees and most memory leaks.
- ⭐ **When does an object become eligible for garbage collection?** When it is no longer reachable from any GC root, meaning no live thread can get to it through any chain of references.
- ⭐ **What are the ways to make an object eligible?** Set its reference to `null`; reassign the reference to another object; let a method-local reference go out of scope when the method returns; create an anonymous object; or leave objects in an island of isolation.
- **What is an island of isolation?** A group of objects that refer to each other but are not referenced from anywhere outside the group. Because the GC works by reachability from roots, the whole group is eligible for collection.
- ⭐ **Can you force garbage collection? What do `System.gc()` and `Runtime.getRuntime().gc()` do?** No. Both calls only request a collection; the JVM may ignore or delay it. `System.gc()` simply calls `Runtime.getRuntime().gc()`.
- **What is the `finalize()` method? When is it called, and how many times?** A protected method of `Object` that the GC may call once before reclaiming an object, intended for cleanup. It is called at most once per object.
- **Is `finalize()` guaranteed to run?** No. The object may never be collected before the JVM exits, and there is no guarantee about when or on which thread it runs.
- **Which thread runs the garbage collector? What is a daemon thread?** GC runs on low-priority background daemon threads managed by the JVM. A daemon thread is a service thread that does not keep the JVM alive: the JVM exits when only daemon threads remain.
- **Does GC guarantee a program will not run out of memory?** No. If objects stay reachable (for example in a growing static collection), they cannot be collected and the program can still throw `OutOfMemoryError`.
- **Does Java have destructors?** No. Memory is reclaimed by the GC. Other resources are released explicitly with `close()`, normally through try-with-resources.

### Deep dive

- ⭐ **How does the GC decide an object is unreachable? What are GC roots?** It starts from a set of roots and marks everything reachable from them; whatever is unmarked is garbage. Roots are local variables and parameters on thread stacks, static fields of loaded classes, active threads, and JNI references.
- **Why does Java use reachability analysis rather than reference counting?** Reference counting cannot collect cyclic references (two objects pointing to each other), and updating counters on every assignment is costly. Reachability handles cycles naturally.
- **What is the mark-and-sweep algorithm? What are mark-compact and copying collection?** Mark-and-sweep marks reachable objects and then frees the unmarked ones, which leaves fragmentation. Mark-compact also slides live objects together to remove fragmentation. Copying collection copies live objects into an empty space and discards the old space; it is fast when few objects survive.
- ⭐ **Explain generational GC.** Most objects die young, so the heap is split by age. New objects are allocated in eden. A minor GC copies survivors between two survivor spaces; objects that survive enough cycles are promoted to the old generation. A major GC collects the old generation, and a full GC collects the whole heap and is the most expensive.
- **What is a stop-the-world pause?** A period during which all application threads are suspended so the GC can work safely. Modern collectors aim to keep these pauses short.
- **Name the collectors. Which is the default in modern Java?** Serial (single-threaded, small heaps), Parallel (throughput), CMS (concurrent, removed in Java 14), G1 (region-based, the default since Java 9), ZGC and Shenandoah (very low pause, concurrent).
- **What is object promotion and the tenuring threshold?** Promotion is moving an object from the young to the old generation. The tenuring threshold is the number of minor collections an object must survive first (up to 15 by default; set with `-XX:MaxTenuringThreshold`).
- ⭐ **Can a Java program have a memory leak? Give examples.** Yes: objects that are no longer needed but still reachable. Examples are static collections that keep growing, caches without eviction, listeners or callbacks never unregistered, unclosed resources, `ThreadLocal` values in thread pools, inner class instances holding their outer object, and mutated keys stuck in a `HashMap`.
- ⭐ **What are strong, soft, weak and phantom references? Where is each used?** Strong: a normal reference; never collected while reachable. Soft: collected only when memory is low; used for caches. Weak: collected at the next GC once no strong reference exists; used in `WeakHashMap`. Phantom: `get()` always returns null; enqueued after the object is finalized; used for post-mortem cleanup.
- **What is `WeakHashMap`?** A map whose keys are held by weak references. When a key has no other strong reference, its entry is removed automatically after GC. Useful for caches and metadata attached to objects.
- **Why is `finalize()` deprecated? What are the alternatives?** It was deprecated in Java 9 and marked for removal in Java 18. It is unpredictable, slows collection, can resurrect objects, can hide exceptions and is a security risk. Use try-with-resources with `AutoCloseable`, or `java.lang.ref.Cleaner` as a safety net.
- **Can an object resurrect itself inside `finalize()`? What happens the next time it becomes unreachable?** Yes, by storing `this` in a reachable variable. Since `finalize()` runs only once per object, the next time it becomes unreachable it is collected without `finalize()` being called again.
- **What happens if `finalize()` throws an exception?** The exception is ignored, finalization of that object stops, and the object is still collected.
- **What is the difference between `OutOfMemoryError: Java heap space`, `GC overhead limit exceeded` and `Metaspace`?** "Java heap space": the heap is full of reachable objects. "GC overhead limit exceeded": the JVM is spending almost all its time in GC and recovering very little. "Metaspace": class metadata space is exhausted, usually due to too many loaded classes or a class loader leak.
- **How would you diagnose a memory leak?** Watch heap usage over time with `jstat` or GC logs; if the level after full GCs keeps rising, take a heap dump (`jmap` or `-XX:+HeapDumpOnOutOfMemoryError`), open it in Eclipse MAT or VisualVM, look at the dominator tree for the largest retained objects, and trace their path to GC roots.
- **Which JVM flags tune the heap and GC?** `-Xms`, `-Xmx` (heap size), `-Xmn` (young generation), `-XX:+UseG1GC` / `-XX:+UseZGC` (collector), `-XX:MaxGCPauseMillis` (pause goal), `-XX:MaxMetaspaceSize`, and `-Xlog:gc*` for GC logging.
- **Are static fields and string pool entries garbage collected?** Objects referenced by static fields stay alive as long as the class is loaded. String pool entries are on the heap since Java 7 and are collected when nothing references them.
- **What is `Runtime.totalMemory()`, `freeMemory()` and `maxMemory()`?** `totalMemory()` is the heap currently reserved by the JVM, `freeMemory()` the unused part of it, and `maxMemory()` the maximum the heap may grow to (`-Xmx`). Used memory is `totalMemory() - freeMemory()`.

### Code and scenario

- **How many objects are eligible for GC after each line? `A a1 = new A(); A a2 = new A(); a1 = a2; a2 = null; a1 = null;`** After `a1 = a2`, the first object is eligible (1). After `a2 = null`, still 1, because `a1` still refers to the second object. After `a1 = null`, both are eligible (2).
- **Two objects reference each other and both external references are set to `null`. Are they collected?** Yes. They form an island of isolation and are unreachable from any GC root, so both are eligible.
- **A method creates an object and returns it. Is it eligible after the method ends? What if it does not return it?** If it is returned and the caller keeps the reference, it stays alive. If it is not returned or stored anywhere, it becomes eligible when the method ends.
- **Override `finalize()` to print a message, call `System.gc()`, and explain why the message may or may not appear.** `System.gc()` is only a request, finalizers run later on a separate thread, and the JVM may exit first. On recent JDKs finalization may also be disabled, so the message is not guaranteed.
- **A long-lived static `List` keeps getting entries added. What is the problem and how do you fix it?** It is a memory leak: everything in the list stays reachable for the life of the class. Remove entries when done, bound its size, use a cache with eviction (LRU or time-based), or use weak references.
- **Write a stack implementation with an array and show the "obsolete reference" memory leak on `pop`.**
  ```java
  Object pop() {
      Object result = elements[--size];
      elements[size] = null; // without this line the array still references the popped object
      return result;
  }
  ```
- **An object is passed into a method and the parameter is set to `null` inside. Is the object eligible for GC?** No. Only the method's local copy of the reference is nulled; the caller's variable still refers to the object.

---

## 11. Wrapper classes and Strings (Sessions 13–14)

### Wrapper classes

- ⭐ **What are wrapper classes? Why are they needed?** Classes that wrap primitives in objects: `Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`, `Character`, `Boolean`. They are needed for collections and generics (which take only objects), to represent `null`, and for utility methods such as parsing and conversion.
- ⭐ **What are autoboxing and unboxing? Which methods does the compiler call?** Autoboxing is the automatic conversion of a primitive to its wrapper; unboxing is the reverse. The compiler inserts `Integer.valueOf(i)` for boxing and `intValue()` for unboxing.
- **What is the class hierarchy of the wrappers?** `Byte`, `Short`, `Integer`, `Long`, `Float` and `Double` extend the abstract class `Number`. `Character` and `Boolean` extend `Object` directly. All implement `Comparable` and `Serializable`.
- **Are wrapper classes immutable? Final?** Yes to both. A wrapper's value cannot change after creation, and the classes cannot be extended.
- ⭐ **What is the Integer cache? What range does it cover? Which other wrappers cache?** `Integer.valueOf` returns cached objects for values from −128 to 127, so boxing in that range gives the same object. `Byte`, `Short` and `Long` cache the same range, `Character` caches 0 to 127, and `Boolean` caches `TRUE` and `FALSE`. `Float` and `Double` do not cache.
- **What is the difference between `Integer.valueOf()`, `Integer.parseInt()` and `new Integer()`? Why is the constructor deprecated?** `parseInt(String)` returns a primitive `int`. `valueOf` returns an `Integer`, using the cache. `new Integer()` always creates a new object, wasting memory, which is why it has been deprecated since Java 9.
- **How do you convert between `String`, primitive and wrapper?** String to primitive: `Integer.parseInt(s)`. String to wrapper: `Integer.valueOf(s)`. Primitive to String: `String.valueOf(i)` or `Integer.toString(i)`. Wrapper to primitive: `intValue()` or unboxing. Primitive to wrapper: `Integer.valueOf(i)` or autoboxing.
- **What happens when you unbox a `null` wrapper?** A `NullPointerException`, because unboxing calls a method such as `intValue()` on `null`.
- **What do `Integer.MAX_VALUE`, `MIN_VALUE`, `Integer.toBinaryString`, `Integer.compare`, `Character.isDigit` do?** `MAX_VALUE` and `MIN_VALUE` are the limits of `int`. `toBinaryString` returns the binary text of a number. `Integer.compare(a, b)` returns a negative, zero or positive result safely without overflow. `Character.isDigit` tests whether a char is a digit; similar helpers are `isLetter`, `isUpperCase` and `toUpperCase`.
- **What is the performance cost of autoboxing in a loop?** Each boxing outside the cache range allocates an object. `Long sum = 0L; for (...) sum += i;` creates a new `Long` on every iteration, which is much slower and creates garbage. Use primitives for arithmetic.
- **How do `==` and `equals` behave between `Integer` and `Long` holding the same number?** `==` between an `Integer` reference and a `Long` reference does not compile (incomparable types). `equals` returns `false`, because it checks the class first.
- **What is `NumberFormatException`?** An unchecked exception thrown when a string cannot be parsed as a number, for example `Integer.parseInt("abc")` or `Integer.parseInt("")`.
- **Why can collections not hold primitives?** Generics work only with reference types because of type erasure: type parameters become `Object`, and primitives are not objects. Autoboxing hides this by wrapping them.

### String

- ⭐ **Why is `String` immutable? What are the benefits?** So strings can be shared safely. Benefits: the string pool can reuse literals to save memory; strings are thread-safe; the hash code can be cached, making them fast map keys; and it is secure, since file paths, URLs and class names cannot be changed after validation.
- ⭐ **What is the String constant pool? Where is it stored?** A table of unique string literals maintained by the JVM, so identical literals share one object. It was in PermGen before Java 7 and has been in the heap since Java 7, where it can be garbage collected.
- ⭐ **What is the difference between `String s = "abc"` and `String s = new String("abc")`? How many objects does each create?** The literal form uses the pool and creates at most one object (none if `"abc"` is already pooled). `new String("abc")` always creates a new object on the heap, plus the pooled literal if it was not already there, so up to two.
- ⭐ **What is the difference between `==` and `equals()` for strings?** `==` checks whether both references point to the same object. `equals()` checks whether the character sequences are the same. Always use `equals()` to compare string content.
- ⭐ **What is the difference between `String`, `StringBuffer` and `StringBuilder`?** `String` is immutable; every modification creates a new object. `StringBuffer` is mutable and thread-safe because its methods are synchronized, so it is slower. `StringBuilder` is mutable and not synchronized, so it is the fastest; use it in single-threaded code.
- **What does `intern()` do?** It returns the pooled copy of the string, adding it to the pool if absent. After `s = s.intern()`, `s` can be compared with literals using `==`.
- **Why is `String` final?** To stop subclasses from overriding its behaviour and breaking immutability, which pooling, hashing and security depend on.
- **Why is `String` a good `HashMap` key? Why is its hash code cached?** It is immutable, so its hash code never changes after insertion, and it implements `equals` and `hashCode` properly. Because the content cannot change, the hash is computed once and stored in a field, making repeated lookups fast.
- **Why is `char[]` preferred over `String` for passwords?** A `String` is immutable and stays in memory, possibly in the pool, until garbage collected, so it can appear in a heap dump. A `char[]` can be overwritten with zeros immediately after use.
- **How does `+` concatenation compile? Why is concatenation in a loop slow?** Before Java 9 the compiler used `StringBuilder.append` calls; from Java 9 it uses `invokedynamic` with `StringConcatFactory`. In a loop, each iteration still creates a new String and copies all previous characters, giving O(n²) work. Use one `StringBuilder` outside the loop.
- **What are compact strings (Java 9)? How is a `String` stored internally?** Since Java 9 a `String` holds a `byte[]` plus a `coder` flag instead of a `char[]`. Strings with only Latin-1 characters use one byte per character; others use UTF-16 with two bytes. This roughly halves memory for typical text.
- **Explain common String methods.** `charAt(i)`: character at index. `substring(a, b)`: part from a to b−1. `indexOf`: first position or −1. `split(regex)`: array of parts. `trim`/`strip`: remove surrounding whitespace. `replace`: literal replacement. `replaceAll`: regex replacement. `compareTo`: lexicographic comparison. `equalsIgnoreCase`: case-insensitive equality. `contains`: substring test. `String.join`: join with a delimiter. `String.format`: formatted text. `isEmpty`: length is 0. `isBlank`: empty or only whitespace. `repeat(n)`: repeat n times. `chars()`: an `IntStream` of characters.
- **What is the difference between `trim()` and `strip()`? Between `isEmpty()` and `isBlank()`?** `trim()` removes characters with code ≤ U+0020 (ASCII whitespace). `strip()` (Java 11) is Unicode-aware. `isEmpty()` is true only for length 0; `isBlank()` (Java 11) is also true for whitespace-only strings.
- **What is the default capacity of `StringBuilder`? How does it grow?** 16 characters, or the length of the initial string plus 16. When full, the new capacity is (old capacity × 2) + 2, and the contents are copied to a new array.
- **Does `StringBuilder` override `equals` and `hashCode`?** No. It inherits them from `Object`, so `equals` compares references. Compare content with `sb1.toString().equals(sb2.toString())` or `sb1.compareTo(sb2) == 0` (Java 11).
- **Is `String` thread-safe? Is `StringBuffer`? How?** `String` is thread-safe because it is immutable. `StringBuffer` is thread-safe because its methods are synchronized. `StringBuilder` is not thread-safe.
- **What are text blocks (Java 15)?** Multi-line string literals delimited by triple quotes `"""`. They keep line breaks and need no escaping of quotes, which is handy for JSON, SQL and HTML.
- **What is the difference between `substring` behaviour in Java 6 and Java 7+?** In Java 6, `substring` shared the original character array, so a small substring could keep a huge array alive, a memory leak. From Java 7 update 6 it copies the needed characters into a new array.
- **How does `String.hashCode()` work?** It computes `s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]` using `int` arithmetic and caches the result. 31 is an odd prime and multiplication by it is cheap (`(x << 5) - x`).
- **What does `compareTo` return, and how is it different from `equals`?** It returns a negative number, zero or a positive number depending on lexicographic order: the difference of the first differing characters, or of the lengths. `equals` returns only a boolean for equality.

### Code and scenario

- ⭐ **What is printed? `Integer a = 127, b = 127; a == b` and `Integer c = 128, d = 128; c == d`** `true` then `false`. 127 is inside the Integer cache, so both refer to the same object; 128 is outside it, so two different objects are created. Use `equals` to compare wrapper values.
- **What is printed? `Long l = 10L; System.out.println(l.equals(10));`** `false`. `10` is boxed to an `Integer`, and `Long.equals` returns false for a non-`Long` argument.
- **What happens? `Integer i = null; int j = i;`** `NullPointerException` at runtime during unboxing.
- ⭐ **What is printed? `s1 = "java"; s2 = "java"; s3 = new String("java");` then `s1 == s2`, `s1 == s3`, `s1 == s3.intern()`** `true`, `false`, `true`. Both literals share the pooled object; `new` creates a separate heap object; `intern()` returns the pooled one.
- **What is printed? `String s = "a"; s.concat("b"); System.out.println(s);`** `a`. `concat` returns a new string, which is discarded; the original is unchanged because strings are immutable.
- **What is printed? `"ja" + "va" == "java"`, and the same when `"ja"` is held in a non-final variable, then in a final variable.** `true` for the two literals, because the compiler folds constants into `"java"`. `false` with a non-final variable, because concatenation happens at runtime and creates a new object. `true` with a final variable, because it is a compile-time constant again.
- **How many objects are created by `String s = new String("a") + new String("b");`?** The two literals `"a"` and `"b"` in the pool, two `String` objects from `new`, and the result `"ab"` on the heap (not in the pool). Before Java 9 a temporary `StringBuilder` is also created, so the usual answer is five or six objects.
- **What is printed? A method appends to a `StringBuilder` parameter, versus the same with `String`.** The `StringBuilder` change is visible to the caller because the same mutable object is modified. The `String` version shows no change because concatenation creates a new object assigned only to the local parameter.
- **Write code to reverse a string, check a palindrome, check an anagram, count vowels, count character frequencies, find the first non-repeated character, remove duplicates, and reverse the words in a sentence.**
  ```java
  String rev = new StringBuilder(s).reverse().toString();
  boolean palin = s.equals(rev);
  boolean anagram(String a, String b) { char[] x = a.toCharArray(), y = b.toCharArray(); Arrays.sort(x); Arrays.sort(y); return Arrays.equals(x, y); }
  long vowels = s.toLowerCase().chars().filter(c -> "aeiou".indexOf(c) >= 0).count();
  Map<Character, Integer> freq = new LinkedHashMap<>();
  for (char c : s.toCharArray()) freq.merge(c, 1, Integer::sum);
  Character firstUnique = freq.entrySet().stream().filter(e -> e.getValue() == 1).map(Map.Entry::getKey).findFirst().orElse(null);
  String noDup = s.chars().distinct().mapToObj(c -> String.valueOf((char) c)).collect(Collectors.joining());
  String[] w = sentence.trim().split("\\s+"); Collections.reverse(Arrays.asList(w)); String reversedWords = String.join(" ", w);
  ```
- **Write code to check whether one string is a rotation of another.** `boolean isRotation(String a, String b) { return a.length() == b.length() && (a + a).contains(b); }`. Every rotation of `a` appears inside `a + a`.
- **Write code to find the longest substring without repeating characters.**
  ```java
  int longest(String s) {
      Map<Character, Integer> last = new HashMap<>(); int best = 0, start = 0;
      for (int i = 0; i < s.length(); i++) {
          char c = s.charAt(i);
          if (last.containsKey(c) && last.get(c) >= start) start = last.get(c) + 1;
          last.put(c, i); best = Math.max(best, i - start + 1);
      }
      return best; // sliding window, O(n)
  }
  ```
- **Write code to convert a `String` to an `int` without `parseInt`.**
  ```java
  int toInt(String s) {
      int i = 0, sign = 1, n = 0;
      if (s.charAt(0) == '-') { sign = -1; i = 1; }
      for (; i < s.length(); i++) {
          char c = s.charAt(i);
          if (c < '0' || c > '9') throw new NumberFormatException(s);
          n = n * 10 + (c - '0');
      }
      return sign * n;
  }
  ```
- **Which overload runs for `m(10)` given `m(long)`, `m(Integer)` and `m(Object)`?** `m(long)`. Widening `int` to `long` is preferred over boxing to `Integer`.

---

## 12. Exception handling (Sessions 15–16)

### Basics

- ⭐ **What is an exception? What is exception handling?** An exception is an event that disrupts the normal flow of a program at runtime, represented by an object. Exception handling is the mechanism (`try`, `catch`, `finally`, `throw`, `throws`) for dealing with it so the program can recover or fail gracefully.
- ⭐ **Draw the exception hierarchy from `Throwable`.** `Throwable` has two children. `Error` (for example `OutOfMemoryError`, `StackOverflowError`) covers serious JVM problems. `Exception` covers recoverable conditions: `RuntimeException` and its subclasses (`NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBoundsException`, `ClassCastException`, `IllegalArgumentException`) are unchecked; all other `Exception` subclasses (`IOException`, `SQLException`, `ClassNotFoundException`) are checked.
- ⭐ **What is the difference between `Error` and `Exception`?** An `Error` is a serious problem in the JVM or environment that an application should not try to handle. An `Exception` is a condition the application can reasonably anticipate and recover from.
- ⭐ **What is the difference between checked and unchecked exceptions? Give examples.** Checked exceptions are verified at compile time: you must catch them or declare them with `throws` (`IOException`, `SQLException`). Unchecked exceptions are subclasses of `RuntimeException` (and `Error`); the compiler does not force handling (`NullPointerException`, `ArithmeticException`).
- ⭐ **What is the difference between `throw` and `throws`?** `throw` is a statement inside a method that actually throws one exception object. `throws` is part of the method signature and declares the exceptions the method might throw; it can list several.
- ⭐ **What is the `finally` block? Does it always execute?** A block after try/catch that runs whether or not an exception occurred, used for cleanup. It does not run if `System.exit()` is called, the JVM crashes, the thread is killed, or the try block never ends.
- **What is a try-catch-finally block? Which combinations are legal?** Legal forms are try-catch, try-finally, try-catch-finally, and try-with-resources alone. A plain `try` with neither `catch` nor `finally` is not allowed.
- **What is a multi-catch block (Java 7)? What restriction applies?** `catch (IOException | SQLException e)` handles several exception types in one block. The types must not be related by inheritance, and the parameter `e` is implicitly final.
- **What is the order of catch blocks? What happens if a parent type comes first?** Catch blocks go from most specific to most general. If a parent type comes before its child, the child's block is unreachable and the code does not compile.
- ⭐ **How do you create a custom checked exception and a custom unchecked exception?** Extend `Exception` for a checked one and `RuntimeException` for an unchecked one, and provide constructors that pass the message and cause to `super`.
- **What is exception propagation?** If a method does not catch an exception, it passes up the call stack to the caller, and so on until it is caught or reaches the top, where the thread terminates. Unchecked exceptions propagate automatically; checked ones need `throws` on each method.
- **What do `getMessage()`, `printStackTrace()`, `getCause()` and `getStackTrace()` do?** `getMessage()` returns the detail message. `printStackTrace()` prints the exception and its call stack to standard error. `getCause()` returns the wrapped original exception. `getStackTrace()` returns the stack frames as an array.
- **Name common runtime exceptions and when each occurs.** `NullPointerException`: using a null reference. `ArithmeticException`: integer division by zero. `ArrayIndexOutOfBoundsException`: invalid array index. `ClassCastException`: invalid cast. `NumberFormatException`: bad numeric string. `IllegalArgumentException`: invalid argument. `IllegalStateException`: call at the wrong time. `ConcurrentModificationException`: collection modified during iteration.
- **What is a nested try block?** A try block inside another try (or catch or finally) block. If the inner block has no matching catch, the outer block's handlers are checked.

### Deep dive

- ⭐ **What is try-with-resources? What are `AutoCloseable` and `Closeable`?** `try (Resource r = ...) { }` closes the resources automatically when the block ends, even on an exception. A resource must implement `AutoCloseable` (`close() throws Exception`). `Closeable` extends it for I/O classes (`close() throws IOException`).
- **In what order are resources closed? What are suppressed exceptions?** In the reverse order of declaration. If the try body throws and `close()` also throws, the body's exception is propagated and the close exceptions are attached to it as suppressed exceptions, available through `getSuppressed()`.
- ⭐ **In which cases does `finally` not execute?** When `System.exit()` or `Runtime.halt()` is called, the JVM crashes or the process is killed, a daemon thread is abandoned at JVM shutdown, or the try block loops forever.
- **What happens if both `try` and `finally` have a `return`? If `finally` throws?** The `return` in `finally` overrides the one in `try`, and it also discards any pending exception. If `finally` throws, that exception replaces whatever `try` returned or threw. Both are bad practice.
- **What happens to a returned primitive versus a returned object reference modified in `finally`?** The return value is evaluated and saved before `finally` runs. Changing a primitive variable in `finally` does not affect the saved value. For an object, the saved reference is unchanged, but changes to the object's state are visible to the caller.
- **How do checked and unchecked exceptions propagate differently?** Unchecked exceptions propagate up the stack automatically. Checked exceptions also propagate, but only if every method on the way declares them with `throws`; otherwise the code does not compile.
- **What are the rules for exceptions with method overriding?** The overriding method may throw the same checked exceptions, subclasses of them, or none. It may not throw new or broader checked exceptions. It may throw any unchecked exception.
- **What is exception chaining? Why keep the cause?** Wrapping a low-level exception in a higher-level one: `throw new ServiceException("Save failed", e);`. Keeping the cause preserves the original stack trace, so you can see the root problem while exposing an exception that fits your layer.
- **What is rethrowing? What is precise rethrow (Java 7)?** Rethrowing is catching an exception and throwing it again, usually after logging. With precise rethrow, if you `catch (Exception e)` and rethrow `e` without modifying it, the compiler knows the actual checked types the try block can throw, so the method can declare just those.
- **Can you catch `Error` or `Throwable`? Should you?** You can, but generally should not, because errors indicate conditions you cannot recover from. It is acceptable at the top level of a thread or framework to log and shut down cleanly.
- **Can you catch `StackOverflowError` or `OutOfMemoryError`?** Technically yes, since they are `Throwable`s, but the JVM may be in an unstable state afterwards, so recovery is unreliable.
- **What is the difference between `ClassNotFoundException` and `NoClassDefFoundError`?** `ClassNotFoundException` is a checked exception from loading a class by name at runtime when it is not on the classpath. `NoClassDefFoundError` is an error when a class that compiled fine is missing at runtime or its static initializer failed.
- **What happens when an exception is thrown from a static initializer?** The JVM wraps it in `ExceptionInInitializerError`, and the class is marked as failed. Later attempts to use the class throw `NoClassDefFoundError`.
- **What happens to an uncaught exception in `main`? In another thread? What is an `UncaughtExceptionHandler`?** In either case that thread terminates and the stack trace is printed; other threads keep running. An `UncaughtExceptionHandler`, set per thread or as a global default, is a callback invoked when a thread dies from an uncaught exception, used for logging or alerts.
- **When would you design a checked versus an unchecked exception?** Checked for recoverable conditions the caller is expected to handle, such as a missing file. Unchecked for programming errors or violated preconditions, such as an invalid argument. Many modern codebases favour unchecked exceptions to keep APIs clean.
- **What are exception handling best practices?** Catch specific exceptions; never leave a catch block empty; do not use exceptions for normal control flow; either log or rethrow, not both; keep the cause when wrapping; use try-with-resources; throw early and catch late; and write meaningful messages.
- **What is the performance cost of exceptions? Where does it come from?** Most of the cost is filling in the stack trace when the exception object is created, plus unwinding the stack. A try block with no exception thrown costs almost nothing.
- **Can a constructor, static block or `main` declare `throws`?** A constructor and `main` can. A static block cannot declare `throws` and must not let a checked exception escape; it has to catch it.
- **Can you throw an exception from a lambda used in a stream?** Unchecked exceptions, yes. Checked exceptions, no, because `Function`, `Predicate` and the others do not declare any. Catch inside the lambda and wrap in a `RuntimeException`, or write a wrapper helper.
- **What is the helpful `NullPointerException` message feature (Java 14)?** The JVM now reports exactly which variable or method call was null, for example `Cannot invoke "String.length()" because "name" is null`. It is enabled by default from Java 15.
- **Why is a catch block for a checked exception that the try body never throws a compile error, while `catch (Exception e)` is fine?** The compiler knows precisely which checked exceptions the try body can throw, so catching an impossible one is flagged as unreachable. `Exception` also covers unchecked exceptions, which any code can throw.

### Code and scenario

- ⭐ **What is returned? `try { return 1; } catch (Exception e) { return 2; } finally { return 3; }`** `3`. The `return` in `finally` overrides the one in `try`.
- **What is returned? `int x = 1; try { return x; } finally { x = 2; }`** `1`. The value to return is saved before `finally` runs, so changing `x` afterwards has no effect.
- **What is printed? `try` calls `System.exit(0)` and `finally` prints a line.** Nothing from `finally`. The JVM stops immediately.
- **Does this compile? `catch (Exception e)` followed by `catch (IOException e)`.** No. `IOException` has already been caught by the broader block, so the second block is unreachable.
- **Does this compile? `catch (IOException | FileNotFoundException e)`.** No. The alternatives in a multi-catch cannot be related by subclassing.
- **What is printed? A method throws in `try`, `finally` also throws. Which exception reaches the caller?** The exception from `finally`. The original exception from `try` is lost.
- **What is printed? Three-method call chain where the deepest throws an `ArithmeticException` and only `main` catches.** The exception propagates up through the two intermediate methods, skipping the rest of their code, and `main`'s catch block handles it. The stack trace lists all three methods.
- **Write `InsufficientFundsException` (checked) and `InvalidAgeException` (unchecked) and use them.**
  ```java
  class InsufficientFundsException extends Exception { InsufficientFundsException(String msg) { super(msg); } }
  class InvalidAgeException extends RuntimeException { InvalidAgeException(String msg) { super(msg); } }
  void withdraw(double amt) throws InsufficientFundsException {
      if (amt > balance) throw new InsufficientFundsException("Balance is only " + balance);
      balance -= amt;
  }
  void setAge(int age) { if (age < 18) throw new InvalidAgeException("Age must be 18 or more"); this.age = age; }
  ```
- **Rewrite a try-finally that closes a `BufferedReader` using try-with-resources.**
  ```java
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
      String line;
      while ((line = br.readLine()) != null) System.out.println(line);
  } catch (IOException e) { e.printStackTrace(); }
  ```
- **Write a custom `AutoCloseable` resource and show the close order for two resources.**
  ```java
  class Res implements AutoCloseable {
      private final String name; Res(String name) { this.name = name; System.out.println("open " + name); }
      @Override public void close() { System.out.println("close " + name); }
  }
  try (Res a = new Res("A"); Res b = new Res("B")) { System.out.println("work"); }
  // open A, open B, work, close B, close A
  ```
- **Wrap a low-level `SQLException` in a custom exception while preserving the cause.**
  ```java
  class DataAccessException extends RuntimeException { DataAccessException(String msg, Throwable cause) { super(msg, cause); } }
  try { repository.save(emp); }
  catch (SQLException e) { throw new DataAccessException("Could not save employee " + emp.getId(), e); }
  ```

---

## 13. java.io, java.nio, serialization, cloning (Session 17)

### java.io

- ⭐ **What is a stream in Java I/O? What is the difference between byte streams and character streams?** A stream is a sequence of data flowing from a source or to a destination. Byte streams (`InputStream`, `OutputStream`) handle raw 8-bit data such as images and binary files. Character streams (`Reader`, `Writer`) handle 16-bit Unicode text and apply a character encoding.
- **What are `InputStream`, `OutputStream`, `Reader` and `Writer`? Are they interfaces or abstract classes?** They are the four root types of java.io, and all four are abstract classes, not interfaces. `InputStream` and `OutputStream` read and write bytes; `Reader` and `Writer` read and write characters.
- **What is the difference between `FileInputStream` and `FileReader`?** `FileInputStream` reads raw bytes and suits binary files. `FileReader` reads characters, decoding bytes with a charset, and suits text files.
- **What are buffered streams? Why are they faster?** `BufferedInputStream`, `BufferedOutputStream`, `BufferedReader` and `BufferedWriter` keep an in-memory buffer (8 KB by default). They read or write large chunks at once, which reduces the number of expensive system calls to the disk.
- **What are `InputStreamReader` and `OutputStreamWriter`? What is a charset?** Bridge classes that convert between byte streams and character streams using a charset. A charset is a mapping between characters and bytes, such as UTF-8 or ISO-8859-1. Always specify it explicitly to avoid platform-dependent behaviour.
- **What are `DataInputStream`/`DataOutputStream` and `ObjectInputStream`/`ObjectOutputStream`?** Data streams read and write primitives and strings in a portable binary format (`readInt`, `writeDouble`). Object streams read and write whole objects through serialization (`readObject`, `writeObject`).
- **What is `PrintStream` versus `PrintWriter`? What are `System.in`, `System.out` and `System.err`?** `PrintStream` writes formatted output as bytes; `PrintWriter` writes it as characters. Neither throws `IOException` from print methods. `System.in` is an `InputStream` for standard input; `System.out` and `System.err` are `PrintStream`s for standard output and error.
- **What is the `File` class? Does creating a `File` object create a file?** `File` represents a file or directory path and provides operations such as `exists`, `length`, `delete`, `mkdir` and `listFiles`. Creating a `File` object does not create anything on disk; call `createNewFile()` or write through a stream.
- **What is the difference between `Scanner` and `BufferedReader`?** `Scanner` parses tokens into types (`nextInt`, `nextDouble`) using regular expressions, has a small buffer and is slower. `BufferedReader` only reads characters and lines, has a larger buffer, is faster and is synchronized.
- **Which design pattern does java.io use?** The decorator pattern. Streams wrap other streams to add behaviour: `new BufferedReader(new InputStreamReader(new FileInputStream(f)))`.
- **What is `RandomAccessFile`?** A class that reads and writes at any position in a file using a file pointer you can move with `seek()`. It supports both reading and writing in one object.
- **What does `flush()` do? What does `close()` do?** `flush()` forces buffered data to be written to the destination. `close()` flushes and then releases the underlying system resource; the stream cannot be used afterwards.
- **How do you read a text file line by line? How do you copy an image file?** Text: `BufferedReader.readLine()` in a loop, or `Files.lines(path)`. Image: read bytes from a `FileInputStream` into a buffer and write them to a `FileOutputStream`, or simply `Files.copy(src, dst)`.
- **Why should you always close streams, and what is the safest way?** Open streams hold file descriptors and may hold unflushed data; not closing them leaks resources and can lose data. Use try-with-resources, which closes them automatically even when an exception occurs.

### java.nio

- ⭐ **What is the difference between IO and NIO?** Classic IO is stream-oriented and blocking: a thread waits for each read or write. NIO is buffer-oriented, uses channels, and supports non-blocking mode with selectors, so one thread can manage many connections.
- **What are buffers, channels and selectors?** A buffer is a fixed-size container of data that you read from and write to. A channel is a two-way connection to a file or socket that transfers data to and from buffers. A selector lets one thread monitor many channels for readiness events.
- **What are a buffer's capacity, position and limit? What do `flip()`, `clear()`, `rewind()` and `compact()` do?** Capacity is the fixed size; position is the next index to read or write; limit is the first index that must not be read or written. `flip()` switches from writing to reading (limit = position, position = 0). `clear()` resets for writing. `rewind()` sets position to 0 to reread. `compact()` moves unread data to the start so writing can continue.
- **What is the difference between blocking and non-blocking I/O?** In blocking I/O the thread waits until data is available or written. In non-blocking I/O the call returns immediately with whatever is ready, and the thread can do other work and come back later.
- **What are `Path`, `Paths` and `Files` (NIO.2)? How do they improve on `File`?** `Path` represents a file path, `Paths.get()` (or `Path.of()`) creates one, and `Files` has static methods for file operations. Compared with `File`, they throw meaningful exceptions instead of returning `false`, support symbolic links, file attributes, directory walking and watching, and have simple one-line read and write methods.
- **What do `Files.readAllLines`, `Files.lines`, `Files.write`, `Files.copy` and `Files.walk` do?** `readAllLines` loads the whole file into a `List<String>`. `lines` returns a lazy `Stream<String>`, suitable for large files. `write` writes bytes or lines to a file. `copy` copies a file or stream. `walk` returns a stream of all paths under a directory, recursively.
- **What is a direct buffer versus a heap buffer?** A heap buffer is backed by a byte array on the Java heap. A direct buffer is allocated in native memory outside the heap, which avoids a copy during I/O and is faster for large or long-lived buffers, but costs more to allocate and free.
- **What is a memory-mapped file?** A file region mapped directly into memory with `FileChannel.map()`, so you access the file like an array and the OS handles paging. It is very fast for large files and random access.
- **What is `WatchService`?** An API to watch a directory for changes. You register a `Path` for create, modify and delete events and poll for `WatchKey`s.

### Serialization

- ⭐ **What is serialization? What is deserialization?** Serialization converts an object's state into a byte stream so it can be saved to a file or sent over a network. Deserialization rebuilds the object from that byte stream.
- ⭐ **What is the `Serializable` interface? Why is it a marker interface?** An interface in `java.io` that a class implements to allow its objects to be serialized. It has no methods; it only marks the class so `ObjectOutputStream` knows it is permitted, otherwise it throws `NotSerializableException`.
- ⭐ **What is `serialVersionUID`? What happens if it is not declared, or if it changes?** A version number for a serializable class, checked during deserialization. If not declared, the JVM computes one from the class structure, so any change to the class changes it. If the stream's value does not match the class's value, deserialization fails with `InvalidClassException`. Declare it explicitly to stay in control.
- ⭐ **What is the `transient` keyword?** A modifier that excludes a field from serialization. After deserialization the field has its default value. Use it for passwords, derived values and non-serializable fields.
- **Are static fields serialized?** No. They belong to the class, not the object, so they are not part of the object's state.
- **What happens if a class has a non-serializable field?** Serialization throws `NotSerializableException` at runtime, unless the field is `transient` or `null`.
- **What happens when the parent is serializable but the child is not, and the reverse? Which constructors run during deserialization?** If the parent is serializable, the child is automatically serializable. If only the child is, the parent's fields are not saved; during deserialization the no-arg constructor of the first non-serializable superclass runs to initialize them, and it must be accessible or you get `InvalidClassException`. Constructors of serializable classes do not run.
- **What is `Externalizable`? How does it differ from `Serializable`?** An interface extending `Serializable` with `writeExternal` and `readExternal`, giving you full control over what is written. It requires a public no-arg constructor, which is called on deserialization. It can be faster and more compact, but you write all the logic yourself.
- **What are `writeObject`, `readObject`, `readResolve` and `writeReplace`?** Private `writeObject` and `readObject` methods customize how an object is written and read, for example to encrypt a field or validate state. `readResolve` replaces the deserialized object with another, used to keep singletons single. `writeReplace` substitutes a different object to be serialized.
- **How does serialization break a singleton, and how do you prevent it?** Deserialization creates a new object without calling the constructor, giving a second instance. Add `private Object readResolve() { return INSTANCE; }`, or use an enum singleton.
- **Is a constructor called during deserialization?** Not for serializable classes; the object is created without running their constructors. Only the no-arg constructor of the nearest non-serializable superclass runs.
- **How do you prevent a subclass of a serializable class from being serialized?** Implement private `writeObject` and `readObject` in the subclass and throw `NotSerializableException` from them.
- **What is the security risk of Java deserialization?** Deserializing untrusted data can run code through carefully chosen classes on the classpath ("gadget chains"), leading to remote code execution. Never deserialize untrusted input, use serialization filters (`ObjectInputFilter`), or use a data format such as JSON.
- **What happens if the same object is written twice to an `ObjectOutputStream`?** The second write stores only a back-reference to the first. Changes made to the object between the two writes are not saved unless you call `reset()` or use `writeUnshared()`.
- **What is the combination `transient final` or `transient static`?** `transient static` is redundant because static fields are never serialized. `transient final` is legal, but after deserialization the field has its default value and cannot be reassigned normally; a final field initialized with a compile-time constant keeps its value.
- **What are the alternatives to Java serialization?** JSON (Jackson, Gson), XML, Protocol Buffers, Avro and Kryo. They are language-neutral, usually smaller and faster, and safer.

### Shallow and deep copy

- ⭐ **What is the difference between a shallow copy and a deep copy?** A shallow copy creates a new object but copies references, so the copy and the original share the same nested objects. A deep copy also copies the nested objects recursively, so the two are fully independent.
- **What is the `Cloneable` interface? What does `Object.clone()` do?** `Cloneable` is a marker interface that permits cloning. `Object.clone()` creates a new object of the same class and copies all fields one by one, which is a shallow copy.
- **What happens if you call `clone()` on a class that does not implement `Cloneable`?** `Object.clone()` throws `CloneNotSupportedException`.
- **Why is `clone()` protected in `Object`?** So that objects cannot be cloned by arbitrary code unless the class opts in by overriding `clone()` as public and implementing `Cloneable`.
- **How do you implement a deep copy?** Override `clone()`, call `super.clone()`, then clone or copy each mutable field. Alternatively write a copy constructor or static factory, or serialize and deserialize the object (simple but slow, and everything must be serializable).
- **Why is the copy constructor often preferred over `clone()`?** It needs no marker interface, no cast, no checked exception, works with final fields, and makes it explicit how deep the copy is. *Effective Java* recommends it over `clone()`.
- **Is a constructor called during `clone()`?** No. `Object.clone()` creates the object by copying memory, without running any constructor.
- **How do you clone an array? Is that shallow or deep?** `int[] b = a.clone();`. It is shallow: for an array of primitives the result is independent, but for an array of objects both arrays refer to the same elements.
- **How does cloning interact with final fields and with immutable fields like `String`?** A final field cannot be reassigned after `super.clone()`, so you cannot deep-copy a mutable final field in `clone()`; use a copy constructor instead. Immutable fields such as `String` and wrappers can be shared safely and need no copying.

### Code and scenario

- **Write code to read a text file and count lines, words and characters.**
  ```java
  int lines = 0, words = 0, chars = 0;
  try (BufferedReader br = Files.newBufferedReader(Path.of("data.txt"))) {
      String line;
      while ((line = br.readLine()) != null) {
          lines++; chars += line.length();
          if (!line.isBlank()) words += line.trim().split("\\s+").length;
      }
  }
  ```
- **Write code to copy an image file using byte streams with a buffer.**
  ```java
  try (InputStream in = new FileInputStream("a.jpg"); OutputStream out = new FileOutputStream("b.jpg")) {
      byte[] buf = new byte[8192]; int n;
      while ((n = in.read(buf)) != -1) out.write(buf, 0, n);
  }
  ```
- **Write code to serialize an `Employee` to a file and read it back, with a transient `password`.**
  ```java
  class Employee implements Serializable {
      private static final long serialVersionUID = 1L;
      int id; String name; transient String password;
  }
  try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("emp.ser"))) { oos.writeObject(emp); }
  try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("emp.ser"))) {
      Employee e = (Employee) ois.readObject(); // e.password is null
  }
  ```
- **After deserialization, what are the values of a transient `int`, a transient `String` and a static field changed between write and read?** The transient `int` is 0 and the transient `String` is `null`. The static field shows whatever value the class currently has in the JVM (the changed value if in the same run), because it was never serialized.
- **`Employee` (serializable) has an `Address` field (not serializable). What happens? Give three fixes.** `NotSerializableException` at runtime. Fixes: make `Address` implement `Serializable`; mark the field `transient`; or mark it transient and write its parts by hand in custom `writeObject` and `readObject` methods.
- **Implement a deep `clone()` for `Employee` with an `Address` and a `List<String>`.**
  ```java
  class Employee implements Cloneable {
      String name; Address address; List<String> skills;
      @Override public Employee clone() {
          try {
              Employee copy = (Employee) super.clone();
              copy.address = address.clone();          // Address must also override clone()
              copy.skills = new ArrayList<>(skills);
              return copy;
          } catch (CloneNotSupportedException e) { throw new AssertionError(e); }
      }
  }
  ```
- **What is printed? Clone an object with a mutable field, modify the field through the clone, print the original.** With the default shallow `clone()`, the original shows the modified value, because both objects share the same nested object. With a deep copy the original is unchanged.
- **Write the same file read with `BufferedReader` and with `Files.lines`.**
  ```java
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) { String l; while ((l = br.readLine()) != null) System.out.println(l); }
  try (Stream<String> lines = Files.lines(Path.of("data.txt"))) { lines.forEach(System.out::println); }
  ```

---

## 14. Object class and date/time APIs (Session 18)

### Object class

- ⭐ **What are the methods of the `Object` class?** `equals`, `hashCode`, `toString`, `getClass`, `clone`, `finalize` (deprecated), and the thread coordination methods `wait` (three overloads), `notify` and `notifyAll`.
- ⭐ **What is the contract between `equals()` and `hashCode()`?** If two objects are equal according to `equals`, they must return the same hash code. Unequal objects may share a hash code, though distinct codes give better performance. The hash code must stay the same while the fields used in `equals` do not change.
- ⭐ **What happens if you override `equals()` but not `hashCode()`? The reverse?** Equal objects may get different hash codes, so `HashSet` and `HashMap` treat them as different: duplicates appear and lookups fail. Overriding only `hashCode` leaves `equals` as reference comparison, so objects with the same data still are not considered equal.
- ⭐ **What is the default behaviour of `equals()`, `hashCode()` and `toString()`?** `equals` compares references (`==`). `hashCode` returns an identity-based integer, typically derived from a random number or memory address. `toString` returns the class name, `@`, and the hash code in hex.
- **What are the properties `equals` must satisfy?** Reflexive (`x.equals(x)` is true), symmetric (`x.equals(y)` equals `y.equals(x)`), transitive, consistent (same result on repeated calls if nothing changes), and `x.equals(null)` is false.
- **Why override `toString()`? When is it called implicitly?** To give a readable description of the object for logs and debugging. It is called implicitly in string concatenation, by `System.out.println(obj)`, by `String.valueOf(obj)` and by debuggers.
- **What does the default `toString()` output look like?** `getClass().getName() + "@" + Integer.toHexString(hashCode())`, for example `com.app.Employee@1b6d3586`.
- **Can two unequal objects have the same hash code? Can two equal objects have different hash codes?** Unequal objects can share a hash code; that is a collision and is allowed. Equal objects must never have different hash codes; that breaks the contract.
- **Should `equals` use `getClass()` or `instanceof`? What are the trade-offs with inheritance?** `getClass()` treats a parent and child as never equal, which keeps symmetry safe but breaks substitution for subclasses (and for proxies such as Hibernate entities). `instanceof` allows subclasses to equal the parent, but can break symmetry and transitivity if a subclass adds fields to the comparison. Use `instanceof` when the class is final or equality is defined entirely by the parent.
- **Why should mutable fields not be used in `hashCode()` for keys in a hash-based collection?** If a field changes after the object is inserted, its hash code changes and the object is in the wrong bucket. `contains` and `get` can no longer find it and it cannot be removed, which is also a memory leak.
- **What do `Objects.equals`, `Objects.hash` and `Objects.requireNonNull` do?** `Objects.equals(a, b)` is a null-safe equality check. `Objects.hash(f1, f2, ...)` builds a hash code from several fields. `Objects.requireNonNull(x, msg)` throws `NullPointerException` with a message if `x` is null.
- **What is the difference between `hashCode()` and `System.identityHashCode()`?** `hashCode()` may be overridden by the class. `System.identityHashCode(obj)` always returns the default identity hash that `Object.hashCode()` would give, regardless of overrides.
- **Why are `wait`, `notify` and `notifyAll` in `Object` rather than `Thread`?** Because they operate on an object's monitor (lock), and every object has one. Threads wait on and are notified through the shared object they synchronize on, not through each other.
- **What does `getClass()` return? Why is it final?** The runtime `Class` object of the instance. It is final so that no class can lie about its type, which the type system, security and reflection rely on.
- **How do records (Java 16) generate `equals`, `hashCode` and `toString`?** The compiler generates them from all record components: two records are equal if all components are equal, the hash combines all components, and `toString` prints `Name[field=value, ...]`. You can override any of them.

### Date and time

- **What are `java.util.Date` and `java.util.Calendar`? What are their problems?** `Date` represents an instant in milliseconds; `Calendar` does date arithmetic and field access. Both are mutable and not thread-safe, months are zero-based, the API is confusing (years offset from 1900 in `Date`), and time-zone handling is poor.
- **What does a `Date` object actually store?** A single `long`: the number of milliseconds since 1 January 1970 00:00:00 UTC. It has no time zone; `toString()` just formats it in the default zone.
- **How do you get the current date and time?** Legacy: `new Date()` or `Calendar.getInstance()`. Modern: `LocalDate.now()`, `LocalDateTime.now()`, `ZonedDateTime.now()` or `Instant.now()`.
- ⭐ **How do you convert a `Date` to a `String` and a `String` to a `Date` with `SimpleDateFormat`?** `SimpleDateFormat sdf = new SimpleDateFormat("dd-MM-yyyy");` then `String s = sdf.format(date);` and `Date d = sdf.parse("25-12-2025");`. `parse` throws the checked `ParseException`.
- **What do the pattern letters mean? What is the difference between `MM` and `mm`, `yyyy` and `YYYY`?** `dd` day of month, `MM` month number, `MMM` month name, `yyyy` calendar year, `HH` hour 0–23, `hh` hour 1–12, `mm` minutes, `ss` seconds, `a` AM/PM, `EEE` day name. `MM` is month and `mm` is minutes. `YYYY` is the week-based year, which gives the wrong year around 1 January, so use `yyyy`.
- **What is `ParseException`?** A checked exception thrown by `SimpleDateFormat.parse()` when the text does not match the pattern.
- ⭐ **Why is `SimpleDateFormat` not thread-safe? How do you work around it?** It stores intermediate state in an internal `Calendar` field, so concurrent calls corrupt each other and produce wrong dates or exceptions. Create a new instance per use, keep one per thread with `ThreadLocal`, synchronize access, or better, use the immutable `DateTimeFormatter`.
- **Why are months zero-based in `Calendar`?** It is inherited from the C `struct tm` convention, where months index an array of names. `Calendar.JANUARY` is 0 and December is 11, a common source of off-by-one bugs.
- **How do you add days or months with `Calendar`?** `Calendar c = Calendar.getInstance(); c.add(Calendar.DAY_OF_MONTH, 10); c.add(Calendar.MONTH, -1); Date d = c.getTime();`.
- **What is the difference between `java.util.Date` and `java.sql.Date`?** `java.util.Date` holds date and time. `java.sql.Date` is a subclass used with JDBC that represents only a date, with the time set to zero. Modern JDBC works with `LocalDate` directly.
- ⭐ **What is the `java.time` API (Java 8)? What are its main classes?** The modern date-time API. `LocalDate`: date without time. `LocalTime`: time without date. `LocalDateTime`: both, without a zone. `ZonedDateTime`: date-time with a time zone. `Instant`: a point on the UTC timeline. `Duration`: an amount of time in seconds and nanoseconds. `Period`: an amount in years, months and days.
- **Why is `java.time` better?** All classes are immutable and thread-safe, the API is clear and fluent (`plusDays`, `withMonth`), months are 1-based, it separates human time from machine time, and it handles time zones and daylight saving correctly.
- **What is `DateTimeFormatter`?** The immutable, thread-safe formatter and parser of `java.time`: `DateTimeFormatter f = DateTimeFormatter.ofPattern("dd/MM/yyyy"); String s = date.format(f); LocalDate d = LocalDate.parse(s, f);`.
- **How do you find the number of days between two dates in the old and the new API?** Old: `(d2.getTime() - d1.getTime()) / (1000 * 60 * 60 * 24)`. New: `ChronoUnit.DAYS.between(date1, date2)`, or `Period.between` for years, months and days.
- **How do you convert between `Date` and `LocalDateTime`?** `LocalDateTime ldt = date.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();` and `Date d = Date.from(ldt.atZone(ZoneId.systemDefault()).toInstant());`.
- **How do you handle time zones and daylight saving?** Store and transfer instants in UTC (`Instant`), and convert to `ZonedDateTime` with a region `ZoneId` such as `Asia/Kolkata` for display. Region IDs carry daylight saving rules; fixed offsets do not.
- **What is the epoch?** The reference instant 1970-01-01T00:00:00Z. `Date.getTime()` and `Instant.toEpochMilli()` count milliseconds from it.
- **How do you check for a leap year?** `Year.isLeap(year)` or `LocalDate.of(year, 1, 1).isLeapYear()`. Manually: `(y % 4 == 0 && y % 100 != 0) || y % 400 == 0`.

### Code and scenario

- **Write `equals`, `hashCode` and `toString` for `Employee` with `id`, `name` and `salary`.**
  ```java
  @Override public boolean equals(Object o) {
      if (this == o) return true;
      if (!(o instanceof Employee)) return false;
      Employee e = (Employee) o;
      return id == e.id && Double.compare(salary, e.salary) == 0 && Objects.equals(name, e.name);
  }
  @Override public int hashCode() { return Objects.hash(id, name, salary); }
  @Override public String toString() { return "Employee{id=" + id + ", name=" + name + ", salary=" + salary + "}"; }
  ```
- **What is printed? Two `Employee` objects with the same data compared with `==` and `equals`, with and without overriding.** `==` is `false` in both cases because they are different objects. `equals` is `false` without overriding (reference comparison) and `true` once `equals` compares fields.
- **What is printed? Add two equal employees to a `HashSet` with only `equals` overridden.** Size is 2. Their default hash codes differ, so they land in different buckets and `equals` is never called to compare them.
- **What is printed? Put an object in a `HashSet`, mutate a field used in `hashCode`, then call `contains`.** `false`. The set looks in the bucket for the new hash code, but the object is stored in the bucket for the old one.
- **`equals(Employee e)` is written instead of `equals(Object o)`. What goes wrong?** It overloads rather than overrides. Collections call `equals(Object)`, which is still the inherited reference comparison, so `contains`, `remove` and `HashSet` do not use your logic. `@Override` would catch this at compile time.
- **Write a `DateManipulator` that converts String to Date, Date to String, and finds days between two dates.**
  ```java
  class DateManipulator {
      private final SimpleDateFormat sdf = new SimpleDateFormat("dd-MM-yyyy");
      Date toDate(String s) throws ParseException { return sdf.parse(s); }
      String toString(Date d) { return sdf.format(d); }
      long daysBetween(Date a, Date b) { return TimeUnit.MILLISECONDS.toDays(Math.abs(b.getTime() - a.getTime())); }
  }
  ```
- **Parse `"31-02-2025"` with a lenient and a non-lenient `SimpleDateFormat`. What happens?** By default the formatter is lenient and rolls the date over to 3 March 2025. After `setLenient(false)` it throws `ParseException`.
- **Print today's date as `dd/MM/yyyy` and as `EEEE, d MMMM yyyy`.** `LocalDate.now().format(DateTimeFormatter.ofPattern("dd/MM/yyyy"))` gives for example `11/11/2026`, and the pattern `"EEEE, d MMMM yyyy"` gives `Wednesday, 11 November 2026`.
- **Compute a person's age from a date of birth.** `int age = Period.between(dob, LocalDate.now()).getYears();`.

---

## 15. Collections: List, Queue, Comparable, Comparator (Sessions 19–22)

### Framework basics

- ⭐ **What is the Java Collections Framework? Why use it over arrays?** A set of interfaces, implementations and algorithms in `java.util` for storing and processing groups of objects. Unlike arrays, collections grow dynamically, offer ready-made data structures (lists, sets, maps, queues) and algorithms (sort, search), and are type-safe with generics.
- ⭐ **Draw the collection hierarchy.** `Iterable` → `Collection` → `List` (`ArrayList`, `LinkedList`, `Vector` → `Stack`), `Set` (`HashSet` → `LinkedHashSet`; `SortedSet` → `NavigableSet` → `TreeSet`), `Queue` (`PriorityQueue`; `Deque` → `ArrayDeque`, `LinkedList`). Separately, `Map` (`HashMap` → `LinkedHashMap`; `Hashtable`; `SortedMap` → `NavigableMap` → `TreeMap`).
- ⭐ **What is the difference between `Collection` and `Collections`?** `Collection` is the root interface for lists, sets and queues. `Collections` is a utility class with static methods such as `sort`, `reverse`, `shuffle`, `unmodifiableList` and `synchronizedList`.
- **Why does `Map` not extend `Collection`?** A `Collection` holds single elements, while a `Map` holds key-value pairs. Methods such as `add(E)` do not fit a map, so it has its own interface, with collection views through `keySet()`, `values()` and `entrySet()`.
- **What is the difference between an array and an `ArrayList`?** An array has a fixed size, can hold primitives or objects, and uses `length`. An `ArrayList` resizes automatically, holds only objects, uses `size()`, and provides methods for insertion, removal and search.
- **What is the difference between `List`, `Set`, `Queue` and `Map`?** `List`: ordered, indexed, allows duplicates. `Set`: no duplicates. `Queue`: holds elements for processing, usually FIFO. `Map`: key-value pairs with unique keys.
- **What is `Iterable`? How does the for-each loop use it?** An interface with one method, `iterator()`. The for-each loop is compiled into a call to `iterator()` followed by `hasNext()` and `next()` calls, so any class implementing `Iterable` can be used in for-each.
- ⭐ **What is the difference between `Iterator` and `ListIterator`? What about `Enumeration`?** `Iterator` works on any collection, moves forward only and can remove. `ListIterator` works only on lists, moves both ways, and can add, set and get indexes. `Enumeration` is the legacy read-only iterator for `Vector` and `Hashtable`.
- ⭐ **What are fail-fast and fail-safe iterators? What is `ConcurrentModificationException`?** Fail-fast iterators (`ArrayList`, `HashMap`) throw `ConcurrentModificationException` if the collection is structurally modified during iteration other than through the iterator. Fail-safe iterators (`CopyOnWriteArrayList`, `ConcurrentHashMap`) work on a snapshot or a weakly consistent view and never throw it.
- **How does `modCount` work?** The collection increments `modCount` on every structural change. The iterator saves its value when created and compares on each `next()`; a mismatch throws `ConcurrentModificationException`.
- **How do you safely remove elements while iterating?** Use `iterator.remove()`, `collection.removeIf(predicate)`, iterate over a copy, or loop backwards by index for a list.
- **Why were generics added to collections?** For compile-time type safety and to remove casts. Before generics, collections held `Object`, so wrong types were found only at runtime as `ClassCastException`.
- **What are legacy classes?** `Vector`, `Stack`, `Hashtable`, `Dictionary` and `Enumeration`, from Java 1.0. They are synchronized and were retrofitted into the framework. Prefer `ArrayList`, `ArrayDeque`, `HashMap` or `ConcurrentHashMap`.

### List

- ⭐ **What is the difference between `ArrayList` and `LinkedList`? When would you use each?** `ArrayList` is backed by a dynamic array: O(1) access by index, slow insert and delete in the middle because elements shift. `LinkedList` is a doubly linked list: O(1) insert and delete at the ends, O(n) access by index, and more memory per element. Use `ArrayList` by default; use `LinkedList` or `ArrayDeque` for queue-like usage.
- ⭐ **What is the difference between `ArrayList` and `Vector`?** `Vector` is synchronized, so it is thread-safe but slower, and it doubles its capacity when full. `ArrayList` is not synchronized and grows by 50%. `Vector` is a legacy class.
- ⭐ **How does `ArrayList` work internally? What is its default capacity and growth factor?** It keeps an `Object[]` array. The default capacity is 10, allocated on the first add. When full, it creates a new array 1.5 times larger (`old + old >> 1`) and copies the elements with `Arrays.copyOf`.
- **How does `Vector` grow? What is its capacity increment?** It doubles its capacity by default. If a `capacityIncrement` is given in the constructor, it grows by that fixed amount instead.
- **What is the time complexity of `add`, `add(index)`, `get`, `remove` and `contains` for `ArrayList` and `LinkedList`?** `ArrayList`: add at end O(1) amortized, add or remove at index O(n), get O(1), contains O(n). `LinkedList`: add or remove at the ends O(1), add or remove at index O(n) to reach the node, get O(n), contains O(n).
- **How is `LinkedList` implemented? Which interfaces does it implement?** As a doubly linked list of nodes holding the item and `prev` and `next` references, with `first` and `last` pointers. It implements `List`, `Deque` (and therefore `Queue`), `Cloneable` and `Serializable`.
- **What is `RandomAccess`?** A marker interface that says a list supports fast, constant-time index access (`ArrayList`, `Vector`). Algorithms check it to choose between an indexed loop and an iterator.
- **What do `ensureCapacity` and `trimToSize` do?** `ensureCapacity(n)` grows the internal array in advance to avoid repeated resizing before a bulk insert. `trimToSize()` shrinks the array to the current size to free memory.
- **How do you make an `ArrayList` thread-safe? What is `CopyOnWriteArrayList`?** Wrap it with `Collections.synchronizedList(list)` (and synchronize manually while iterating), or use `CopyOnWriteArrayList`, which copies the whole array on every write so readers need no locks. It suits read-heavy, write-rare cases.
- **How do you make a list read-only? What is the difference between `Collections.unmodifiableList`, `List.of` and `Arrays.asList`?** `Collections.unmodifiableList` returns a read-only view; changes to the underlying list show through. `List.of` returns a truly immutable list that rejects nulls. `Arrays.asList` returns a fixed-size list backed by the array: `set` works, `add` and `remove` throw `UnsupportedOperationException`.
- **What is the difference between `remove(int index)` and `remove(Object o)` on a `List<Integer>`?** `list.remove(1)` removes the element at index 1. To remove the value 1, call `list.remove(Integer.valueOf(1))`. The `int` overload is chosen because no boxing is needed.
- **How do you convert between an array and a list?** Array to list: `Arrays.asList(arr)`, `List.of(arr)` or `new ArrayList<>(Arrays.asList(arr))`. List to array: `list.toArray(new String[0])`.
- **What does `subList` return? Is it a view?** A view of part of the original list, not a copy. Changes through the view affect the original. Structurally modifying the original afterwards makes the view throw `ConcurrentModificationException`.
- **Why is `Stack` discouraged? What should you use instead?** It extends `Vector`, so it is synchronized (slow) and exposes index-based methods that break stack semantics. Use `Deque<Integer> stack = new ArrayDeque<>();` with `push`, `pop` and `peek`.
- **How do you remove duplicates from a list? Reverse it? Find the max?** Duplicates: `new ArrayList<>(new LinkedHashSet<>(list))` or `list.stream().distinct().toList()`. Reverse: `Collections.reverse(list)`. Max: `Collections.max(list)`.
- **In practice, why is `ArrayList` often faster than `LinkedList` even for insertions?** Array elements are contiguous in memory, so the CPU cache works well and shifting is a fast block copy. Linked list nodes are scattered, so traversal causes cache misses, and each node needs extra allocation.

### Collections class, Comparable, Comparator

- **What utility methods does `Collections` provide?** `sort`, `reverse`, `shuffle`, `binarySearch`, `max`, `min`, `frequency`, `swap`, `unmodifiableList/Set/Map`, `synchronizedList/Set/Map`, `emptyList`, `singletonList`, `nCopies` and `addAll`.
- ⭐ **What is the difference between `Comparable` and `Comparator`?** `Comparable` is implemented by the class itself and defines one natural ordering through `compareTo`. `Comparator` is a separate object that defines an external ordering through `compare`, so you can have many orderings and sort classes you cannot modify.
- **Which methods do they declare, and in which packages are they?** `Comparable<T>` is in `java.lang` with `int compareTo(T o)`. `Comparator<T>` is in `java.util` with `int compare(T a, T b)`.
- **What is natural ordering?** The default order defined by a class's `compareTo`: numeric for numbers, lexicographic for strings, chronological for dates.
- **How do you sort a list of custom objects by multiple fields?** Chain comparators: `list.sort(Comparator.comparing(Employee::getDept).thenComparing(Employee::getSalary, Comparator.reverseOrder()).thenComparing(Employee::getName));`.
- **What are `Comparator.comparing`, `thenComparing`, `reversed`, `nullsFirst` and `naturalOrder`?** `comparing` builds a comparator from a key extractor. `thenComparing` adds a tie-breaker. `reversed` flips the order. `nullsFirst` and `nullsLast` make a comparator null-safe. `naturalOrder` returns a comparator that uses `compareTo`.
- **What is the contract of `compareTo`? Why should it be consistent with `equals`?** It returns negative, zero or positive; it must be antisymmetric and transitive. If `compareTo` returns 0 for objects that are not `equals`, sorted collections such as `TreeSet` and `TreeMap` treat them as duplicates while hash collections do not, giving inconsistent behaviour.
- **Why is `return a - b` in a comparator risky?** The subtraction can overflow for large or negative values and give the wrong sign. Use `Integer.compare(a, b)`.
- **Which algorithm do `Collections.sort` and `Arrays.sort` use? Is the sort stable?** For objects, TimSort, a stable hybrid of merge sort and insertion sort with O(n log n) worst case. For primitive arrays, a dual-pivot quicksort, which is not stable, but stability does not matter for primitives.
- **What happens when you sort a list of objects that are not `Comparable`?** `Collections.sort(list)` does not compile, because the method requires `T extends Comparable`. With raw types, or when adding to a `TreeSet`, you get `ClassCastException` at runtime. Supply a `Comparator` instead.
- **What is required before calling `Collections.binarySearch`?** The list must already be sorted in the same order used for the search. Otherwise the result is undefined.
- **What is the difference between `Collections.sort(list)` and `list.sort(comparator)`?** None in behaviour. Since Java 8, `Collections.sort` delegates to `List.sort`. `list.sort(null)` uses natural ordering.

### Queue

- ⭐ **What is the `Queue` interface? What are its methods?** A collection for holding elements before processing, usually first-in first-out. Its methods are `add`/`offer` to insert, `remove`/`poll` to take the head, and `element`/`peek` to look at the head.
- **What is the difference between `add`/`offer`, `remove`/`poll` and `element`/`peek`?** The first of each pair throws an exception on failure (`IllegalStateException` when full, `NoSuchElementException` when empty). The second returns a special value: `false` or `null`.
- **What is a `Deque`? What is `ArrayDeque`? Why is it preferred over `Stack` and `LinkedList`?** A double-ended queue that supports insertion and removal at both ends, so it works as both a queue and a stack. `ArrayDeque` is a resizable circular array implementation. It is faster than `Stack` (no synchronization) and than `LinkedList` (no node allocation, better cache behaviour). It does not allow nulls.
- ⭐ **What is a `PriorityQueue`? How is it implemented? What are its time complexities?** A queue that always returns the smallest element by natural order or a comparator. It is implemented as a binary min-heap in an array. `offer` and `poll` are O(log n), `peek` is O(1), and `remove(Object)` and `contains` are O(n).
- **Does iterating a `PriorityQueue` give sorted order?** No. The iterator follows the internal heap array. To get sorted order, call `poll()` repeatedly.
- **Can a `PriorityQueue` hold `null`? Non-comparable objects?** No nulls (`NullPointerException`). Non-comparable objects cause `ClassCastException` unless you provide a comparator.
- **How do you make a max-heap with `PriorityQueue`?** `new PriorityQueue<>(Comparator.reverseOrder())` or `new PriorityQueue<>((a, b) -> Integer.compare(b, a))`.
- **What is a `BlockingQueue`? Name its implementations.** A thread-safe queue where `put` blocks when full and `take` blocks when empty, ideal for producer–consumer. Implementations: `ArrayBlockingQueue`, `LinkedBlockingQueue`, `PriorityBlockingQueue`, `DelayQueue`, `SynchronousQueue`.
- **How do you implement a stack using queues, or a queue using stacks?** Stack with one queue: after each push, rotate the previous elements behind the new one so the newest is at the head. Queue with two stacks: push to the input stack; on pop, if the output stack is empty, move everything from input to output, then pop from output. That gives amortized O(1).

### Code and scenario

- **What is printed? Removing elements inside a for-each loop over an `ArrayList`.** `ConcurrentModificationException` on the next iteration. (One quirk: removing the second-last element exits the loop silently, because `hasNext()` becomes false.)
- **What is printed? `List<Integer> l = new ArrayList<>(List.of(1, 2, 3)); l.remove(1); System.out.println(l);`** `[1, 3]`. `remove(1)` removes the element at index 1.
- **What happens? `List<Integer> l = Arrays.asList(1, 2, 3); l.add(4);` What about `l.set(0, 9)`?** `add` throws `UnsupportedOperationException` because the list is fixed-size. `set` works and writes through to the backing array.
- **What happens? `int[] a = {1, 2, 3}; List list = Arrays.asList(a); System.out.println(list.size());`** `1`. A primitive array is treated as a single object, so you get a `List<int[]>` with one element. Use `Integer[]` or `Arrays.stream(a).boxed().toList()`.
- **Create a list of `Integer`, then insert, delete, search, iterate and sort it.**
  ```java
  List<Integer> list = new ArrayList<>(List.of(5, 2, 8));
  list.add(1); list.add(0, 9);                 // insert
  list.remove(Integer.valueOf(8));             // delete by value
  boolean found = list.contains(2); int idx = list.indexOf(2); // search
  for (int x : list) System.out.print(x + " "); // iterate
  Collections.sort(list);                      // sort
  ```
- **Sort a `List<Employee>` by salary using `Comparable`, then by name using `Comparator`, then by department and salary descending.**
  ```java
  class Employee implements Comparable<Employee> {
      public int compareTo(Employee o) { return Double.compare(salary, o.salary); }
  }
  Collections.sort(list);                                           // by salary (natural order)
  list.sort(Comparator.comparing(Employee::getName));               // by name
  list.sort(Comparator.comparing(Employee::getDept)
                      .thenComparing(Employee::getSalary, Comparator.reverseOrder()));
  ```
- **Implement a queue using `LinkedList`, and implement your own queue with an array.**
  ```java
  Queue<Integer> q = new LinkedList<>(); q.offer(1); q.offer(2); q.poll(); q.peek();
  class ArrayQueue {
      private final int[] data; private int head, tail, size;
      ArrayQueue(int cap) { data = new int[cap]; }
      void enqueue(int x) { if (size == data.length) throw new IllegalStateException("full"); data[tail] = x; tail = (tail + 1) % data.length; size++; }
      int dequeue() { if (size == 0) throw new NoSuchElementException(); int x = data[head]; head = (head + 1) % data.length; size--; return x; }
  }
  ```
- **Find the k largest elements in an array using a `PriorityQueue`.**
  ```java
  PriorityQueue<Integer> heap = new PriorityQueue<>();      // min-heap of size k
  for (int x : arr) { heap.offer(x); if (heap.size() > k) heap.poll(); }
  // heap now holds the k largest; O(n log k)
  ```
- **Merge two sorted lists.**
  ```java
  List<Integer> merged = new ArrayList<>(); int i = 0, j = 0;
  while (i < a.size() && j < b.size()) merged.add(a.get(i) <= b.get(j) ? a.get(i++) : b.get(j++));
  while (i < a.size()) merged.add(a.get(i++));
  while (j < b.size()) merged.add(b.get(j++));
  ```
- **Find the intersection and union of two lists.** Intersection: `List<Integer> in = new ArrayList<>(a); in.retainAll(b);`. Union without duplicates: `Set<Integer> un = new LinkedHashSet<>(a); un.addAll(b);`.
- **Implement your own `ArrayList` with `add`, `get` and `remove`.**
  ```java
  class MyList<T> {
      private Object[] data = new Object[10]; private int size;
      void add(T t) { if (size == data.length) data = Arrays.copyOf(data, size + (size >> 1)); data[size++] = t; }
      @SuppressWarnings("unchecked") T get(int i) { Objects.checkIndex(i, size); return (T) data[i]; }
      T remove(int i) { T old = get(i); System.arraycopy(data, i + 1, data, i, size - i - 1); data[--size] = null; return old; }
  }
  ```
- **Reverse a `LinkedList` without `Collections.reverse`.** For `java.util.LinkedList`: `LinkedList<Integer> rev = new LinkedList<>(); for (Integer x : list) rev.addFirst(x);`. For a custom singly linked list, walk it with three pointers: `prev = null; while (cur != null) { next = cur.next; cur.next = prev; prev = cur; cur = next; } head = prev;`.

---

## 16. Collections: Set, Map, concurrent collections, Streams (Sessions 23–26)

### Set

- ⭐ **What is a `Set`? How does it guarantee uniqueness?** A collection that contains no duplicate elements. Hash-based sets use `hashCode()` to find the bucket and `equals()` to compare; tree-based sets use `compareTo` or a comparator, treating a result of 0 as a duplicate.
- ⭐ **What is the difference between `HashSet`, `LinkedHashSet` and `TreeSet`?** `HashSet`: no ordering, O(1) operations, one null allowed. `LinkedHashSet`: keeps insertion order with a linked list through the entries, slightly slower. `TreeSet`: keeps elements sorted, O(log n) operations, no nulls, backed by a red-black tree.
- ⭐ **How does `HashSet` work internally?** It is backed by a `HashMap`. Each element is stored as a key, with a shared dummy object (`PRESENT`) as the value. `add(e)` calls `map.put(e, PRESENT)` and returns true if the key was new.
- **What happens when you add a duplicate to a `HashSet`? What does `add` return?** The set is unchanged, the existing element is kept, and `add` returns `false`. No exception is thrown.
- **Can a `HashSet` hold `null`? A `TreeSet`?** `HashSet` allows one `null`. `TreeSet` with natural ordering throws `NullPointerException`, because it must call `compareTo` on the element.
- **How is `TreeSet` implemented? What is the time complexity of its operations?** It is backed by a `TreeMap`, which is a red-black tree (a self-balancing binary search tree). `add`, `remove` and `contains` are O(log n).
- **What are `SortedSet` and `NavigableSet`? What do `floor`, `ceiling`, `higher`, `lower`, `first`, `last`, `pollFirst` do?** `SortedSet` keeps elements in order; `NavigableSet` adds navigation methods. `floor(e)`: greatest element ≤ e. `ceiling(e)`: smallest ≥ e. `lower(e)`: greatest < e. `higher(e)`: smallest > e. `first`/`last`: the ends. `pollFirst`/`pollLast`: remove and return an end.
- **What are backed collections (`headSet`, `tailSet`, `subSet`)? What happens when you modify the backing set or the view?** They return views of a range of the original set, not copies. Changes in either are visible in the other, as long as the element is within the view's range. Adding an out-of-range element through the view throws `IllegalArgumentException`.
- **What happens if you add non-comparable objects to a `TreeSet`?** `ClassCastException` at runtime, unless the set was created with a `Comparator`.
- **Does `TreeSet` use `equals` or `compareTo` to detect duplicates?** `compareTo` (or the comparator). If it returns 0, the element is treated as a duplicate, even if `equals` would return false.
- **How do you make a set immutable or synchronized?** Immutable: `Set.of(...)`, `Set.copyOf(set)` or `Collections.unmodifiableSet(set)`. Synchronized: `Collections.synchronizedSet(set)`, or use `ConcurrentHashMap.newKeySet()`.

### Map

- ⭐ **What is a `Map`? What are its main methods?** An object that maps unique keys to values. Main methods: `put`, `get`, `remove`, `containsKey`, `containsValue`, `size`, `keySet`, `values`, `entrySet`, `getOrDefault` and `putIfAbsent`.
- ⭐ **How does `HashMap` work internally? Walk through `put` and `get`.** It holds an array of buckets. `put` computes the key's `hashCode()`, spreads it, and takes `index = hash & (n - 1)`. If the bucket is empty, it stores a new node. Otherwise it walks the nodes there: if a key with the same hash and `equals` is found, the value is replaced; if not, a new node is appended. `get` computes the same index and searches that bucket using `equals`. If the size exceeds capacity × load factor, the table doubles.
- ⭐ **What is a hash collision? How does `HashMap` handle it?** Two different keys mapping to the same bucket. `HashMap` chains them in a linked list in that bucket, and since Java 8 converts a long chain into a red-black tree.
- ⭐ **What changed in `HashMap` in Java 8? What are the thresholds?** A bucket's linked list becomes a red-black tree when it has more than 8 nodes and the table has at least 64 buckets (otherwise the table is resized instead). It goes back to a list when it shrinks to 6. This improves the worst case from O(n) to O(log n).
- ⭐ **What is the default capacity and load factor of `HashMap`? What is rehashing?** Capacity 16 and load factor 0.75, so the threshold is 12 entries. Rehashing (resizing) happens when the size exceeds the threshold: the table doubles and entries are redistributed into the new buckets.
- **Why is the capacity always a power of two? How is the bucket index calculated?** So the index can be computed with a fast bit mask, `hash & (n - 1)`, instead of a slower modulo. It also makes resizing simple: each entry either stays at its index or moves by exactly the old capacity.
- **Why does `HashMap` spread the hash with `h ^ (h >>> 16)`?** The index uses only the low bits of the hash. XOR-ing the high 16 bits into the low 16 lets the high bits influence the bucket, which reduces collisions for poor hash functions.
- ⭐ **What is the difference between `HashMap` and `Hashtable`?** `HashMap` is not synchronized, allows one null key and many null values, and is faster. `Hashtable` is synchronized on every method, allows no nulls, and is a legacy class. For thread safety use `ConcurrentHashMap`.
- ⭐ **What is the difference between `HashMap`, `LinkedHashMap` and `TreeMap`?** `HashMap`: no order, O(1). `LinkedHashMap`: keeps insertion order (or access order), O(1). `TreeMap`: keys sorted by natural order or comparator, O(log n), backed by a red-black tree.
- **Can a `HashMap` have a `null` key or `null` values? What about `Hashtable`, `TreeMap` and `ConcurrentHashMap`?** `HashMap`: one null key (stored in bucket 0) and any number of null values. `Hashtable` and `ConcurrentHashMap`: neither. `TreeMap`: no null key with natural ordering, null values allowed.
- **What happens when you put a duplicate key?** The new value replaces the old one, `put` returns the old value, and the size does not change.
- **What makes a good `HashMap` key? Why should keys be immutable?** A key should be immutable and have consistent, well-distributed `equals` and `hashCode`. If a key's state changes after insertion, its hash code changes and the entry can no longer be found.
- **What happens if all keys return the same hash code?** Every entry goes into one bucket. Lookups degrade to O(n) with a linked list, or O(log n) once the bucket is treeified in Java 8+. The map still works correctly, only slowly.
- **What is the time complexity of `get` and `put` in the best and worst cases?** Average O(1). Worst case O(n) before Java 8 and O(log n) from Java 8, when many keys collide in one bucket.
- **How do you iterate over a map? Which is most efficient?** Over `entrySet()`, `keySet()`, `values()`, or with `map.forEach((k, v) -> ...)`. `entrySet()` is the most efficient when you need both key and value, because iterating `keySet()` and calling `get` does an extra lookup per key.
- **What is `LinkedHashMap` access order? How do you build an LRU cache with it?** Constructed with `accessOrder = true`, it moves an entry to the end every time it is accessed, so the first entry is the least recently used. Override `removeEldestEntry` to return `size() > capacity` and the oldest entry is evicted automatically.
- **How is `TreeMap` implemented? What are `SortedMap` and `NavigableMap`?** As a red-black tree ordered by key. `SortedMap` adds `firstKey`, `lastKey`, `headMap`, `tailMap` and `subMap`. `NavigableMap` adds `floorKey`, `ceilingKey`, `lowerKey`, `higherKey`, `pollFirstEntry` and `descendingMap`.
- **What are backed map collections?** `headMap`, `tailMap`, `subMap`, `keySet`, `values` and `entrySet` return views backed by the map. Changes to the map appear in the view and the reverse; removing from `keySet` removes the entry from the map.
- **What do `getOrDefault`, `putIfAbsent`, `computeIfAbsent`, `computeIfPresent`, `merge` do?** `getOrDefault(k, d)`: value or a default. `putIfAbsent(k, v)`: insert only if absent. `computeIfAbsent(k, fn)`: compute and store a value if absent, good for `map.computeIfAbsent(k, x -> new ArrayList<>()).add(v)`. `computeIfPresent(k, fn)`: update an existing value. `merge(k, v, fn)`: insert `v` or combine it with the old value, good for counting: `map.merge(word, 1, Integer::sum)`.
- **What are `IdentityHashMap`, `WeakHashMap` and `EnumMap`?** `IdentityHashMap` compares keys with `==` instead of `equals`. `WeakHashMap` holds keys weakly so entries vanish when the key is garbage collected. `EnumMap` is a fast array-based map for enum keys.
- **Why is `HashMap` not thread-safe? What can go wrong under concurrent modification?** Its operations are not atomic and nothing is synchronized. Concurrent puts can lose updates, corrupt the size, or lose entries during resize; in Java 7 a concurrent resize could create a cycle in a bucket and cause an infinite loop.
- **How do you sort a map by value?** `map.entrySet().stream().sorted(Map.Entry.comparingByValue()).collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (a, b) -> a, LinkedHashMap::new));`. The `LinkedHashMap` keeps the sorted order.

### Concurrent collections

- ⭐ **What is the difference between `HashMap`, `Collections.synchronizedMap` and `ConcurrentHashMap`?** `HashMap` is not thread-safe. `synchronizedMap` wraps a map with one lock for every operation, so only one thread works at a time, and iteration needs manual synchronization. `ConcurrentHashMap` locks at the bucket level, lets reads proceed without locking, and gives much better throughput.
- ⭐ **How does `ConcurrentHashMap` work internally?** In Java 7 it was split into 16 segments, each with its own lock. In Java 8 it is a single table of nodes: an insert into an empty bucket uses a lock-free CAS, and an insert into an occupied bucket synchronizes only on that bucket's head node. Reads are lock-free using volatile fields.
- **Why does `ConcurrentHashMap` not allow `null` keys or values?** With concurrency, `get(key)` returning `null` would be ambiguous: you could not tell "key absent" from "value is null", and a follow-up `containsKey` check could be invalidated by another thread.
- **Are compound operations on `ConcurrentHashMap` atomic? How do you make them atomic?** No. `if (!map.containsKey(k)) map.put(k, v)` is a race. Use the atomic methods: `putIfAbsent`, `computeIfAbsent`, `compute`, `merge` and `replace`.
- **What is `CopyOnWriteArrayList`? When is it a good or bad choice?** A thread-safe list that copies the entire array on each modification. Iterators work on a snapshot and never throw `ConcurrentModificationException`. It is good for many reads and few writes, such as listener lists, and bad for frequent writes or large lists.
- **What is `CopyOnWriteArraySet`?** A thread-safe set backed by a `CopyOnWriteArrayList`. It has the same trade-offs, and `contains` is O(n).
- **What are `ConcurrentLinkedQueue`, `ConcurrentSkipListMap` and `ConcurrentSkipListSet`?** `ConcurrentLinkedQueue` is an unbounded, lock-free, non-blocking FIFO queue. `ConcurrentSkipListMap` and `ConcurrentSkipListSet` are thread-safe sorted collections based on skip lists, the concurrent counterparts of `TreeMap` and `TreeSet`.
- **What are `ArrayBlockingQueue`, `LinkedBlockingQueue`, `PriorityBlockingQueue`, `DelayQueue` and `SynchronousQueue`?** `ArrayBlockingQueue`: bounded, array-based. `LinkedBlockingQueue`: optionally bounded, linked nodes. `PriorityBlockingQueue`: unbounded, ordered by priority. `DelayQueue`: elements become available only after their delay expires. `SynchronousQueue`: no capacity; each put waits for a take.
- **What does "weakly consistent iterator" mean?** The iterator never throws `ConcurrentModificationException`, reflects the state at some point at or after its creation, and may or may not show concurrent modifications.
- **What is the difference between a synchronized collection and a concurrent collection?** Synchronized collections (`Collections.synchronizedXxx`, `Vector`, `Hashtable`) use one lock for the whole collection, so threads queue up. Concurrent collections use finer-grained locking or lock-free techniques, so they scale much better.

### Stream API

- ⭐ **What is the Stream API? How does a stream differ from a collection?** An API for processing sequences of elements in a declarative, functional style. A collection stores data; a stream does not store anything, it carries elements from a source through a pipeline of operations. A stream is lazy, can be used only once, and does not modify its source.
- ⭐ **What are intermediate and terminal operations? Give examples of each.** Intermediate operations return a new stream and are lazy: `filter`, `map`, `flatMap`, `sorted`, `distinct`, `limit`, `skip`, `peek`. Terminal operations produce a result or side effect and trigger execution: `forEach`, `collect`, `reduce`, `count`, `min`, `max`, `anyMatch`, `findFirst`.
- **What does lazy evaluation mean in streams?** Intermediate operations are not executed when they are called; they are only recorded. Processing starts when a terminal operation runs, and elements flow through the pipeline one at a time, so work can stop early.
- ⭐ **What is the difference between `map` and `flatMap`?** `map` transforms each element into exactly one element. `flatMap` transforms each element into a stream and flattens all those streams into one, for example turning a `List<List<String>>` into a stream of `String`.
- **What do `filter`, `map`, `sorted`, `distinct`, `limit`, `skip`, `peek` do?** `filter` keeps elements matching a predicate. `map` transforms each element. `sorted` orders them. `distinct` removes duplicates using `equals`. `limit(n)` keeps the first n. `skip(n)` drops the first n. `peek` runs an action on each element as it passes, mainly for debugging.
- **What do `forEach`, `collect`, `reduce`, `count`, `min`, `max`, `anyMatch`, `allMatch`, `noneMatch`, `findFirst`, `findAny` do?** `forEach` performs an action on each element. `collect` gathers elements into a container. `reduce` combines elements into one value. `count` counts. `min`/`max` return an `Optional` using a comparator. `anyMatch`, `allMatch`, `noneMatch` test a predicate. `findFirst`/`findAny` return an `Optional` element.
- **What is `reduce`? Explain its identity, accumulator and combiner.** It folds a stream into one result. The identity is the starting value and the result for an empty stream. The accumulator combines the running result with the next element. The combiner merges partial results in parallel streams. Example: `stream.reduce(0, Integer::sum)`.
- **What are the common `Collectors`?** `toList`, `toSet`, `toMap(keyFn, valueFn)`; `groupingBy(fn)` groups into a `Map<K, List<T>>`; `partitioningBy(pred)` splits into true and false; `joining(", ")` concatenates strings; `counting`, `summingInt`, `averagingInt` aggregate; `mapping` transforms before collecting downstream.
- **What happens with `Collectors.toMap` on duplicate keys?** It throws `IllegalStateException`. Provide a merge function as the third argument: `toMap(k, v, (a, b) -> a)`.
- **Can a stream be reused? What happens if you try?** No. After a terminal operation the stream is closed, and using it again throws `IllegalStateException`. Create a new stream from the source.
- **What is the difference between `findFirst` and `findAny`?** `findFirst` returns the first element in encounter order. `findAny` may return any element, which is faster in parallel streams where order does not matter.
- **What are stateless and stateful intermediate operations? What is short-circuiting?** Stateless operations (`filter`, `map`) process each element independently. Stateful ones (`sorted`, `distinct`, `limit`) need to remember other elements. Short-circuiting operations (`limit`, `findFirst`, `anyMatch`) can finish without processing the whole stream.
- **What are primitive streams? What are `mapToInt` and `boxed`?** `IntStream`, `LongStream` and `DoubleStream` work on primitives without boxing and add `sum`, `average`, `max` and `range`. `mapToInt` converts an object stream to an `IntStream`; `boxed` converts back to `Stream<Integer>`.
- ⭐ **What is a parallel stream? How does it work? When should you avoid it?** A stream that splits its data into chunks and processes them on several threads of the common `ForkJoinPool`, then combines the results. Avoid it for small data sets, blocking I/O, order-dependent or stateful logic, shared mutable state, and sources that split poorly such as `LinkedList`. Measure before using it.
- **What is `Optional`? Why was it introduced? What are its main methods?** A container that may or may not hold a value, used as a return type to make "no result" explicit instead of returning null. `orElse(x)` returns a default; `orElseGet(supplier)` computes one; `orElseThrow()` throws; `ifPresent(consumer)` runs if a value exists; `map` transforms the value if present.
- **What is the difference between `orElse` and `orElseGet`?** `orElse(value)` always evaluates its argument, even when the Optional has a value. `orElseGet(supplier)` calls the supplier only when the Optional is empty, so use it when the default is expensive to create.
- **What is the difference between `Collection.forEach` and `Stream.forEach`? What is `forEachOrdered`?** `Collection.forEach` iterates the collection in its own order. `Stream.forEach` gives no order guarantee in parallel streams. `forEachOrdered` processes elements in encounter order even in parallel, at some cost.
- **How do you create a stream?** `collection.stream()`, `Stream.of(a, b, c)`, `Arrays.stream(array)`, `Stream.iterate(1, x -> x + 1)` and `Stream.generate(supplier)` for infinite streams, `IntStream.range(0, n)`, and `Files.lines(path)`.
- **Why should stream lambdas avoid side effects?** Streams may process elements in any order or in parallel, so modifying shared state from a lambda causes race conditions and unpredictable results. Use `collect` or `reduce` to produce results instead.
- **What is the difference between `stream().toList()` (Java 16) and `collect(Collectors.toList())`?** `toList()` returns an unmodifiable list. `Collectors.toList()` makes no guarantee about the list type; in practice it returns a mutable `ArrayList`.
- **What is a `Spliterator`?** The object a stream uses to traverse and split its source: it can iterate elements and divide itself into two halves for parallel processing. It also reports characteristics such as `SIZED`, `ORDERED` and `SORTED`.

### Code and scenario

- **Create a `HashSet<Employee>`, add two employees with the same id, and show the set size with and without `equals`/`hashCode`.** Without overriding, the size is 2 because the objects have different identity hash codes. With `equals` and `hashCode` based on `id`, the second `add` returns false and the size is 1.
- **What is printed? A `TreeSet<String>` with mixed-case strings. What about a `TreeSet` with a comparator that always returns 0?** Strings sort by Unicode value, so all uppercase come before lowercase: `[Apple, Zebra, banana]`. With a comparator that always returns 0, every element looks like a duplicate of the first, so the set holds only the first element.
- **What is printed? `map.put("a", 1); map.put("a", 2); System.out.println(map.size() + " " + map.get("a"));`** `1 2`. The second `put` replaces the value for the same key.
- **What is printed? A `HashMap` with keys `1`, `1L` and `"1"`.** Three entries. `Integer`, `Long` and `String` are different types and are never equal to each other.
- **Modify a `TreeSet` after taking a `headSet` view and print both.** The view is live: an element added to the original that falls within the view's range appears in the view, and an element added through the view appears in the original.
- **Count the frequency of each word in a sentence using a `HashMap`, then using streams.**
  ```java
  Map<String, Integer> freq = new HashMap<>();
  for (String w : text.toLowerCase().split("\\s+")) freq.merge(w, 1, Integer::sum);
  Map<String, Long> freq2 = Arrays.stream(text.toLowerCase().split("\\s+"))
      .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
  ```
- **Find the first non-repeated character using a `LinkedHashMap`.**
  ```java
  Map<Character, Integer> m = new LinkedHashMap<>();
  for (char c : s.toCharArray()) m.merge(c, 1, Integer::sum);
  for (Map.Entry<Character, Integer> e : m.entrySet()) if (e.getValue() == 1) return e.getKey();
  ```
- **Sort a `Map<String, Integer>` by value descending.**
  ```java
  Map<String, Integer> sorted = map.entrySet().stream()
      .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
      .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (a, b) -> a, LinkedHashMap::new));
  ```
- **Implement an LRU cache.**
  ```java
  class LRUCache<K, V> extends LinkedHashMap<K, V> {
      private final int capacity;
      LRUCache(int capacity) { super(16, 0.75f, true); this.capacity = capacity; }
      @Override protected boolean removeEldestEntry(Map.Entry<K, V> eldest) { return size() > capacity; }
  }
  ```
  If asked to build it from scratch, use a `HashMap` plus a doubly linked list for O(1) get and put.
- **Implement a simple `HashMap` with `put` and `get`.**
  ```java
  class MyMap<K, V> {
      private static class Node<K, V> { final K key; V val; Node<K, V> next; Node(K k, V v, Node<K, V> n) { key = k; val = v; next = n; } }
      @SuppressWarnings("unchecked") private final Node<K, V>[] table = new Node[16];
      private int index(K key) { return key == null ? 0 : (key.hashCode() & 0x7fffffff) % table.length; }
      void put(K key, V val) {
          int i = index(key);
          for (Node<K, V> n = table[i]; n != null; n = n.next) if (Objects.equals(n.key, key)) { n.val = val; return; }
          table[i] = new Node<>(key, val, table[i]);
      }
      V get(K key) {
          for (Node<K, V> n = table[index(key)]; n != null; n = n.next) if (Objects.equals(n.key, key)) return n.val;
          return null;
      }
  }
  ```
- **Two-sum: find two numbers in an array that add to a target using a `HashMap`.**
  ```java
  Map<Integer, Integer> seen = new HashMap<>();
  for (int i = 0; i < a.length; i++) {
      Integer j = seen.get(target - a[i]);
      if (j != null) return new int[] { j, i };
      seen.put(a[i], i);
  }
  ```
- **Given `List<Employee>`, write the common stream queries.**
  ```java
  Optional<Employee> top = list.stream().max(Comparator.comparingDouble(Employee::getSalary));
  Map<String, List<Employee>> byDept = list.stream().collect(Collectors.groupingBy(Employee::getDept));
  Map<String, Double> avg = list.stream().collect(Collectors.groupingBy(Employee::getDept, Collectors.averagingDouble(Employee::getSalary)));
  List<String> names = list.stream().sorted(Comparator.comparingDouble(Employee::getSalary)).map(Employee::getName).toList();
  Optional<Double> second = list.stream().map(Employee::getSalary).distinct().sorted(Comparator.reverseOrder()).skip(1).findFirst();
  Map<Boolean, List<Employee>> parts = list.stream().collect(Collectors.partitioningBy(e -> e.getSalary() > 50000));
  String csv = list.stream().map(Employee::getName).collect(Collectors.joining(", "));
  ```
- **Given `List<Integer>`, write the common stream queries.**
  ```java
  int sumSq = nums.stream().filter(n -> n % 2 == 0).mapToInt(n -> n * n).sum();
  Set<Integer> seen = new HashSet<>();
  Set<Integer> dups = nums.stream().filter(n -> !seen.add(n)).collect(Collectors.toSet());
  int max = nums.stream().mapToInt(Integer::intValue).max().orElseThrow();
  int min = Collections.min(nums);
  List<Integer> uniqueDesc = nums.stream().distinct().sorted(Comparator.reverseOrder()).toList();
  ```
- **Flatten a `List<List<String>>` with `flatMap`.** `List<String> flat = nested.stream().flatMap(List::stream).toList();`.
- **Use `map`, `reduce`, `count` and `sorted` in one pipeline.** `int total = words.stream().map(String::length).sorted().reduce(0, Integer::sum);` and `long n = words.stream().filter(w -> w.length() > 3).count();`. A pipeline has only one terminal operation, so `reduce` and `count` need separate streams.
- **What is printed? A stream with `peek` and no terminal operation.** Nothing. Without a terminal operation the pipeline never runs.

---

## 17. Multithreading basics (Session 27)

### Basics

- ⭐ **What is a thread? What is the difference between a process and a thread?** A thread is the smallest unit of execution within a process. A process is a running program with its own memory space; threads of one process share its heap and resources but each has its own stack. Threads are cheaper to create and switch, and communicate through shared memory.
- **What is multithreading? What are its advantages and drawbacks?** Running several threads in one program concurrently. Advantages: better CPU use, responsive applications, faster handling of independent tasks. Drawbacks: race conditions, deadlocks, harder debugging, and context-switching overhead.
- **What is the difference between multitasking, multiprocessing and multithreading?** Multitasking is the OS running several tasks at once. Multiprocessing uses more than one CPU or core. Multithreading runs several threads inside a single process.
- **What is the difference between concurrency and parallelism?** Concurrency is dealing with many tasks by interleaving them, possibly on one core. Parallelism is executing many tasks at the same instant on multiple cores.
- ⭐ **What are the ways to create a thread in Java?** Extend `Thread` and override `run()`; implement `Runnable` and pass it to a `Thread`; implement `Callable` and submit it to an `ExecutorService`; or use lambdas with any of these. In real code you normally use an executor rather than creating threads directly.
- ⭐ **What is the difference between extending `Thread` and implementing `Runnable`? Which is preferred, and why?** Extending `Thread` uses up your single inheritance slot and ties the task to the thread. Implementing `Runnable` leaves you free to extend another class, separates the task from the thread that runs it, lets the same task be shared and submitted to thread pools, and works with lambdas. `Runnable` is preferred.
- ⭐ **What is the difference between `start()` and `run()`?** `start()` creates a new thread and the JVM calls `run()` on it. Calling `run()` directly just executes it as a normal method on the current thread; no new thread is created.
- **What happens if you call `start()` twice on the same thread?** The second call throws `IllegalThreadStateException`. A thread can be started only once.
- **What happens if you call `run()` directly?** The code runs synchronously in the calling thread, like any other method call.
- ⭐ **Explain the thread life cycle and states.** `NEW`: created, not yet started. `RUNNABLE`: started; running or ready to run. `BLOCKED`: waiting to acquire a monitor lock. `WAITING`: waiting indefinitely for another thread (`wait()`, `join()`). `TIMED_WAITING`: waiting with a timeout (`sleep`, `wait(ms)`, `join(ms)`). `TERMINATED`: `run()` has finished.
- ⭐ **What does `sleep()` do? Does it release the lock?** `Thread.sleep(ms)` pauses the current thread for the given time and puts it in `TIMED_WAITING`. It does not release any locks it holds. It throws `InterruptedException` if interrupted.
- ⭐ **What does `join()` do?** `t.join()` makes the calling thread wait until thread `t` finishes. It is used to wait for results or to run threads in sequence.
- **What does `yield()` do? Is it guaranteed?** `Thread.yield()` hints to the scheduler that the current thread is willing to give up the CPU to others of the same priority. The scheduler may ignore it, so it is not guaranteed and is rarely used.
- **What are thread priorities? Are priorities guaranteed to be honoured?** An integer from 1 (`MIN_PRIORITY`) to 10 (`MAX_PRIORITY`), with 5 (`NORM_PRIORITY`) as the default, inherited from the parent thread. They are only hints; behaviour depends on the OS scheduler, so never rely on them for correctness.
- **What is the main thread? How do you get a reference to the current thread?** The thread the JVM starts to run `main()`. `Thread.currentThread()` returns the thread that is executing the code.
- **What is a daemon thread? How do you create one? When must `setDaemon` be called?** A background service thread that does not prevent the JVM from exiting. Call `t.setDaemon(true)` before `t.start()`; calling it afterwards throws `IllegalThreadStateException`.
- **What is a `ThreadGroup`? Is it still recommended?** A way to group threads so they can be managed together (interrupt all, set max priority). It is considered obsolete; use the Executor framework instead.
- **How do you name a thread and get its name and id?** `new Thread(task, "worker-1")` or `t.setName("worker-1")`; read with `t.getName()` and `t.getId()` (`threadId()` from Java 19).

### Deep dive

- **What is the thread scheduler? Is scheduling pre-emptive or time-sliced in Java?** The part of the OS, used by the JVM, that decides which runnable thread gets the CPU. Java does not specify the policy; on modern operating systems it is pre-emptive with time slicing.
- **What is context switching? What does it cost?** Saving the state of the running thread and restoring another's so the CPU can switch between them. It costs CPU time and invalidates caches, so too many threads reduce throughput.
- **What is the difference between a user thread and a daemon thread at JVM shutdown?** The JVM waits for all user threads to finish. When only daemon threads remain, the JVM exits and daemon threads are abandoned abruptly, without running their `finally` blocks.
- ⭐ **What is the difference between `sleep()` and `wait()`?** `sleep()` is a static method of `Thread`, pauses for a time, keeps its locks, and needs no synchronized block. `wait()` is a method of `Object`, releases the object's lock, must be called inside a synchronized block, and resumes on `notify()`/`notifyAll()` or a timeout.
- **What is the difference between `sleep()` and `yield()`?** `sleep()` pauses for at least the given time and moves the thread to `TIMED_WAITING`. `yield()` only suggests giving up the CPU; the thread stays `RUNNABLE` and may continue immediately.
- **What is `InterruptedException`? How does thread interruption work?** Interruption is a cooperative way to ask a thread to stop. `t.interrupt()` sets the thread's interrupt flag; if the thread is blocked in `sleep`, `wait` or `join`, it throws `InterruptedException` and clears the flag. `isInterrupted()` reads the flag; the static `Thread.interrupted()` reads and clears it.
- **How do you stop a thread safely? Why are `stop()`, `suspend()` and `resume()` deprecated?** Use a `volatile` boolean flag or interruption that the thread checks and then exits cleanly. `stop()` kills the thread immediately and releases its locks, leaving shared data inconsistent. `suspend()` pauses a thread while it holds locks, which easily causes deadlock.
- **What happens when an exception is thrown in a thread's `run()`?** That thread terminates and its uncaught exception handler prints the stack trace. Other threads, including main, continue running.
- **Can `run()` be overloaded? Can `start()` be overridden?** `run()` can be overloaded, but `start()` only calls the no-arg version. `start()` can be overridden, but if you do not call `super.start()`, no new thread is created.
- **What is the difference between `Runnable` and `Callable`? What is a `Future`?** `Runnable.run()` returns nothing and cannot throw checked exceptions. `Callable.call()` returns a value and can throw checked exceptions. A `Future` represents the pending result of a submitted task; `get()` blocks until it is ready, and you can `cancel` it or check `isDone`.
- **What is the Executor framework? Why use a thread pool rather than creating threads?** A framework (`Executor`, `ExecutorService`, `ThreadPoolExecutor`) that separates task submission from thread management. A pool reuses threads, avoiding the cost of creation, and limits the number of threads so the system is not overwhelmed. It also provides queuing, scheduling and lifecycle control.
- **What kinds of thread pools does `Executors` provide? What are the parameters of `ThreadPoolExecutor`?** `newFixedThreadPool(n)`, `newCachedThreadPool()`, `newSingleThreadExecutor()`, `newScheduledThreadPool(n)` and `newWorkStealingPool()`. `ThreadPoolExecutor` takes core pool size, maximum pool size, keep-alive time, a work queue, a thread factory and a rejection handler.
- **What is `CompletableFuture`?** A `Future` that supports non-blocking composition: you chain steps with `thenApply`, `thenCompose` and `thenCombine`, handle errors with `exceptionally`, and combine many with `allOf`, without blocking on `get()`.
- **What are virtual threads (Java 21)? How do they differ from platform threads?** Lightweight threads managed by the JVM rather than the OS. Many virtual threads share a few OS carrier threads; when one blocks on I/O it is unmounted so the carrier can run another. You can create millions, which suits high-concurrency I/O-bound servers. They do not make CPU-bound work faster.
- **What does `Thread.State.BLOCKED` mean versus `WAITING`?** `BLOCKED`: the thread is trying to enter a synchronized block and waiting for the monitor lock. `WAITING`: the thread called `wait()`, `join()` or `park()` and is waiting for another thread's action.
- **How does `join()` work internally?** It is a synchronized method that loops calling `wait()` on the target `Thread` object while that thread is alive. When the thread terminates, the JVM calls `notifyAll()` on it.
- **How do you ensure three threads T1, T2, T3 run in sequence?** Start T1 and call `t1.join()`, then start T2 and `t2.join()`, then T3. Alternatively submit the three tasks to a single-thread executor.
- **What is a shutdown hook?** A thread registered with `Runtime.getRuntime().addShutdownHook(thread)` that the JVM runs when it is shutting down normally (exit or Ctrl+C), used to release resources or flush data.
- **What is `ThreadLocal`? Where is it used, and how can it leak memory in thread pools?** A variable for which each thread has its own independent copy. It is used for per-thread context such as user sessions, transactions and non-thread-safe objects like `SimpleDateFormat`. Pool threads live for a long time, so a value that is never removed stays referenced and may be seen by the next task; always call `remove()` in a `finally` block.

### Code and scenario

- **Create two threads, one by extending `Thread` and one by implementing `Runnable`.**
  ```java
  class MyThread extends Thread { public void run() { System.out.println("Thread: " + getName()); } }
  class MyTask implements Runnable { public void run() { System.out.println("Runnable: " + Thread.currentThread().getName()); } }
  new MyThread().start();
  new Thread(new MyTask()).start();
  ```
- **Assign the same task to three threads, then a different task to each.** Same task: create one `Runnable r` and start `new Thread(r)` three times; all threads share that object and its fields. Different tasks: create three different `Runnable` objects and pass one to each thread.
- **What is printed? `t.run()` versus `t.start()` printing `Thread.currentThread().getName()`.** `t.run()` prints `main`, because it runs on the calling thread. `t.start()` prints the new thread's name, such as `Thread-0`.
- **What happens? `t.start(); t.start();`** The second call throws `IllegalThreadStateException`.
- **What is printed? Main starts a thread and prints a line with and without `t.join()`.** Without `join`, the order is not guaranteed; main's line usually prints first. With `t.join()` before the print, the thread's output always appears first.
- **Write a program that prints 1–10 with a one-second delay between numbers.**
  ```java
  for (int i = 1; i <= 10; i++) {
      System.out.println(i);
      try { Thread.sleep(1000); } catch (InterruptedException e) { Thread.currentThread().interrupt(); return; }
  }
  ```
- **What is printed? A daemon thread running an infinite loop when `main` ends.** It prints for as long as `main` is running. When `main`, the last user thread, finishes, the JVM exits and the daemon thread is stopped.
- **Create threads using an anonymous inner class and using a lambda.**
  ```java
  new Thread(new Runnable() { public void run() { System.out.println("anonymous"); } }).start();
  new Thread(() -> System.out.println("lambda")).start();
  ```
- **Show `sleep`, `join` and `yield` in one demo and explain the output.** Start a worker that prints numbers with `Thread.sleep(500)` between them; the main thread calls `worker.join()` and then prints "done". The numbers appear at half-second intervals and "done" always comes last because of `join`. Adding `Thread.yield()` in a loop may change the interleaving of two threads, but its effect is not guaranteed.
- **Two threads increment a shared counter 10,000 times each without synchronization. What is the result and why?** Usually less than 20,000 and different on each run. `count++` is three steps (read, add, write), so two threads can read the same value and one update is lost. This is a race condition.

---

## 18. Synchronization and inter-thread communication (Sessions 28–29)

### Synchronization

- ⭐ **What is a race condition? What is a critical section?** A race condition is a bug where the result depends on the timing of threads accessing shared mutable data without proper coordination. A critical section is the piece of code that accesses shared data and must be executed by only one thread at a time.
- ⭐ **What is synchronization? Why is it needed?** A mechanism that lets only one thread at a time execute a block of code guarded by a lock. It is needed to prevent race conditions and to make changes by one thread visible to others.
- ⭐ **What is the difference between a synchronized method and a synchronized block? Which is better?** A synchronized method locks the whole method on `this` (or on the `Class` object if static). A synchronized block locks only the statements inside, on any object you choose. The block is usually better because it holds the lock for less time and lets you use a private lock object.
- ⭐ **What is the difference between object-level and class-level locking?** An object-level lock is the monitor of an instance, used by synchronized instance methods and `synchronized(this)`; each object has its own. A class-level lock is the monitor of the `Class` object, used by static synchronized methods and `synchronized(MyClass.class)`; there is one per class.
- **What is a monitor (intrinsic lock)? Which object is the lock for an instance method, a static method and a block?** Every Java object has a built-in lock called its monitor. A synchronized instance method locks `this`; a static synchronized method locks the `Class` object; a block locks the object in the parentheses.
- **Can two threads execute two different synchronized instance methods of the same object at the same time? A synchronized and a non-synchronized method?** No for two synchronized methods, because both need the same object lock. Yes for a synchronized and a non-synchronized method, because the non-synchronized one needs no lock.
- **Can a static synchronized method and an instance synchronized method run concurrently on the same object?** Yes. They use different locks: the `Class` object and the instance.
- **What is reentrancy? Is `synchronized` reentrant?** A thread that already holds a lock can acquire it again without blocking. `synchronized` is reentrant, so a synchronized method can call another synchronized method on the same object. The JVM keeps a hold count.
- **Can a constructor be synchronized?** No, the keyword is not allowed on constructors. You can use a synchronized block inside one, but it is rarely needed because the object is not yet shared.
- **What happens to the lock if a synchronized method throws an exception?** The lock is released automatically when the method or block exits, whether normally or by exception.
- **Why is synchronizing on a `String` literal, a boxed `Integer` or a non-final field a bad idea?** String literals and small boxed integers are shared across the whole JVM, so unrelated code may lock on the same object, causing contention or deadlock. A non-final field can be reassigned, so different threads could lock on different objects. Use a `private final Object lock = new Object();`.
- ⭐ **What is `volatile`? How does it differ from `synchronized`?** `volatile` guarantees that reads and writes of a variable go to main memory, so every thread sees the latest value, and it prevents reordering around it. It gives visibility but not atomicity and does not block. `synchronized` gives both mutual exclusion and visibility, but can block threads.
- **What are visibility, atomicity and ordering?** Visibility: a change by one thread is seen by others. Atomicity: an operation happens entirely or not at all, with no interleaving. Ordering: operations appear to execute in the expected sequence despite compiler and CPU reordering.
- **Is `count++` atomic? Does `volatile` make it atomic?** No and no. It is read, increment, write. `volatile` only makes each read and write visible; two threads can still interleave. Use `synchronized` or `AtomicInteger`.
- **What is the Java Memory Model? What is a happens-before relationship?** The JMM defines how threads interact through memory and when one thread's writes are guaranteed to be visible to another. Happens-before is that guarantee: if action A happens-before B, B sees A's effects. Examples: unlocking a monitor before the next lock of it, a volatile write before a later read, `start()` before the thread's actions, and a thread's actions before `join()` returns.
- **What are atomic classes? What is CAS? What is the ABA problem?** `AtomicInteger`, `AtomicLong`, `AtomicReference` and others provide lock-free thread-safe operations. They use compare-and-swap: update the value only if it still equals the expected value, otherwise retry. The ABA problem is when a value changes from A to B and back to A, so CAS succeeds although something changed; `AtomicStampedReference` solves it with a version stamp.
- **What is `ReentrantLock`? How does it compare with `synchronized`?** An explicit lock from `java.util.concurrent.locks`. Unlike `synchronized` it offers `tryLock()` with or without a timeout, interruptible locking, an optional fairness policy, and multiple `Condition` objects. You must release it yourself in a `finally` block.
- **What is `ReadWriteLock`? What is `StampedLock`?** `ReadWriteLock` allows many concurrent readers or one writer, improving throughput for read-heavy data. `StampedLock` adds optimistic reads: read without locking and then validate, falling back to a real lock if a write occurred.
- **What are `CountDownLatch`, `CyclicBarrier`, `Semaphore` and `Exchanger`?** `CountDownLatch`: threads wait until a counter reaches zero; one-time use. `CyclicBarrier`: a group of threads wait for each other at a common point; reusable. `Semaphore`: limits the number of threads accessing a resource through permits. `Exchanger`: two threads swap objects at a meeting point.
- ⭐ **How do you write a thread-safe singleton? Explain double-checked locking and why `volatile` is needed.** Check if the instance is null, synchronize, check again, then create.
  ```java
  class Singleton {
      private static volatile Singleton instance;
      private Singleton() { }
      static Singleton getInstance() {
          if (instance == null) {
              synchronized (Singleton.class) { if (instance == null) instance = new Singleton(); }
          }
          return instance;
      }
  }
  ```
  `volatile` is needed because `new Singleton()` is not atomic: the reference can be assigned before the constructor finishes, so another thread could see a non-null but half-built object.
- **What is the initialization-on-demand holder idiom?** A lazy, thread-safe singleton with no locking: `private static class Holder { static final Singleton INSTANCE = new Singleton(); }` and `getInstance()` returns `Holder.INSTANCE`. The JVM loads the holder class only on first use and guarantees class initialization is thread-safe.
- **What makes a class thread-safe? How does immutability help?** It behaves correctly when used by multiple threads without extra coordination by the caller. You get there by having no shared state, by immutability, by synchronization, or by using atomic and concurrent classes. Immutable objects cannot change, so there is nothing to race on.
- **What is `ThreadLocal`? How do you give each thread its own data?** A container that holds a separate value per thread: `ThreadLocal<Integer> counter = ThreadLocal.withInitial(() -> 0);` then `counter.get()` and `counter.set(...)`. Since nothing is shared, no synchronization is needed.
- **What is the performance cost of synchronization? What are lock coarsening, lock elision and biased locking?** An uncontended lock is cheap; a contended one causes blocking and context switches. Lock coarsening merges adjacent locks on the same object into one. Lock elision removes locks on objects that escape analysis proves are thread-local. Biased locking made repeated locking by a single thread nearly free; it was deprecated in Java 15 and later removed.

### Deadlock

- ⭐ **What is a deadlock? Give an example.** A situation where two or more threads wait forever for locks held by each other. Thread 1 holds lock A and waits for B, while thread 2 holds lock B and waits for A.
- ⭐ **What are the four necessary conditions for a deadlock?** Mutual exclusion (a resource is held by one thread at a time), hold and wait (a thread holds one resource while waiting for another), no pre-emption (a resource cannot be taken away), and circular wait (a cycle of threads each waiting for the next). All four must hold.
- ⭐ **How do you prevent or avoid deadlock?** Always acquire locks in the same global order; use `tryLock` with a timeout and back off; avoid nested locks and hold locks for as short a time as possible; do not call unknown code while holding a lock; prefer concurrent utilities over manual locking.
- **How do you detect a deadlock in a running JVM?** Take a thread dump with `jstack <pid>` or `jcmd <pid> Thread.print`; the JVM reports "Found one Java-level deadlock" with the threads and locks involved. JConsole and VisualVM have a deadlock detection button, and `ThreadMXBean.findDeadlockedThreads()` does it programmatically.
- **What is the difference between deadlock, livelock and starvation?** Deadlock: threads are blocked forever waiting on each other. Livelock: threads are active and keep reacting to each other but make no progress. Starvation: a thread never gets the CPU or lock because others keep taking it.
- **Can a single thread deadlock itself?** Not with `synchronized` or `ReentrantLock`, because they are reentrant. It can with a non-reentrant lock, or by waiting on its own result, such as a task in a single-thread executor waiting for another task submitted to the same executor.
- **What is the dining philosophers problem?** Five philosophers sit around a table with one fork between each pair, and each needs two forks to eat. If all pick up their left fork at once, they deadlock. It illustrates deadlock and starvation; solutions include ordering the forks or limiting how many may try at once.

### wait, notify, notifyAll

- ⭐ **What do `wait()`, `notify()` and `notifyAll()` do?** `wait()` makes the current thread release the object's lock and wait until notified. `notify()` wakes one thread waiting on that object. `notifyAll()` wakes all threads waiting on it. They let threads coordinate on a shared condition.
- ⭐ **Why must they be called from a synchronized context? What exception is thrown otherwise?** They operate on the object's monitor, so the thread must own it; this also prevents a race between checking the condition and waiting. Otherwise `IllegalMonitorStateException` is thrown.
- ⭐ **Why are they defined in `Object` and not `Thread`?** Locks belong to objects, and threads communicate through the shared object they lock. Every object can be a lock, so these methods are in `Object`.
- ⭐ **What is the difference between `notify()` and `notifyAll()`? When is `notify()` unsafe?** `notify()` wakes one arbitrary waiting thread; `notifyAll()` wakes all, and they compete for the lock. `notify()` is unsafe when threads wait for different conditions on the same object, because the wrong one may be woken and the signal lost. Prefer `notifyAll()` unless all waiters are identical.
- **Does `wait()` release the lock? Does `notify()` release it immediately?** `wait()` releases the lock at once. `notify()` does not; the notifying thread keeps the lock until it leaves the synchronized block, and only then can a woken thread reacquire it.
- ⭐ **Why should `wait()` always be called in a loop? What is a spurious wakeup?** After waking, the condition may no longer be true, because another thread may have acted first, or because of a spurious wakeup, which is a thread waking with no notification (permitted by the JVM). Use `while (!condition) wait();` so the condition is rechecked.
- **What is a lost notification?** `notify()` is called before the other thread has started waiting, so the signal is missed and the waiter waits forever. Avoid it by checking a state variable in a loop inside the synchronized block instead of relying on the notification alone.
- **What is the difference between `wait()` and `wait(timeout)`?** `wait()` waits indefinitely until notified or interrupted. `wait(timeout)` also returns when the time elapses, putting the thread in `TIMED_WAITING`.
- **What state is a thread in after `wait()`, and after being notified but before reacquiring the lock?** `WAITING` (or `TIMED_WAITING`) after `wait()`. After notification it becomes `BLOCKED` until it reacquires the monitor, then `RUNNABLE`.
- **What is `Condition` and how does it improve on wait/notify?** A `Condition` is created from a `Lock` with `lock.newCondition()` and provides `await`, `signal` and `signalAll`. One lock can have several conditions (for example "not full" and "not empty"), so you can wake exactly the right group of threads.

### Producer–consumer

- ⭐ **What is the producer–consumer problem?** Producers add items to a shared bounded buffer and consumers remove them. Producers must wait when the buffer is full, consumers must wait when it is empty, and access to the buffer must be thread-safe.
- ⭐ **Implement it with `wait` and `notify`.**
  ```java
  class Buffer {
      private final Queue<Integer> q = new LinkedList<>(); private final int cap = 5;
      synchronized void produce(int v) throws InterruptedException {
          while (q.size() == cap) wait();
          q.add(v); notifyAll();
      }
      synchronized int consume() throws InterruptedException {
          while (q.isEmpty()) wait();
          int v = q.poll(); notifyAll(); return v;
      }
  }
  ```
- **Implement it with a `BlockingQueue`. Why is this simpler?**
  ```java
  BlockingQueue<Integer> q = new ArrayBlockingQueue<>(5);
  Runnable producer = () -> { try { for (int i = 0; ; i++) q.put(i); } catch (InterruptedException e) { Thread.currentThread().interrupt(); } };
  Runnable consumer = () -> { try { while (true) System.out.println(q.take()); } catch (InterruptedException e) { Thread.currentThread().interrupt(); } };
  ```
  `put` and `take` do the waiting and locking internally, so there is no manual `wait`/`notify` to get wrong.
- **Implement it with `ReentrantLock` and two `Condition`s.**
  ```java
  final Lock lock = new ReentrantLock(); final Condition notFull = lock.newCondition(), notEmpty = lock.newCondition();
  void produce(int v) throws InterruptedException {
      lock.lock();
      try { while (q.size() == cap) notFull.await(); q.add(v); notEmpty.signal(); } finally { lock.unlock(); }
  }
  int consume() throws InterruptedException {
      lock.lock();
      try { while (q.isEmpty()) notEmpty.await(); int v = q.poll(); notFull.signal(); return v; } finally { lock.unlock(); }
  }
  ```
- **How do you handle multiple producers and multiple consumers?** With wait/notify, use `while` loops and `notifyAll()` so the right kind of thread always gets a chance. Simpler: share one `BlockingQueue` among all of them; it is already safe for any number of producers and consumers.
- **How do you shut down a producer–consumer system cleanly?** Put a special "poison pill" object on the queue, one per consumer; a consumer stops when it takes it. Alternatively interrupt the consumers or use `ExecutorService.shutdown()`.
- **What is back-pressure?** Slowing producers down when consumers cannot keep up, so memory is not exhausted. A bounded queue gives it naturally: `put` blocks when the queue is full.

### Code and scenario

- **Write a class that reliably deadlocks two threads, then fix it.**
  ```java
  Object a = new Object(), b = new Object();
  new Thread(() -> { synchronized (a) { sleep(100); synchronized (b) { } } }).start();
  new Thread(() -> { synchronized (b) { sleep(100); synchronized (a) { } } }).start();
  // fix: both threads take the locks in the same order, a then b
  ```
  (`sleep` here stands for `Thread.sleep` wrapped in try/catch.)
- **Fix the shared-counter race with `synchronized`, then with `AtomicInteger`, then with `ReentrantLock`.**
  ```java
  synchronized void inc() { count++; }
  AtomicInteger count = new AtomicInteger(); count.incrementAndGet();
  lock.lock(); try { count++; } finally { lock.unlock(); }
  ```
- **Print odd and even numbers alternately using two threads.**
  ```java
  class Printer {
      private int n = 1; private final int max = 10;
      synchronized void print(boolean odd) throws InterruptedException {
          while (n <= max) {
              if ((n % 2 == 1) == odd) { System.out.println(Thread.currentThread().getName() + " " + n++); notify(); }
              else wait();
          }
          notify();
      }
  }
  // thread "odd" calls print(true); thread "even" calls print(false)
  ```
- **Print "ping" and "pong" alternately using wait/notify.** Keep a shared boolean `pingTurn` guarded by one lock. The ping thread loops: `while (!pingTurn) wait();` print "ping", set `pingTurn = false`, `notifyAll()`. The pong thread does the mirror image.
- **Print 1 to N in order using three threads.** Share a counter and a lock. Thread `i` (0, 1, 2) waits while `(counter - 1) % 3 != i`, then prints the counter, increments it and calls `notifyAll()`. Each thread exits when the counter exceeds N.
- **What happens? `obj.wait()` called without holding `obj`'s monitor.** `IllegalMonitorStateException` at runtime.
- **What is printed? Two threads call a synchronized instance method on two different objects. Is there contention? What if the method is static?** No contention: each thread locks its own object, so they run in parallel. If the method is static, both need the single class-level lock, so they run one after the other.
- **A flag checked in `while (!done) {}` is set by another thread but the loop never ends. Why, and how do you fix it?** The reading thread may never see the update, because the JIT can cache the value in a register or hoist the read out of the loop. Declare the flag `volatile`, or use `AtomicBoolean` or synchronized access.
- **Use `ThreadLocal` so each thread has its own `SimpleDateFormat` or its own counter.**
  ```java
  static final ThreadLocal<SimpleDateFormat> FMT = ThreadLocal.withInitial(() -> new SimpleDateFormat("dd-MM-yyyy"));
  String s = FMT.get().format(new Date());
  static final ThreadLocal<Integer> COUNT = ThreadLocal.withInitial(() -> 0);
  COUNT.set(COUNT.get() + 1); // each thread counts independently
  ```
- **Implement a bounded blocking queue.** It is the same `Buffer` class as the wait/notify producer–consumer above: `put` waits while the queue is full, `take` waits while it is empty, and each calls `notifyAll()` after changing the queue.
- **Implement a simple thread-safe cache.**
  ```java
  class Cache<K, V> {
      private final ConcurrentHashMap<K, V> map = new ConcurrentHashMap<>();
      V get(K key, Function<K, V> loader) { return map.computeIfAbsent(key, loader); } // atomic: loader runs once per key
  }
  ```
- **Given a thread dump excerpt showing two threads each "waiting to lock" what the other holds, explain what it means.** It is a deadlock. Each thread is `BLOCKED` on a monitor the other owns (shown as "locked <0x...>"), forming a cycle. The stack traces show where each lock was taken; fix it by making both code paths acquire the locks in the same order.

---

## 19. Generics and Reflection (Session 30)

### Generics

- ⭐ **What are generics? Why were they introduced?** A feature (Java 5) that lets classes, interfaces and methods take types as parameters, such as `List<String>`. They give compile-time type safety, remove the need for casts, and let you write reusable algorithms.
- **What problems occur with raw types?** A raw type such as `List` skips generic type checking, so you can add any object and get a `ClassCastException` later when reading. The compiler gives only unchecked warnings. They exist for backward compatibility.
- ⭐ **How do you write a generic class? A generic method? A generic interface?** Class: `class Box<T> { private T value; T get() { return value; } }`. Method: `static <T> void print(T item) { }`, with the type parameter before the return type. Interface: `interface Repository<T, ID> { T findById(ID id); }`.
- **What do the conventional type parameter names mean?** `T` type, `E` element, `K` key, `V` value, `N` number, `R` return type; `S` and `U` for additional types.
- ⭐ **What is type erasure? What does the compiler generate?** The compiler checks generic types and then removes them. Type parameters are replaced by their bound (or `Object`), casts are inserted where values are read, and bridge methods are added where needed. At runtime `List<String>` and `List<Integer>` are the same class, `List`.
- **What are bounded type parameters? What are multiple bounds?** `<T extends Number>` restricts `T` to `Number` or its subclasses, so you can call `Number` methods on it. Multiple bounds: `<T extends Number & Comparable<T>>`; a class bound, if any, must come first.
- ⭐ **What are wildcards? Explain `?`, `? extends T` and `? super T`.** A wildcard `?` stands for an unknown type. `List<?>` is a list of some unknown type; you can only read `Object`s. `List<? extends Number>` is a list of `Number` or a subtype; you can read `Number`s but not add. `List<? super Integer>` is a list of `Integer` or a supertype; you can add `Integer`s but read only `Object`.
- ⭐ **What is the PECS rule?** "Producer extends, consumer super." If a parameter only gives you values (you read from it), use `? extends T`. If it only takes values (you write to it), use `? super T`. `Collections.copy(List<? super T> dest, List<? extends T> src)` is the classic example.
- **Why is `List<Integer>` not a subtype of `List<Number>`? What is invariance?** If it were, you could add a `Double` to a list of integers through the `List<Number>` reference. Generics are invariant: `List<A>` and `List<B>` are unrelated even when `A` extends `B`. Use `List<? extends Number>` to accept both.
- **What can you add to a `List<? extends Number>`? What can you read from a `List<? super Integer>`?** You can add only `null` to a `? extends` list, because the exact type is unknown. From a `? super Integer` list you can read only `Object`.
- **What is the difference between `List<?>`, `List<Object>` and raw `List`?** `List<?>` accepts a list of any type but is read-only (apart from null). `List<Object>` accepts only a `List<Object>` and you can add anything to it. Raw `List` turns off type checking entirely.
- **What is the difference between `<T extends Comparable<T>>` and `<T extends Comparable<? super T>>`?** The first requires `T` to be comparable to exactly itself. The second also accepts types whose comparison is defined in a superclass, for example a subclass of a class that implements `Comparable<Parent>`. The second is more flexible and is what `Collections.sort` uses.
- **What is the diamond operator?** `<>` (Java 7), which lets the compiler infer the type arguments: `List<String> list = new ArrayList<>();`.
- **Why can you not create a generic array, instantiate `new T()`, or use `instanceof List<String>`?** Because of type erasure, `T` is unknown at runtime. `new T()` has no class to instantiate; `new T[n]` cannot perform the runtime element-type check arrays need; and `instanceof List<String>` cannot be checked because only `List` exists at runtime.
- **Why can you not use primitives as type arguments?** Type parameters are erased to `Object`, and primitives are not objects. Use wrapper classes; autoboxing handles the conversion.
- **Why can a generic class not have a static field of type `T`?** Static members are shared by all instances of the class, across all parameterizations, so there is no single `T` for them. `T` belongs to an instance.
- **Can you overload `m(List<String>)` and `m(List<Integer>)`? Why not?** No. After erasure both become `m(List)`, so they have the same signature: a "name clash" compile error.
- **What are bridge methods?** Synthetic methods the compiler generates to preserve polymorphism after erasure. If a class implements `Comparable<String>` with `compareTo(String)`, the compiler adds `compareTo(Object)` that casts and delegates to it.
- **What is heap pollution? What is `@SafeVarargs`?** Heap pollution is when a variable of a parameterized type refers to an object that is not of that type, usually through raw types or generic varargs, leading to a later `ClassCastException`. `@SafeVarargs` is the author's assertion that a generic varargs method does not do anything unsafe with the array, which suppresses the warning.
- **What is an unchecked warning? When is `@SuppressWarnings("unchecked")` acceptable?** A compiler warning that an operation cannot be verified for type safety because of erasure, such as casting to `T` or using raw types. Suppress it only when you have proved the code is safe, on the smallest possible scope, with a comment explaining why.
- **What is a reifiable type?** A type whose full type information is available at runtime: primitives, non-generic classes, raw types, unbounded wildcards such as `List<?>`, and arrays of these. `List<String>` is not reifiable.
- **What is wildcard capture?** The compiler gives the unknown type of a wildcard a temporary name so the value can be used type-safely. A common trick is a private generic helper: `void swap(List<?> l) { swapHelper(l); }` with `<T> void swapHelper(List<T> l)`.
- **What is a recursive type bound?** A type parameter bounded by an expression involving itself: `<T extends Comparable<T>>`, meaning "any type that can be compared with itself". `Enum<E extends Enum<E>>` is another example.
- **How do generics differ from C++ templates?** C++ generates a separate copy of the code for each type at compile time and supports primitives and specialization. Java compiles one version, erases the types, and supports only reference types.
- **Can a generic class extend `Throwable`? Can you catch a type parameter?** No to both. The catch mechanism works at runtime, where generic type information does not exist. You can, however, use a type parameter in a `throws` clause.
- **How do you obtain generic type information at runtime?** Generic information in class, field and method declarations is kept in the class file and readable by reflection (`getGenericSuperclass()`, `ParameterizedType`). The "super type token" trick creates an anonymous subclass, such as `new TypeReference<List<String>>() {}`, and reads its generic superclass.

### Reflection and metadata

- ⭐ **What is reflection? What can you do with it?** The ability of a program to inspect and manipulate classes, methods, fields and constructors at runtime through the `java.lang.reflect` API. You can discover a class's structure, create objects, call methods and read or write fields, including private ones, without knowing them at compile time.
- **What are the ways to obtain a `Class` object?** `MyClass.class`, `obj.getClass()` and `Class.forName("com.app.MyClass")`.
- **What do `getMethods` versus `getDeclaredMethods`, `getFields` versus `getDeclaredFields` return?** `getMethods()` and `getFields()` return public members including inherited ones. `getDeclaredMethods()` and `getDeclaredFields()` return all members declared in that class, including private ones, but not inherited ones.
- ⭐ **How do you invoke a private method or access a private field with reflection? What does `setAccessible(true)` do?** Get it with `getDeclaredMethod` or `getDeclaredField`, call `setAccessible(true)`, then `invoke(obj, args)` or `get(obj)`/`set(obj, value)`. `setAccessible(true)` suppresses Java's access checks for that member.
- **How do you create an object using reflection? Why is `Class.newInstance()` deprecated?** `clazz.getDeclaredConstructor().newInstance()`, or pass parameter types to pick a constructor. `Class.newInstance()` (deprecated in Java 9) only uses the no-arg constructor and lets checked exceptions from the constructor escape without being wrapped.
- **What are the drawbacks of reflection?** It is slower than direct calls, bypasses compile-time type checking so errors appear at runtime, breaks encapsulation, can be blocked by security rules or modules, and makes code harder to read and refactor.
- **Where is reflection used in practice?** Dependency injection in Spring, ORM mapping in Hibernate, JSON libraries such as Jackson, test frameworks like JUnit and Mockito, IDEs and debuggers, and annotation processing at runtime.
- **How does reflection break a singleton? How do you defend against it?** It makes the private constructor accessible and calls it to create a second instance. Throw an exception from the constructor if an instance already exists, or use an enum singleton, which reflection cannot instantiate.
- **Can you change a `final` field through reflection?** An instance final field can usually be changed after `setAccessible(true)`, but code may not see the change if the value was inlined as a compile-time constant. Static final fields and record fields cannot be changed this way, and newer JDKs are restricting final-field mutation further.
- **How does the module system (Java 9+) restrict reflective access? What is `InaccessibleObjectException`?** Reflection into non-public members works only if the target package is `opens` to your module. Otherwise `setAccessible(true)` throws `InaccessibleObjectException`. The `--add-opens` flag can open a package at launch.
- ⭐ **What are annotations (metadata)? Name the built-in ones.** Annotations attach metadata to code that the compiler, tools or frameworks can read; they do not change logic by themselves. Built-in: `@Override`, `@Deprecated`, `@SuppressWarnings`, `@FunctionalInterface` and `@SafeVarargs`.
- **What are meta-annotations?** Annotations applied to annotation definitions. `@Retention` sets how long the annotation is kept; `@Target` sets where it can be used; `@Inherited` lets subclasses inherit it; `@Documented` includes it in Javadoc; `@Repeatable` allows it more than once on one element.
- **What are the retention policies?** `SOURCE`: discarded by the compiler (`@Override`). `CLASS`: stored in the class file but not available at runtime; the default. `RUNTIME`: available through reflection at runtime, needed by frameworks.
- **How do you create a custom annotation and read it at runtime?** Declare it with `@interface` and `@Retention(RetentionPolicy.RUNTIME)`, then read it with `clazz.getAnnotation(MyAnnotation.class)` or `isAnnotationPresent`.
- **What is a dynamic proxy?** A class created at runtime with `Proxy.newProxyInstance` that implements given interfaces and routes every method call to an `InvocationHandler`. It is the basis of AOP, transactions and mocking. For classes without interfaces, libraries such as CGLIB or ByteBuddy generate subclasses.
- **What is the difference between `Class.forName()` and `ClassLoader.loadClass()`?** `Class.forName(name)` loads and initializes the class, running its static blocks. `ClassLoader.loadClass(name)` only loads it; initialization happens on first use.
- **What exceptions does reflection throw?** `ClassNotFoundException` (no such class), `NoSuchMethodException` and `NoSuchFieldException` (no such member), `IllegalAccessException` (access not allowed), `InstantiationException` (abstract class or interface), and `InvocationTargetException`, which wraps an exception thrown by the invoked method.
- **What are method handles and how do they compare with reflection?** `java.lang.invoke.MethodHandle` is a typed, directly executable reference to a method. Access is checked once at lookup rather than on each call, and the JIT can inline it, so it is faster than reflection. It is what `invokedynamic` and lambdas use.

### Code and scenario

- **Write a generic class `Box<T>` and a generic `Pair<K, V>`.**
  ```java
  class Box<T> { private T value; void set(T v) { value = v; } T get() { return value; } }
  class Pair<K, V> { private final K key; private final V value; Pair(K k, V v) { key = k; value = v; } K getKey() { return key; } V getValue() { return value; } }
  ```
- **Write a generic method that swaps two elements of an array, and one that returns the maximum of a list.**
  ```java
  static <T> void swap(T[] a, int i, int j) { T t = a[i]; a[i] = a[j]; a[j] = t; }
  static <T extends Comparable<? super T>> T max(List<? extends T> list) {
      T best = list.get(0);
      for (T x : list) if (x.compareTo(best) > 0) best = x;
      return best;
  }
  ```
- **Does this compile? `List<Number> list = new ArrayList<Integer>();`** No. Generics are invariant, so `ArrayList<Integer>` is not a `List<Number>`.
- **Does this compile? `List<? extends Number> list = new ArrayList<Integer>(); list.add(1);`** The assignment compiles; `list.add(1)` does not. The compiler does not know the exact element type, so it allows adding only `null`.
- **Does this compile? `List<? super Integer> list = new ArrayList<Number>(); list.add(1); Integer x = list.get(0);`** The assignment and `add(1)` compile. `Integer x = list.get(0)` does not, because `get` returns `Object`.
- **Write `copy(List<? super T> dest, List<? extends T> src)` and explain the wildcards.**
  ```java
  static <T> void copy(List<? super T> dest, List<? extends T> src) { for (T item : src) dest.add(item); }
  ```
  `src` produces `T` values, so it uses `extends`. `dest` consumes them, so it uses `super`. This lets you copy a `List<Integer>` into a `List<Number>`.
- **What is printed? `new ArrayList<String>().getClass() == new ArrayList<Integer>().getClass()`** `true`. After type erasure both are simply `ArrayList` at runtime.
- **Add an `Integer` to a `List<String>` using a raw type. Where does the exception occur?** `List raw = strings; raw.add(10);` compiles with a warning and runs without error. The `ClassCastException` occurs later, where an element is read as a `String` (`String s = strings.get(0);`), because that is where the compiler inserted the cast.
- **Use reflection to invoke a private method of another class.**
  ```java
  Method m = Secret.class.getDeclaredMethod("hidden", String.class);
  m.setAccessible(true);
  Object result = m.invoke(new Secret(), "arg");
  ```
- **Use reflection to print all fields and methods of a class and to read a private field.**
  ```java
  Class<?> c = Employee.class;
  for (Field f : c.getDeclaredFields()) System.out.println(f.getType().getSimpleName() + " " + f.getName());
  for (Method m : c.getDeclaredMethods()) System.out.println(m.getName());
  Field f = c.getDeclaredField("salary"); f.setAccessible(true);
  Object value = f.get(employee);
  ```
- **Write a custom annotation `@Info(author, version)` and read it at runtime.**
  ```java
  @Retention(RetentionPolicy.RUNTIME) @Target(ElementType.TYPE)
  @interface Info { String author(); int version() default 1; }
  @Info(author = "Asha", version = 2) class Service { }
  Info info = Service.class.getAnnotation(Info.class);
  System.out.println(info.author() + " v" + info.version());
  ```
- **Create a second instance of a singleton using reflection, then prevent it.**
  ```java
  Constructor<Singleton> c = Singleton.class.getDeclaredConstructor();
  c.setAccessible(true);
  Singleton second = c.newInstance(); // a second instance
  // prevention, inside the class:
  private Singleton() { if (instance != null) throw new IllegalStateException("Already created"); }
  ```
  The most robust prevention is an enum singleton.

---

## 20. Cross-topic and design questions

For design questions, state your assumptions, name the classes and relationships, and then discuss trade-offs. The answers below are outlines to speak from.

### Conceptual

- **Walk through everything that happens from `javac Hello.java` to the output appearing on the console.** `javac` parses the source, checks types and writes `Hello.class` containing bytecode. `java Hello` starts the JVM, which uses the class loaders to load `Hello` and the core classes, verifies the bytecode, prepares static fields, resolves references and runs static initializers. The JVM then calls `main` on the main thread; the interpreter executes bytecode and the JIT compiles hot code. `System.out.println` writes through a `PrintStream` to the OS standard output. When all non-daemon threads finish, the JVM exits.
- **Walk through everything that happens when `new Employee("A")` executes.** If `Employee` is not yet loaded, the JVM loads, links and initializes it, parent first, running static blocks. It allocates memory on the heap for all fields including inherited ones, and zeroes them. The constructor is invoked: it first calls the parent constructor up to `Object`; then for each class on the way down, field initializers and instance blocks run, followed by the constructor body. Finally the reference is returned and stored in the variable on the stack.
- **How would you design an immutable class that contains a mutable collection and a `Date`?** Make the class final and all fields private and final; provide no setters; in the constructor copy the inputs (`new Date(d.getTime())`, `List.copyOf(list)`); in getters return a copy of the date and an unmodifiable list. Better still, use `LocalDate`, which is already immutable.
- **Explain every way a singleton can be broken and how to defend against each.** Reflection calls the private constructor: throw from the constructor if an instance exists. Serialization creates a new object: implement `readResolve()`. Cloning: do not implement `Cloneable`, or throw from `clone()`. Multiple class loaders give one instance per loader: load the class from a shared parent loader. Multithreading creates two instances in lazy initialization: use the holder idiom, double-checked locking with `volatile`, or eager initialization. An enum singleton handles reflection, serialization and threading automatically.
- **Compare all ways to write a singleton.** Eager (`static final` instance): simple and thread-safe, but created even if unused. Lazy without synchronization: not thread-safe. Synchronized `getInstance()`: safe but locks on every call. Double-checked locking with `volatile`: safe and fast but easy to get wrong. Holder idiom: lazy, thread-safe and lock-free; a good default. Enum: shortest and safe against reflection and serialization, but cannot extend a class or be lazily configured.
- **Why is `String` used so widely as a `HashMap` key?** It is immutable, so its hash code cannot change after insertion and can be cached in the object, making lookups fast. It has a well-defined `equals` and `hashCode`. And literals are pooled, so equal keys often share one object and `equals` short-circuits on the reference check.
- **How do `equals`, `hashCode`, `Comparable` and `Comparator` each affect `HashSet`, `TreeSet`, `HashMap` and `TreeMap`?** `HashSet` and `HashMap` use `hashCode` to choose the bucket and `equals` to match the element or key; `compareTo` is not used for equality. `TreeSet` and `TreeMap` use only `compareTo` or the supplied `Comparator` for both ordering and uniqueness; `equals` and `hashCode` are ignored. So keep `compareTo` consistent with `equals`, or the two families will disagree about duplicates.
- **Which collection would you choose, and why?** Fast lookup by key: `HashMap`. Sorted unique elements: `TreeSet`. Insertion order with no duplicates: `LinkedHashSet`. FIFO processing: `ArrayDeque` (or a `BlockingQueue` between threads). Frequent middle insertions: `LinkedList` if you already hold an iterator at the position, otherwise `ArrayList` is usually still faster. Thread-safe read-mostly list: `CopyOnWriteArrayList`. Thread-safe map with heavy writes: `ConcurrentHashMap`.
- **List the main features added in Java 8, 9, 11, 17 and 21.** Java 8: lambdas, Stream API, `Optional`, default and static interface methods, `java.time`, method references. Java 9: modules, private interface methods, `List.of`/`Map.of`, JShell. Java 10: `var`. Java 11: new `String` methods (`isBlank`, `strip`, `repeat`, `lines`), the HTTP client, `var` in lambdas, running a source file directly. Java 17: sealed classes, with records, text blocks, switch expressions and pattern matching for `instanceof` finalized in 14–16. Java 21: virtual threads, pattern matching for `switch`, record patterns, sequenced collections.
- **What are records, sealed classes, pattern matching for `switch` and text blocks?** A record is a compact immutable data class: `record Point(int x, int y) {}` generates the constructor, accessors, `equals`, `hashCode` and `toString`. A sealed class restricts which classes may extend it using `permits`. Pattern matching for `switch` lets cases test types and bind variables: `case Circle c -> ...`, with exhaustiveness checked for sealed types. Text blocks are multi-line string literals in triple quotes.
- **Where can memory leaks come from in Java?** Static collections that only grow; caches with no eviction; listeners and callbacks never unregistered; non-static inner or anonymous classes holding their outer object; `ThreadLocal` values not removed in pooled threads; unclosed streams, connections and other resources; keys mutated after being put in a `HashMap`; and class loader leaks on redeploy.
- **Which design patterns appear in the JDK? Give one example of each.** Singleton: `Runtime.getRuntime()`. Factory: `Calendar.getInstance()`, `List.of()`. Builder: `StringBuilder`, `HttpRequest.newBuilder()`. Decorator: `BufferedReader` wrapping a `Reader`. Iterator: `Iterator`. Observer: `PropertyChangeListener`. Strategy: `Comparator`. Adapter: `Arrays.asList()`, `InputStreamReader`. Proxy: `java.lang.reflect.Proxy`. Template method: `AbstractList`, `InputStream.read(byte[])`.
- **What is the difference between sort stability and hash-based ordering guarantees?** A stable sort keeps equal elements in their original relative order; `Collections.sort` and `List.sort` are stable, which makes multi-key sorting by successive sorts work. Hash-based collections (`HashMap`, `HashSet`) give no ordering guarantee at all, and the order can change after a resize. Use `LinkedHashMap` for insertion order or `TreeMap` for sorted order.
- **How would you make a class with a `List` field safe to share across threads?** Make the list immutable (`List.copyOf`) and the field final, if the data does not change. Otherwise use a thread-safe implementation (`CopyOnWriteArrayList` for mostly reads) or guard every access with the same lock. In all cases keep the field private and never return the internal list; return a copy or an unmodifiable view.

### Design and coding

- **Design an employee management system.** An abstract `Employee` (id, name, base salary, abstract `calculateSalary()`) with `Manager`, `Engineer` and `Intern` subclasses. An `EmployeeService` holds a `Map<Integer, Employee>` for O(1) lookup by id and offers add, remove, find and list. Custom exceptions: `EmployeeNotFoundException` and `DuplicateEmployeeException`. Sorting uses `Comparator`s (by name, salary, department). Persistence: `Employee implements Serializable` with a `serialVersionUID`, saved and loaded through object streams behind an `EmployeeRepository` interface so it can later be swapped for a database.
- **Design a parking lot.** `ParkingLot` has `Floor`s, each with `Slot`s. Enums `VehicleType` (BIKE, CAR, TRUCK) and `SlotType`. `Vehicle` is abstract with subclasses. `Ticket` records the vehicle, slot and entry time. A `FeeStrategy` interface computes the charge (strategy pattern). The slot allocator must be thread-safe: keep free slots per type in a `ConcurrentLinkedQueue` or guard allocation with a lock so two entry gates never get the same slot.
- **Design a library management system and identify association, aggregation and composition.** `Library` has `Book`s and `Member`s: aggregation, since they can exist independently. `Book` has `BookCopy` items that cannot exist without the book: composition. `Loan` links a `Member` and a `BookCopy` with dates: association. Add `LibraryService` with `issue`, `return` and `search`, and exceptions for "not available" and "limit exceeded".
- **Implement an LRU cache that is thread-safe.** Simplest: wrap the `LinkedHashMap`-based LRU cache with `Collections.synchronizedMap`, or make `get` and `put` synchronized. Note that `get` also changes the order in access-order mode, so reads need the lock too. For high concurrency, use a library such as Caffeine, or a `ConcurrentHashMap` with an approximate eviction policy.
- **Implement a custom `ArrayList`, `LinkedList`, `HashMap`, `Stack` and `Queue`.** `ArrayList`: an `Object[]` that grows by 1.5× with `Arrays.copyOf`. `LinkedList`: nodes with `prev` and `next`, plus `head` and `tail`. `HashMap`: an array of buckets with chained nodes, index from `hashCode`, match with `equals`, resize at 0.75 load. `Stack`: an array with a `top` index, or a linked list adding and removing at the head. `Queue`: a circular array with `head`, `tail` and `size`. Sketches of `ArrayList`, `HashMap` and the array queue are in sections 15 and 16.
- **Implement a thread pool with a fixed number of worker threads and a blocking task queue.**
  ```java
  class SimplePool {
      private final BlockingQueue<Runnable> queue = new LinkedBlockingQueue<>();
      private volatile boolean running = true;
      SimplePool(int n) {
          for (int i = 0; i < n; i++) new Thread(() -> {
              while (running || !queue.isEmpty()) {
                  try { Runnable r = queue.poll(1, TimeUnit.SECONDS); if (r != null) r.run(); }
                  catch (InterruptedException e) { Thread.currentThread().interrupt(); return; }
              }
          }, "worker-" + i).start();
      }
      void submit(Runnable task) { if (!running) throw new IllegalStateException("shut down"); queue.offer(task); }
      void shutdown() { running = false; }
  }
  ```
- **Implement a rate limiter.** A token bucket: the bucket holds up to `capacity` tokens and refills at a fixed rate; each request takes one token and is rejected if none is left.
  ```java
  class RateLimiter {
      private final int capacity; private final double perNano; private double tokens; private long last = System.nanoTime();
      RateLimiter(int capacity, double perSecond) { this.capacity = capacity; this.perNano = perSecond / 1e9; this.tokens = capacity; }
      synchronized boolean tryAcquire() {
          long now = System.nanoTime();
          tokens = Math.min(capacity, tokens + (now - last) * perNano); last = now;
          if (tokens >= 1) { tokens -= 1; return true; }
          return false;
      }
  }
  ```
- **Implement an immutable, generic `Pair<K, V>` with correct `equals`, `hashCode` and `toString`.**
  ```java
  public final class Pair<K, V> {
      private final K key; private final V value;
      public Pair(K key, V value) { this.key = key; this.value = value; }
      public K getKey() { return key; } public V getValue() { return value; }
      @Override public boolean equals(Object o) {
          if (this == o) return true;
          if (!(o instanceof Pair)) return false;
          Pair<?, ?> p = (Pair<?, ?>) o;
          return Objects.equals(key, p.key) && Objects.equals(value, p.value);
      }
      @Override public int hashCode() { return Objects.hash(key, value); }
      @Override public String toString() { return "(" + key + ", " + value + ")"; }
  }
  ```
  On Java 16+, `record Pair<K, V>(K key, V value) {}` does the same.
- **Read a large log file, count the occurrences of each error type, and print the top five using streams.**
  ```java
  try (Stream<String> lines = Files.lines(Path.of("app.log"))) {
      lines.filter(l -> l.contains("ERROR"))
           .map(l -> l.split("\\s+")[2])                       // assume the error type is the third token
           .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
           .entrySet().stream()
           .sorted(Map.Entry.<String, Long>comparingByValue().reversed())
           .limit(5)
           .forEach(e -> System.out.println(e.getKey() + " = " + e.getValue()));
  }
  ```
  `Files.lines` is lazy, so the file is not loaded into memory at once.
- **Given a list of transactions, group by customer and compute totals, flagging invalid entries with a custom exception.**
  ```java
  class InvalidTransactionException extends RuntimeException { InvalidTransactionException(String m) { super(m); } }
  Map<String, Double> totals = txns.stream()
      .peek(t -> { if (t.getAmount() <= 0 || t.getCustomer() == null) throw new InvalidTransactionException("Bad txn " + t.getId()); })
      .collect(Collectors.groupingBy(Transaction::getCustomer, Collectors.summingDouble(Transaction::getAmount)));
  ```
  To report invalid entries without stopping, partition the list into valid and invalid first and total only the valid ones.
- **Write a generic method that finds duplicates in any `Collection<T>`.**
  ```java
  static <T> Set<T> duplicates(Collection<T> items) {
      Set<T> seen = new HashSet<>(), dups = new LinkedHashSet<>();
      for (T t : items) if (!seen.add(t)) dups.add(t);
      return dups;
  }
  ```
- **Write a bank transfer between two accounts that cannot deadlock.**
  ```java
  void transfer(Account from, Account to, double amount) {
      Account first = from.getId() < to.getId() ? from : to;   // always lock the lower id first
      Account second = first == from ? to : from;
      synchronized (first) {
          synchronized (second) {
              if (from.getBalance() < amount) throw new IllegalStateException("Insufficient funds");
              from.debit(amount); to.credit(amount);
          }
      }
  }
  ```
  A fixed global lock order removes the circular wait, so two opposite transfers cannot deadlock.

### Debugging scenarios

- **An application throws `OutOfMemoryError` after running for several hours. How do you investigate?** It suggests a leak. Check GC logs or `jstat -gcutil` to confirm the heap level after full GCs keeps climbing. Capture a heap dump (`-XX:+HeapDumpOnOutOfMemoryError` or `jmap -dump`), open it in Eclipse MAT, and look at the dominator tree and leak suspects to find what retains the most memory. Trace the path to GC roots; common culprits are static maps, unbounded caches, listeners and `ThreadLocal`s. Fix the cause before considering a bigger heap.
- **An application hangs with no CPU usage. How do you investigate?** Take two or three thread dumps a few seconds apart with `jstack` or `jcmd Thread.print`. Look for a reported deadlock, or threads that stay `BLOCKED` or `WAITING` on the same lock, an exhausted thread or connection pool, or threads stuck on a socket read with no timeout.
- **An application is at 100% CPU. How do you find the offending thread?** Run `top -H -p <pid>` to find the hot thread's id, convert it to hex, and find the matching `nid` in a `jstack` dump to see its stack trace. Typical causes are an infinite loop, constant garbage collection (check GC logs), or a runaway regular expression. A profiler such as Java Flight Recorder or async-profiler confirms it.
- **A `HashMap` lookup returns `null` for a key you just inserted. What are the possible causes?** The key class does not override `equals` and `hashCode` consistently; the key was mutated after insertion so its hash changed; you are looking up with a different type (`1` versus `1L`, or `String` versus `StringBuilder`); the value stored really is `null`; or another thread modified the non-thread-safe map.
- **A `ConcurrentModificationException` appears in single-threaded code. Why?** The collection was structurally modified while being iterated, usually by calling `list.remove()` or `add()` inside a for-each loop. Use `Iterator.remove()`, `removeIf`, or iterate over a copy.
- **A `NullPointerException` is thrown on a line with no obvious null, such as `int x = map.get(key);`. Why?** `map.get` returned `null` because the key is absent, and auto-unboxing the null `Integer` to `int` throws. Use `getOrDefault`, or assign to an `Integer` and check for null.
- **A deserialization call fails with `InvalidClassException`. Why?** The `serialVersionUID` in the stored data does not match the current class, usually because the class changed and no explicit UID was declared. It can also mean a non-serializable parent lacks an accessible no-arg constructor. Declare a fixed `serialVersionUID` and keep changes compatible.
- **Code works locally but fails in production with `NoClassDefFoundError`. What do you check?** That the dependency JAR is actually on the production classpath and was packaged (not `provided` or test scope); that versions do not conflict, with a different version of the library winning in production; and whether an earlier `ExceptionInInitializerError` shows the class's static initializer failed.
- **A date parsed with `SimpleDateFormat` is occasionally wrong under load. Why?** A single `SimpleDateFormat` instance is being shared between threads, and it is not thread-safe because it keeps mutable internal state. Use `DateTimeFormatter`, a `ThreadLocal` formatter, or a new instance per call.
- **A counter updated by many threads shows a lower value than expected. Why, and what are three fixes?** `count++` is not atomic, so concurrent updates overwrite each other and increments are lost. Fixes: make the increment `synchronized`; use `AtomicInteger.incrementAndGet()`; or use `LongAdder`, which is best under heavy contention. A `ReentrantLock` also works.
