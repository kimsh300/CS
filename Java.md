# ☕ Java 핵심 개념 정리

> Java 개발자를 위한 핵심 개념 요약 가이드

## 📑 목차
- [Java의 장단점](#-java의-장단점)
- [데이터 타입](#-데이터-타입)
- [static vs non-static](#-static-vs-non-static)
- [final, finally, finalize()](#-final-finally-finalize)
- [Call by Value](#-call-by-value)
- [클래스, 객체, 인스턴스](#-클래스-객체-인스턴스)
- [객체지향 프로그래밍](#-객체지향-프로그래밍-oop)
- [OOP 4가지 특징](#-oop의-4가지-특징)
- [SOLID 원칙](#-solid-원칙)
- [오버로딩 vs 오버라이딩](#-오버로딩-vs-오버라이딩)

---

## 🎯 Java의 장단점

### ✅ 장점

| 특징 | 설명 |
|------|------|
| **운영체제 독립적** | JVM에서 실행되어 플랫폼에 구애받지 않음 (Write Once, Run Anywhere) |
| **객체지향 언어** | 캡슐화, 상속, 추상화, 다형성 등 객체지향 패러다임 지원 |
| **자동 메모리 관리** | Garbage Collector가 자동으로 메모리 관리 |
| **오픈소스** | OpenJDK 기반의 풍부한 생태계와 라이브러리 |
| **멀티스레드 지원** | 운영체제에 관계없이 쉽게 멀티스레드 구현 가능 |
| **동적 로딩** | 필요한 시점에 클래스를 로딩하여 유지보수 용이 |

### ❌ 단점

- **상대적으로 느린 속도**: JVM을 거쳐 실행되기 때문에 C/C++보다 느림 *(JIT 컴파일러로 많이 개선됨)*
- **예외처리 강제**: Checked Exception은 반드시 처리해야 함

---

## 📦 데이터 타입

### Primitive Type (기본 타입) - Stack 영역

```java
// 정수형
byte b = 1;
short s = 100;
int i = 1000;
long l = 10000L;

// 실수형
float f = 3.14f;
double d = 3.14159;

// 논리형
boolean flag = true;

// 문자형
char c = 'A';
```

**특징**: 고정 크기, Stack 메모리에 저장

### Reference Type (참조 타입) - Heap 영역

```java
// 클래스, 배열, 인터페이스, Enum
String str = "Hello";
int[] arr = new int[10];
List<String> list = new ArrayList<>();
MyClass obj = new MyClass();
```

**특징**:
- `new` 키워드로 객체 생성 (String과 배열은 예외)
- 객체의 주소를 참조
- 더 이상 참조가 없으면 GC에 의해 제거
- 가변적인 크기로 Heap 메모리에 저장

---

## 🔄 static vs non-static

### Instance Member (non-static)

```java
public class Person {
    private String name;  // 인스턴스 변수
    
    public void introduce() {  // 인스턴스 메서드
        System.out.println("My name is " + name);
    }
}
```

| 특성 | 설명 |
|------|------|
| **공간** | 객체마다 별도로 존재 |
| **시간** | 객체 생성 시 멤버 생성 |
| **공유** | 공유되지 않음 |

### Class Member (static)

```java
public class Counter {
    private static int count = 0;  // 클래스 변수
    
    public static void increment() {  // 클래스 메서드
        count++;
    }
}
```

| 특성 | 설명 |
|------|------|
| **공간** | 클래스당 하나만 생성 |
| **시간** | 클래스 로딩 시 생성 (객체 생성 전) |
| **공유** | 모든 객체가 공유 |

---

## 🔒 final, finally, finalize()

### `final` 키워드

```java
// 변수: 상수 (값 변경 불가)
final int MAX_VALUE = 100;

// 메서드: 오버라이드 불가
public final void print() { }

// 클래스: 상속 불가
public final class Utility { }
```

### `finally` 블록

```java
try {
    // 예외 발생 가능 코드
    FileReader file = new FileReader("test.txt");
} catch (IOException e) {
    // 예외 처리
    e.printStackTrace();
} finally {
    // 항상 실행되는 코드 (자원 해제 등)
    file.close();
}
```

### `finalize()` 메서드

```java
@Override
protected void finalize() throws Throwable {
    // GC가 객체를 메모리에서 제거하기 전 호출
    // 자원 정리 작업
    super.finalize();
}
```

> ⚠️ **참고**: Java 9부터 `finalize()`는 deprecated되었으며, `try-with-resources`나 `Cleaner` 사용 권장

---

## 📞 Call by Value

### ⚠️ 중요: Java는 항상 Call by Value

#### 기본 타입 (Primitive Type)

```java
public void changeValue(int x) {
    x = 100;  // 복사본만 변경됨
}

int a = 10;
changeValue(a);
System.out.println(a);  // 출력: 10 (변경 안됨)
```

#### 참조 타입 (Reference Type)

```java
public void changeName(Person p) {
    p.setName("Kim");  // 객체 내용 변경 가능
}

public void assignNewPerson(Person p) {
    p = new Person("Lee");  // p 자체는 변경 안됨 (복사본만 변경)
}

Person person = new Person("Park");
changeName(person);
System.out.println(person.getName());  // 출력: Kim (내용 변경됨)

assignNewPerson(person);
System.out.println(person.getName());  // 출력: Kim (p는 여전히 원래 객체 참조)
```

**핵심**: 참조 타입도 "참조값의 복사본"이 전달되므로 Call by Value

---

## 🏗️ 클래스, 객체, 인스턴스

```java
// 클래스: 설계도
public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// 객체/인스턴스: 실체
Person person = new Person("Kim", 25);  // Person 클래스의 인스턴스
```

| 용어 | 설명 | 관점 |
|------|------|------|
| **클래스** | 객체를 만들기 위한 설계도 또는 틀 | 개념 |
| **객체** | 클래스 타입으로 선언된 것 | 포괄적 |
| **인스턴스** | 메모리에 할당된 실체 | 구체적 관계 |

---

## 🎨 객체지향 프로그래밍 (OOP)

### 정의
각자의 역할을 지닌 객체들끼리 메시지를 주고받으며 동작하도록 프로그래밍하는 방법론

### ✅ 장점

- 사람의 관점에서 이해하기 쉬움
- 강한 응집력 (Strong Cohesion)
- 약한 결합력 (Weak Coupling)
- 높은 재사용성, 확장성, 융통성
- 디버깅과 유지보수 용이

### ❌ 단점

- 객체 간 메시지 교환으로 인한 오버헤드
- 객체 상태로 인한 예측 불가능한 부작용 가능성

---

## 🔑 OOP의 4가지 특징

### 1. 추상화 (Abstraction)
공통적인 특징을 파악해 하나의 개념으로 정의

```java
abstract class Animal {
    abstract void makeSound();
}
```

### 2. 캡슐화 (Encapsulation)
정보 은닉으로 높은 응집도와 낮은 결합도 유지

```java
public class BankAccount {
    private double balance;  // private으로 숨김
    
    public void deposit(double amount) {  // public 메서드로 접근
        if (amount > 0) balance += amount;
    }
}
```

### 3. 상속 (Inheritance)
공통된 특성을 부모 클래스로 정의

```java
class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Woof!");
    }
}
```

### 4. 다형성 (Polymorphism)
같은 메시지, 다른 동작

```java
Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.makeSound();  // Woof!
animal2.makeSound();  // Meow!
```

---

## 🏛️ SOLID 원칙

| 원칙 | 설명 | 예시 |
|------|------|------|
| **S**RP<br>Single Responsibility | 단일 책임 원칙<br>하나의 클래스는 하나의 책임만 | `UserService`는 사용자 관련 로직만 담당 |
| **O**CP<br>Open-Closed | 개방-폐쇄 원칙<br>확장에는 열려있고 수정에는 닫혀있음 | 인터페이스를 통한 확장 |
| **L**SP<br>Liskov Substitution | 리스코프 치환 원칙<br>자식은 부모를 대체 가능 | `Rectangle` → `Square` 관계 |
| **I**SP<br>Interface Segregation | 인터페이스 분리 원칙<br>클라이언트에 특화된 인터페이스 | 필요한 메서드만 구현 |
| **D**IP<br>Dependency Inversion | 의존 역전 원칙<br>추상화에 의존, 구체화에 의존 X | 인터페이스 의존 |

---

## 🔄 오버로딩 vs 오버라이딩

### 오버로딩 (Overloading)
**같은 이름, 다른 매개변수**

```java
public class Calculator {
    // 매개변수 개수가 다름
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // 매개변수 타입이 다름
    public double add(double a, double b) {
        return a + b;
    }
}
```

### 오버라이딩 (Overriding)
**부모 메서드를 자식이 재정의**

```java
public abstract class Shape {
    public void printMe() {
        System.out.println("Shape");
    }
    
    public abstract double computeArea();
}

public class Circle extends Shape {
    private double radius = 5;
    
    @Override  // 어노테이션 권장
    public void printMe() {
        System.out.println("Circle");
    }
    
    @Override
    public double computeArea() {
        return radius * radius * Math.PI;
    }
}
```

| 구분 | 오버로딩 | 오버라이딩 |
|------|----------|------------|
| **관계** | 같은 클래스 내 | 상속 관계 |
| **메서드명** | 동일 | 동일 |
| **매개변수** | 다름 | 동일 |
| **리턴타입** | 상관없음 | 동일 (공변 반환 타입 제외) |

---

## 📚 참고 자료

- [Oracle Java Documentation](https://docs.oracle.com/javase/tutorial/)
- [Effective Java](https://www.oracle.com/java/technologies/effectivejava.html)
- [Java Design Patterns](https://java-design-patterns.com/)

---

## 📝 License

MIT License

---

<div align="center">

**⭐ 도움이 되었다면 Star를 눌러주세요! ⭐**

Made with ☕ by [Your Name]

</div>
