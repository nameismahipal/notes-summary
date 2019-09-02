[source ](https://winterbe.com/posts/2014/03/16/java-8-tutorial/)

- Default Methods (also known as Extension Methods)
    > enables us to add non-abstract method implementations to interfaces by utilizing the `default` keyword. 
- Lamdba Expressions
- Functional Interfaces (@FunctionalInterface)
    > Each lambda corresponds to a given type, specified by an interface.

     > Functional interface must contain exactly one abstract method declaration.
- Method and Constructor References (::)
- Lambda Scopes
    >You can access final variables from the local outer scope as well as instance fields and static variables.
- Accessing fields and static variables
    > have both read and write access to instance fields and static variables from within lambda expressions.
- Accessing Default Interface Methods
    > Default Methods can't be accessed from within Lambda Expressios.
- Built In Interfaces
    - Predicates
        - Predicates are boolean-valued functions of one argument.
    - Functions
        - Functions accept one argument and produce a result.
        - Default methods can be used to chain multiple functions together (compose, andThen).
    - Suppliers
        - Suppliers produce a result of a given generic type.
        - Unlike Functions, Suppliers don't accept arguments. 
    - Consumers
        - Consumers represents operations to be performed on a single input argument.
    - Comparators
        - Java 8 adds various default methods to this interface.
    - Optionals
        - Optionals are not functional interfaces, instead it's a nifty utility to prevent `NullPointerException`.
        - Optional is a simple container for a value which may be null or non-null. Think of a method which may return a non-null result but sometimes return nothing. Instead of returning null you return an Optional in Java 8.     
- Streams
    - A java.util.Stream represents a sequence of elements on which one or more operations can be performed. 
    - Stream operations are either `intermediate` or `terminal`.
        - Terminal operations return a result of a certain type.
        - Intermediate operations return the stream itself so you can chain multiple method calls in a row. 
    
    1. Sequential Streams
        - Filter
            - Filter accepts a predicate to filter all elements of the stream. 
            - This operation is intermediate which enables us to call another stream operation (forEach) on the result.
            - ForEach is a terminal operation. It's void, so we cannot call another stream operation.
        - Sorted
            - Sorted is an intermediate operation which returns a sorted view of the stream. 
            - The elements are sorted in natural order unless you pass a custom Comparator.
        - Map
            - The intermediate operation map converts each element into another object via the given function.
            - you can also use map to transform each object into another type.
        - Match
            - Various matching operations can be used to check whether a certain predicate matches the stream. 
            - All of those operations are terminal and return a boolean result.
        - Count
            - Count is a terminal operation returning the number of elements in the stream as a long.
        - Reduce
            - This terminal operation performs a reduction on the elements of the stream with the given function. - The result is an Optional holding the reduced value.
    2. Parallel Streams
        - Sequential Sort vs Parallel Sort
        - Map
            - Maps don't support streams. 
            - Maps now support various new and useful methods for doing common tasks.

- Date API

    > Package: `java.time`

    - Clock
        - Clock provides access to the current date and time.
        - Clocks are aware of a timezone and may be used instead of System.currentTimeMillis() to retrieve the current milliseconds. 
    - Timezones
        - Timezones are represented by a ZoneId. 
        - They can easily be accessed via static factory methods. 
    - LocalTime
    - LocalDate

- Annotations
    > Annotations in Java 8 are repeatable.    
## Default Methods for Interfaces

Also known as 'Extension Methods'

enables us to add non-abstract method implementations to interfaces by utilizing the `default` keyword. 

Example: 
```Java
interface Formula {
    double calculate(int a);

    default double sqrt(int a) {
        return Math.sqrt(a);
    }
}
```
## Lambda Expressions

prior versions of Java:
```Java
List<String> names = Arrays.asList("peter", "anna", "mike", "xenia");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return b.compareTo(a);
    }
});
```
The static utility method Collections.sort accepts a list and a comparator in order to sort the elements of the given list. 

Java 8 comes with a much shorter syntax, lambda expressions:

```Java
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
```
it gets even shorter:
```Java
Collections.sort(names, (String a, String b) -> b.compareTo(a));
```
it gets even more shorter:
```Java
Collections.sort(names, (a, b) -> b.compareTo(a));
```
## Functional Interfaces

How does lambda expressions fit into Javas type system? 

Each lambda corresponds to a given type, specified by an interface. 

**Functional interface must contain exactly one abstract method declaration.**

To ensure that your interface meet the requirements, you should add the `@FunctionalInterface` annotation. 
(compiler error as soon as you try to add a second abstract method declaration to the interface.)

```java
@FunctionalInterface
interface Converter<F, T> {
    T convert(F from);
}
```

```java
Converter<String, Integer> converter = (from) -> Integer.valueOf(from);

Integer converted = converter.convert("123");
System.out.println(converted);    // 123
```

## Method and Constructor References (::)

Java 8 enables you to pass references of methods or constructors via the `::` keyword.

### Method References

Above example can also be referenced as 
```java
Converter<String, Integer> converter = Integer::valueOf;
Integer converted = converter.convert("123");
System.out.println(converted);   // 123
```

Anotoher Example:
```
class Something {
    String startsWith(String s) {
        return String.valueOf(s.charAt(0));
    }
}
```
```java
Something something = new Something();
Converter<String, String> converter = something::startsWith;
String converted = converter.convert("Java");
System.out.println(converted);    // "J"
```

### Constructor References

```
class Person {
    String firstName;
    String lastName;

    Person() {}

    Person(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}
```

Next we specify a person factory interface to be used for creating new persons:
```java
interface PersonFactory<P extends Person> {
    P create(String firstName, String lastName);
}
```

Instead of implementing the factory manually, we glue everything together via constructor references:
```java
PersonFactory<Person> personFactory = Person::new;
Person person = personFactory.create("Peter", "Parker");
```
We create a reference to the Person constructor via `Person::new`. 

The Java compiler automatically chooses the right constructor by matching the signature of `PersonFactory.create`.

## Lambda Scopes

Accessing outer scope variables from lambda expressions is very similar to anonymous objects. 

You can access final variables from the local outer scope as well as instance fields and static variables.

We can read final local variables from the outer scope of lambda expressions:
```java
final int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

stringConverter.convert(2);     // 3
```

Unlinke anonymous objects, final is not required.

This code is also valid:
```java
int num = 1;

Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);

stringConverter.convert(2);     // 3
```
However num must be implicitly final for the code to compile. 

The following code does not compile:
```java
int num = 1;
Converter<Integer, String> stringConverter =
        (from) -> String.valueOf(from + num);
num = 3;
```
Writing to num from within the lambda expression is also prohibited.

## Accessing fields and static variables

In constrast to local variables, we `have both read and write access to instance fields and static variables` from within lambda expressions.

```java
class Lambda4 {
    static int outerStaticNum;
    int outerNum;

    void testScopes() {
        Converter<Integer, String> stringConverter1 = (from) -> {
            outerNum = 23;
            return String.valueOf(from);
        };

        Converter<Integer, String> stringConverter2 = (from) -> {
            outerStaticNum = 72;
            return String.valueOf(from);
        };
    }
}
```

## Accessing Default Interface Methods
> Default Methods can't be accessed from within Lambda Expressios.

## Built-in Functional Interfaces

*The JDK 1.8 API contains many built-in functional interfaces.* 

*Some of them are well known from older versions of Java like `Comparator or Runnable.`* 

*`Those existing interfaces are extended to enable Lambda support via the @FunctionalInterface annotation.`*

But the Java 8 API is also full of new functional interfaces to make your life easier:
- Predicates
    > boolean-valued functions of one argument.

### Predicates

Predicates are `boolean-valued functions of one argument`.

The interface contains various default methods for composing predicates to complex logical terms (and, or, negate)
```java
Predicate<String> predicate = (s) -> s.length() > 0;

predicate.test("foo");              // true
predicate.negate().test("foo");     // false

Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();
```

Predicate<String> predicate = (s) -> s.length() > 0;

predicate.test("foo");              // true
predicate.negate().test("foo");     // false

Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();

### Functions
> Functions accept one argument and produce a result. 

Default methods can be used to chain multiple functions together (compose, andThen).

```java
Function<String, Integer> toInteger = Integer::valueOf;
Function<String, String> backToString = toInteger.andThen(String::valueOf);

backToString.apply("123");     // "123"
```
### Suppliers
>Suppliers produce a result of a given generic type. Unlike Functions, Suppliers don't accept arguments.
```
Supplier<Person> personSupplier = Person::new;
personSupplier.get();   // new Person
```
### Consumers
>Consumers represents operations to be performed on a single input argument.
```
Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);

greeter.accept(new Person("Luke", "Skywalker"));
```

### Comparators
Comparators are well known from older versions of Java. Java 8 adds various default methods to the interface.
```
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);

Person p1 = new Person("John", "Doe");
Person p2 = new Person("Alice", "Wonderland");

comparator.compare(p1, p2);             // > 0
comparator.reversed().compare(p1, p2);  // < 0
```
### Optionals
> Optionals are not functional interfaces, instead it's a nifty utility to prevent NullPointerException. 

It's an important concept for the next section, so let's have a quick look at how Optionals work.

Optional is a simple container for a value which may be null or non-null.

Think of a method which may return a non-null result but sometimes return nothing. Instead of returning null you return an Optional in Java 8.
```
Optional<String> optional = Optional.of("bam");

optional.isPresent();           // true
optional.get();                 // "bam"
optional.orElse("fallback");    // "bam"

optional.ifPresent((s) -> System.out.println(s.charAt(0)));     // "b"
```

# Streams

A `java.util.Stream` represents a sequence of elements on which one or more operations can be performed. 

Stream operations are either `intermediate` or `terminal`. 

**Terminal operations** return a result of a certain type, 

**Intermediate operations** return the stream itself so you can chain multiple method calls in a row. 

Streams are created on a source, 
e.g. a java.util.Collection like lists or sets (maps are not supported). 

Stream operations can either be executed sequential or parallel.

## Sequential Streams
Let's first look how sequential streams work. First we create a sample source in form of a list of strings:

```java
List<String> stringList = new ArrayList<>();
stringList.add("ddd2");
stringList.add("aaa2");
stringList.add("bbb1");
stringList.add("aaa1");
stringList.add("bbb3");
stringList.add("ccc");
stringList.add("bbb2");
stringList.add("ddd1");
```

Collections in Java 8 are extended so you can simply create streams either by calling Collection.stream() or Collection.parallelStream(). 

The following sections explain the most common stream operations.

### Filter

Filter accepts a predicate to filter all elements of the stream. 

This operation is *intermediate* which enables us to call another stream operation (`forEach`) on the result. 

`ForEach` accepts a consumer to be executed for each element in the filtered stream. 

ForEach is a terminal operation. It is void, so we cannot call another stream operation.

```java
stringList
    .stream()
    .filter((s) -> s.startsWith("a"))
    .forEach(System.out::println);

// "aaa2", "aaa1"
```

### Sorted

Sorted is an intermediate operation which returns a sorted view of the stream. 

The elements are sorted in natural order unless you pass a custom Comparator.
```java
stringList
    .stream()
    .sorted()
    .filter((s) -> s.startsWith("a"))
    .forEach(System.out::println);

// "aaa1", "aaa2"
```
Keep in mind that sorted does only create a sorted view of the stream without manipulating the ordering of the backed collection. The ordering of stringCollection is untouched:
```java
System.out.println(stringCollection);
// ddd2, aaa2, bbb1, aaa1, bbb3, ccc, bbb2, ddd1
```

### Map
The intermediate operation map converts each element into another object via the given function. 

The following example converts each string into an upper-cased string. 

But you can also use map to transform each object into another type. 

The generic type of the resulting stream depends on the generic type of the function you pass to map.

```java
stringCollection
    .stream()
    .map(String::toUpperCase)
    .sorted((a, b) -> b.compareTo(a))
    .forEach(System.out::println);

// "DDD2", "DDD1", "CCC", "BBB3", "BBB2", "AAA2", "AAA1"
```

### Match

Various matching operations can be used to check whether a certain predicate matches the stream. 

All of those operations are terminal and return a boolean result.

```java
boolean anyStartsWithA =
    stringCollection
        .stream()
        .anyMatch((s) -> s.startsWith("a"));

System.out.println(anyStartsWithA);      // true

boolean allStartsWithA =
    stringCollection
        .stream()
        .allMatch((s) -> s.startsWith("a"));

System.out.println(allStartsWithA);      // false

boolean noneStartsWithZ =
    stringCollection
        .stream()
        .noneMatch((s) -> s.startsWith("z"));

System.out.println(noneStartsWithZ);      // true
```

### Count

Count is a terminal operation returning the number of elements in the stream as a long.

```java
long startsWithB =
    stringCollection
        .stream()
        .filter((s) -> s.startsWith("b"))
        .count();

System.out.println(startsWithB);    // 3
```

### Reduce

This terminal operation performs a reduction on the elements of the stream with the given function. 

The result is an Optional holding the reduced value.

```java
Optional<String> reduced =
    stringCollection
        .stream()
        .sorted()
        .reduce((s1, s2) -> s1 + "#" + s2);

reduced.ifPresent(System.out::println);
// "aaa1#aaa2#bbb1#bbb2#bbb3#ccc#ddd1#ddd2"
```
## Parallel Streams

Operations on sequential streams are performed on a single thread.

Operations on parallel streams are performed concurrent on multiple threads.

This Example demonstrates how easy it is to increase the performance by using parallel streams.

Step 1: First we create a large list of unique elements
```java
int max = 1000000;

List<String> values = new ArrayList<>(max);

for (int i = 0; i < max; i++) {

    UUID uuid = UUID.randomUUID();
    values.add(uuid.toString());

}
```

Now we **measure the time** it takes to sort a stream of this collection.

### Sequential Sort
```java
long t0 = System.nanoTime();

long count = values.stream().sorted().count();
System.out.println(count);

long t1 = System.nanoTime();

long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("sequential sort took: %d ms", millis));

// sequential sort took: 899 ms
```

### Parallel Sort
```java
long t0 = System.nanoTime();

long count = values.parallelStream().sorted().count();
System.out.println(count);

long t1 = System.nanoTime();

long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("parallel sort took: %d ms", millis));

// parallel sort took: 472 ms
```

### Map

As already mentioned maps don't support streams. 

Instead maps now support various new and useful methods for doing common tasks.

```java
Map<Integer, String> map = new HashMap<>();

for (int i = 0; i < 10; i++) {
    map.putIfAbsent(i, "val" + i);
}

map.forEach((id, val) -> System.out.println(val));
```

The above code should be self-explaining: 

`putIfAbsent` prevents us from writing additional if null checks.

`forEach` accepts a consumer to perform operations for each value of the map.

This example shows how to compute code on the map by utilizing functions:
```java
map.computeIfPresent(3, (num, val) -> val + num);
map.get(3);             // val33

map.computeIfPresent(9, (num, val) -> null);
map.containsKey(9);     // false

map.computeIfAbsent(23, num -> "val" + num);
map.containsKey(23);    // true

map.computeIfAbsent(3, num -> "bam");
map.get(3);             // val33
```

Removing entries for a a given key, only if it's currently mapped to a given value:

```java
map.remove(3, "val3");
map.get(3);             // val33

map.remove(3, "val33");
map.get(3);             // null
Another helpful method:

map.getOrDefault(42, "not found");  // not found
```

Merging entries of a map is quite easy:

```java
map.merge(9, "val9", (value, newValue) -> value.concat(newValue));
map.get(9);             // val9

map.merge(9, "concat", (value, newValue) -> value.concat(newValue));
map.get(9);             // val9concat
```