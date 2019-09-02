[source ](hhttps://winterbe.com/posts/2018/09/24/java-11-tutorial/)

## Local Variable Type Inference

Java 10 has introduced a new language keyword `var` which optionally replaces the type information when declaring local variables.

Prior to Java 10 you would declare variables like this:
```java
String text = "Hello Java 9";
```
Now you can replace String with var. 
```java
var text = "Hello Java 10";
```
Variables declared with var are still statically typed. You cannot reassign incompatible types to such variables. This code snippet does not compile:
```java
var text = "Hello Java 11";
text = 23;  // Incompatible types
```
You can also use final in conjunction with var to forbid reassigning the variable with another value:
```java
final var text = "Banana";
text = "Joe";   // Cannot assign a value to final variable 'text'
```
Also var is not allowed when the compiler is incapable of infering the correct type of the variable. 

All of the following code samples result in compiler errors:
```java
// Cannot infer type:
var a;
var nothing = null;
var lambda = () -> System.out.println("Pity!");
var method = this::someMethod;
```
Local variable type inference really shines with generics involved.

 In the next example `current` has a rather verbose type of `Map<String, List<Integer>>` which can be reduced to a single var keyword, saving you from typing a lot of boilerplate:
```java
var myList = new ArrayList<Map<String, List<Integer>>>();

for (var current : myList) {
    // current is infered to type: Map<String, List<Integer>>
    System.out.println(current);
}
```
As of Java 11 the var keyword is also allowed for lambda parameters which enables you to add annotations to those parameters:
```
Predicate<String> predicate = (@Nullable var a) -> true;
```

### HTTP Client
- Java 9 introduced a new incubating `HttpClient` API for dealing with HTTP requests. 
- available in the standard libraries package java.net. 

The new `HttpClient` can be used either synchronously or asynchronously. 

A synchronous request blocks the current thread until the reponse is available. 

`BodyHandlers` define the expected type of response body (e.g. as string, byte-array or file):

Synchronous:

```java
var request = HttpRequest.newBuilder()
    .uri(URI.create("https://winterbe.com"))
    .GET()
    .build();

var client = HttpClient.newHttpClient();

HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

System.out.println(response.body());
```
Asynchronous:

Calling sendAsync does not block the current thread and instead returns a CompletableFuture to construct asynchronous operation pipelines.
```java
var request = HttpRequest.newBuilder()
    .uri(URI.create("https://winterbe.com"))
    .build();

var client = HttpClient.newHttpClient();

client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
    .thenApply(HttpResponse::body)
    .thenAccept(System.out::println);
```   
We can omit the .GET() call as it's the default request method.

Below example sends data to a given URL via POST. 

Similiar to BodyHandlers you use `BodyPublishers` to define the type of data you want to send as body of the request such as strings, byte-arrays, files or input-streams:

```java
var request = HttpRequest.newBuilder()
    .uri(URI.create("https://postman-echo.com/post"))
    .header("Content-Type", "text/plain")
    .POST(HttpRequest.BodyPublishers.ofString("Hi there!"))
    .build();
var client = HttpClient.newHttpClient();
var response = client.send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.statusCode());      // 200
```

The last sample demonstrates how to perform authorization via BASIC-AUTH:
```java
var request = HttpRequest.newBuilder()
    .uri(URI.create("https://postman-echo.com/basic-auth"))
    .build();
var client = HttpClient.newBuilder()
    .authenticator(new Authenticator() {
        @Override
        protected PasswordAuthentication getPasswordAuthentication() {
            return new PasswordAuthentication("postman", "password".toCharArray());
        }
    })
    .build();
var response = client.send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.statusCode());      // 200
```

### Collections

Collections such as List, Set and Map have been extended with new methods.

`List.of` created a new immutable list from the given arguments. 

`List.copyOf` creates an immutable copy of the list.

```java
var list = List.of("A", "B", "C");
var copy = List.copyOf(list);
System.out.println(list == copy);   // true
```
Because list is already immutable there's no practical need to actually create a copy of the list-instance, therefore list and copy are the same instance. However if you copy a mutable list, copy is indeed a new instance so it's garanteed there's no side-effects when mutating the original list:
```java
var list = new ArrayList<String>();
var copy = List.copyOf(list);
System.out.println(list == copy);   // false
```
When creating immutable maps you don't have to create map entries yourself but instead pass keys and values as alternating arguments:
```java
var map = Map.of("A", 1, "B", 2);
System.out.println(map);    // {B=2, A=1}
```

>Immutable collections in Java 11 still use the same interfaces from the old Collection API. However if you try to modify an immutable collection by adding or removing elements, a java.lang.UnsupportedOperationException is thrown. Luckily Intellij IDEA warns via an inspection if you try to mutate immutable collections.

### Streams

Streams were introduced in Java 8 and now receive three new methods. 

`Stream.ofNullable` constructs a stream from a single element:

```java
Stream.ofNullable(null)
    .count()   // 0
```

The methods `dropWhile` and `takeWhile` both accept a predicate to determine which elements to abandon from the stream:

```java
Stream.of(1, 2, 3, 2, 1)
    .dropWhile(n -> n < 3)
    .collect(Collectors.toList());  // [3, 2, 1]
```

```java
Stream.of(1, 2, 3, 2, 1)
    .takeWhile(n -> n < 3)
    .collect(Collectors.toList());  // [1, 2]
```


### Optionals
Optionals also receive a few quite handy new methods, 

e.g. you can now simply turn optionals into streams or provide another optional as fallback for an empty optional:
```java
Optional.of("foo").orElseThrow();     // foo
Optional.of("foo").stream().count();  // 1
Optional.ofNullable(null)
    .or(() -> Optional.of("fallback"))
    .get();                           // fallback
```

### Strings
One of the most basic classes String gets a few helper methods for trimming or checking whitespace and for streaming the lines of a string:
```java
" ".isBlank();                // true
" Foo Bar ".strip();          // "Foo Bar"
" Foo Bar ".stripTrailing();  // " Foo Bar"
" Foo Bar ".stripLeading();   // "Foo Bar "
"Java".repeat(3);             // "JavaJavaJava"
"A\nB\nC".lines().count();    // 3
```

### InputStreams
`InputStream` finally gets a super useful method to transfer data to an `OutputStream`, a usecase that's very common when working with streams of raw data.
```java
var classLoader = ClassLoader.getSystemClassLoader();
var inputStream = classLoader.getResourceAsStream("myFile.txt");

var tempFile = File.createTempFile("myFileCopy", "txt");

try (var outputStream = new FileOutputStream(tempFile)) {
    inputStream.transferTo(outputStream);
}
```
