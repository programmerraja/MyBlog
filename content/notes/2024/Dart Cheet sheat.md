---
title : Dart Cheet sheat
date : 2024-07-06T06:55:55.5555+05:30
draft : true
tags : 
---

### Intro
- The Dart language itself is implemented in C++ and Dart.
- The Dart VM (Virtual Machine) is written in C++.
- **Strongly Typed Language:**
- **Static Type Checking:** Types are checked at compile-time, reducing runtime errors and improving code quality.
- **Sound Null Safety:** Helps eliminate null reference errors by distinguishing between nullable and non-nullable types. Null safety is enforced at compile time.
- Everything in dart is object

### Dart Compilation

**Compilation Process:**
- **Ahead-of-Time (AOT) Compilation:** For optimized, fast-running, native machine code, mainly used for mobile apps. Produces efficient code but requires a longer compilation time.
- **Just-in-Time (JIT) Compilation:** For faster development cycles, especially during development. Provides hot reload capabilities, allowing for immediate feedback.
- **Transpilation to JavaScript:** When targeting web platforms, Dart code is compiled to JavaScript using the `dart2js` compiler.

**Tools:**
- **Dart SDK:** Includes tools like `dart2js`, `dartanalyzer`, and `dartdevc` (Dart Development Compiler).
- **Flutter:** Uses Dart and provides a framework for building natively compiled applications for mobile, web, and desktop from a single codebase.

### How Dart Works Internally

**Dart Virtual Machine (Dart VM):**
- Executes Dart code in a virtualized environment.
- Supports JIT compilation for fast development cycles and AOT compilation for optimized performance.

**Garbage Collection:**
- Dart uses a generational garbage collector.
- Objects are allocated in the young generation and promoted to the old generation if they survive garbage collection cycles.

**Isolates:**
- Dart’s concurrency model uses isolates, which are independent workers with their own memory and event loop.
- Communication between isolates is done through message passing.

```dart
import 'dart:isolate';

void entryPoint(SendPort sendPort) {
  sendPort.send('Hello from isolate!');
}

void main() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(entryPoint, receivePort.sendPort);
  receivePort.listen((message) {
    print(message); // Output: Hello from isolate!
  });
}
```


**Package Manager (pub):**
- Dart uses `pub` for package management, allowing for easy dependency management and package distribution.

**Cross-Platform Development:**
- With Flutter, Dart enables the creation of apps that run on iOS, Android, web, and desktop with a single codebase.

### Performance Optimization

**Tree Shaking:**
- Removes unused code during the compilation process, resulting in smaller and faster binaries.

**Ahead-of-Time Compilation:**
- Produces highly optimized native code for better performance on mobile and desktop platforms.

**Hot Reload:**
- Provides quick iterations by injecting updated code into the running app without restarting the entire application, mainly used in Flutter development.

`dart run hello.dart`
`dart create my_project`

### Dart Basics

`main` function, which is the entry point of every Dart application. The `main` function is where the execution of the program begins.

```dart
void main() {
  print('Hello, Dart!');
}

//`main` Function with Command-Line Arguments:
void main(List<String> arguments) {
  if (arguments.isNotEmpty) {
    print('Hello, ${arguments[0]}!');
  } else {
    print('Hello, Dart!');
  }
}

//dart run main.dart Alice
```

**Variables:**
```dart
var name = 'Dart';
String language = 'Dart';
int number = 42;
double pi = 3.14;
bool isCool = true;
```

**Control Flow:**
```dart
if (age > 18) {
  print('Adult');
} else {
  print('Minor');
}

for (var i = 0; i < 5; i++) {
  print(i);
}

while (isRunning) {
  run();
}
```

**Collections:**
```dart
List<int> numbers = [1, 2, 3];
Set<String> names = {'Alice', 'Bob'};
Map<String, int> ages = {'Alice': 30, 'Bob': 25};
```

### Functions

```dart
void printMessage(String message) {
  print(message);
}

int add(int a, int b) {
  return a + b;
}
//Arrow function
String greet(String name) => 'Hello, $name!';
```

**Calling Functions:**
```dart
void main() {
  printMessage('Hello, Dart!');
  int sum = add(2, 3);
  print(sum); // Output: 5
  print(greet('Alice')); // Output: Hello, Alice!
}
```

### Optional Positional Parameters

**Defining Functions with Optional Positional Parameters:**
```dart
void describe(String name, [int age, String city]) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (city != null) print('City: $city');
}

void main() {
  describe('Alice'); // Output: Name: Alice
  describe('Bob', 30); // Output: Name: Bob Age: 30
  describe('Charlie', 25, 'New York'); // Output: Name: Charlie Age: 25 City: New York
}
```

### Named Parameters

**Defining Functions with Named Parameters:**
```dart
void describe({String name, int age, String city}) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (city != null) print('City: $city');
}

void main() {
  describe(name: 'Alice'); // Output: Name: Alice
  describe(name: 'Bob', age: 30); // Output: Name: Bob Age: 30
  describe(name: 'Charlie', age: 25, city: 'New York'); // Output: Name: Charlie Age: 25 City: New York
}
```

**Required Named Parameters:**
```dart
void describe({required String name, int age = 0, String city = 'Unknown'}) {
  print('Name: $name');
  print('Age: $age');
  print('City: $city');
}

void main() {
  describe(name: 'Alice'); // Output: Name: Alice Age: 0 City: Unknown
  describe(name: 'Bob', age: 30); // Output: Name: Bob Age: 30 City: Unknown
  describe(name: 'Charlie', age: 25, city: 'New York'); // Output: Name: Charlie Age: 25 City: New York
}
```

### Optional and Named Parameters Combined

**Combining Optional Positional and Named Parameters:**
```dart
void describe(String name, {int age, String city}) {
  print('Name: $name');
  if (age != null) print('Age: $age');
  if (city != null) print('City: $city');
}

void main() {
  describe('Alice'); // Output: Name: Alice
  describe('Bob', age: 30); // Output: Name: Bob Age: 30
  describe('Charlie', age: 25, city: 'New York'); // Output: Name: Charlie Age: 25 City: New York
}
```

### Default Parameter Values

**Using Default Parameter Values:**
```dart
void describe({String name = 'Unknown', int age = 0, String city = 'Unknown'}) {
  print('Name: $name');
  print('Age: $age');
  print('City: $city');
}

void main() {
  describe(); // Output: Name: Unknown Age: 0 City: Unknown
  describe(name: 'Alice'); // Output: Name: Alice Age: 0 City: Unknown
  describe(name: 'Bob', age: 30); // Output: Name: Bob Age: 30 City: Unknown
  describe(name: 'Charlie', age: 25, city: 'New York'); // Output: Name: Charlie Age: 25 City: New York
}
```

### Anonymous Functions

**Defining and Using Anonymous Functions:**
```dart
void main() {
  var list = ['apples', 'bananas', 'oranges'];
  
  list.forEach((item) {
    print('${list.indexOf(item)}: $item');
  });

  list.forEach((item) => print('${list.indexOf(item)}: $item'));
}
```

### Closures

**Using Closures:**
```dart
Function makeAdder(int addBy) {
  return (int i) => addBy + i;
}

void main() {
  var add2 = makeAdder(2);
  var add4 = makeAdder(4);

  print(add2(3)); // Output: 5
  print(add4(3)); // Output: 7
}
```

### Object-Oriented Programming

**Classes:**
```dart
class Person {
  String name;
  int age;
  //Dart will assgin vaule passed to this.name and this.age
  Person(this.name, this.age);
  
  void greet() {
    print('Hello, my name is $name.');
  }
}

void main() {
  var person = Person('Alice', 30);
  person.greet();
}
```

**Inheritance:**
```dart
class Employee extends Person {
  int employeeId;
  
  Employee(String name, int age, this.employeeId) : super(name, age);
  
  @override
  void greet() {
    print('Hello, my name is $name and my ID is $employeeId.');
  }
}
```
### Classes and Objects

**Defining a Class:**
```dart
class Animal {
  String name;
  int age;

  Animal(this.name, this.age);

  void makeSound() {
    print('$name makes a sound.');
  }
}
```

**Creating an Object:**
```dart
void main() {
  Animal cat = Animal('Whiskers', 2);
  cat.makeSound(); // Output: Whiskers makes a sound.
}
```

### Constructors

**Default Constructor:**
```dart
class Dog {
  String name;
  Dog(this.name);
}

void main() {
  Dog dog = Dog('Buddy');
  print(dog.name); // Output: Buddy
}
```

**Named Constructors:**
```dart
class Dog {
  String name;
  int age;

  Dog(this.name, this.age);

  Dog.named(String name) {
    this.name = name;
    this.age = 0;
  }
}

void main() {
  Dog puppy = Dog.named('Buddy');
  print('${puppy.name} is ${puppy.age} years old.'); // Output: Buddy is 0 years old.
}
```

**Factory Constructors:**
```dart
class Square {
  double side;

  Square(this.side);

  factory Square.fromArea(double area) {
    return Square(Math.sqrt(area));
  }
}

void main() {
  Square square = Square.fromArea(16);
  print(square.side); // Output: 4.0
}
```

### Inheritance

**Extending a Class:**
```dart
class Animal {
  String name;
  Animal(this.name);

  void makeSound() {
    print('$name makes a sound.');
  }
}

class Dog extends Animal {
  Dog(String name) : super(name);

  @override
  void makeSound() {
    print('$name barks.');
  }
}

void main() {
  Dog dog = Dog('Buddy');
  dog.makeSound(); // Output: Buddy barks.
}
```

### Abstract Classes

**Defining an Abstract Class:**
```dart
abstract class Shape {
  void draw();
}

class Circle extends Shape {
  @override
  void draw() {
    print('Drawing a circle');
  }
}

void main() {
  Circle circle = Circle();
  circle.draw(); // Output: Drawing a circle
}
```

### Interfaces

**Implementing Interfaces:**
```dart
class Flyer {
  void fly() {
    print('Flying');
  }
}

class Bird implements Flyer {
  @override
  void fly() {
    print('Bird is flying');
  }
}

void main() {
  Bird bird = Bird();
  bird.fly(); // Output: Bird is flying
}
```

### Mixins

**Using Mixins:**
```dart
mixin Swimmer {
  void swim() {
    print('Swimming');
  }
}

class Animal {
  String name;
  Animal(this.name);
}

class Fish extends Animal with Swimmer {
  Fish(String name) : super(name);
}

void main() {
  Fish fish = Fish('Goldfish');
  fish.swim(); // Output: Swimming
}
```

### Encapsulation

**Getters and Setters:**
```dart
class Person {
  String _name;

  String get name => _name;

  set name(String newName) {
    if (newName.length > 3) {
      _name = newName;
    } else {
      print('Name is too short.');
    }
  }
}

void main() {
  Person person = Person();
  person.name = 'John';
  print(person.name); // Output: John
  person.name = 'Jo'; // Output: Name is too short.
}
```

### Polymorphism

**Method Overriding:**
```dart
class Animal {
  void sound() {
    print('Animal makes a sound');
  }
}

class Cat extends Animal {
  @override
  void sound() {
    print('Cat meows');
  }
}

void main() {
  Animal myCat = Cat();
  myCat.sound(); // Output: Cat meows
}
```

### Static Members

**Static Variables and Methods:**
```dart
class MathUtils {
  static const double pi = 3.14159;

  static double square(double number) {
    return number * number;
  }
}

void main() {
  print(MathUtils.pi); // Output: 3.14159
  print(MathUtils.square(4)); // Output: 16
}
```


### Asynchronous Programming

**Async/Await:**
```dart
Future<void> fetchData() async {
  var data = await fetchFromServer();
  print(data);
}

Future<String> fetchFromServer() {
  return Future.delayed(Duration(seconds: 2), () => 'Data from server');
}
```

**Streams:**
```dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    yield i;
  }
}

void main() async {
  await for (var value in countStream(5)) {
    print(value);
  }
}
```

### Error Handling

**Try/Catch:**
```dart
try {
  int result = 10 ~/ 0;
} catch (e) {
  print('Error: $e');
}
```

## Packages

## Core Libraries 

`dart:core`

**Basic Types:**
- `int`
- `double`
- `String`
- `bool`

**Collections:**
- `List<T>`
- `Set<T>`
- `Map<K, V>`

**Common Methods:**

**String Methods:**
```dart
String text = 'Hello, Dart!';
text.length; // 12
text.toLowerCase(); // 'hello, dart!'
text.toUpperCase(); // 'HELLO, DART!'
text.contains('Dart'); // true
text.replaceAll('Dart', 'World'); // 'Hello, World!'
text.split(', '); // ['Hello', 'Dart!']
```

**List Methods:**
```dart
List<int> numbers = [1, 2, 3];
numbers.add(4); // [1, 2, 3, 4]
numbers.remove(2); // [1, 3, 4]
numbers.length; // 3
numbers.contains(3); // true
numbers.sort(); // [1, 3, 4]
```

**Map Methods:**
```dart
Map<String, int> ages = {'Alice': 30, 'Bob': 25};
ages['Charlie'] = 20; // {'Alice': 30, 'Bob': 25, 'Charlie': 20}
ages.remove('Bob'); // {'Alice': 30, 'Charlie': 20}
ages.keys; // ('Alice', 'Charlie')
ages.values; // (30, 20)
```

### `dart:async`

**Future:**
```dart
Future<int> fetchData() async {
  return 42;
}

void main() async {
  int data = await fetchData();
  print(data); // 42
}
```

**Stream:**
```dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    yield i;
  }
}

void main() async {
  await for (int i in countStream(3)) {
    print(i); // 1, 2, 3
  }
}
```

### `dart:collection`

**Queue:**
```dart
import 'dart:collection';

Queue<int> queue = Queue();
queue.addAll([1, 2, 3]);
queue.removeFirst(); // 1
queue.removeLast(); // 3
```

**LinkedHashMap:**
```dart
LinkedHashMap<String, int> map = LinkedHashMap();
map['one'] = 1;
map['two'] = 2;
```

### `dart:convert`

**JSON Encoding/Decoding:**
```dart
import 'dart:convert';

String jsonString = '{"name": "Alice", "age": 30}';
Map<String, dynamic> user = jsonDecode(jsonString);
print(user['name']); // Alice

String jsonString = jsonEncode({'name': 'Alice', 'age': 30});
print(jsonString); // {"name":"Alice","age":30}
```

### `dart:io`

**File Operations:**
```dart
import 'dart:io';

void main() async {
  String path = 'example.txt';

  // Write to a file
  File file = File(path);
  await file.writeAsString('Hello, Dart!');

  // Read from a file
  String contents = await file.readAsString();
  print(contents); // Hello, Dart!
}
```

**HttpClient:**
```dart
import 'dart:io';

void main() async {
  HttpClient client = HttpClient();
  HttpClientRequest request = await client.getUrl(Uri.parse('https://example.com'));
  HttpClientResponse response = await request.close();
  String contents = await response.transform(utf8.decoder).join();
  print(contents);
}
```

### `dart:math`

**Math Functions:**
```dart
import 'dart:math';

void main() {
  print(pi); // 3.141592653589793
  print(sqrt(16)); // 4.0
  print(pow(2, 3)); // 8
  print(max(5, 10)); // 10
  print(min(5, 10)); // 5
}
```

**Random Numbers:**
```dart
import 'dart:math';

void main() {
  Random random = Random();
  print(random.nextInt(100)); // Random integer between 0 and 99
  print(random.nextDouble()); // Random double between 0.0 and 1.0
  print(random.nextBool()); // Random boolean value
}
```

### `dart:typed_data`

**Typed Data:**
```dart
import 'dart:typed_data';

void main() {
  Uint8List bytes = Uint8List(4);
  bytes[0] = 1;
  bytes[1] = 2;
  bytes[2] = 3;
  bytes[3] = 4;
  print(bytes); // [1, 2, 3, 4]
}
```

### `dart:developer`

**Debugging:**
```dart
import 'dart:developer';

void main() {
  debugger();
  log('This is a log message.');
}
```

### `dart:isolate`

**Isolates:**
```dart
import 'dart:isolate';

void entryPoint(SendPort sendPort) {
  sendPort.send('Hello from isolate!');
}

void main() async {
  ReceivePort receivePort = ReceivePort();
  await Isolate.spawn(entryPoint, receivePort.sendPort);
  receivePort.listen((message) {
    print(message); // Hello from isolate!
  });
}
```



**Using Packages:**
1. Add dependency in `pubspec.yaml`:
    ```yaml
    dependencies:
      http: ^0.13.3
    ```

2. Import and use:
    ```dart
    import 'package:http/http.dart' as http;
    
    void main() async {
      var response = await http.get(Uri.parse('https://example.com'));
      print(response.body);
    }
    ```




