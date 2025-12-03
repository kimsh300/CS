# Network (네트워크)

> 컴퓨터 네트워크의 핵심 개념과 프로토콜 정리

## 📋 목차

- [OSI 7계층](#osi-7계층)
- [TCP/IP 개념](#tcpip-개념)
- [TCP와 UDP](#tcp와-udp)
- [TCP와 UDP 헤더 분석](#tcp와-udp-헤더-분석)
- [TCP 3-way Handshake와 4-way Handshake](#tcp-3-way-handshake와-4-way-handshake)
- [HTTP와 HTTPS](#http와-https)
- [HTTP 요청/응답 헤더](#http-요청응답-헤더)
- [HTTP와 HTTPS 동작 과정](#http와-https-동작-과정)
- [CORS](#cors)
- [GET과 POST](#get과-post)
- [쿠키와 세션](#쿠키와-세션)
- [DNS](#dns)
- [REST와 RESTful](#rest와-restful)
- [소켓](#소켓)
- [Socket.io와 WebSocket](#socketio와-websocket)
- [Frame, Packet, Segment, Datagram](#frame-packet-segment-datagram)

---

## OSI 7계층

### 개념
OSI(Open Systems Interconnection) 모델은 국제표준화기구(ISO)에서 1984년에 개발한 네트워크 통신의 표준 모델입니다. 컴퓨터 네트워크 프로토콜 디자인과 통신을 7개의 계층으로 나누어 설명합니다.

### 목적
- 서로 다른 시스템 간의 통신을 표준화
- 네트워크 문제 해결을 위한 체계적 접근
- 각 계층의 독립적 개발 및 유지보수 가능

### 7계층 구조

```
┌─────────────────────────────┐
│   7. Application Layer      │  응용 계층
├─────────────────────────────┤
│   6. Presentation Layer     │  표현 계층
├─────────────────────────────┤
│   5. Session Layer          │  세션 계층
├─────────────────────────────┤
│   4. Transport Layer        │  전송 계층
├─────────────────────────────┤
│   3. Network Layer          │  네트워크 계층
├─────────────────────────────┤
│   2. Data Link Layer        │  데이터 링크 계층
├─────────────────────────────┤
│   1. Physical Layer         │  물리 계층
└─────────────────────────────┘
```

### 각 계층 상세 설명

#### 1. 물리 계층 (Physical Layer)
**기능**
- 실제 장치들을 연결하기 위한 전기적, 물리적 세부 사항 정의
- 비트(0과 1) 단위로 데이터 전송
- 전송 매체, 신호 방식, 전압 레벨 등 규정

**프로토콜/장비**
- 케이블, 리피터, 허브
- RS-232, V.35

**전송 단위**: Bit

#### 2. 데이터 링크 계층 (Data Link Layer)
**기능**
- Point-to-Point 간 신뢰성 있는 전송 보장
- 물리적 주소(MAC Address) 지정
- 오류 검출 및 수정 (CRC)
- 흐름 제어

**프로토콜/장비**
- 이더넷(Ethernet), PPP, HDLC
- 스위치, 브리지
- MAC 주소

**전송 단위**: Frame

**부계층**
- LLC (Logical Link Control): 흐름 제어, 오류 제어
- MAC (Media Access Control): 물리적 주소 지정

#### 3. 네트워크 계층 (Network Layer)
**기능**
- 논리적 주소(IP Address) 지정
- 라우팅: 최적의 경로 결정
- 패킷 포워딩
- 혼잡 제어

**프로토콜/장비**
- IP, ICMP, IGMP, ARP
- 라우터
- IPv4, IPv6

**전송 단위**: Packet (Datagram)

#### 4. 전송 계층 (Transport Layer)
**기능**
- End-to-End 신뢰성 있는 데이터 전송
- 포트 번호를 통한 프로세스 식별
- 흐름 제어 및 오류 제어
- 데이터 분할 및 재조립

**프로토콜**
- TCP (신뢰성 보장)
- UDP (빠른 전송)

**전송 단위**: Segment (TCP), Datagram (UDP)

#### 5. 세션 계층 (Session Layer)
**기능**
- 통신 세션 구성 및 관리
- 연결 설정, 유지, 종료
- 동기화 및 체크포인트
- 대화 제어

**프로토콜**
- NetBIOS, RPC, PPTP

**통신 모드**
- Simplex (단방향)
- Half-Duplex (반이중)
- Full-Duplex (전이중)

#### 6. 표현 계층 (Presentation Layer)
**기능**
- 데이터 형식 변환 (코드 간 번역)
- 암호화 및 복호화
- 압축 및 압축 해제
- 데이터 표현 방식 통일

**프로토콜/포맷**
- JPEG, MPEG, GIF (이미지/비디오)
- ASCII, EBCDIC (문자 인코딩)
- SSL/TLS (암호화)

#### 7. 응용 계층 (Application Layer)
**기능**
- 사용자와 가장 가까운 계층
- 네트워크 서비스를 최종 사용자에게 제공
- 애플리케이션 프로세스 간 통신

**프로토콜/서비스**
- HTTP/HTTPS (웹)
- FTP (파일 전송)
- SMTP (메일 전송)
- DNS (도메인 이름 해석)
- Telnet, SSH

### 데이터 흐름

**송신 과정 (Encapsulation)**
```
Application Data
    ↓ (7-5계층)
Segment/Datagram + L4 Header
    ↓ (4계층)
Packet + L3 Header
    ↓ (3계층)
Frame + L2 Header + L2 Trailer
    ↓ (2계층)
Bits
    ↓ (1계층)
```

**수신 과정 (Decapsulation)**
```
Bits
    ↑ (1계층)
Frame 해석
    ↑ (2계층)
Packet 해석
    ↑ (3계층)
Segment 해석
    ↑ (4계층)
Application Data
    ↑ (7-5계층)
```

### OSI 모델의 장점
- **표준화**: 서로 다른 시스템 간 통신 가능
- **모듈화**: 각 계층의 독립적 개발
- **문제 해결**: 계층별 문제 격리 및 해결
- **교육적 가치**: 네트워크 개념 이해에 유용

### OSI vs TCP/IP

| 구분 | OSI 모델 | TCP/IP 모델 |
|------|---------|------------|
| 계층 수 | 7계층 | 4계층 |
| 개발 | ISO | DoD |
| 접근 방식 | 프로토콜 독립적 | 프로토콜 중심 |
| 신뢰성 | 이론적 모델 | 실용적 모델 |
| 사용 | 교육, 참조 | 실제 인터넷 |

---

## TCP/IP 개념

### 개념
TCP/IP(Transmission Control Protocol/Internet Protocol)는 인터넷의 기반이 되는 프로토콜 집합입니다. 1970년대 미국 국방부(DoD)에서 개발되었으며, 현재 인터넷의 표준 프로토콜로 사용됩니다.

### TCP/IP 4계층

```
┌─────────────────────────────┐
│   Application Layer         │  응용 계층
│   (OSI 5-7계층)             │
├─────────────────────────────┤
│   Transport Layer           │  전송 계층
│   (OSI 4계층)               │
├─────────────────────────────┤
│   Internet Layer            │  인터넷 계층
│   (OSI 3계층)               │
├─────────────────────────────┤
│   Network Access Layer      │  네트워크 액세스 계층
│   (OSI 1-2계층)             │
└─────────────────────────────┘
```

### 계층별 프로토콜

**1. Application Layer (응용 계층)**
- HTTP, HTTPS, FTP, SMTP, DNS, Telnet, SSH

**2. Transport Layer (전송 계층)**
- TCP, UDP

**3. Internet Layer (인터넷 계층)**
- IP, ICMP, ARP, RARP

**4. Network Access Layer (네트워크 액세스 계층)**
- Ethernet, Wi-Fi, PPP

---

## TCP와 UDP

### TCP (Transmission Control Protocol)

#### 특징
- **연결 지향형 프로토콜**: 3-way handshake로 연결 설정
- **신뢰성 보장**: 데이터 전송 보장, 순서 보장
- **흐름 제어**: 송신자와 수신자의 데이터 처리 속도 조절
- **혼잡 제어**: 네트워크 혼잡 상황 대응
- **전이중(Full-Duplex)**: 양방향 동시 통신
- **점대점(Point-to-Point)**: 1:1 통신만 가능

#### 동작 방식
```
클라이언트                     서버
    |                           |
    |  1. SYN (연결 요청)       |
    |-------------------------->|
    |                           |
    |  2. SYN+ACK (수락+요청)   |
    |<--------------------------|
    |                           |
    |  3. ACK (수락 확인)       |
    |-------------------------->|
    |                           |
    |  === 데이터 전송 ===     |
    |                           |
```

#### 흐름 제어 (Flow Control)
**목적**: 수신자의 버퍼 오버플로우 방지

**방법**
- **Stop-and-Wait**: 응답 확인 후 다음 데이터 전송
- **Sliding Window**: 윈도우 크기만큼 연속 전송

#### 혼잡 제어 (Congestion Control)
**목적**: 네트워크 혼잡 방지

**알고리즘**
- **Slow Start**: 지수적 증가
- **Congestion Avoidance**: 선형적 증가
- **Fast Retransmit**: 빠른 재전송
- **Fast Recovery**: 빠른 회복

#### 사용 사례
- 웹 브라우징 (HTTP/HTTPS)
- 이메일 (SMTP, POP3, IMAP)
- 파일 전송 (FTP)
- 원격 접속 (SSH, Telnet)

### UDP (User Datagram Protocol)

#### 특징
- **비연결형 프로토콜**: 연결 설정 없이 전송
- **신뢰성 없음**: 데이터 전송 보장 안 함
- **순서 보장 안 함**: 패킷이 순서대로 도착하지 않을 수 있음
- **빠른 전송**: 오버헤드가 적어 속도가 빠름
- **브로드캐스트/멀티캐스트 지원**

#### 동작 방식
```
송신자                         수신자
    |                           |
    |  데이터그램 전송          |
    |-------------------------->|
    |  (연결 설정 없음)         |
    |                           |
    |  데이터그램 전송          |
    |-------------------------->|
    |  (확인 응답 없음)         |
```

#### 사용 사례
- 실시간 스트리밍 (음성, 영상)
- DNS 질의
- 온라인 게임
- DHCP
- SNMP
- VoIP

### TCP vs UDP 비교

| 특성 | TCP | UDP |
|------|-----|-----|
| 연결 방식 | 연결 지향 | 비연결형 |
| 신뢰성 | 보장 | 보장 안 함 |
| 순서 보장 | 보장 | 보장 안 함 |
| 속도 | 느림 | 빠름 |
| 오버헤드 | 높음 | 낮음 |
| 헤더 크기 | 20 bytes | 8 bytes |
| 흐름 제어 | 있음 | 없음 |
| 혼잡 제어 | 있음 | 없음 |
| 브로드캐스트 | 불가 | 가능 |
| 용도 | 신뢰성 중요 | 속도 중요 |

---

## TCP와 UDP 헤더 분석

### TCP 헤더 구조

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |       |C|E|U|A|P|R|S|F|                               |
| Offset| Resv  |W|C|R|C|S|S|Y|I|            Window             |
|       |       |R|E|G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### TCP 헤더 필드

| 필드 | 크기 | 설명 |
|------|------|------|
| Source Port | 16 bits | 송신 포트 번호 |
| Destination Port | 16 bits | 수신 포트 번호 |
| Sequence Number | 32 bits | 전송되는 바이트의 순서 번호 |
| Acknowledgment Number | 32 bits | 수신 확인 번호 (다음에 받을 바이트) |
| Data Offset | 4 bits | TCP 헤더 길이 (4바이트 단위) |
| Reserved | 6 bits | 예약 필드 (0으로 설정) |
| Flags | 6 bits | 제어 플래그 (아래 참조) |
| Window | 16 bits | 수신 윈도우 크기 |
| Checksum | 16 bits | 오류 검사 |
| Urgent Pointer | 16 bits | 긴급 데이터 포인터 |
| Options | 가변 | 추가 옵션 |

#### TCP 플래그 비트

| 플래그 | 이름 | 설명 |
|-------|------|------|
| URG | Urgent | 긴급 데이터 |
| ACK | Acknowledgment | 응답 확인 |
| PSH | Push | 즉시 전달 |
| RST | Reset | 연결 재설정 |
| SYN | Synchronize | 연결 설정 |
| FIN | Finish | 연결 종료 |

### UDP 헤더 구조

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Length             |           Checksum            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### UDP 헤더 필드

| 필드 | 크기 | 설명 |
|------|------|------|
| Source Port | 16 bits | 송신 포트 번호 |
| Destination Port | 16 bits | 수신 포트 번호 |
| Length | 16 bits | UDP 헤더 + 데이터 전체 길이 |
| Checksum | 16 bits | 오류 검사 (선택적) |

### 포트 번호

**Well-Known Ports (0-1023)**
- 20, 21: FTP
- 22: SSH
- 23: Telnet
- 25: SMTP
- 53: DNS
- 80: HTTP
- 443: HTTPS

**Registered Ports (1024-49151)**
- 3306: MySQL
- 5432: PostgreSQL
- 8080: HTTP Alternative

**Dynamic Ports (49152-65535)**
- 클라이언트가 임시로 사용

---

## TCP 3-way Handshake와 4-way Handshake

### 3-way Handshake (연결 설정)

#### 목적
TCP 연결을 설정하여 양쪽이 데이터 전송 준비가 되었음을 보장

#### 과정

```
클라이언트                                    서버
(CLOSED)                                  (LISTEN)
    |                                         |
    |  1. SYN (seq=x)                         |
    |  [SYN_SENT]                             |
    |=======================================> |
    |                              [SYN_RECEIVED]
    |                                         |
    |  2. SYN+ACK (seq=y, ack=x+1)           |
    | <=======================================|
[ESTABLISHED]                                 |
    |                                         |
    |  3. ACK (ack=y+1)                      |
    |=======================================> |
    |                              [ESTABLISHED]
    |                                         |
    |  ===== 데이터 전송 시작 =====          |
```

#### 단계별 설명

**1단계: SYN**
- 클라이언트가 서버에 연결 요청
- 클라이언트의 ISN(Initial Sequence Number) 전송
- 상태: CLOSED → SYN_SENT

**2단계: SYN + ACK**
- 서버가 요청을 수락하고 클라이언트에게도 연결 요청
- 서버의 ISN 전송 및 클라이언트 SYN 확인
- 상태: LISTEN → SYN_RECEIVED

**3단계: ACK**
- 클라이언트가 서버의 응답 확인
- 연결 설정 완료
- 상태: SYN_SENT → ESTABLISHED (클라이언트)
- 상태: SYN_RECEIVED → ESTABLISHED (서버)

### 4-way Handshake (연결 종료)

#### 목적
TCP 연결을 안전하게 종료

#### 과정

```
클라이언트                                    서버
(ESTABLISHED)                          (ESTABLISHED)
    |                                         |
    |  1. FIN (seq=x)                         |
    |  [FIN_WAIT_1]                           |
    |=======================================> |
    |                              [CLOSE_WAIT]
    |                                         |
    |  2. ACK (ack=x+1)                      |
    | <=======================================|
[FIN_WAIT_2]                                  |
    |                                         |
    |  3. FIN (seq=y)                        |
    | <=======================================|
    |  [TIME_WAIT]                 [LAST_ACK]|
    |                                         |
    |  4. ACK (ack=y+1)                      |
    |=======================================> |
    |                                [CLOSED] |
[TIME_WAIT]                                   |
    |                                         |
(2MSL 대기)                                   |
    |                                         |
[CLOSED]                                      |
```

#### 단계별 설명

**1단계: FIN (클라이언트 → 서버)**
- 클라이언트가 연결 종료 요청
- 상태: ESTABLISHED → FIN_WAIT_1

**2단계: ACK (서버 → 클라이언트)**
- 서버가 종료 요청 확인
- 상태: ESTABLISHED → CLOSE_WAIT (서버)
- 상태: FIN_WAIT_1 → FIN_WAIT_2 (클라이언트)

**3단계: FIN (서버 → 클라이언트)**
- 서버가 연결 종료 준비 완료
- 상태: CLOSE_WAIT → LAST_ACK (서버)

**4단계: ACK (클라이언트 → 서버)**
- 클라이언트가 최종 확인
- 상태: FIN_WAIT_2 → TIME_WAIT (클라이언트)
- 상태: LAST_ACK → CLOSED (서버)

#### TIME_WAIT 상태

**목적**
- 지연 패킷 처리
- 안전한 연결 종료 보장

**대기 시간**
- 2MSL (Maximum Segment Lifetime)
- 일반적으로 30초~2분

### 관련 질문

**Q1. 3-way와 4-way의 단계 차이 이유는?**
- 연결 종료 시 서버가 아직 전송할 데이터가 남아있을 수 있음
- ACK와 FIN을 따로 전송하여 안전하게 종료

**Q2. ISN을 난수로 설정하는 이유는?**
- 이전 연결의 패킷과 구분하기 위함
- 보안상 연결 예측 공격 방지

**Q3. TIME_WAIT이 필요한 이유는?**
- 지연 도착 패킷 처리
- 최종 ACK 유실 시 재전송 대응

---

## HTTP와 HTTPS

### HTTP (HyperText Transfer Protocol)

#### 개념
웹 상에서 클라이언트와 서버 간에 데이터를 주고받기 위한 프로토콜입니다.

#### 특징
- **비연결성 (Connectionless)**: 요청/응답 후 연결 종료
- **무상태 (Stateless)**: 상태 정보를 저장하지 않음
- **TCP 기반**: 신뢰성 있는 전송
- **포트 번호**: 80번 포트 사용

#### HTTP 메서드

```java
// HTTP 메서드 예제
public class HttpMethods {
    // GET: 리소스 조회
    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    // POST: 리소스 생성
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
    
    // PUT: 리소스 전체 수정
    @PutMapping("/users/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user);
    }
    
    // PATCH: 리소스 부분 수정
    @PatchMapping("/users/{id}")
    public User patchUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
        return userService.patch(id, updates);
    }
    
    // DELETE: 리소스 삭제
    @DeleteMapping("/users/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

#### HTTP 상태 코드

**1xx (정보)**
- 100 Continue: 계속 진행

**2xx (성공)**
- 200 OK: 요청 성공
- 201 Created: 생성 성공
- 204 No Content: 성공, 응답 본문 없음

**3xx (리다이렉션)**
- 301 Moved Permanently: 영구 이동
- 302 Found: 임시 이동
- 304 Not Modified: 수정되지 않음

**4xx (클라이언트 오류)**
- 400 Bad Request: 잘못된 요청
- 401 Unauthorized: 인증 필요
- 403 Forbidden: 권한 없음
- 404 Not Found: 리소스 없음

**5xx (서버 오류)**
- 500 Internal Server Error: 서버 오류
- 502 Bad Gateway: 게이트웨이 오류
- 503 Service Unavailable: 서비스 불가

### HTTPS (HTTP Secure)

#### 개념
HTTP에 SSL/TLS 프로토콜을 결합하여 보안을 강화한 프로토콜입니다.

#### 특징
- **암호화**: 데이터를 암호화하여 전송
- **인증**: 서버 신원 확인 (SSL 인증서)
- **무결성**: 데이터 변조 방지
- **포트 번호**: 443번 포트 사용

#### SSL/TLS 동작 방식

**대칭키 + 비대칭키 하이브리드**
- 대칭키: 데이터 암호화 (빠름)
- 비대칭키: 대칭키 전달 (안전함)

#### HTTPS 통신 과정

```
클라이언트                                    서버
    |                                         |
    |  1. Client Hello                        |
    |  (지원 암호화 방식, 랜덤 데이터)         |
    |=======================================> |
    |                                         |
    |  2. Server Hello                        |
    |  (선택된 암호화 방식, 랜덤 데이터)       |
    |  + SSL 인증서                            |
    | <=======================================|
    |                                         |
    |  3. 인증서 검증                          |
    |  Pre-Master Secret 생성                 |
    |  (공개키로 암호화)                       |
    |=======================================> |
    |                                         |
    |  4. Session Key 생성 (양쪽)             |
    |                                         |
    |  5. Finished (암호화 완료)               |
    | <=====================================> |
    |                                         |
    |  ===== 암호화된 데이터 전송 =====          |
```

### HTTP vs HTTPS

| 구분 | HTTP | HTTPS |
|------|------|-------|
| 보안 | 평문 전송 | 암호화 전송 |
| 포트 | 80 | 443 |
| 속도 | 빠름 | 상대적으로 느림 |
| 인증서 | 불필요 | SSL 인증서 필요 |
| SEO | 불리 | 유리 |
| 비용 | 무료 | 인증서 비용 |

---

## HTTP 요청/응답 헤더

### HTTP 요청 메시지 구조

```
POST /api/users HTTP/1.1              ← 요청 라인
Host: www.example.com                 ← 헤더
Content-Type: application/json
Content-Length: 48
Authorization: Bearer token123
                                      ← 빈 줄
{"name": "John", "age": 30}           ← 본문
```

### 주요 요청 헤더 (Request Headers)

#### 일반 헤더

| 헤더 | 설명 | 예시 |
|------|------|------|
| Host | 요청하는 호스트 정보 (필수) | `Host: www.example.com` |
| User-Agent | 클라이언트 소프트웨어 정보 | `User-Agent: Mozilla/5.0` |
| Accept | 클라이언트가 원하는 미디어 타입 | `Accept: application/json` |
| Accept-Language | 선호 언어 | `Accept-Language: ko-KR` |
| Accept-Encoding | 선호 인코딩 방식 | `Accept-Encoding: gzip` |
| Connection | 연결 옵션 | `Connection: keep-alive` |
| Cookie | 쿠키 정보 | `Cookie: sessionId=abc123` |
| Referer | 이전 페이지 주소 | `Referer: http://example.com` |
| Authorization | 인증 정보 | `Authorization: Bearer token` |
| Origin | 요청 출처 (CORS) | `Origin: https://example.com` |

### 주요 응답 헤더 (Response Headers)

| 헤더 | 설명 | 예시 |
|------|------|------|
| Content-Type | 응답 데이터 타입 | `Content-Type: application/json` |
| Content-Length | 응답 본문 크기 | `Content-Length: 1234` |
| Content-Encoding | 압축 방식 | `Content-Encoding: gzip` |
| Set-Cookie | 쿠키 설정 | `Set-Cookie: id=a3fWa; Expires=...` |
| Server | 서버 정보 | `Server: Apache/2.4.1` |
| Date | 응답 생성 시간 | `Date: Mon, 01 Dec 2025` |
| Location | 리다이렉트 위치 | `Location: /new-page` |
| Cache-Control | 캐싱 정책 | `Cache-Control: no-cache` |
| Access-Control-Allow-Origin | CORS 허용 출처 | `Access-Control-Allow-Origin: *` |

### CORS 관련 헤더

```java
// Spring Boot CORS 설정 예제
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("https://example.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }
}
```

---

## HTTP와 HTTPS 동작 과정

### HTTP 동작 과정

```
1. URL 입력
   ↓
2. DNS 조회 (도메인 → IP 주소 변환)
   ↓
3. TCP 연결 (3-way handshake)
   ↓
4. HTTP 요청 전송
   ↓
5. 서버 처리
   ↓
6. HTTP 응답 전송
   ↓
7. TCP 연결 종료 (4-way handshake)
   ↓
8. 브라우저 렌더링
```

### HTTPS (SSL/TLS) Handshake 상세

```java
// Java HTTPS 연결 예제
import javax.net.ssl.HttpsURLConnection;
import java.net.URL;

public class HttpsExample {
    public static void main(String[] args) throws Exception {
        // HTTPS URL 연결
        URL url = new URL("https://api.example.com/data");
        HttpsURLConnection conn = (HttpsURLConnection) url.openConnection();
        
        // 요청 설정
        conn.setRequestMethod("GET");
        conn.setRequestProperty("Content-Type", "application/json");
        
        // 응답 받기
        int responseCode = conn.getResponseCode();
        System.out.println("Response Code: " + responseCode);
        
        // SSL 인증서 정보
        System.out.println("Cipher Suite: " + conn.getCipherSuite());
        System.out.println("Certificates: " + 
            conn.getServerCertificates()[0].toString());
    }
}
```

### SSL/TLS Handshake 단계

**1. Client Hello**
- 클라이언트 지원 SSL/TLS 버전
- 암호화 알고리즘 목록
- 랜덤 데이터 (Client Random)

**2. Server Hello**
- 선택된 SSL/TLS 버전
- 선택된 암호화 알고리즘
- 랜덤 데이터 (Server Random)
- SSL 인증서

**3. 인증서 검증**
- CA(Certificate Authority) 확인
- 인증서 유효기간 확인
- 도메인 일치 확인

**4. Pre-Master Secret 전송**
- 클라이언트가 생성
- 서버의 공개키로 암호화
- 서버는 개인키로 복호화

**5. Session Key 생성**
- Client Random + Server Random + Pre-Master Secret
- 양쪽이 동일한 Session Key 생성

**6. 암호화 통신 시작**
- Session Key로 대칭키 암호화
- 데이터 송수신

---

## CORS

### 개념
CORS(Cross-Origin Resource Sharing)는 웹 브라우저가 보안상의 이유로 다른 출처(Origin)의 리소스 접근을 제한하는 정책을 완화하는 메커니즘입니다.

### Origin (출처)

```
https://www.example.com:443/api/users
└─┬─┘   └────┬────────┘ └┬┘ └───┬───┘
Protocol   Domain      Port   Path

Origin = Protocol + Domain + Port
```

### SOP (Same-Origin Policy)

**동일 출처 정책**
- 기본적으로 같은 출처의 리소스만 접근 가능
- 보안을 위한 브라우저의 기본 정책

**다른 출처의 예**
```
현재 페이지: https://www.example.com

https://api.example.com      - 도메인 다름 (X)
http://www.example.com       - 프로토콜 다름 (X)
https://www.example.com:8080 - 포트 다름 (X)
https://www.example.com/api  - 경로만 다름 (O)
```

### CORS 동작 과정

#### Simple Request
```
클라이언트                                    서버
    |                                         |
    |  GET /api/data                          |
    |  Origin: https://client.com             |
    |=======================================> |
    |                                         |
    |  Access-Control-Allow-Origin: *         |
    |  (또는 https://client.com)              |
    | <=======================================|
```

#### Preflight Request

```
클라이언트                                    서버
    |                                         |
    |  OPTIONS /api/data (Preflight)          |
    |  Origin: https://client.com             |
    |  Access-Control-Request-Method: POST    |
    |=======================================> |
    |                                         |
    |  Access-Control-Allow-Origin: *         |
    |  Access-Control-Allow-Methods: POST     |
    | <=======================================|
    |                                         |
    |  POST /api/data (실제 요청)             |
    |=======================================> |
    |                                         |
    | <=======================================|
```

### CORS 헤더

**요청 헤더**
- `Origin`: 요청 출처
- `Access-Control-Request-Method`: 사용할 HTTP 메서드
- `Access-Control-Request-Headers`: 사용할 헤더

**응답 헤더**
- `Access-Control-Allow-Origin`: 허용할 출처
- `Access-Control-Allow-Methods`: 허용할 메서드
- `Access-Control-Allow-Headers`: 허용할 헤더
- `Access-Control-Max-Age`: Preflight 캐싱 시간
- `Access-Control-Allow-Credentials`: 인증 정보 포함 여부

### CORS 설정 예제

```java
// Spring Boot - WebMvcConfigurer 사용
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("https://client.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }
}

// @CrossOrigin 어노테이션 사용
@RestController
@CrossOrigin(origins = "https://client.com")
public class ApiController {
    
    @GetMapping("/api/data")
    public ResponseEntity<Data> getData() {
        return ResponseEntity.ok(data);
    }
}

// 특정 메서드에만 적용
@CrossOrigin(origins = "https://client.com", methods = RequestMethod.POST)
@PostMapping("/api/data")
public ResponseEntity<Data> createData(@RequestBody Data data) {
    return ResponseEntity.ok(data);
}
```

---

## GET과 POST

### GET 메서드

#### 특징
- 리소스 조회 목적
- URL에 쿼리 파라미터로 데이터 전송
- 브라우저 히스토리에 기록
- 북마크 가능
- 캐싱 가능
- 길이 제한 있음 (브라우저마다 다름, 보통 2048자)
- 멱등성(Idempotent) 보장
- 안전(Safe) 메서드

#### 사용 예제

```java
// Spring Boot GET 요청
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // 쿼리 파라미터
    @GetMapping
    public List<User> getUsers(@RequestParam(required = false) String name,
                                @RequestParam(defaultValue = "0") int page) {
        return userService.findUsers(name, page);
    }
    
    // 경로 변수
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    // 여러 파라미터
    @GetMapping("/search")
    public List<User> searchUsers(@RequestParam String keyword,
                                   @RequestParam(defaultValue = "10") int size) {
        return userService.search(keyword, size);
    }
}

// 요청 예시
// GET /api/users?name=John&page=1
// GET /api/users/123
// GET /api/users/search?keyword=developer&size=20
```

### POST 메서드

#### 특징
- 리소스 생성 목적
- HTTP 메시지 Body에 데이터 전송
- 브라우저 히스토리에 기록 안 됨
- 북마크 불가
- 캐싱 불가
- 길이 제한 없음
- 멱등성 보장 안 함
- 안전하지 않은 메서드

#### 사용 예제

```java
// Spring Boot POST 요청
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // JSON 데이터 받기
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity
                .created(URI.create("/api/users/" + savedUser.getId()))
                .body(savedUser);
    }
    
    // Form 데이터 받기
    @PostMapping("/form")
    public User createUserFromForm(@RequestParam String name,
                                   @RequestParam String email,
                                   @RequestParam int age) {
        User user = new User(name, email, age);
        return userService.save(user);
    }
    
    // 파일 업로드
    @PostMapping("/upload")
    public ResponseEntity<String> uploadFile(
            @RequestParam("file") MultipartFile file) {
        String fileName = fileService.store(file);
        return ResponseEntity.ok(fileName);
    }
}
```

### GET vs POST 비교

| 특성 | GET | POST |
|------|-----|------|
| 용도 | 조회 | 생성 |
| 데이터 위치 | URL | Body |
| 캐싱 | 가능 | 불가능 |
| 북마크 | 가능 | 불가능 |
| 길이 제한 | 있음 | 없음 |
| 보안 | 취약 | 상대적으로 안전 |
| 멱등성 | O | X |
| 브라우저 히스토리 | 저장 | 저장 안 됨 |
| 뒤로가기 | 안전 | 재전송 경고 |

### 멱등성 (Idempotent)

**정의**: 동일한 요청을 여러 번 해도 결과가 같음

```
GET /api/users/123    - 멱등 (O) - 조회는 반복해도 같은 결과
POST /api/users       - 멱등 (X) - 매번 새로운 리소스 생성
PUT /api/users/123    - 멱등 (O) - 전체 수정, 결과 동일
PATCH /api/users/123  - 멱등 (X) - 부분 수정, 구현에 따라 다름
DELETE /api/users/123 - 멱등 (O) - 삭제는 반복해도 결과 같음
```

---

## 쿠키와 세션

### HTTP의 특성

**1. 비연결성 (Connectionless)**
- 요청/응답 후 연결 종료
- 서버 리소스 절약

**2. 무상태 (Stateless)**
- 이전 요청을 기억하지 않음
- 각 요청은 독립적

**문제점**: 로그인 상태 유지 불가능
**해결책**: 쿠키와 세션

### 쿠키 (Cookie)

#### 개념
클라이언트(브라우저)에 저장되는 작은 데이터 파일

#### 구성 요소
- **Name**: 쿠키 이름
- **Value**: 쿠키 값
- **Domain**: 쿠키가 전송될 도메인
- **Path**: 쿠키가 전송될 경로
- **Expires/Max-Age**: 만료 시간
- **Secure**: HTTPS에서만 전송
- **HttpOnly**: JavaScript 접근 차단
- **SameSite**: CSRF 방지

#### 동작 과정

```
클라이언트                                    서버
    |                                         |
    |  1. 로그인 요청                          |
    |=======================================> |
    |                                         |
    |  2. Set-Cookie: sessionId=abc123        |
    | <=======================================|
    |  (쿠키 저장)                             |
    |                                         |
    |  3. 다음 요청                            |
    |  Cookie: sessionId=abc123               |
    |=======================================> |
    |                                         |
    |  4. 인증된 응답                          |
    | <=======================================|
```

#### 쿠키 설정 예제

```java
// Spring Boot - 쿠키 설정
@RestController
public class CookieController {
    
    @PostMapping("/login")
    public ResponseEntity<String> login(
            @RequestBody LoginRequest request,
            HttpServletResponse response) {
        
        // 로그인 처리
        User user = authService.login(request);
        
        // 쿠키 생성
        Cookie cookie = new Cookie("userId", user.getId().toString());
        cookie.setMaxAge(7 * 24 * 60 * 60); // 7일
        cookie.setPath("/");
        cookie.setHttpOnly(true);          // XSS 방지
        cookie.setSecure(true);            // HTTPS only
        
        response.addCookie(cookie);
        return ResponseEntity.ok("Login successful");
    }
    
    @GetMapping("/profile")
    public User getProfile(@CookieValue("userId") String userId) {
        return userService.findById(Long.parseLong(userId));
    }
    
    @PostMapping("/logout")
    public ResponseEntity<String> logout(HttpServletResponse response) {
        Cookie cookie = new Cookie("userId", null);
        cookie.setMaxAge(0);
        cookie.setPath("/");
        response.addCookie(cookie);
        return ResponseEntity.ok("Logout successful");
    }
}
```

### 세션 (Session)

#### 개념
서버에 저장되는 상태 정보

#### 동작 과정

```
클라이언트                                    서버
    |                                         |
    |  1. 로그인 요청                          |
    |=======================================> |
    |                            [세션 생성]   |
    |                            SessionID    |
    |  2. Set-Cookie: JSESSIONID=xyz789       |
    | <=======================================|
    |  (SessionID 저장)                       |
    |                                         |
    |  3. 다음 요청                            |
    |  Cookie: JSESSIONID=xyz789              |
    |=======================================> |
    |                    [세션에서 사용자 확인]|
    |  4. 인증된 응답                          |
    | <=======================================|
```

#### 세션 사용 예제

```java
// Spring Boot - 세션 사용
@RestController
public class SessionController {
    
    @PostMapping("/login")
    public ResponseEntity<String> login(
            @RequestBody LoginRequest request,
            HttpSession session) {
        
        // 로그인 처리
        User user = authService.login(request);
        
        // 세션에 사용자 정보 저장
        session.setAttribute("user", user);
        session.setMaxInactiveInterval(1800); // 30분
        
        return ResponseEntity.ok("Login successful");
    }
    
    @GetMapping("/profile")
    public User getProfile(HttpSession session) {
        User user = (User) session.getAttribute("user");
        if (user == null) {
            throw new UnauthorizedException("Not logged in");
        }
        return user;
    }
    
    @PostMapping("/logout")
    public ResponseEntity<String> logout(HttpSession session) {
        session.invalidate(); // 세션 무효화
        return ResponseEntity.ok("Logout successful");
    }
}

// 세션 설정 (application.yml)
// server:
//   servlet:
//     session:
//       timeout: 30m
//       cookie:
//         name: JSESSIONID
//         http-only: true
//         secure: true
```

### 쿠키 vs 세션

| 특성 | 쿠키 | 세션 |
|------|------|------|
| 저장 위치 | 클라이언트 | 서버 |
| 보안 | 취약 | 안전 |
| 속도 | 빠름 | 느림 |
| 용량 | 제한적 (4KB) | 제한 없음 |
| 만료 시점 | 설정 가능 | 브라우저 종료 시 |
| 서버 부하 | 없음 | 있음 |

### JWT (JSON Web Token)

최근에는 쿠키/세션 대신 JWT를 많이 사용

```java
// JWT 인증 예제
@RestController
public class JwtController {
    
    @Autowired
    private JwtTokenProvider jwtTokenProvider;
    
    @PostMapping("/login")
    public ResponseEntity<TokenResponse> login(
            @RequestBody LoginRequest request) {
        
        User user = authService.login(request);
        
        // JWT 토큰 생성
        String accessToken = jwtTokenProvider.createAccessToken(user.getId());
        String refreshToken = jwtTokenProvider.createRefreshToken(user.getId());
        
        return ResponseEntity.ok(new TokenResponse(accessToken, refreshToken));
    }
    
    @GetMapping("/profile")
    public User getProfile(@RequestHeader("Authorization") String token) {
        // Bearer 토큰에서 실제 토큰 추출
        String jwtToken = token.substring(7);
        
        // 토큰 검증 및 사용자 ID 추출
        Long userId = jwtTokenProvider.getUserId(jwtToken);
        return userService.findById(userId);
    }
}
```

---

## DNS

### 개념
DNS(Domain Name System)는 사람이 읽을 수 있는 도메인 이름을 컴퓨터가 이해할 수 있는 IP 주소로 변환하는 시스템입니다.

### DNS 구조

```
.                           ← Root DNS
├── com                     ← TLD (Top Level Domain)
│   ├── google.com          ← SLD (Second Level Domain)
│   │   ├── www             ← Subdomain
│   │   └── mail
│   └── facebook.com
├── org
└── net
```

### DNS 조회 과정

```
1. 브라우저 캐시 확인
   ↓
2. OS 캐시 확인 (/etc/hosts)
   ↓
3. 로컬 DNS 서버 (ISP)
   ↓
4. Root DNS 서버
   ↓
5. TLD DNS 서버 (.com)
   ↓
6. Authoritative DNS 서버 (example.com)
   ↓
7. IP 주소 반환
```

### DNS 레코드 타입

| 타입 | 설명 | 예시 |
|------|------|------|
| A | IPv4 주소 | `example.com → 93.184.216.34` |
| AAAA | IPv6 주소 | `example.com → 2606:2800:220:1:...` |
| CNAME | 별칭 | `www.example.com → example.com` |
| MX | 메일 서버 | `example.com → mail.example.com` |
| NS | 네임 서버 | `example.com → ns1.example.com` |
| TXT | 텍스트 정보 | SPF, DKIM 레코드 |

### DNS 조회 예제

```java
// Java DNS 조회
import java.net.InetAddress;

public class DnsLookup {
    public static void main(String[] args) {
        try {
            // 도메인으로 IP 조회
            InetAddress address = InetAddress.getByName("www.google.com");
            System.out.println("Host: " + address.getHostName());
            System.out.println("IP: " + address.getHostAddress());
            
            // 모든 IP 주소 조회
            InetAddress[] addresses = InetAddress.getAllByName("www.google.com");
            for (InetAddress addr : addresses) {
                System.out.println(addr.getHostAddress());
            }
            
            // 로컬 호스트 정보
            InetAddress localhost = InetAddress.getLocalHost();
            System.out.println("Local Host: " + localhost.getHostName());
            System.out.println("Local IP: " + localhost.getHostAddress());
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

## REST와 RESTful

### REST (Representational State Transfer)

#### 개념
HTTP URI를 통해 자원을 명시하고, HTTP Method를 통해 해당 자원에 대한 CRUD 연산을 적용하는 아키텍처 스타일

#### REST 구성 요소

**1. 자원 (Resource) - URI**
```
/users          - 사용자 목록
/users/123      - 특정 사용자
/users/123/posts - 특정 사용자의 게시글
```

**2. 행위 (Verb) - HTTP Method**
- GET: 조회
- POST: 생성
- PUT: 전체 수정
- PATCH: 부분 수정
- DELETE: 삭제

**3. 표현 (Representation)**
- JSON, XML, HTML 등

#### REST 제약 조건

1. **Client-Server**: 클라이언트와 서버 분리
2. **Stateless**: 무상태성
3. **Cacheable**: 캐시 가능
4. **Layered System**: 계층화된 시스템
5. **Uniform Interface**: 일관된 인터페이스
6. **Code-On-Demand** (선택적)

### RESTful API 설계 원칙

#### 1. URI는 리소스를 표현

```
# Good
GET /users
GET /users/123
POST /users
DELETE /users/123

# Bad
GET /getUsers
POST /createUser
GET /deleteUser/123
```

#### 2. 리소스에 대한 행위는 HTTP Method로 표현

```
# Good
DELETE /users/123

# Bad
GET /users/delete/123
POST /users/123/delete
```

#### 3. 슬래시(/)는 계층 관계 표현

```
GET /users/123/posts          # 사용자 123의 게시글 목록
GET /users/123/posts/456      # 사용자 123의 게시글 456
```

#### 4. URI 마지막에 슬래시 사용하지 않음

```
# Good
GET /users/123

# Bad
GET /users/123/
```

#### 5. 하이픈(-)으로 가독성 향상

```
# Good
GET /user-management/users

# Bad
GET /user_management/users
```

#### 6. 소문자 사용

```
# Good
GET /users/profile-image

# Bad
GET /Users/Profile-Image
```

#### 7. 파일 확장자는 URI에 포함하지 않음

```
# Good
GET /users/123/photo
Accept: image/jpg

# Bad
GET /users/123/photo.jpg
```

### REST API 구현 예제

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserRestController {
    
    @Autowired
    private UserService userService;
    
    // 사용자 목록 조회
    @GetMapping
    public ResponseEntity<List<User>> getUsers(
            @RequestParam(required = false) String name,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size) {
        
        Pageable pageable = PageRequest.of(page, size);
        List<User> users = userService.findAll(name, pageable);
        return ResponseEntity.ok(users);
    }
    
    // 특정 사용자 조회
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("User not found"));
        return ResponseEntity.ok(user);
    }
    
    // 사용자 생성
    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody UserRequest request) {
        User user = userService.create(request);
        URI location = URI.create("/api/v1/users/" + user.getId());
        return ResponseEntity.created(location).body(user);
    }
    
    // 사용자 전체 수정
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(
            @PathVariable Long id,
            @Valid @RequestBody UserRequest request) {
        User user = userService.update(id, request);
        return ResponseEntity.ok(user);
    }
    
    // 사용자 부분 수정
    @PatchMapping("/{id}")
    public ResponseEntity<User> patchUser(
            @PathVariable Long id,
            @RequestBody Map<String, Object> updates) {
        User user = userService.patch(id, updates);
        return ResponseEntity.ok(user);
    }
    
    // 사용자 삭제
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
    
    // 사용자의 게시글 목록 조회
    @GetMapping("/{userId}/posts")
    public ResponseEntity<List<Post>> getUserPosts(@PathVariable Long userId) {
        List<Post> posts = postService.findByUserId(userId);
        return ResponseEntity.ok(posts);
    }
}

// DTO 예제
public class UserRequest {
    @NotBlank(message = "Name is required")
    private String name;
    
    @Email(message = "Invalid email format")
    private String email;
    
    @Min(value = 0, message = "Age must be positive")
    private int age;
    
    // getters and setters
}
```

### HTTP 상태 코드 사용

```java
// 상태 코드별 응답 예제
@RestController
@RequestMapping("/api")
public class ResponseExampleController {
    
    // 200 OK - 성공
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
    
    // 201 Created - 생성 성공
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody UserRequest request) {
        User user = userService.create(request);
        return ResponseEntity
                .status(HttpStatus.CREATED)
                .body(user);
    }
    
    // 204 No Content - 성공, 응답 본문 없음
    @DeleteMapping("/users/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
    
    // 400 Bad Request - 잘못된 요청
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationError(
            MethodArgumentNotValidException ex) {
        ErrorResponse error = new ErrorResponse("Validation failed", 
                ex.getBindingResult().getAllErrors());
        return ResponseEntity.badRequest().body(error);
    }
    
    // 404 Not Found - 리소스 없음
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(
            ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    // 500 Internal Server Error - 서버 오류
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleServerError(Exception ex) {
        ErrorResponse error = new ErrorResponse("Internal server error");
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body(error);
    }
}
```

### RESTful의 장단점

**장점**
- HTTP 표준 프로토콜을 따르므로 별도 인프라 불필요
- HTTP를 지원하는 모든 플랫폼에서 사용 가능
- REST API 메시지만 보고도 의도 파악 가능
- 서버와 클라이언트 역할 명확히 분리

**단점**
- 표준이 없어 정의가 필요
- HTTP Method 형태가 제한적
- 구형 브라우저 지원 부족

---

## 소켓

### 개념
소켓(Socket)은 네트워크 통신의 끝점(Endpoint)으로, 프로세스 간 통신을 위한 인터페이스입니다.

### 소켓의 구성
```
IP Address + Port Number = Socket
예: 192.168.0.1:8080
```

### 소켓 통신 과정

```
서버                                        클라이언트
socket()                                   socket()
   ↓                                          ↓
bind()                                    (자동 할당)
   ↓                                          ↓
listen()                                      ↓
   ↓                                          ↓
accept() ←──────── connect() ──────────── connect()
   ↓                                          ↓
read/write ←────── 데이터 송수신 ──────→ read/write
   ↓                                          ↓
close() ←────────── close() ───────────── close()
```

### TCP 소켓 예제

```java
// TCP Server
import java.io.*;
import java.net.*;

public class TcpServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            System.out.println("Server started on port 8080");
            
            while (true) {
                // 클라이언트 연결 대기
                Socket clientSocket = serverSocket.accept();
                System.out.println("Client connected: " + 
                    clientSocket.getInetAddress());
                
                // 새 스레드에서 클라이언트 처리
                new Thread(() -> handleClient(clientSocket)).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    private static void handleClient(Socket clientSocket) {
        try (BufferedReader in = new BufferedReader(
                new InputStreamReader(clientSocket.getInputStream()));
             PrintWriter out = new PrintWriter(
                clientSocket.getOutputStream(), true)) {
            
            String message;
            while ((message = in.readLine()) != null) {
                System.out.println("Received: " + message);
                out.println("Echo: " + message);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                clientSocket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

// TCP Client
public class TcpClient {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 8080);
             BufferedReader in = new BufferedReader(
                new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(
                socket.getOutputStream(), true);
             BufferedReader stdIn = new BufferedReader(
                new InputStreamReader(System.in))) {
            
            System.out.println("Connected to server");
            
            String userInput;
            while ((userInput = stdIn.readLine()) != null) {
                out.println(userInput);
                String response = in.readLine();
                System.out.println("Server: " + response);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### UDP 소켓 예제

```java
// UDP Server
import java.net.*;

public class UdpServer {
    public static void main(String[] args) {
        try (DatagramSocket socket = new DatagramSocket(9090)) {
            System.out.println("UDP Server started on port 9090");
            
            byte[] buffer = new byte[1024];
            
            while (true) {
                DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
                socket.receive(packet);
                
                String message = new String(packet.getData(), 0, packet.getLength());
                System.out.println("Received: " + message);
                
                // Echo back
                DatagramPacket response = new DatagramPacket(
                    packet.getData(), packet.getLength(),
                    packet.getAddress(), packet.getPort());
                socket.send(response);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

// UDP Client
public class UdpClient {
    public static void main(String[] args) {
        try (DatagramSocket socket = new DatagramSocket()) {
            InetAddress address = InetAddress.getByName("localhost");
            
            String message = "Hello UDP Server";
            byte[] buffer = message.getBytes();
            
            DatagramPacket packet = new DatagramPacket(
                buffer, buffer.length, address, 9090);
            socket.send(packet);
            
            // Receive response
            byte[] responseBuffer = new byte[1024];
            DatagramPacket response = new DatagramPacket(
                responseBuffer, responseBuffer.length);
            socket.receive(response);
            
            String responseMessage = new String(response.getData(), 
                0, response.getLength());
            System.out.println("Server: " + responseMessage);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

## Socket.io와 WebSocket

### WebSocket

#### 개념
웹 브라우저와 서버 간 양방향 실시간 통신을 위한 프로토콜입니다.

#### 특징
- **양방향 통신**: 클라이언트와 서버 모두 메시지 전송 가능
- **실시간 통신**: 지속적인 연결 유지
- **낮은 지연시간**: HTTP 폴링보다 효율적
- **표준 프로토콜**: HTML5 표준

#### WebSocket Handshake

```
클라이언트                                    서버
    |                                         |
    |  GET /chat HTTP/1.1                     |
    |  Upgrade: websocket                     |
    |  Connection: Upgrade                    |
    |  Sec-WebSocket-Key: x3JJHMbDL1E...     |
    |=======================================> |
    |                                         |
    |  HTTP/1.1 101 Switching Protocols      |
    |  Upgrade: websocket                     |
    |  Connection: Upgrade                    |
    |  Sec-WebSocket-Accept: HSmrc0sM...     |
    | <=======================================|
    |                                         |
    |  ===== WebSocket 연결 수립 =====       |
```

#### WebSocket 예제 (Java)

```java
// Spring Boot WebSocket 설정
@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer {
    
    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(chatHandler(), "/chat")
                .setAllowedOrigins("*");
    }
    
    @Bean
    public WebSocketHandler chatHandler() {
        return new ChatHandler();
    }
}

// WebSocket Handler
public class ChatHandler extends TextWebSocketHandler {
    
    private static final List<WebSocketSession> sessions = 
        new CopyOnWriteArrayList<>();
    
    @Override
    public void afterConnectionEstablished(WebSocketSession session) {
        sessions.add(session);
        System.out.println("New connection: " + session.getId());
    }
    
    @Override
    protected void handleTextMessage(WebSocketSession session, 
                                     TextMessage message) throws Exception {
        String payload = message.getPayload();
        System.out.println("Received: " + payload);
        
        // 모든 클라이언트에게 브로드캐스트
        for (WebSocketSession s : sessions) {
            if (s.isOpen()) {
                s.sendMessage(new TextMessage(payload));
            }
        }
    }
    
    @Override
    public void afterConnectionClosed(WebSocketSession session, 
                                     CloseStatus status) {
        sessions.remove(session);
        System.out.println("Connection closed: " + session.getId());
    }
}

// JavaScript 클라이언트
const socket = new WebSocket('ws://localhost:8080/chat');

socket.onopen = function(event) {
    console.log('Connected to WebSocket');
    socket.send('Hello Server!');
};

socket.onmessage = function(event) {
    console.log('Received: ' + event.data);
};

socket.onclose = function(event) {
    console.log('Disconnected from WebSocket');
};

socket.onerror = function(error) {
    console.error('WebSocket Error: ', error);
};
```

### Socket.io

#### 개념
WebSocket을 기반으로 한 실시간 통신 라이브러리로, WebSocket을 지원하지 않는 브라우저에서도 동작합니다.

#### 특징
- **자동 재연결**: 연결이 끊기면 자동으로 재연결
- **방(Room) 지원**: 특정 그룹에게만 메시지 전송
- **네임스페이스**: 하나의 연결에서 여러 채널 사용
- **Fallback**: WebSocket 불가 시 폴링으로 전환
- **이벤트 기반**: 커스텀 이벤트 사용 가능

#### Socket.io 예제

```java
// Socket.IO 서버 (Node.js)
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

io.on('connection', (socket) => {
    console.log('User connected:', socket.id);
    
    // 커스텀 이벤트 수신
    socket.on('chat message', (msg) => {
        console.log('Message:', msg);
        // 모든 클라이언트에게 브로드캐스트
        io.emit('chat message', msg);
    });
    
    // 특정 방에 참여
    socket.on('join room', (room) => {
        socket.join(room);
        io.to(room).emit('user joined', socket.id);
    });
    
    // 특정 방에 메시지 전송
    socket.on('room message', (data) => {
        io.to(data.room).emit('room message', data.message);
    });
    
    // 연결 종료
    socket.on('disconnect', () => {
        console.log('User disconnected:', socket.id);
    });
});

http.listen(3000, () => {
    console.log('Server listening on port 3000');
});
```

```javascript
// Socket.IO 클라이언트
<script src="/socket.io/socket.io.js"></script>
<script>
    const socket = io('http://localhost:3000');
    
    socket.on('connect', () => {
        console.log('Connected:', socket.id);
    });
    
    // 메시지 전송
    function sendMessage(message) {
        socket.emit('chat message', message);
    }
    
    // 메시지 수신
    socket.on('chat message', (msg) => {
        console.log('Received:', msg);
        displayMessage(msg);
    });
    
    // 방 참여
    function joinRoom(roomName) {
        socket.emit('join room', roomName);
    }
    
    // 방에 메시지 전송
    function sendRoomMessage(room, message) {
        socket.emit('room message', { room, message });
    }
    
    socket.on('disconnect', () => {
        console.log('Disconnected');
    });
</script>
```

### WebSocket vs Socket.io

| 특성 | WebSocket | Socket.io |
|------|-----------|-----------|
| 타입 | 프로토콜 | 라이브러리 |
| 브라우저 호환성 | 제한적 | 광범위 |
| 재연결 | 수동 | 자동 |
| 방/네임스페이스 | 없음 | 있음 |
| Fallback | 없음 | 폴링 지원 |
| 이벤트 시스템 | 기본 | 커스텀 가능 |

---

## Frame, Packet, Segment, Datagram

### PDU (Protocol Data Unit)

각 계층에서 처리되는 데이터의 단위를 PDU라고 합니다.

```
┌─────────────────────────────┐
│  Application                │  → Data/Message
├─────────────────────────────┤
│  Transport                  │  → Segment (TCP) / Datagram (UDP)
├─────────────────────────────┤
│  Network                    │  → Packet / Datagram
├─────────────────────────────┤
│  Data Link                  │  → Frame
├─────────────────────────────┤
│  Physical                   │  → Bit
└─────────────────────────────┘
```

### 각 PDU 상세 설명

#### 1. Bit (물리 계층)
- 0과 1의 비트 스트림
- 전기 신호, 광 신호, 전파로 전송

#### 2. Frame (데이터 링크 계층)
```
┌──────────┬──────────┬──────────┬─────────┐
│  Header  │   Data   │  Trailer │   FCS   │
└──────────┴──────────┴──────────┴─────────┘
  - MAC 주소
  - 오류 검출 코드 (CRC)
```

**구성**
- 출발지/목적지 MAC 주소
- 데이터
- FCS (Frame Check Sequence): 오류 검출

#### 3. Packet/Datagram (네트워크 계층)
```
┌────────────┬──────────────────┐
│ IP Header  │       Data       │
└────────────┴──────────────────┘
  - 출발지 IP
  - 목적지 IP
  - TTL, Protocol 등
```

**구성**
- 출발지/목적지 IP 주소
- TTL (Time To Live)
- 프로토콜 정보
- 데이터

#### 4. Segment (전송 계층 - TCP)
```
┌─────────────┬──────────────────┐
│ TCP Header  │       Data       │
└─────────────┴──────────────────┘
  - 출발지/목적지 Port
  - Sequence Number
  - ACK Number
  - Flags (SYN, ACK, FIN 등)
```

**구성**
- 출발지/목적지 포트 번호
- Sequence Number
- Acknowledgment Number
- 제어 플래그 (SYN, ACK, FIN, etc.)

#### 5. Datagram (전송 계층 - UDP)
```
┌─────────────┬──────────────────┐
│ UDP Header  │       Data       │
└─────────────┴──────────────────┘
  - 출발지/목적지 Port
  - Length
  - Checksum
```

**구성**
- 출발지/목적지 포트 번호
- 길이
- 체크섬

### 데이터 캡슐화 (Encapsulation)

송신 과정: 각 계층을 거치며 헤더 추가

```
Application:  [Data]
     ↓
Transport:    [TCP Header][Data]
     ↓
Network:      [IP Header][TCP Header][Data]
     ↓
Data Link:    [Frame Header][IP Header][TCP Header][Data][Frame Trailer]
     ↓
Physical:     010101010101...
```

### 역캡슐화 (Decapsulation)

수신 과정: 각 계층을 거치며 헤더 제거

```
Physical:     010101010101...
     ↓
Data Link:    [Frame Header][IP Header][TCP Header][Data][Frame Trailer]
     ↓ (헤더/트레일러 제거)
Network:      [IP Header][TCP Header][Data]
     ↓ (IP 헤더 제거)
Transport:    [TCP Header][Data]
     ↓ (TCP 헤더 제거)
Application:  [Data]
```

### 예제: 웹 페이지 요청

```java
// HTTP 요청 과정
public class DataTransmission {
    public static void main(String[] args) {
        // 1. Application Layer
        String httpRequest = "GET /index.html HTTP/1.1\r\n" +
                           "Host: www.example.com\r\n\r\n";
        
        // 2. Transport Layer (TCP)
        // - 출발지 포트: 50000 (임의)
        // - 목적지 포트: 80 (HTTP)
        // - Sequence Number: 1000
        // TCP Segment = [TCP Header] + [HTTP Request]
        
        // 3. Network Layer (IP)
        // - 출발지 IP: 192.168.0.10
        // - 목적지 IP: 93.184.216.34
        // IP Packet = [IP Header] + [TCP Segment]
        
        // 4. Data Link Layer (Ethernet)
        // - 출발지 MAC: AA:BB:CC:DD:EE:FF
        // - 목적지 MAC: 11:22:33:44:55:66
        // Ethernet Frame = [Ethernet Header] + [IP Packet] + [FCS]
        
        // 5. Physical Layer
        // Bits: 010101010101...
    }
}
```

---
### 공식 문서
- [RFC 793 - TCP](https://tools.ietf.org/html/rfc793)
- [RFC 9293 - TCP (Updated)](https://datatracker.ietf.org/doc/html/rfc9293)
- [RFC 768 - UDP](https://tools.ietf.org/html/rfc768)
- [RFC 2616 - HTTP/1.1](https://tools.ietf.org/html/rfc2616)
- [RFC 7540 - HTTP/2](https://tools.ietf.org/html/rfc7540)
- [ISO/IEC 7498-1 - OSI Model](https://www.iso.org/standard/20269.html)

### 추가 학습 자료
- GeeksforGeeks - Computer Networks
- Cloudflare Learning Center
- MDN Web Docs - HTTP
- AWS - Networking Fundamentals

