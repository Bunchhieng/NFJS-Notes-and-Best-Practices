# NFJS notes and best practices

## Java 8 Recipes
[Java_8_recipes git repo](https://github.com/kousen/java_8_recipes)

#### Lambda Expression
    - lambda can access local variable outside of it owns scope but it can not modify it.                   

                        
```java
// Sum using loop (iterative and shared mutable state)
// What we usually do (not so good)
int total = 0;
List<Integer> nums = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6, 5);
for (int n : nums) {
    total += n;
}

// Functional, but shared mutable state (won't compile)
total = 0;
nums.forEach(n -> {
            // Can't modify "total"
            // total += n;
        }
);

// Functional, no shared mutable state
// best
total = nums.stream()
        .mapToInt(Integer::valueOf)
        .sum();
```                

* Lambda always has its context that's why in example above `n` doesn't need to provide type, it can automatically inferred type based on the context.

```java
(s1, s2) -> Integer.compare(s1.length(), s2.length())
```                      

#### Functional interface
    - only a single abstract method in it. i.e: `Runnable` is functional interface because `void	run()` is the only abstract method in it.
    * lambda can only assigned to functional interfaces
    * The `@FunctionalInterface` annotation is optional, therefore it doesn't need to be annotate for interface.
    * All functional interface belong to `java.util.function package`.

```java
// Optional
@FunctionalInterface
public interface Runnable
```

    - Types of interface from `java.util.function` package
        - Supplier: no argument and return a single result

```java
// Is a supplier
public Integer getCount(){
    return this.count;
}

// Lambda supplier from java.util.function package
Supplier<String> i  = () -> "hello world";
```

        Its instance method reference `myClass::getCount` is an instance of `Supplier<Integer>`

        - Consumer: takes arguments and returns nothing

```java
public void setCount(int count){
    this.count = count;
}
```

        Its instance method reference `myClass::setCount` is an instance of `Consumer<Integer>` and `IntConsumer`.

        - Function: takes an argument of one type, and returns another. Also call a transformation.

```java
public Integer getCount(){
    return this.count;
}
```

        Its class method reference `MyClass::getCount` is an instance of `Function<MyClass,Integer>` and `ToIntFunction<MyClass>`.

        - Predicate: evaluate a condition and return boolean.

```java
// example 1
Predicate<Employee> isAgeMoreThan(Integer age) {
    return p -> p.getAge() > age;
}
// example 2
s -> s.length % 2 == 0;
```

    * Method Reference: shorthand syntax for a lambda expression that executes just ONE method.
        - can call with static, instance method and also constructor method reference `ClassName::new` (Supplier)

```java
// Syntax: Object :: methodName
// what we usually do
Consumer<String> c = s -> System.out.println(s);
// method reference
Consumer<String> c = System.out::println;
```

#### Java 8 Interface
    * can have default method and static method

```java
@FunctionalInterface
public interface MyInterface {
    int myMethod();
    // int myOtherMethod();

    default String sayHello() {
        return "Hello, World";
    }

    static void myStaticMethod() {
        System.out.println("I'm a static method in an interface");
    }
}
```

    - Conflict between `class` methods and `interface` (default or static) method
        - class methods always win (overrided)

#### `Stream()` is LAZY

    - A sequence of elements supporting sequential and parallel aggregate operations.
              
    - Pipeline of stream: Source -> intermediate operation -> terminal operation
        - Intermediate operation: not evaluated until we chain it with terminal operation.

```java
Stream.map(), Stream.filter(), Stream.limit() 
```

        - Terminal operation: responsible for giving final output for a stream in operation.
        
```java
findAny(), allMatch(), forEach() 
```        

    - Stream example:    

```java
// stream example
List<String> strings = Arrays.asList("yolo", "swag", "icecream");

strings.stream()
        .filter(s -> s.length % 2 == 0) // intermediate operation
        .collect(Collectors.toList());  // terminal operation
```

## Streaming architecture using Kafka

## More functional Java and Java Slang (VAVR)

## <? extends Generic> in Java

## Machine learning with Tensorflow

## High computing big data on JVM

