# Deque 덱
일반적인 `Queue`와 `Stack`은 한 쪽 방향으로만 데이터를 빼고 넣을 수 있다.

하지만 `Deque`은 `Queue`의 특징을 가지고 있으나, 양쪽 방향으로 데이터를 빼고 넣을 수 있는 자료구조이다.

> 양방향을 데이터를 넣고 뺄 수 있기 때문에 스택으로도 사용 가능하다. <br>
> 때문에 `stack`과 `pop`을 사용할 수 있다.

## Deque의 선언
```java
Deque<String> deque1 = new ArrayDeque<>();
Deque<String> deque2 = new LinkedBlockingDeque<>();
Deque<String> deque3 = new ConcurrentLinkedDeque<>();
Deque<String> linkedList = new LinkedList<>();
```

## Deque의 사용
### Deque의 값 추가
- addFirst()
- offerFirst()
- add()
- addLast()
- offerLast()

#### addFirst() / offerFirst()
`Deque`의 앞 쪽에 엘리먼트를 삽입한다.

#### add() / addLast() / offerLast()
`Deque`의 뒤 쪽에 엘리먼트를 삽입한다.

### Deque의 값 삭제
- removeFirst()
- pollFirst()
- remove()
- poll()
- removeLast()
- pollLast()
- removeFirstOccurrence(Object o)
- removeLastOccurrence(Object o)

#### removeFirst() / pollFirst() / remove() / poll()
`Deque`의 앞 쪽에 있는 엘리먼트를 삭제하거나 해당 값을 반환한다.

#### removeLast() / pollLast()
`Deque`의 뒤 쪽에 있는 엘리먼트를 삭제하거나 해당 값을 반환한다.

#### removeFirstOccurrence(Object o) / removeLastOccurrence(Object o)
`removeFirstOccurrence(Object o)`는 `Deque`의 앞 쪽에서 부터 탐색하여 입력 받은 `Object o`와 동일한 첫 엘리먼트를 제거한다.

반대로 `removeLastOccurrence(Object o)`는 뒤 쪽에서부터 탐색한다.

이때, 같은 엘리먼트가 없다면 `Deque`에 변경이 일어나지 않는다.

### Deque의 값 추출
- getFirst()
- peekFirst()
- peek()
- getLast()
- peekLast()

#### getFirst() / peekFirst() / peek()
`Deque`의 앞 쪽에 있는 엘리먼트를 삭제하지 않고 반환한다.

#### getLast() / peekLast()
`Deque`의 뒤 쪽에 있는 엘리먼트를 삭제하지 않고 반환한다.

#### get과 peek의 차이
`get`은 `Deque`이 비어있으면 예외가 발생하지만, `peek`은 덱이 비어있으면 null이 반환된다.

### Deque의 순회
#### for을 이용한 순회
```java
for (String elem : deque1) {
    System.out.println(elem);
}
```

#### Iterator를 사용한 순회
```java
// Iterator를 이용한 순회
Iterator<String> iterator = deque1.iterator();
while (iterator.hasNext()) {
    String elem = iterator.next();
    System.out.println(elem);
}

// 역순순회 
Iterator<String> reverseIterator = deque1.descendingIterator();
while (reverseIterator.hasNext()) {
    String elem = reverseIterator.next();
    System.out.println(elem);
}
```

----
## reference
- [https://velog.io/@chosj1526/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Deque-%EB%8D%B1-JAVA](https://velog.io/@chosj1526/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Deque-%EB%8D%B1-JAVA)