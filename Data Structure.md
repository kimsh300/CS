# Data Structure (자료구조)

> 효율적인 데이터 관리를 위한 핵심 자료구조 정리

## 📋 목차

- [Array](#array)
- [LinkedList](#linkedlist)
- [HashTable](#hashtable)
- [Stack](#stack)
- [Queue](#queue)
- [Graph](#graph)
- [Tree](#tree)
- [그래프와 트리의 차이점](#그래프와-트리의-차이점)
- [Binary Heap](#binary-heap)
- [Red-Black Tree](#red-black-tree)
- [B+ Tree](#b-tree)

---

## Array

### 개념
배열은 데이터를 연속된 메모리 공간에 저장하는 가장 기본적인 선형 자료구조입니다. 동일한 타입의 요소들이 인덱스를 통해 접근 가능하며, 각 요소는 고유한 인덱스 번호를 가집니다.

### 특징
- **고정 크기**: 생성 시 크기가 결정되며 변경 불가능
- **연속 메모리 할당**: 요소들이 메모리에 연속적으로 배치
- **빠른 접근**: 인덱스를 통한 O(1) 시간 복잡도의 임의 접근
- **캐시 친화적**: 메모리 지역성으로 인한 캐시 효율성

### 장단점

**장점**
- 인덱스를 통한 빠른 접근 (O(1))
- 구현이 간단하고 메모리 효율적
- 순차 접근 시 캐시 성능 우수

**단점**
- 크기가 고정되어 있어 유연성 부족
- 삽입/삭제 시 요소 이동 필요 (O(n))
- 메모리 낭비 가능성 (사용하지 않는 공간)

### 시간 복잡도

| 연산 | 시간 복잡도 |
|------|------------|
| 접근 | O(1) |
| 검색 | O(n) |
| 삽입 | O(n) |
| 삭제 | O(n) |

### 구현 예제

```java
public class ArrayExample {
    public static void main(String[] args) {
        // 배열 선언 및 초기화
        int[] array = new int[5];
        array[0] = 10;
        array[1] = 20;
        array[2] = 30;
        
        // 초기화와 함께 선언
        int[] numbers = {1, 2, 3, 4, 5};
        
        // 배열 순회
        for (int i = 0; i < numbers.length; i++) {
            System.out.println(numbers[i]);
        }
        
        // Enhanced for loop
        for (int num : numbers) {
            System.out.println(num);
        }
    }
}

// 동적 배열 구현 (ArrayList와 유사)
class DynamicArray {
    private int[] arr;
    private int size;
    private int capacity;
    
    public DynamicArray() {
        capacity = 10;
        arr = new int[capacity];
        size = 0;
    }
    
    public void add(int element) {
        if (size == capacity) {
            resize();
        }
        arr[size++] = element;
    }
    
    public int get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        return arr[index];
    }
    
    public void remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        for (int i = index; i < size - 1; i++) {
            arr[i] = arr[i + 1];
        }
        size--;
    }
    
    private void resize() {
        capacity *= 2;
        int[] newArr = new int[capacity];
        System.arraycopy(arr, 0, newArr, 0, size);
        arr = newArr;
    }
    
    public int size() {
        return size;
    }
}
```

### 활용 사례
- 고정된 크기의 데이터 저장
- 빠른 인덱스 접근이 필요한 경우
- 다차원 데이터 표현 (행렬, 이미지)
- 다른 자료구조의 기반 구현

---

## LinkedList

### 개념
연결 리스트는 각 노드가 데이터와 다음 노드를 가리키는 참조로 구성된 선형 자료구조입니다. 메모리 크기를 실행 시간에 동적으로 할당하거나 해제할 수 있습니다.

### 구조
각 노드는 두 부분으로 구성됩니다:
- **Data**: 실제 저장되는 데이터 값
- **Next**: 다음 노드를 가리키는 포인터/참조

```
Head -> [Data|Next] -> [Data|Next] -> [Data|Next] -> NULL
```

### 종류

**1. 단일 연결 리스트 (Singly Linked List)**
- 각 노드가 다음 노드만 가리킴
- 한 방향으로만 순회 가능

**2. 이중 연결 리스트 (Doubly Linked List)**
- 각 노드가 이전과 다음 노드를 모두 가리킴
- 양방향 순회 가능

**3. 원형 연결 리스트 (Circular Linked List)**
- 마지막 노드가 첫 번째 노드를 가리킴
- 순환 구조 형성

### 배열과의 비교

| 특성 | Array | LinkedList |
|------|-------|------------|
| 메모리 할당 | 연속적 | 비연속적 |
| 크기 | 고정 | 동적 |
| 접근 시간 | O(1) | O(n) |
| 삽입/삭제 | O(n) | O(1)* |
| 메모리 오버헤드 | 낮음 | 높음 (포인터) |

*포인터를 알고 있는 경우

### 장단점

**장점**
- 삽입과 삭제가 상수 시간에 가능 (포인터를 알고 있는 경우)
- 동적 크기 조절 가능
- 메모리 재할당 불필요

**단점**
- 임의 접근 불가 (순차 접근만 가능)
- 추가 메모리 필요 (포인터 저장)
- 캐시 지역성 낮음

### 시간 복잡도

| 연산 | 시간 복잡도 |
|------|------------|
| 접근 | O(n) |
| 검색 | O(n) |
| 삽입 (앞) | O(1) |
| 삽입 (끝) | O(n) 또는 O(1)* |
| 삭제 | O(1)** |

*tail 포인터 사용 시  
**삭제할 노드의 위치를 알고 있는 경우

### 구현 예제

```java
// 단일 연결 리스트 구현
class Node {
    int data;
    Node next;
    
    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

class SinglyLinkedList {
    private Node head;
    private int size;
    
    public SinglyLinkedList() {
        this.head = null;
        this.size = 0;
    }
    
    // 앞에 삽입
    public void insertAtBegin(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
        size++;
    }
    
    // 끝에 삽입
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
            size++;
            return;
        }
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
        size++;
    }
    
    // 특정 위치에 삽입
    public void insertAt(int index, int data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException();
        }
        if (index == 0) {
            insertAtBegin(data);
            return;
        }
        Node newNode = new Node(data);
        Node current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current.next;
        }
        newNode.next = current.next;
        current.next = newNode;
        size++;
    }
    
    // 삭제
    public void delete(int data) {
        if (head == null) return;
        
        if (head.data == data) {
            head = head.next;
            size--;
            return;
        }
        
        Node current = head;
        while (current.next != null && current.next.data != data) {
            current = current.next;
        }
        
        if (current.next != null) {
            current.next = current.next.next;
            size--;
        }
    }
    
    // 검색
    public boolean search(int data) {
        Node current = head;
        while (current != null) {
            if (current.data == data) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
    
    // 출력
    public void printList() {
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("NULL");
    }
    
    public int size() {
        return size;
    }
}

// 이중 연결 리스트 구현
class DoublyNode {
    int data;
    DoublyNode prev;
    DoublyNode next;
    
    DoublyNode(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}

class DoublyLinkedList {
    private DoublyNode head;
    private DoublyNode tail;
    private int size;
    
    public DoublyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    
    public void insertAtBegin(int data) {
        DoublyNode newNode = new DoublyNode(data);
        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }
    
    public void insertAtEnd(int data) {
        DoublyNode newNode = new DoublyNode(data);
        if (tail == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
        size++;
    }
    
    public void printForward() {
        DoublyNode current = head;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.next;
        }
        System.out.println("NULL");
    }
    
    public void printBackward() {
        DoublyNode current = tail;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.prev;
        }
        System.out.println("NULL");
    }
}
```

### 활용 사례
- 동적인 메모리 할당이 필요한 경우
- 삽입/삭제가 빈번한 경우
- 스택, 큐, 그래프 등의 구현
- LRU 캐시 구현
- 브라우저의 앞으로/뒤로 가기 기능
- 음악 플레이어의 재생 목록

---

## HashTable

### 개념
해시테이블은 키를 값에 매핑하는 자료구조입니다. Key-Value 쌍을 1:1로 연관지어 저장하며, 해시 함수를 사용하여 키를 배열의 인덱스로 변환합니다.

### 구조

```
Key → Hash Function → Hash Code → Index → Value
```

**구성 요소**
- **Key**: 고유한 식별자
- **Hash Function**: 키를 해시 코드로 변환하는 함수
- **Hash Code**: 해시 함수의 결과값
- **Bucket/Slot**: 실제 값이 저장되는 공간
- **Value**: 저장되는 실제 데이터

### 주요 연산

- **put(key, value)**: 키-값 쌍을 저장
- **get(key)**: 키에 해당하는 값을 반환
- **remove(key)**: 키에 해당하는 항목을 제거
- **containsKey(key)**: 특정 키의 존재 여부 확인

### 해시 함수

좋은 해시 함수의 조건:
1. 계산이 빠를 것
2. 충돌을 최소화할 것
3. 균등하게 분산시킬 것

```java
// 간단한 해시 함수 예제
public int hashFunction(String key, int tableSize) {
    int hash = 0;
    for (int i = 0; i < key.length(); i++) {
        hash = (hash * 31 + key.charAt(i)) % tableSize;
    }
    return Math.abs(hash);
}
```

### 해시 충돌 (Hash Collision)

서로 다른 키가 동일한 해시 값을 가질 때 발생하며, 단일 버킷에 여러 항목이 저장되어 순차 검색이 필요합니다.

**해결 방법**

**1. Separating Chaining (분리 연결법)**
- 연결 리스트를 사용하여 동일한 해시 값을 가진 요소들을 연결
- 충돌 발생 시 해당 인덱스의 연결 리스트에 노드 추가
- Java에서는 데이터 6개 이하는 LinkedList, 8개 이상은 Red-Black Tree 사용

```
Index 0: [Key1, Value1] -> [Key5, Value5]
Index 1: [Key2, Value2]
Index 2: [Key3, Value3] -> [Key6, Value6] -> [Key7, Value7]
```

**2. Open Addressing (개방 주소법)**
- 추가 메모리 사용 없이 빈 버킷을 찾아 저장
- Linear Probing: 순차적으로 다음 빈 슬롯 탐색
- Quadratic Probing: 제곱수만큼 떨어진 슬롯 탐색
- Double Hashing: 두 번째 해시 함수 사용

**3. Resizing**
- Load Factor가 0.75에 도달하면 용량을 증가
- 일반적으로 크기를 2배로 확장하여 성능 유지

### HashTable vs HashMap

| 특성 | HashTable | HashMap |
|------|-----------|---------|
| 동기화 | 동기화 O | 동기화 X |
| Null 허용 | Key/Value 모두 불가 | 허용 |
| 스레드 안전 | Thread-safe | Not thread-safe |
| 성능 | 상대적으로 느림 | 빠름 |
| Legacy | JDK 1.0 | JDK 1.2 |

### 시간 복잡도

| 연산 | 평균 | 최악 |
|------|------|------|
| 삽입 | O(1) | O(n) |
| 삭제 | O(1) | O(n) |
| 검색 | O(1) | O(n) |

### 구현 예제

```java
import java.util.LinkedList;

// Separating Chaining 방식의 HashTable 구현
class HashTable<K, V> {
    private class Entry<K, V> {
        K key;
        V value;
        
        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private LinkedList<Entry<K, V>>[] table;
    private int capacity;
    private int size;
    private static final double LOAD_FACTOR = 0.75;
    
    @SuppressWarnings("unchecked")
    public HashTable(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.table = new LinkedList[capacity];
        for (int i = 0; i < capacity; i++) {
            table[i] = new LinkedList<>();
        }
    }
    
    public HashTable() {
        this(16);
    }
    
    private int hash(K key) {
        return Math.abs(key.hashCode() % capacity);
    }
    
    public void put(K key, V value) {
        if ((double) size / capacity >= LOAD_FACTOR) {
            resize();
        }
        
        int index = hash(key);
        LinkedList<Entry<K, V>> bucket = table[index];
        
        // 키가 이미 존재하면 값 업데이트
        for (Entry<K, V> entry : bucket) {
            if (entry.key.equals(key)) {
                entry.value = value;
                return;
            }
        }
        
        // 새로운 키-값 쌍 추가
        bucket.add(new Entry<>(key, value));
        size++;
    }
    
    public V get(K key) {
        int index = hash(key);
        LinkedList<Entry<K, V>> bucket = table[index];
        
        for (Entry<K, V> entry : bucket) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
        }
        return null;
    }
    
    public void remove(K key) {
        int index = hash(key);
        LinkedList<Entry<K, V>> bucket = table[index];
        
        Entry<K, V> toRemove = null;
        for (Entry<K, V> entry : bucket) {
            if (entry.key.equals(key)) {
                toRemove = entry;
                break;
            }
        }
        
        if (toRemove != null) {
            bucket.remove(toRemove);
            size--;
        }
    }
    
    public boolean containsKey(K key) {
        return get(key) != null;
    }
    
    @SuppressWarnings("unchecked")
    private void resize() {
        LinkedList<Entry<K, V>>[] oldTable = table;
        capacity *= 2;
        table = new LinkedList[capacity];
        size = 0;
        
        for (int i = 0; i < capacity; i++) {
            table[i] = new LinkedList<>();
        }
        
        for (LinkedList<Entry<K, V>> bucket : oldTable) {
            for (Entry<K, V> entry : bucket) {
                put(entry.key, entry.value);
            }
        }
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}

// Java 내장 HashTable 사용 예제
import java.util.Hashtable;
import java.util.HashMap;

public class HashTableExample {
    public static void main(String[] args) {
        // Hashtable 사용
        Hashtable<String, Integer> table = new Hashtable<>();
        table.put("Apple", 100);
        table.put("Banana", 200);
        table.put("Cherry", 300);
        
        System.out.println(table.get("Apple"));  // 100
        System.out.println(table.containsKey("Banana"));  // true
        
        table.remove("Banana");
        
        // HashMap 사용 (더 일반적)
        HashMap<String, Integer> map = new HashMap<>();
        map.put("Dog", 1);
        map.put("Cat", 2);
        map.put("Bird", 3);
        
        // 순회
        for (String key : map.keySet()) {
            System.out.println(key + ": " + map.get(key));
        }
    }
}
```

### 활용 사례
- 데이터베이스 인덱싱
- 캐시 구현 (LRU Cache)
- 중복 제거
- 빠른 검색이 필요한 경우
- 심볼 테이블 (컴파일러)
- 라우팅 테이블 (네트워크)
- 전화번호부, 사전 구현

---

## Stack

### 개념
스택은 LIFO(Last In First Out) 또는 FILO(First In Last Out) 순서를 따르는 선형 자료구조입니다. 한쪽 끝에서만 데이터의 삽입과 삭제가 이루어집니다.

### 주요 연산

- **push(item)**: 스택의 맨 위에 항목 추가
- **pop()**: 스택의 맨 위 항목을 제거하고 반환
- **peek()**: 스택의 맨 위 항목을 제거하지 않고 반환
- **isEmpty()**: 스택이 비어있는지 확인
- **size()**: 스택의 크기 반환

### 특징
- LIFO 원칙: 마지막에 들어간 데이터가 가장 먼저 나옴
- Top 포인터: 스택의 가장 위 요소를 가리킴
- 단방향 접근: Top에서만 데이터 접근 가능
- 제한적 접근: 중간 요소에 직접 접근 불가

### 시간 복잡도

| 연산 | 시간 복잡도 |
|------|------------|
| Push | O(1) |
| Pop | O(1) |
| Peek | O(1) |
| Search | O(n) |

### 구현 예제

```java
// 배열 기반 스택 구현
class ArrayStack {
    private int[] arr;
    private int top;
    private int capacity;
    
    public ArrayStack(int size) {
        arr = new int[size];
        capacity = size;
        top = -1;
    }
    
    public void push(int item) {
        if (isFull()) {
            throw new StackOverflowError("Stack is full");
        }
        arr[++top] = item;
    }
    
    public int pop() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return arr[top--];
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return arr[top];
    }
    
    public boolean isEmpty() {
        return top == -1;
    }
    
    public boolean isFull() {
        return top == capacity - 1;
    }
    
    public int size() {
        return top + 1;
    }
}

// 연결 리스트 기반 스택 구현
class LinkedStack {
    private class Node {
        int data;
        Node next;
        
        Node(int data) {
            this.data = data;
            this.next = null;
        }
    }
    
    private Node top;
    private int size;
    
    public LinkedStack() {
        this.top = null;
        this.size = 0;
    }
    
    public void push(int item) {
        Node newNode = new Node(item);
        newNode.next = top;
        top = newNode;
        size++;
    }
    
    public int pop() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        int data = top.data;
        top = top.next;
        size--;
        return data;
    }
    
    public int peek() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return top.data;
    }
    
    public boolean isEmpty() {
        return top == null;
    }
    
    public int size() {
        return size;
    }
}

// Java Stack 클래스 사용 예제
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        
        // Push
        stack.push(10);
        stack.push(20);
        stack.push(30);
        
        // Peek
        System.out.println("Top element: " + stack.peek());  // 30
        
        // Pop
        System.out.println("Popped: " + stack.pop());  // 30
        System.out.println("Popped: " + stack.pop());  // 20
        
        // Size
        System.out.println("Size: " + stack.size());  // 1
        
        // isEmpty
        System.out.println("Is empty: " + stack.isEmpty());  // false
    }
    
    // 괄호 검사 예제
    public static boolean isValidParentheses(String s) {
        Stack<Character> stack = new Stack<>();
        
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else {
                if (stack.isEmpty()) return false;
                
                char top = stack.pop();
                if (c == ')' && top != '(') return false;
                if (c == '}' && top != '{') return false;
                if (c == ']' && top != '[') return false;
            }
        }
        
        return stack.isEmpty();
    }
    
    // 후위 표기법 계산 예제
    public static int evaluatePostfix(String expression) {
        Stack<Integer> stack = new Stack<>();
        
        for (String token : expression.split(" ")) {
            if (token.matches("-?\\d+")) {
                stack.push(Integer.parseInt(token));
            } else {
                int b = stack.pop();
                int a = stack.pop();
                
                switch (token) {
                    case "+": stack.push(a + b); break;
                    case "-": stack.push(a - b); break;
                    case "*": stack.push(a * b); break;
                    case "/": stack.push(a / b); break;
                }
            }
        }
        
        return stack.pop();
    }
}
```

### 활용 사례
- **재귀 알고리즘**: 함수 호출 스택
- **웹 브라우저**: 방문 기록 (뒤로 가기)
- **실행 취소 (Undo)**: 편집기, 포토샵
- **수식 계산**: 후위 표기법, 중위 표기법 변환
- **괄호 검사**: 올바른 괄호 문자열 판단
- **DFS (깊이 우선 탐색)**
- **함수 호출**: 콜 스택

---

## Queue

### 개념
큐는 FIFO(First In First Out) 또는 LILO(Last In Last Out) 순서를 따르는 선형 자료구조입니다. 한쪽 끝(rear)에서는 삽입만, 다른 쪽 끝(front)에서는 삭제만 이루어집니다.

### 주요 연산

- **enqueue(item)**: 큐의 뒤쪽에 항목 추가
- **dequeue()**: 큐의 앞쪽 항목을 제거하고 반환
- **peek()**: 큐의 앞쪽 항목을 제거하지 않고 반환
- **isEmpty()**: 큐가 비어있는지 확인
- **size()**: 큐의 크기 반환

### 특징
- FIFO 원칙: 먼저 들어간 데이터가 먼저 나옴
- Front와 Rear 포인터: 각각 앞과 뒤를 가리킴
- 양방향 접근: 삽입은 rear, 삭제는 front

### 종류

**1. 선형 큐 (Linear Queue)**
- 기본적인 큐 구조
- rear가 끝에 도달하면 앞 공간이 비어있어도 사용 불가

**2. 원형 큐
