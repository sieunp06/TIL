### 목차
1. [Stream 생성하기](#stream-생성하기)
    - [배열 스트림](#배열-스트림)
    - [컬렉션 스트림](#배열-스트림)
    - [비어있는 스트림](#비어있는-스트림)
    - [Stream.builder()](#streambuilder)
    - [Stream.generate()](#streamgenerate)
    - [Stream.iterate()](#streamiterate)
    - [기본 타입형 스트림](#기본-타입형-스트림)
    - [문자열 스트림](#문자열-스트림)
2. [Stream 가공하기](#stream-가공하기)
    - [Filter](#filter)
    - [Sorting](#sorting)
    - [Iterating](#iterating)
3. [Stream 결과 만들기](#stream-결과-만들기)
    - [Calculating](#calculating)
    - [Reduction](#reduction)
    - [Collecting](#collecting)
    - [Matching](#matching)
    - [Iterating](#iterating)

---
## Stream 생성하기
### 배열 스트림 
`Arrays.stream()`을 사용하여 배열을 스트림으로 만들 수 있다.

```java
String[] arr = new String[]{"a", "b", "c"};

Stream<String> stream = Arrays.stream(arr);
Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);    // b, c
```

### 컬렉션 스트림
Collection(Collection, List, Set)의 경우, 인터페이스 내부에 `stream()`이 존재하기 때문에 이를 사용하여 Collection을 Stream으로 만들 수 있다.

```java
List<String> list = new Arrays.asList("a", "b", "c");

Stream<String> stream = list.stream();
Stream<String> parallelStream = list.parallelStream();  // 병렬 처리 스트림
```

### 비어있는 스트림
```java
Stream<String> stream = Stream.empty();
```

### Stream.builder()
`builder()`를 사용하여 Stream을 만들 수 있다.

```java
Stream<String> builderStream = 
            Stream.<String>builder()
                .add("a").add("b").add("b")
                .build();   // a, b, c
```

### Stream.generate()
`generate()`를 사용해서 Stream을 만들 수 있다.

```java
public static<T> Stream<T> generate(Supplier<T> s) { ... }
```

`generate()`는 함수형 인터페이스인 `Supplier`을 사용하여 람다로 값을 넣을 수 있다.

> 📌 [functional interface 함수형 인터페이스 - Supplier](https://github.com/sieunp06/TIL/blob/main/Java/Functional-Interface.md#suppliert)

<br>

이때, `generate()`로 생성되는 Stream은 크기가 정해져있지 않기 때문에 이를 따로 제한해야 한다.

```java
Stream<String> generatedStream = 
            Stream.generate(() -> "a").limit(5);   // a, a, a, a, a
```

`limit()`을 사용해서 크기를 5로 제한하였기 때문에 Stream 내부에 `a`가 제한된 크기인 5개 들어간 Stream이 생성되었다.

### Stream.iterate()
`iterate()`는 초기값과 해당 값을 다루는 람다를 이용하여 Stream에 들어갈 요소를 만든다.

```java
Stream<Integer> iteratedStream = 
        Stream.iterate(30, n -> n + 2).limit(5);    // 30, 32, 34, 36, 38
```

`iterate()`는 `generate()`와 동일하게 크기가 정해져 있지 않기 때문에 `limit()`을 사용하여 크기를 제한해야 한다.

### 기본 타입형 스트림
제네릭을 사용하지 않고 기본 타입 스트림을 만들 수 있다.
```java
IntStream intStream = IntStream.range(1, 10);   // 1 ~ 9
LongStream longStream = LongStream.range(1, 1000);  // 1 ~ 999
```

위 스트림은 제네릭을 사용하지 않기 떄문에 불필요한 오토 박싱이 일어나지 않는다.

```java
Stream<Integet> boxedIntStream = IntStream.range(1, 5).boxed();
```

위처럼 `boxed()`를 사용하여 박싱을 할 수 있다.

#### Random()
이때 `Random()` 클래스를 통해 랜덤 값을 스트림으로 만들 수 있다.

```java
Doublestream stream = new Random().double(3);   // Random 값 3개 생성
```

### 문자열 스트림
`char()`을 사용하여 문자열 스트림을 만들 수 있다.

```java
IntStream stream = "Hello".chars();     // 72, 101, 108, 108, 111
```

아래와 같이 정규표현식을 통해 문자열을 잘라, 각 요소로 스트림을 만들 수도 있다.

```java
Stream<String> stringStream = 
            Pattern.compile(", ").splitAsStream("a, b, c"); // a, b, c
```

## 스트림 연결
`concat()`을 사용하여 두 개의 스트림을 하나의 스트림으로 묶을 수 있다.

```java
Stream<String> stream1 = Stream.of("1", "2", "3");
Stream<String> stream2 = Stream.of("a", "b", "c");

Stream<String> result = Stream.concat(stream1, stream2);    // 1, 2, 3, a, b, c
```

## Stream 가공하기
### Filter
```java
Stream<T> filter(Predicate<? super T> predicate);
```

`filter`는 스트림 내 요소들 중 특정 요소만을 걸러낸다.

```java
Stream<String> stream = names.stream()
                .filter(name -> name.contains("a"));
```

### Mapping
```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```
`map`은 스트림 내의 요소를 특정 값으로 변환한다.

```java
Stream<String> stream = names.stream()
                .map(String::toUpperCase);
```

### Sorting
```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```

`sorted`는 스트림 내의 요소들을 정렬한다.

```java
IntStream.of(14, 11, 20, 39, 23)
            .sorted()
            .boxed()
            .collect(Collectors.toList());
```

위 예시처럼 `sorted()`를 아무 인자 없이 사용하면 오름차순으로 정렬된다.

```java
List<String> lang = Arrays.asList("Java", "Scala", "Groovy", "Python", "Go", "Swift");

lang.stream()
    .sorted()
    .collect(Collectors.toList());
// [Go, Groovy, Java, Python, Scala, Swift]

lang.stream()
    .sorted(Comparator.reverseOrder())
    .collect(Collectors.toList());
// [Swift, Scala, Python, Java, Groovy, Go]
```

만약 정렬 기준을 커스텀하고 싶다면 `Comparator`을 사용하면 된다.

```java
lang.stream()
    .sorted(Comparator.comparingInt(String::length))
    .collect(Collectors.toList());
// [Go, Java, Scala, Swift, Groovy, Python]

lang.stream()
    .sorted((s1, s2) -> s2.length() - s1.length())
    .collect(Collectors.toList());
// [Groovy, Python, Scala, Swift, Java, Go]
```

### Iterating
```java
Stream<T> peek(Consumer<? super T> action);
```

`peek`은 스트림 내의 요소들을 대상으로 특정 연산을 수행한다.

```java
int sum = IntStream.of(1, 3, 5, 7, 9)
    .peek(System.out::println)
    .sum();
```

`Consumer`를 사용하였기 때문에 return 하는 값은 없다.

때문에 위 예제처럼 연산 값을 확인하는 용도로 사용할 수 있다.

## Stream 결과 만들기
### Calculating
스트림 내에 있는 요소의 최소, 최대, 합, 평균 등의 연산을 할 수 있다.

```java
long count = IntStream.of(1, 3, 5, 7, 9).count();
long sum = LongStream.of(1, 3, 5, 7, 9).sum();
```

만약 스트림이 비어 있는 경우, `count()`와 `sum()`은 0을 출력하면 되지만, 평균, 최소, 최대값의 경우 표현할 수 없기 때문에 `Optional`을 사용해 리턴한다.

```java
OptionalInt min = IntStream.of(1, 3, 5, 7, 9).min();
OptionalInt max = IntStream.of(1, 3, 5, 7, 9).max();
```

`ifPresent()`를 사용하여 바로 `Optional` 처리를 할 수 있다.

```java
DoubleStream.of(1.1, 2.2, 3.3, 4.4, 5.5)
    .average()
    .ifPresent(System.out::println);
```

### Reduction
```java
// 1개 (accumulator)
Optional<T> reduce(BinaryOperator<T> accumulator);

// 2개 (identity)
T reduce(T identity, BinaryOperator<T> accumulator);

// 3개 (combiner)
<U> U reduce(U identity,
    BiFunction<U, ? super T, U> accumulator,
    BinaryOperator<U> combiner);
```
- accumulator: 각 요소를 처리하여 계산한다. 각 요소가 올 때마다 중간 결과를 생성한다.
- identity: 계산을 위한 초기값으로 스트림이 비어 계산할 내용이 없어도 해당 값을 리턴한다.
- combiner: 병렬 스트림에서 나눠 계산한 결과를 하나로 합친다.

<br>

#### 📌 인자가 1개인 경우
```java
Optional<T> reduce(BinaryOperator<T> accumulator);
```

인자가 1개인 경우, 스트림 내의 요소들을 어떻게 계산할지에 대한 인자값을 받는다.

```java
OptionalInt reduced = IntStream.range(1, 4) // [1, 2, 3]
    .reduce((a, b) -> {
        return Integer.sum(a, b);
    });
```

위 예제의 결과는 `1 + 2 + 3`인 6이 된다.

#### 📌 인자가 2개인 경우
```java
T reduce(T identity, BinaryOperator<T> accumulator);
```

인자가 2개인 경우, 첫 번째 인자로 초기값을 입력 받고 두 번째 인자로 스트림 내의 값을 어떻게 계산할지에 대한 인자를 받는다.

```java
int reducedTwoParams = IntStream.range(1, 4) // [1, 2, 3]
    .reduce(10, Integer::sum);
```

위 예제는 초기값이 `10`이기 때문에 `10 + 1 + 2 + 3`으로 16이 된다.

### Collecting
#### Collectors.toList()
`Collectors.toList()`는 스트림에서 작업한 결과를 `List`로 리턴한다.

```java
List<String> collectorCollection = productList.stream()
            .map(Product::getName)
            .collect(Collectors.toList());
// [potatoes, orange, lemon, bread, sugar]
```

#### Collectors.joining()
`Collectors.joining()`은 스트림에서 작업한 결과를 하나의 문자열로 만들어 리턴한다.

```java
String listToString = productList.stream()
            .map(Product::getName)
            .collect(Collectors.joining());
// potatoesorangelemonbreadsugar
```

<br>

`Collectors.joining()`은 다음과 같은 인자를 받을 수 있다.
- delimeter: 요소를 구분시켜주는 구분자
- prefix: 결과 맨 앞에 붙는 문자
- suffix: 결과 맨 뒤에 붙는 문자

```java
String listToString = productList.stream()
            .map(Product::getName)
            .collect(Collectors.joining(", ", "<", ">"));
// <potatoes, orange, lemon, bread, sugar>
```

#### Collectors.averageInt()
`Collectors.averageInt()`는 숫자 값의 평균 값을 계산한다.

```java
Double averageAmount = productList.stream()
    .collect(Collectors.averagingInt(Product::getAmount));
```

#### Collectors.summingInt()
`Collectors.summingInt()`는 숫자 값의 합을 계산한다.

```java
Integer summingAmount = productList.stream()
    .collect(Collectors.summingInt(Product::getAmount));    // 86
```

#### Collectors.summarizingInt()
`Collectors.summarizingInt()`은 숫자 값의 편균과 합 등의 정보를 한 번에 생성할 수 있다.

```java
IntSummaryStatistics statistics = productList.stream()
    .collect(Collectors.summarizingInt(Product::getAmount));
```

`IntSummartStaticstics` 객체는 개수, 합계, 평균, 최소, 최대에 대한 정보를 가지고 있으며, getter를 사용하여 해당 정보에 접근할 수 있다.

#### Collectors.groupingBy()
`Collectors.groupingBy()`는 특정 조건으로 스트림 내의 요소를 그룹화할 수 있다.

```java
Map<Integer, List<Product>> collectorMapOfLists = productList.stream()
    .collect(Collectors.groupingBy(Product::getAmount));
```

```
{23=[Product{amount=23, name='potatoes'}, 
     Product{amount=23, name='bread'}], 
 13=[Product{amount=13, name='lemon'}, 
     Product{amount=13, name='sugar'}], 
 14=[Product{amount=14, name='orange'}]}
```

#### Collectors.partitioningBy()
`Collectors.partitioningBy()`는 `groupingBy()`와 유사하게 특정 조건을 만족하는 요소들을 `true`와 `false`로 묶어서 반환한다.

```java
Map<Boolean, List<Product>> mapPartitioned = productList.stream()
    .collect(Collectors.partitioningBy(el -> el.getAmount() > 15));
```

```
{false=[Product{amount=14, name='orange'}, 
        Product{amount=13, name='lemon'}, 
        Product{amount=13, name='sugar'}], 
 true=[Product{amount=23, name='potatoes'}, 
       Product{amount=23, name='bread'}]}
```

#### Collectors.collectingAndThen()
```java
public static<T,A,R,RR> Collector<T,A,RR> collectingAndThen(
    Collector<T,A,R> downstream,
    Function<R,RR> finisher) { ... }
```

`Collectors.collectingAndThen()`은 `collect`를 한 후 추가적으로 작업이 필요한 경우 사용한다.

### Matching
`Matching`은 스트림 내에 해당 조건을 만족하는 요소가 있는지 확인할 때 사용한다.

```java
boolean anyMatch(Predicate<? super T> predicate);
boolean allMatch(Predicate<? super T> predicate);
boolean noneMatch(Predicate<? super T> predicate);
```

- anyMatch: 하나라도 조건을 만족하는 요소가 있는지
- allMatch: 모두 조건을 만족하는지
- noneMatch: 모두 조건을 만족하지 않는지

```java
List<String> names = Arrays.asList("Eric", "Elena", "Java");

boolean anyMatch = names.stream()
    .anyMatch(name -> name.contains("a"));
boolean allMatch = names.stream()
    .allMatch(name -> name.length() > 3);
boolean noneMatch = names.stream()
    .noneMatch(name -> name.endsWith("s"));
```

### Iterating
`forEach`는 요소를 돌면서 실행되는 최종 작업이다.

```java
names.stream().forEach(System.out::println);
```

---
## references
- [https://futurecreator.github.io/2018/08/26/java-8-streams/](https://futurecreator.github.io/2018/08/26/java-8-streams/)
- [https://hbase.tistory.com/171](https://hbase.tistory.com/171)