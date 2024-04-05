# PriorityQueue 우선순위 큐
`PriorityQueue`는 일반적인 `Queue`와 다르게 우선순위를 먼저 결정하고, 우선순위가 높은 데이터가 먼저 출력된다.

## PriorityQueue의 특징
- 높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조.
- 내부 요소는 힙(Heap)으로 구성되어 이진트리 구조로 이루어짐.
- 내부 구조가 힙으로 구성되어 있기 떄문에 시간 복잡도는 O(NLogN)

> 📌 [Heap]()

## PriorityQueue의 선언
### import
```java
import java.util.PriorityQueue;
```

### 선언
#### 📌오름차순 정렬
매개변수에 아무 것도 넣지 않고 `PriorityQueue`를 생성하면 오름차순으로 정렬된다.
```java
PriorityQueue<Integer> priorityQueue1 = new PriorityQueue<>();
PriorityQueue<String> priorityQueue2 = new PriorityQueue<>();
```

#### 📌내림차순 정렬
`Collecions.reverseOrder()`을 통해 내림차순으로 정렬되도록 할 수 있다.
```java
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());
```

#### 📌클래스로 구성된 PriorityQueue
```java
PriorityQueue<Car> priorityQueue = new PriorityQueue<>();
```
클래스로 구성된 `PriorityQueue`의 경우 `poll()`을 할 때 `Comparable`가 구현되지 않았다는 오류가 발생한다.

### PriorityQueue의 우선순위 커스텀
#### 📌Comparable
`Comparable` 인터페이스를 상속 받아 `compareTo`를 구현하는 방법이다.
```java
class Car implements Comparable<Car> {
    String name;
    int price;

    public Car(String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public int compareTo(Car c) {
        return this.price - c.price;
    }
}
```
```java
PriorityQueue<Car> carQueue = new PriorityQueue<>();
carQueue.offer(new Car("car1", 1000));
carQueue.offer(new Car("car2", 2000));
carQueue.offer(new Car("car3", 3000));
```
위 예시에서 `price`를 기준으로 `compareTo` 메서드를 구현했기 때문에, `price`를 기준으로 정렬된다.

즉, `car1(price=1000)`, `car2(price=2000)`, `car3(price=3000)` 순으로 우선순위를 두게 된다.

#### 📌Comparator
`PriorityQueue` 생성 시에 매개변수로 `Comparator` 클래스의 `compare` 메서드를 구현하는 방법이다.

```java
class Car implements Comparable<Car> {
    String name;
    int price;

    public Car(String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public int compareTo(Car c) {
        return this.price - c.price;
    }
}
```
```java
PriorityQueue<Car> carQueue = new PriorityQueue<>(new Comparator<Car>() {
    @Override
    public int comapre(Car c1, Car c2) {
        return c1.price - c2.price;
    }
});
```

위 코드는 `java8` 람다식을 이용하여 다음과 같이 표현할 수 있다.

```java
PriorityQueue<Car> carQueue = new PriorityQueue<>((c1, c2) -> m1.price - m2.price);
```

## PriorityQueue의 사용
### PriorityQueue 값 추가
`add(value)`나 `offer(value)`를 사용하여 `PriorityQueue`에 값을 추가한다.
```java
priorityQueue1.add(3);
priorityQueue1.add(5);
priorityQueue1.offer(7);
```

> 📌 [Queue add vs offer](https://github.com/sieunp06/TIL/blob/main/Java/Difference-between-add-and-offer-in-queue.md)

### PriorityQueue 값 삭제
#### 📌poll()과 remove()
`poll()`과 `remove()`는 queue의 맨 앞에 있는 값을 반환하고 제거한다.

만약 queue가 비어있다면 `null`을 반환한다.

#### 📌remove(object o)
`remove(object o)`는 `remove()`와 달리 queue 내의 값 중 파라미터로 주어진 값과 동일하면 제거한다.

제거에 성공했을 때 `true`를 반환하고, queue 내에 파라미터와 동일한 값이 없어 제거하지 못했다면 `false`를 반환한다.

#### 📌clear()
`clear()`는 queue를 초기화한다.

### PriorityQueue에서 우선순위가 가장 높은 값 반환
`PriorityQueue`에서 우선순위가 가장 높은 값을 반환할 때는 `peek()`을 사용한다.

`peek()`은 우선순위가 가장 높은 값을 삭제하지 않고 반환한다.

❗이때 `PriorityQueue`가 비어있다면 `null`을 반환한다.
```java
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();
priorityQueue.offer(2);
priorityQueue.offer(1);
priorityQueue.offer(3);

priorityQueue.peek();  // 1
```

### PriorityQueue에 특정 값이 존재하는지 확인
특정 값이 `PriorityQueue`에 존재하는지 확인하기 위해서 `contains(object o)`를 사용한다.
```java
priorityQueue1.contains(3);
priorityQueue1.contains(8);
```
queue 안에 특정 값이 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.

-----
## 💎 References
- [Java API Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/PriorityQueue.html)
- [https://coding-factory.tistory.com/603](https://coding-factory.tistory.com/603)
- [https://innovation123.tistory.com/112](https://innovation123.tistory.com/112)