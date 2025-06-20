연결리스트는 데이터를 저장하는 자료구조로, 각 데이터 요소(노드)가 포인터(링크)를 통해 서로 연결되어 있는 구조다. 각 노드는 실제 데이터와 다음 노드의 주소(포인터)를 함께 저장한다. 이 구조 덕분에 데이터가 메모리상 연속적으로 배치될 필요가 없고, 삽입·삭제 연산이 배열보다 효율적이다. 하지만 임의의 위치에 접근할 때는 처음부터 순차적으로 탐색해야 하므로 접근 속도는 느릴 수 있다.

**노드(Node)의 구조**
- 데이터 필드: 실제 저장할 데이터
- 링크 필드: 다음(혹은 이전) 노드의 주소를 저장하는 포인터

**연결리스트의 종류**

- 단일 연결 리스트(singly linked list)
  
  ![](https://velog.velcdn.com/images/ekdeon/post/9be69c4e-5b47-4e8a-af43-4e4fccb6cb6e/image.png)
  - 각 노드는 데이터와 다음 노드를 가리키는 포인터(next)만을 가짐.
  - 리스트의 첫 번째 노드는 head, 마지막 노드는 next가 null임.
  - 한 방향(앞→뒤)으로만 탐색 가능하며, 역방향 탐색은 불가능함.
  - 중간 삽입·삭제가 배열보다 효율적이며, 동적 메모리 할당으로 크기 조절이 쉬움.
  - 임의의 노드 접근 시 head부터 순차적으로 탐색해야 하므로 접근 속도는 느림.
  - 구조가 단순하고 구현이 쉬우며, 메모리 사용이 상대적으로 적음.

- 이중 연결 리스트(doubly linked list)
  
  ![](https://velog.velcdn.com/images/ekdeon/post/6407cfb1-b1cf-4947-accf-eed197ac9f8e/image.png)
  - 각 노드는 데이터, 이전 노드(prev), 다음 노드(next)를 모두 가짐.
  - 양방향(앞↔뒤) 탐색이 가능해 삽입·삭제, 탐색이 더 유연함.
  - head는 prev가 null, tail은 next가 null임.
  - 임의의 위치에 더 빠르게 접근할 수 있으며, head 또는 tail에서 시작하여 효율적으로 탐색 가능.
  - 노드마다 포인터가 2개이므로 메모리 사용이 늘고, 구현이 단순 연결 리스트보다 복잡함.

- 원형 이중 연결 리스트(doubly circular linked list)
  ![](https://velog.velcdn.com/images/ekdeon/post/02ab51fc-ab71-4ddd-b66e-1fa5299c279d/image.png)
  - 이중 연결 리스트의 변형으로, 마지막 노드의 next가 head를, head의 prev가 tail을 가리킴.
  - 리스트의 처음과 끝이 연결되어 원형 구조를 이룸.
  - 어느 노드에서든 양방향 순회가 가능하며, 시작점에 제한이 없음.
  - head와 tail의 구분이 모호해질 수 있어, 더미 노드(dummy node)를 도입해 관리하는 경우가 많음.

이미지 출처: [https://inpa.tistory.com](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

---

>
아래부터는 자바에 실제로 구현된 코드를 바탕으로 설명한 내용입니다. 
IntelliJ에서 LinkedList 심볼 위에 커서를 두고 Go to Declaration(Ctrl+클릭) 기능을 사용하면 LinkedList의 내부 구현, 메서드, 필드, 그리고 Javadoc 주석 등 원본 코드를 직접 확인할 수 있습니다.

![](https://velog.velcdn.com/images/ekdeon/post/de7a623e-ba88-42bb-91cc-eac2399b335d/image.png)

Java의 LinkedList 클래스는 이중 연결 리스트(Doubly-linked list)로 구현된 자료구조로, List와 Deque 인터페이스를 모두 구현하고 있다. 이 구현은 순차적 데이터 접근과 양방향 탐색이 가능한 특징을 가지고 있으며, Collections Framework의 핵심 구성 요소 중 하나다.

---

## 기본 특성 및 구조

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     */
    transient Node<E> first;

    /**
     * Pointer to last node.
     */
    transient Node<E> last;
    
    ...
}
```

LinkedList는 AbstractSequentialList를 상속하며, List, Deque, Cloneable, Serializable 인터페이스를 구현한다.

- `transient int size = 0`: 리스트의 요소 개수
- `transient Node first`: 첫 번째 노드에 대한 참조
- `transient Node last`: 마지막 노드에 대한 참조

내부적으로 `Node` 클래스를 통해 이중 연결 구조를 구현하며, 각 노드는 이전/다음 노드 참조와 데이터 항목을 포함한다.

---

## 주요 특징

1. **이중 연결 구조**: 각 노드가 이전 노드와 다음 노드에 대한 참조를 모두 가지고 있어 양방향 탐색이 가능하다.

2. **인덱스 최적화 접근**: 인덱스 기반 작업 시 리스트 크기의 절반을 기준으로 처음 또는 끝에서부터 가까운 쪽에서 탐색을 시작하여 성능을 개선한다.

3. **비동기화**: 기본적으로 동기화되지 않은 구현이므로, 멀티스레드 환경에서 동시 접근 시 외부 동기화가 필요하다.
   ```java
   List list = Collections.synchronizedList(new LinkedList(...));
   ```

4. **Fail-Fast 반복자**: 반복자 생성 이후 구조적 변경이 발생하면 ConcurrentModificationException을 발생시키는 안전한 메커니즘을 제공한다.

---

## 주요 메서드

LinkedList는 다양한 메서드를 제공하며, 크게 다음과 같이 분류할 수 있다. 좀 더 자세한 내용은 [JDK 17 Documentation](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/LinkedList.html)에서 확인할 수 있다.

### 생성자
- `LinkedList()`: 빈 리스트 생성
- `LinkedList(Collection c)`: 주어진 컬렉션으로 리스트 생성

### 리스트 조작 메서드
- **요소 접근**: `getFirst()`, `getLast()`, `get(int index)`
- **요소 추가**: `addFirst(E e)`, `addLast(E e)`, `add(E e)`, `add(int index, E element)`
- **요소 제거**: `removeFirst()`, `removeLast()`, `remove(Object o)`, `remove(int index)`
- **검색**: `indexOf(Object o)`, `lastIndexOf(Object o)`, `contains(Object o)`
- **리스트 정보**: `size()`, `isEmpty()`

### Queue/Deque 인터페이스 메서드
- **추가**: `offer(E e)`, `offerFirst(E e)`, `offerLast(E e)`
- **검색**: `peek()`, `peekFirst()`, `peekLast()`, `element()`
- **제거**: `poll()`, `pollFirst()`, `pollLast()`, `remove()` 
- **스택 연산**: `push(E e)`, `pop()`

### 반복자 메서드
- `iterator()`: 정방향 이터레이터 반환
- `listIterator(int index)`: 지정된 인덱스에서 시작하는 리스트 이터레이터 반환
- `descendingIterator()`: 역방향 이터레이터 반환

### 기타 유틸리티 메서드
- `toArray()`: 리스트 요소를 배열로 반환
- `clone()`: 얕은 복사본 생성
- `spliterator()`: 병렬 처리를 위한 Spliterator 제공
- `reversed()`: Java 21에서 추가된 역순 뷰 제공 메서드

---

## 내부 구현 상세

LinkedList의 내부 동작은 노드 삽입/삭제 연산을 통해 이루어진다.

- `linkFirst(E e)`: 리스트 앞에 요소 추가
- `linkLast(E e)`: 리스트 끝에 요소 추가
- `linkBefore(E e, Node succ)`: 특정 노드 앞에 요소 추가
- `unlinkFirst(Node f)`: 첫 번째 노드 제거
- `unlinkLast(Node l)`: 마지막 노드 제거
- `unlink(Node x)`: 지정된 노드 제거

---

## 내부 클래스

LinkedList는 다음과 같은 내부 클래스를 포함한다.

- `Node`: 이중 연결 리스트의 기본 노드 구현
- `ListItr`: 리스트 이터레이터 구현
- `DescendingIterator`: 역순 반복자 구현
- `LLSpliterator`: 병렬 처리를 위한 Spliterator 구현
- `ReverseOrderLinkedListView`: Java 21에서 추가된 역순 뷰 구현

---
## LinkedList 활용 예제

### 기본 사용법
```java
// LinkedList 생성
LinkedList<String> list = new LinkedList<>();

// 요소 추가
list.add("아메리카노");        // 맨 뒤에 추가
list.addFirst("에스프레소");   // 맨 앞에 추가
list.addLast("라떼");         // 맨 뒤에 추가
list.add(1, "카푸치노");      // 인덱스 1 위치에 추가

// 요소 접근
System.out.println("첫 번째 요소: " + list.getFirst());
System.out.println("마지막 요소: " + list.getLast());
System.out.println("인덱스 2의 요소: " + list.get(2));

// 요소 삭제
list.remove("카푸치노");      // 객체로 삭제
list.removeFirst();          // 첫 번째 요소 삭제
list.removeLast();           // 마지막 요소 삭제

// 리스트 순회
for (String coffee : list) {
    System.out.println(coffee);
}
```

### 스택과 큐로 활용
LinkedList는 Deque 인터페이스를 구현하므로 스택이나 큐로 활용할 수 있다.
```java
// 스택으로 활용
LinkedList<String> stack = new LinkedList<>();
stack.push("A");
stack.push("B");
stack.push("C");
System.out.println(stack.pop()); // C 출력

// 큐로 활용
LinkedList<String> queue = new LinkedList<>();
queue.offer("X");
queue.offer("Y");
queue.offer("Z");
System.out.println(queue.poll()); // X 출력
```
