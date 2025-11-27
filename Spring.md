# 🍃 Spring Framework 핵심 개념 정리

> Spring Framework 개발자를 위한 핵심 개념 요약 가이드

[![Spring](https://img.shields.io/badge/Spring-6DB33F?style=flat-square&logo=spring&logoColor=white)](https://spring.io/)
[![Java](https://img.shields.io/badge/Java-007396?style=flat-square&logo=java&logoColor=white)](https://www.oracle.com/java/)

## 📑 목차
- [Spring Framework란](#-spring-framework란)
- [Bean](#-bean)
- [Container와 IoC](#-container와-ioc)
- [DI (의존성 주입)](#-di-의존성-주입)
- [AOP](#-aop-관점-지향-프로그래밍)
- [POJO](#-pojo)
- [DAO vs DTO](#-dao-vs-dto)
- [Filter vs Interceptor](#-filter-vs-interceptor)

---

## 🌱 Spring Framework란

### 정의
**경량급 오픈소스 자바 애플리케이션 프레임워크**

> Lightweight Java Application Framework

### 목표
- **POJO 기반**의 Enterprise Application 개발을 쉽고 편하게
- Java Application 개발에 필요한 **하부구조(Infrastructure)를 포괄적으로 제공**
- 개발자는 비즈니스 로직에 집중

### 특징
- 동적인 웹 사이트 개발을 위한 다양한 서비스 제공
- 대한민국 **전자정부 표준 프레임워크**의 기반 기술

---

## 🫘 Bean

### Bean이란?
**Spring 컨테이너가 관리하는 객체**

```java
// Bean 등록 방법
@Component
public class UserService {
    // ...
}

// 또는
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService();
    }
}
```

### Bean 생명주기

```
객체 생성 → 의존 설정 → 초기화 → 사용 → 소멸
```

| 단계 | 설명 |
|------|------|
| **생성** | 스프링 컨테이너 초기화 시 빈 객체 생성 |
| **의존 설정** | 의존 객체 주입 |
| **초기화** | 초기화 메서드 실행 |
| **사용** | 애플리케이션에서 빈 사용 |
| **소멸** | 스프링 컨테이너 종료 시 빈 객체 소멸 |

### Bean 초기화 방법

#### 1️⃣ @PostConstruct (권장 ⭐)
```java
@Component
public class MyBean {
    @PostConstruct
    public void init() {
        // 초기화 로직
    }
}
```
**장점**: 간결하고 직관적, 코드만으로 초기화 메서드 파악 가능

#### 2️⃣ InitializingBean 인터페이스
```java
@Component
public class MyBean implements InitializingBean {
    @Override
    public void afterPropertiesSet() throws Exception {
        // 초기화 로직
    }
}
```
**단점**: Spring 인터페이스에 의존, 권장하지 않음

#### 3️⃣ @Bean의 initMethod 속성
```java
@Configuration
public class AppConfig {
    @Bean(initMethod = "init")
    public MyBean myBean() {
        return new MyBean();
    }
}
```

### Bean 소멸 방법

#### 1️⃣ @PreDestroy (권장 ⭐)
```java
@Component
public class MyBean {
    @PreDestroy
    public void cleanup() {
        // 정리 로직
    }
}
```

#### 2️⃣ DisposableBean 인터페이스
```java
@Component
public class MyBean implements DisposableBean {
    @Override
    public void destroy() throws Exception {
        // 정리 로직
    }
}
```

#### 3️⃣ @Bean의 destroyMethod 속성
```java
@Configuration
public class AppConfig {
    @Bean(destroyMethod = "cleanup")
    public MyBean myBean() {
        return new MyBean();
    }
}
```

### Bean Scope

| Scope | 설명 | 생명주기 |
|-------|------|----------|
| **singleton** (기본) | 컨테이너당 한 개의 인스턴스 | 컨테이너와 동일 |
| **prototype** | 요청마다 새로운 인스턴스 생성 | GC에 의해 관리 |
| **request** | HTTP 요청마다 한 개 | HTTP 요청 범위 |
| **session** | HTTP 세션마다 한 개 | HTTP 세션 범위 |
| **application** | ServletContext마다 한 개 | ServletContext 범위 |

```java
@Component
@Scope("prototype")
public class PrototypeBean {
    // 요청마다 새로운 인스턴스
}
```

---

## 📦 Container와 IoC

### Container란?
**인스턴스의 생명주기를 관리하고 추가 기능을 제공하는 존재**

- 객체의 생성과 소멸을 컨트롤
- 개발자가 작성한 코드의 처리 과정을 위임받은 독립적인 존재
- 적절한 설정만 있으면 스스로 객체를 관리

### IoC (Inversion of Control, 제어의 역전)

#### 정의
**객체의 생성부터 생명주기 관리까지 모든 제어권이 개발자가 아닌 프레임워크에게 넘어간 것**

```java
// 기존 방식 (개발자가 제어)
public class UserService {
    private UserRepository repository = new UserRepository();
}

// IoC 방식 (컨테이너가 제어)
@Service
public class UserService {
    private final UserRepository repository;
    
    @Autowired
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

#### 특징
- 프레임워크가 흐름을 주도
- 개발자는 필요한 부품을 개발하고 조립
- 최종 호출은 프레임워크 내부에서 결정

#### 라이브러리 vs 프레임워크

| 구분 | 제어권 | 설명 |
|------|--------|------|
| **라이브러리** | 개발자 | 개발자가 능동적으로 호출 |
| **프레임워크** | 프레임워크 | 프레임워크가 개발자 코드를 호출 |

---

## 💉 DI (의존성 주입)

### DI란?
**Dependency Injection - 객체 간의 의존관계를 컨테이너가 자동으로 연결**

```java
// DI 전 - 직접 객체 생성
public class UserService {
    private UserRepository repository = new UserRepository();
}

// DI 후 - 컨테이너가 주입
@Service
public class UserService {
    private final UserRepository repository;
    
    @Autowired
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

### 의존성이 위험한 이유

❌ **문제점**:
- 하나의 모듈 변경 시 의존한 다른 모듈도 변경 필요
- 유닛 테스트 작성이 어려움
- 모듈 간 결합도 증가

### DI의 장점

✅ **해결책**:
- 클래스 재사용성 증가
- 독립적인 테스트 가능
- 코드의 확장성 향상
- 객체 간 결합도 감소

### DI의 세 가지 방법

#### 1️⃣ 생성자 주입 (권장 ⭐)
```java
@Service
public class UserService {
    private final UserRepository repository;
    
    @Autowired
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```
**장점**: 불변성 보장, 순환 참조 방지, 테스트 용이

#### 2️⃣ Setter 주입
```java
@Service
public class UserService {
    private UserRepository repository;
    
    @Autowired
    public void setRepository(UserRepository repository) {
        this.repository = repository;
    }
}
```

#### 3️⃣ 필드 주입
```java
@Service
public class UserService {
    @Autowired
    private UserRepository repository;
}
```
**단점**: 테스트 어려움, 불변성 보장 안됨

---

## 🎯 AOP (관점 지향 프로그래밍)

### AOP란?
**Aspect Oriented Programming - 횡단 관심사를 모듈화하는 프로그래밍 기법**

### 목적
- 흩어진 관심사(Concern)를 모듈화
- 핵심 로직과 부가 기능 분리
- 코드 중복 제거 및 재사용성 향상

### AOP 개념도

```
┌─────────────────────────────────────┐
│         핵심 비즈니스 로직          │
├─────────────────────────────────────┤
│ ▶ 로깅                              │
│ ▶ 트랜잭션                          │  ← Aspect (부가 기능)
│ ▶ 보안                              │
│ ▶ 성능 측정                         │
└─────────────────────────────────────┘
```

### AOP 주요 용어

| 용어 | 설명 |
|------|------|
| **Aspect** | 흩어진 관심사를 모듈화한 것 (Advice + PointCut) |
| **Target** | Aspect를 적용하는 곳 (클래스, 메서드) |
| **Advice** | 실제로 수행해야 하는 기능 |
| **JoinPoint** | Advice가 적용될 수 있는 위치 |
| **PointCut** | JoinPoint의 상세 스펙 (Advice 실행 지점) |
| **Weaving** | PointCut에 Advice를 삽입하는 과정 |

### AOP 예제

```java
@Aspect
@Component
public class LoggingAspect {
    
    // PointCut 정의
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceLayer() {}
    
    // Before Advice
    @Before("serviceLayer()")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("메서드 실행 전: " + joinPoint.getSignature());
    }
    
    // After Advice
    @After("serviceLayer()")
    public void logAfter(JoinPoint joinPoint) {
        System.out.println("메서드 실행 후: " + joinPoint.getSignature());
    }
    
    // Around Advice
    @Around("serviceLayer()")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long end = System.currentTimeMillis();
        System.out.println("실행 시간: " + (end - start) + "ms");
        return result;
    }
}
```

### Spring AOP 특징

- **프록시 패턴** 기반 구현
- **런타임 시** Bean 생성과 동시에 프록시 생성
- **Spring Bean에만** AOP 적용 가능
- **메서드 JoinPoint만** 지원

### AOP 적용 시점

| 시점 | 방식 | 설명 |
|------|------|------|
| 컴파일 시 | AspectJ | 바이트코드 조작 |
| 로드 시 | AspectJ | 클래스 로딩 시점에 위빙 |
| **런타임 시** | **Spring AOP** | **프록시 기반 (권장)** |

---

## 🎲 POJO

### POJO란?
**Plain Old Java Object - 평범한 자바 객체**

> 특정 프레임워크나 기술에 의존하지 않는 순수한 자바 객체

### POJO가 아닌 예

```java
// EJB에 의존
public class UserService implements SessionBean {
    SessionContext ctx;
    
    public void setSessionContext(SessionContext ctx) {
        this.ctx = ctx;
    }
    // EJB 스펙에 종속된 코드
}
```

### POJO 예

```java
// 순수한 자바 객체
public class UserService {
    private UserRepository repository;
    
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
    
    public User findUser(Long id) {
        return repository.findById(id);
    }
}
```

### POJO의 장점

✅ **장점**:
- **코드의 간결함**: 비즈니스 로직과 특정 환경 분리
- **테스트 용이성**: 환경에 종속되지 않아 단위 테스트 쉬움
- **객체지향 설계 자유로움**: 상속, 다형성 등 자유롭게 활용
- **유지보수 편리함**: 특정 기술에 종속되지 않음

### Spring과 POJO

Spring은 **POJO 기반 프레임워크**로:
- IoC/DI를 통해 POJO 간의 의존관계 관리
- AOP를 통해 POJO에 부가 기능 추가
- PSA를 통해 POJO가 특정 기술에 종속되지 않도록 지원

---

## 📊 DAO vs DTO

### DAO (Data Access Object)

**데이터베이스 접근을 전담하는 객체**

```java
@Repository
public class UserDao {
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public User findById(Long id) {
        String sql = "SELECT * FROM users WHERE id = ?";
        return jdbcTemplate.queryForObject(sql, userRowMapper, id);
    }
    
    public void save(User user) {
        String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
        jdbcTemplate.update(sql, user.getName(), user.getEmail());
    }
}
```

**특징**:
- DB 데이터 조회/조작 기능 전담
- 비즈니스 로직과 DB 접근 로직 분리

### DTO (Data Transfer Object)

**계층 간 데이터 교환을 위한 객체**

```java
public class UserDto {
    private Long id;
    private String name;
    private String email;
    
    // 생성자
    public UserDto(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }
    
    // Getter & Setter
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

**특징**:
- 로직 없는 순수한 데이터 객체
- Getter/Setter만 가짐
- 계층 간 데이터 전달 목적

### DAO vs DTO 비교

| 구분 | DAO | DTO |
|------|-----|-----|
| **목적** | DB 접근 로직 | 데이터 전달 |
| **위치** | Persistence Layer | 모든 계층 |
| **로직** | CRUD 메서드 포함 | 로직 없음 (순수 데이터) |
| **별칭** | Repository | VO (Value Object) |

---

## 🔍 Filter vs Interceptor

### 공통점
애플리케이션의 **공통 기능을 분리하여 관리**

### 실행 흐름

```
HTTP Request
    ↓
┌─────────────────────┐
│   Filter (init)     │ ← Web Application 영역
└─────────────────────┘
    ↓
┌─────────────────────┐
│ DispatcherServlet   │
└─────────────────────┘
    ↓
┌─────────────────────┐
│ Interceptor         │ ← Spring Context 영역
│  - preHandle        │
└─────────────────────┘
    ↓
┌─────────────────────┐
│   Controller        │
└─────────────────────┘
    ↓
┌─────────────────────┐
│ Interceptor         │
│  - postHandle       │
│  - afterCompletion  │
└─────────────────────┘
    ↓
┌─────────────────────┐
│   Filter (doFilter) │
└─────────────────────┘
    ↓
HTTP Response
```

### Filter

**Dispatcher Servlet 이전에 실행**

```java
@Component
public class LogFilter implements Filter {
    
    @Override
    public void init(FilterConfig filterConfig) {
        // 필터 초기화
    }
    
    @Override
    public void doFilter(ServletRequest request, 
                        ServletResponse response, 
                        FilterChain chain) throws IOException, ServletException {
        // 실제 로직
        System.out.println("Filter 실행");
        chain.doFilter(request, response);
    }
    
    @Override
    public void destroy() {
        // 필터 종료
    }
}
```

**특징**:
- WAS 내 ApplicationContext에서 실행
- Spring과 무관한 자원 처리
- web.xml에 설정
- 예외 발생 시 Web Application에서 처리
- **예시**: 인코딩 변환, XSS 방어, 이미지 압축

### Interceptor

**Dispatcher Servlet 이후, Controller 전후에 실행**

```java
@Component
public class AuthInterceptor implements HandlerInterceptor {
    
    @Override
    public boolean preHandle(HttpServletRequest request, 
                            HttpServletResponse response, 
                            Object handler) {
        // Controller 실행 전
        System.out.println("preHandle 실행");
        return true; // false 반환 시 Controller 실행 안됨
    }
    
    @Override
    public void postHandle(HttpServletRequest request, 
                          HttpServletResponse response, 
                          Object handler, 
                          ModelAndView modelAndView) {
        // Controller 실행 후
        System.out.println("postHandle 실행");
    }
    
    @Override
    public void afterCompletion(HttpServletRequest request, 
                               HttpServletResponse response, 
                               Object handler, 
                               Exception ex) {
        // View 렌더링 후
        System.out.println("afterCompletion 실행");
    }
}
```

**특징**:
- Spring Context 내부에서 실행
- 모든 Spring Bean 접근 가능
- servlet-context.xml에 설정
- @ControllerAdvice에서 예외 처리
- **예시**: 로그인 체크, 권한 체크, 로그 기록

### Filter vs Interceptor 비교

| 구분 | Filter | Interceptor |
|------|--------|-------------|
| **관리 주체** | WAS (Web Container) | Spring Container |
| **실행 위치** | Dispatcher Servlet 이전 | Controller 전후 |
| **Spring 접근** | ❌ | ✅ (모든 Bean 접근 가능) |
| **설정 파일** | web.xml | servlet-context.xml |
| **메서드** | init, doFilter, destroy | preHandle, postHandle, afterCompletion |
| **예외 처리** | Web Application | @ControllerAdvice |
| **주 사용처** | 인코딩, 보안, 압축 | 인증, 권한, 로깅 |

### 언제 무엇을 사용할까?

#### Filter 사용
- 모든 요청에 대한 공통 작업
- Spring과 무관한 작업
- 인코딩, XSS 방어, 이미지 압축 등

#### Interceptor 사용
- Spring Bean 접근이 필요한 경우
- Controller 관련 전후 처리
- 로그인 체크, 권한 검사, API 로깅 등

---

## 📚 참고 자료

- [Spring Framework Documentation](https://docs.spring.io/spring-framework/reference/)
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Baeldung Spring Tutorials](https://www.baeldung.com/spring-tutorial)

---
<div align="center">

</div>
