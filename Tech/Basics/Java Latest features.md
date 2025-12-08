- [[Tech Fundamentals]]

## Java 8 (2014)
- **Lambda expressions** - functional programming
- **Stream API** - functional operations on collections
- **Optional** - null safety
- **Method references** - `::` syntax
- **Default methods** - interfaces with implementation
- **Date/Time API** - `java.time` package (LocalDate, LocalTime, etc.)
- **Nashorn JavaScript engine**

## Java 9 (2017)
- **Modules (JPMS)** - module system with `module-info.java`
- **JShell** - REPL tool
- **Factory methods for collections** - `List.of()`, `Set.of()`, `Map.of()`
- **Stream API improvements** - `takeWhile()`, `dropWhile()`
- **Private methods in interfaces**
- **HTTP/2 Client** (incubator)

## Java 10 (2018)
- **Local-Variable Type Inference** - `var` keyword
- **Application Class-Data Sharing** - faster startup

## Java 11 (2018) - LTS
- **String methods** - `isBlank()`, `lines()`, `strip()`, `repeat()`
- **Files methods** - `readString()`, `writeString()`
- **HTTP Client** - standard API (moved from incubator)
- **`var` in lambda parameters**
- **Removed**: Java EE, CORBA modules

## Java 12 (2019)
- **Switch expressions** (preview)
- **String methods** - `indent()`, `transform()`
- **Files.mismatch()** - compare files

## Java 13 (2019)
- **Text blocks** (preview) - multi-line strings with `"""`
- **Switch expressions** (preview) - `yield` keyword
- **ZGC improvements**

## Java 14 (2020)
- **Switch expressions** - standard feature
- **Pattern matching for instanceof** (preview)
- **Records** (preview) - data classes
- **Text blocks** (preview)
- **Helpful NullPointerExceptions**

## Java 15 (2020)
- **Sealed classes** (preview) - restrict inheritance
- **Text blocks** - standard feature
- **Hidden classes**

## Java 16 (2021)
- **Records** - standard feature
- **Pattern matching for instanceof** - standard feature
- **Sealed classes** (preview)
- **Stream.toList()** - convenient collector

## Java 17 (2021) - LTS
- **Sealed classes** - standard feature
- **Pattern matching for switch** (preview)
- **Foreign Function & Memory API** (incubator)
- **Strong encapsulation** - strict module access

## Java 18 (2022)
- **UTF-8 by default**
- **Simple Web Server** - `jwebserver` command
- **Pattern matching for switch** (preview)
- **Code snippets in JavaDoc**

## Java 19 (2022)
- **Virtual threads** (preview) - Project Loom
- **Pattern matching for switch** (preview)
- **Record patterns** (preview)
- **Foreign Function & Memory API** (preview)

## Java 20 (2023)
- **Scoped values** (preview) - thread-local alternative
- **Record patterns** (preview)
- **Virtual threads** (preview)
- **Pattern matching for switch** (preview)

## Java 21 (2023) - LTS
- **Virtual threads** - standard feature (Project Loom)
- **Pattern matching for switch** - standard feature
- **Record patterns** - standard feature
- **Sequenced collections** - `SequencedSet`, `SequencedMap`
- **String templates** (preview) - `STR."..."`

## Java 22 (2024)
- **String templates** (preview)
- **Scoped values** (preview)
- **Structured concurrency** (preview)
- **Foreign Function & Memory API** (preview)

## Java 23 (2024)
- **String templates** (preview)
- **Primitive types in patterns** (preview)
- **Stream gatherers** (preview)

## Code Examples

### Lambda Expressions (Java 8)
```java
// Before
Collections.sort(list, new Comparator<String>() {
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});

// After
Collections.sort(list, (a, b) -> a.compareTo(b));
list.sort(String::compareTo);
```

### Stream API (Java 8)
```java
List<String> names = List.of("Alice", "Bob", "Charlie");

// Filter and map
List<String> upper = names.stream()
    .filter(n -> n.length() > 3)
    .map(String::toUpperCase)
    .collect(Collectors.toList());

// Group by
Map<Integer, List<String>> byLength = names.stream()
    .collect(Collectors.groupingBy(String::length));

// Reduce
Optional<String> longest = names.stream()
    .reduce((a, b) -> a.length() > b.length() ? a : b);
```

### Optional (Java 8)
```java
Optional<String> name = Optional.ofNullable(getName());

// Old way
if (name.isPresent()) {
    System.out.println(name.get());
}

// New way
name.ifPresent(System.out::println);
String result = name.orElse("Default");
String value = name.orElseGet(() -> computeDefault());
```

### Method References (Java 8)
```java
// Static method
list.forEach(System.out::println);

// Instance method
list.stream().map(String::toUpperCase);

// Constructor
Supplier<List<String>> listSupplier = ArrayList::new;
```

### Default Methods in Interfaces (Java 8)
```java
interface Calculator {
    int add(int a, int b);
    
    default int multiply(int a, int b) {
        return a * b;
    }
    
    static int subtract(int a, int b) {
        return a - b;
    }
}
```

### Date/Time API (Java 8)
```java
LocalDate date = LocalDate.now();
LocalDate tomorrow = date.plusDays(1);
LocalDateTime dateTime = LocalDateTime.of(2024, 1, 1, 12, 0);

Duration duration = Duration.between(start, end);
Period period = Period.between(startDate, endDate);

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
String formatted = date.format(formatter);
```

### Factory Methods for Collections (Java 9)
```java
// Immutable collections
List<String> list = List.of("a", "b", "c");
Set<Integer> set = Set.of(1, 2, 3);
Map<String, Integer> map = Map.of("key1", 1, "key2", 2);

// Mutable from immutable
List<String> mutable = new ArrayList<>(List.of("a", "b"));
```

### Stream Improvements (Java 9)
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

// takeWhile - stops when condition is false
numbers.stream()
    .takeWhile(n -> n < 4)
    .forEach(System.out::println); // 1, 2, 3

// dropWhile - skips until condition is false
numbers.stream()
    .dropWhile(n -> n < 4)
    .forEach(System.out::println); // 4, 5, 6
```

### Var Keyword (Java 10)
```java
// Type inference
var list = new ArrayList<String>();
var map = Map.of("key", "value");
var stream = list.stream();

// In loops
for (var item : list) {
    System.out.println(item);
}

// Cannot use var for null, lambda without type, method parameters
```

### String Methods (Java 11)
```java
String text = "  Hello World  ";

text.isBlank();        // true if empty or whitespace
text.strip();          // trim Unicode whitespace
text.stripLeading();   // trim leading whitespace
text.stripTrailing();  // trim trailing whitespace
text.repeat(3);        // repeat string
text.lines();          // Stream<String> of lines

// Files
String content = Files.readString(path);
Files.writeString(path, content);
```

### Switch Expressions (Java 14+)
```java
// Old switch statement
int result;
switch (day) {
    case MONDAY:
    case FRIDAY:
        result = 5;
        break;
    case SATURDAY:
        result = 10;
        break;
    default:
        result = 0;
}

// New switch expression
int result = switch (day) {
    case MONDAY, FRIDAY -> 5;
    case SATURDAY -> 10;
    default -> 0;
};

// With yield for complex logic
int result = switch (day) {
    case MONDAY -> {
        System.out.println("Monday");
        yield 5;
    }
    default -> 0;
};
```

### Pattern Matching instanceof (Java 16+)
```java
// Before
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.length());
}

// After
if (obj instanceof String s) {
    System.out.println(s.length()); // s is in scope
}

// With negation
if (!(obj instanceof String s)) {
    return;
}
System.out.println(s.length()); // s available here too
```

### Records (Java 14+)
```java
// Simple record
record Person(String name, int age) {}

// Record with validation
record Person(String name, int age) {
    Person {
        if (age < 0) throw new IllegalArgumentException("Age must be positive");
    }
}

// Record with methods
record Point(int x, int y) {
    public double distance() {
        return Math.sqrt(x * x + y * y);
    }
}

// Usage
Person p = new Person("Alice", 30);
System.out.println(p.name()); // getter
System.out.println(p);        // toString()
```

### Sealed Classes (Java 17+)
```java
// Base sealed class
sealed class Shape permits Circle, Rectangle, Triangle {}

// Permitted subclasses
final class Circle extends Shape {
    double radius;
}

non-sealed class Rectangle extends Shape {
    double width, height;
}

sealed class Triangle extends Shape permits EquilateralTriangle {}

final class EquilateralTriangle extends Triangle {}
```

### Pattern Matching for Switch (Java 21+)
```java
Object obj = "Hello";

String result = switch (obj) {
    case Integer i -> "Integer: " + i;
    case String s when s.length() > 5 -> "Long string: " + s;
    case String s -> "String: " + s;
    case null -> "null";
    default -> "Unknown";
};

// With sealed classes
Shape shape = new Circle(5.0);
double area = switch (shape) {
    case Circle c -> Math.PI * c.radius() * c.radius();
    case Rectangle r -> r.width() * r.height();
    case Triangle t -> 0.5 * t.base() * t.height();
};
```

### Record Patterns (Java 21+)
```java
record Point(int x, int y) {}
record Line(Point start, Point end) {}

// Deconstructing records
if (obj instanceof Point(int x, int y)) {
    System.out.println(x + ", " + y);
}

// Nested patterns
if (obj instanceof Line(Point(int x1, int y1), Point(int x2, int y2))) {
    System.out.println("Line from (" + x1 + "," + y1 + ") to (" + x2 + "," + y2 + ")");
}

// In switch
double distance = switch (line) {
    case Line(Point(int x1, int y1), Point(int x2, int y2)) ->
        Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
};
```

### Virtual Threads (Java 21+)
```java
// Create virtual thread
Thread vt = Thread.ofVirtual().start(() -> {
    System.out.println("Virtual thread");
});

// Executor with virtual threads
try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> System.out.println("Task 1"));
    executor.submit(() -> System.out.println("Task 2"));
}

// Many virtual threads (millions possible)
for (int i = 0; i < 1_000_000; i++) {
    Thread.ofVirtual().start(() -> {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {}
    });
}
```

### Text Blocks (Java 15+)
```java
// Multi-line string
String html = """
    <html>
        <body>
            <p>Hello World</p>
        </body>
    </html>
    """;

// JSON
String json = """
    {
        "name": "John",
        "age": 30
    }
    """;

// SQL
String query = """
    SELECT id, name
    FROM users
    WHERE age > 18
    """;

// With interpolation (Java 21+ preview)
String name = "Alice";
String message = STR."Hello \{name}!";
```

### Sequenced Collections (Java 21+)
```java
// SequencedSet maintains insertion order
SequencedSet<String> set = new LinkedHashSet<>();
set.add("first");
set.add("second");
set.addFirst("zero");  // add at beginning
set.addLast("third");  // add at end
set.getFirst();        // get first
set.getLast();         // get last

// SequencedMap maintains insertion order
SequencedMap<String, Integer> map = new LinkedHashMap<>();
map.put("a", 1);
map.put("b", 2);
map.firstEntry();      // first entry
map.lastEntry();       // last entry
```

### Stream.toList() (Java 16+)
```java
// Before
List<String> list = stream.collect(Collectors.toList());

// After - returns immutable list
List<String> list = stream.toList();
```

### HTTP Client (Java 11+)
```java
HttpClient client = HttpClient.newHttpClient();

HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .GET()
    .build();

// Synchronous
HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());

// Asynchronous
client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
    .thenApply(HttpResponse::body)
    .thenAccept(System.out::println);
```

