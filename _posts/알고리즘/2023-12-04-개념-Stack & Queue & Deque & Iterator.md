---
title : Stack & Queue & Deque & Iterator
date : 2023-12-04 +09:00
categories : [알고리즘, 개념]
tags : [
  개념,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/1-1.png){:style="width:1000px" } -->

### 1. Stack

```java
import java.util.stack;

Stack<제너릭> stack = new Stack<>();
// 특징
/* 1. LIFO(Last In First Out)
  2. 시스템 해킹 시 버퍼플로우 취약점을 이용한 공격을 할 때 스택 메모리의 영역에서 한다
  3. 인터럽트 처리, 수식의 계산, 서브루틴의 복귀 번지 저장 등에 쓰임
  4. 그래프의 깊이 우선 탐색에 사용
  5. 재귀적 함수 호출에 사용
*/

stack.push(값); // 값 추가하고 해당 값을 반환
stack.pop(); // 맨 위의 값 제거하고 해당 값 반환
stack.clear(); // 값 모두 제거, 반환 없음
stack.peek(); // 가장 상단의 값 출력
stack.size(); // 크기 출력
stack.isEmpty(); // stack이 비어있는지 확인(비어있다면 true)
stack.contains(값); // stack에 값이 있는지 확인(있다면 true)
stack.search(값); // 값을 검색하여 위치를 반환, 해당 값이 여러개일 경우 마지막 위치 반환
                  // 값이 없을 경우 -1 반환
                  // 위치의 값은 끝에서부터 1번. ex) 1, 2, 3을 넣었으면
                  // ex)1, 2, 3 입력 search(3) -> 1, search(2) -> 2, search(1) -> 3
                  // ex)1, 1, 3 입력 search(3) -> 1, search(2) -> -1, search(1) -> 2
```

```java
인터럽트(Interrupt) : 스레드 실행 중에 강제로 멈추게 하는 기능이 없기 때문에 인터럽트를 쓰게 되는데 특정 스레드에게 작업을 멈춰 달라고 요청하는 형태이다.

interrupt() : 해당하는 스레드에 인터럽트를 거는 역할을 함.
isInturrupted() : 해당 스레드에 인터럽트가 걸려있는지 확인해주는 역할을 함.
static interrupted() : 현재 스레드의 인터럽트 상태를 해제하고, 해제하기 이전의 값이 무엇이었는지를 알려줌. 인터럽트 상태를 해제할 수 있는 유일한 방법.
```

### 2. Queue

```java
import java.util.Queue;
import java.util.LinkedList;

Queue<제너릭> que = new LinkedList<>();
// 특징
/* 1. FIFO(First In FIrst Out) 구조
  2. 큐는 한 쪽 끝은 프런트(front)로 정하여 삭제 연사만 수행
  3. 다른 한 쪽은 리어(rear)로 정하여 삽입 연산만 수행
  4. 그래프의 넓이 우서 탐색(BFS)에서 사용
  5. 컴퓨터 버퍼에서 주로 사용, 마구 입력이 되었으나 처리를 하지 못할 때, 버퍼(큐)를 만
      들어 대기 시킴
*/

que.add(값); == que.offer(값);
// 값 추가하고, 성공 시 true 반환. 큐에 여유 공간이 없어 삽입에 실패하면 IllegalStateException을 발생
que.poll(); == que.remove();
// 값 제거하고, 해당 값을 반환.
// poll()의 경우 비어있으면 null을 반환하고, 
// remove()의 경우 비어있으면 NoSuchElementException을 발생
que.clear();
// 전체 값을 제거
que.peek();
// 첫번째로 저장된 값을 제거하지 않고 출력
Iterator iter = que.iterator();
// 모든 값을 출력
que.size();
// 크기 출력
que.isEmpty();
// que가 비어있는지 확인(비었으면 true)
```

### 3. Deque

```java
// 덱(Deque)
/* 어떤 쪽으로 입력하고 어떤 쪽으로 출력하느냐에 따라 Stack과 Queue로 사용할 수 있다
  한 쪽으로만 입력 가능하도록 설정하면 스크롤(scroll)
  한 쪽으로만 출력 가능하도록 설정하면 셸프(shelf)
  ArrayDeque, LinkedBlockingDeque, ConcurrentLinkedDeque, LinkedList 등의 클래스를 
  사용
*/

Deque<제너릭> deq = new ArrayDeque<>();

addFirst(); // 덱의 앞쪽에 E를 삽입.
            // 제한이 있는 덱의 경우, 용량을 초과하면 예외 발생. 반환값 없음
offerFirst(); // 덱의 앞쪽에 E를 삽입. 
              // 삽입 성공하면 true 리턴, 용량 제한에 걸려 실패한 경우 false 리턴

addLast(); // 덱의 마지막에 E를 삽입. 
          // 용량 제한이 있는 덱의 경우, 용량을 초과하면 예외발생. 반환값 없음
add(); // addLast()와 동일
offerLast(); // 덱의 마지막에 E를 삽입. 
            // 삽입 성공하면 true 리턴, 용량 제한에 걸려 실패한 경우 false 리턴
offer(); // offerLast()와 동일

removeFirst(); // 덱의 앞쪽 E를 제거하고, 그 값을 반환. 덱이 비어있으면 예외 발생
remove(); // removeFirst()와 동일
pollFirst(); // 덱의 앞쪽 E를 제거하고, 그 값을 반환. 덱이 비어있으면 null 리턴
poll(); // pollFirst()와 동일

removeLast(); // 덱의 마지막 E를 제거하고, 그 값을 반환. 덱이 비어있으면 예외 발생
pollLast(); // 덱의 마지막 E를 제거하고, 그 값을 반환. 덱이 비어있으면 null 리턴

getFirst(); // 덱의 앞쪽 E를 제거하지 않고 값을 리턴. 덱이 비어있으면 예외 발생
element(); // getFirst()와 동일
peekFirst(); // 덱의 앞쪽 E를 제거하지 않고 값을 리턴. 덱이 비어있으면 null 리턴
peek(); // peekFirst()와 동일

getLast(); // 덱의 마지막 E를 제거하지 않고 값을 리턴. 덱이 비어있으면 예외 발생
peekLast(); // 덱의 마지막 E를 제거하지 않고 값을 리턴. 덱이 비어있으면 null 리턴

removeFirstOccurrence(Object o); // 덱의 앞쪽부터 탐색하여 o와 동일한 첫 E를 제거.
                                // o와 같은 E가 없으면 덱에 변경X, boolean 반환
removeLastOccurrence(Object o); // 덱의 뒤쪽부터 탐색하여 o와 동일한 첫 E를 제거.
                                // o와 같은 E가 없으면 덱에 변경X, boolean 반환

addAll(Collection <? extends E c); // 입력 받은 Collection의 모든 데이터를 덱의 뒤쪽에 
                                  // 삽입. boolean 반환

push(); // addFirst()와 동일
pop(); // removeFirst()와 동일
remove(Object o); // removeFirstOccurrence(Object o);
contain(Object o); // 덱에 o가 포함되어 있는지 확인. boolean 반환
size(); // 덱의 크기 반환
iterator(); // 덱의 앞쪽부터 순차적으로 실행되는 이터레이터를 얻어옴
desendingIterator(); // 덱의 뒤쪽부터 순차적으로 실행되는 이터레이터
isEmpty() // 비어있는지 확인. boolean 반환
clear() // 모두 삭제
```

```java
예외발생
addFirst()    ->   <- addLast()
removeFirst() <-   -> removeLast()  // 제거
getFirst()    <-   -> getLast()     // 제거X

boolean or null 반환
offerFirst()  ->  <- offerLast()  // boolean
pollFirst()   <-  -> pollLast()   // 제거
peekFirst()   <-  -> peekLast()   // 제거X
```

### Iterator

```java
// 컬렉션에서 값을 순차적으로 얻어옴

Iterator<제너릭> iterator = 컬렉션.iterator();

iterator.hasNext(); // 다음 값의 유무 확인. boolean 반환
iterator.next(); // 다음 값 가져오기
iterator.remove(); // iterator.next()에서 가져온 값을 컬렉션에서 삭제
```

### PriorityQueue
```java
Queue<> q = new PriorityQueue<>();
```

- Queue의 원리에서 선입선출이 아닌 우선순위를 두어 우선순위대로 출력하는 것
- 기본은 우선순위가 낮은 숫자가 부여, 높은 숫자를 우선순위로 두려면 Collections.reverseOrder() 메서드를 활용
- 저장공간으로 배열을 사용하며, 각 요소를 힙(heap)이라는 자료구조의 형태로 저장한다.
- heap은 이진트리의 한 종류이다.
  - 낮은 숫자를 우선순위로 정렬하면
  - 데이터 삽입에서는 일단 마지막에 넣고, 부모와 비교하여 더 작으면 교환, 아니면 유지한다.
  - 데이터 제거에서는 우선 순위가 가장 높은 값이 제거되기 위해서, 루트(제일상단노드)와 마지막 노드를 교환 후 마지막 노드를 삭제하여 사이즈를 줄인다.
  - 즉 우선순위가 제일 높은 노드가 삭제된다.
  - 루트의 자식노드들을 비교, 더 작은 자식노드와 루트 노드를 교환, 자식이 된 노드와 그 밑의 자식 노드의 크기를 비교하여 교환 또는 유지
  - 위의 과정을 반복하여, 우선순위가 높은 값을 제일 상단(루트)로 보내고 제거 시 마지막 노드와 교환 후 제거하여 사이즈를 줄이고, 루트가 된 노드와 그 자식노드를 비교 교환, 자식노드가 된 상태에서 그 자식노드의 자식노드들과 비교하여 교환 이런식으로 로직이 반복된다.
