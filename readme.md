# 면접 질문 리스트

[TOC]

## DB

### ORM(Object Relational Mapping)

* 객체와 관계와의 설정
* 객체와 관계형 DB를 Mapping해준다.
* 객체와 테이블을 Mapping하기 때문에 SQL을 직접 날리는 것이 아니라 마치 자바에서 라이브러리 사용하듯이 사용하면 된다.
* 객체와 관계형 데이터에비스와의 설정을 자동으로 해준다.
* 관계형 데이터베이스의 데이터를 객체형 데이터처럼 사용할 수 있다.
* MyBatis, iBatis, JPA, Hibernate
* 장점
  * 객체 지향적인 코드로 인해 더 직관적이고 비즈니스 로직에 집중할 수 있게 도와준다.
  * 선언, 할당, 종료 같은 부수적인 코드가 줄어든다.
  * 재사용 및 유지 보수의 편리성이 증가한다.
  * DBMS에 대한 종속성이 줄어든다.
  * 절차적, 순차적 접근이 아닌 객체 지향적 접근으로 인해 생산성이 증가한다.
* 단점
  * 모든 기능을 ORM으로만 작성하기에는 쿼리가 복잡해지면 쓰기 어렵다.
  * 많은 수의 레코드를 잦은 빈도로 벌크 수행
  * ORM으로만 완벽하게 서비스를 구현하기가 어렵다.
  * 프로시저가 많은 시스템에선 ORM의 객체 지향적인 장점을 활용하기 어렵다.

### MySQL 엔진

* InnoDB
  * 기본값 스토리지 엔진
  * 트랜잭션 safe, 커밋과 롤백, 데이터 복구 기능을 제공한다.
  * row-level locking을 제공한다.
  * 데이터를 clusterd index에 저장하여 pk기반 query의 I/O 비용을 줄인다.
  * fk제약을 제공하여 데이터 무결성을 보장한다.
* MyISAM
  * 트랜잭션을 지원하지 않는다.
  * 테이블 단위의 locking을 제공한다.
  * 특정 세션이 테이블을 변경하는 동안 테이블 단위로 lock이 잡힌다.
* Archive
  * 로그 수집에 적합한 엔진
  * 데이터가 메모리 상에서 압축되고 압축된 상태로 디스크에 저장된다.
  * row-level locking이 가능하다.
  * 한번 insert된 데이터는 update/delete가 불가능하다.
  * 인덱스를 지원하지 않는다.
  * 거의 가공하지 않을 윈시 로그 데이터를 관리하는데 효율적이고, 테이블 파티셔닝도 지원한다.
  * 트랜잭션은 지원하지 않는다.

### 인덱스

* RDBMS에서 검색 속도를 높이기 위해 사용하는 기술
* 색인
* 해당 테이블의 컬럼을 색인화(따로 파일로 저장)하여 검색시 테이블 전체를 full scan 하는 것이 아니라 색인화 되어있는 index 파일을 검색하여 검색 속도를 빠르게 한다.
* B+ 트리로 저장된다.
* index로 설정한 컬럼 값이 변경되거나 추가되면, 인덱스 역시 변경된다. 따라서 적절하게 인덱스를 설정해야 한다.
* 데이터의 삽입, 삭제가 빈번한 경우 index의 성능이 떨어진다. 매번 B+트리를 수정해야 하기 때문이다.
* index가 데이터베이스 공간을 차지하기 때문에 추가적인 공간이 필요하다.(10%)
* index를 생성하는데 시간이 많이 소요될 수 있다.
* SELECT의 WHERE / JOIN시에만 인덱스가 사용되며 SELECT의 검색 속도를 빠르게 하는것이 목적이다.
* WHERE의 타겟이 되는 컬럼을 인덱스로 만드는 것이 좋다.
* 데이터 중복도가 높은 컬럼은 인덱스로 만들어도 효과가 없다.
* 외래키가 사용되는 컬럼은 인덱스를 생성해 주는 것이 좋다.
* 사용하지 않는 인덱스는 제거하는 것이 좋다.

### 정규화

* 관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화 하는 프로세스이다.
* 관계를 재구성하여 작고 잘 조직된 관계를 생성하는 것에 있다.
* 성능상의 이유로 비정규화 될 수 있다.

### 트랜잭션

* 작업의 수행 단위
* 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 약어 : ACID
* Atomicity : 원자성
* Consistency : 일관성
* Isolation : 고립성
* Durability : 지속성

### Partitioning(파티셔닝)

* 수직 파티셔닝
* 큰 테이블이나 인덱스를 관리하기 쉬운 단위로 분리하는 방법을 의미한다.
* 물리적인 파티셔닝으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.
* 큰 테이블을 제거하여 관리를 쉽게 해준다.
* 특정 DML과 쿼리의 성능을 향상시킨다.
* 주로 대용량 데이터 Write 환경에서 효율적이다.
* 많은 데이터 삽입이 있는 OLTP(Online Transaction Processing) 시스템에서 삽입 작업들을 분리된 파티션들로 분산시켜 경합을 줄인다.
* 테이블간 join에 대한 비용이 증가한다.
* 테이블과 인덱스를 별도 파티션 할 수는 없다. 테이블과 인덱스를 같이 파티셔닝해야 한다.

### Sharding(샤딩)

* 수평 파티셔닝
* 대용량의 데이터를 처리하기 위해 테이블을 수평 분할하여 데이터를 분산 저장하고 처리하는 것을 의미한다.
* 같은 타입의 데이터를 다수의 데이터베이스에 쪼개서 저장하는 것
* 샤딩 알고리즘이 매우 쉽게 일반화가 가능하기에 애플리케이션 레벨이나 데이터베이스 레벨이서 구현이 가능하다.

### Replication(리플리케이션)

* 두 개 이상의 DBMS 시스템을 Master/Slave로 나눠서 동일한 데이터를 저장하는 방식
* Master에는 데이터의 수정 사항을 반영만 하고 Replication을 하여 Salve에 실제 데이터를 복사한다.
* 삽입/수정/삭제는 Master가 담당하고 조회는 Slave가 담당해서 성능 향상 효과를 얻을 수 있다.
* 로그 기반 복제
  * Statement Based : SQL을 복사하여 진행
  * Row Based : SQL에 따라 변경된 Row만 기록하는 방식
  * Mixed : 기본적으로 statement Based로 진행하면서 필요에 따라 Row Based를 사용한다.

### MongoDB

* 도큐먼트 지향 데이터베이스 시스템이다.
* NoSQL
* 똑같은 조건으로 설계했을 시 RDBMS보다 빠르다.
* 트랜잭션을 지원하지 않는다.
* 도큐먼트의 집합을 컬렉션이라고 한다. = RDBMS에서 테이블
* Join을 지원하지 않는다.
* 데이터 갱신 및 입력이 바로 디스크에 쓰이지 않는다.(비동기식) 따라서 데이터가 유실될 가능성이 있다.
* 자체적으로 스케일 아웃을 지원한다.

### Redis

* 인 메모리 데이터베이스
* NoSQL
* 키-값 구조
* C 코드로 개발되었다.
* 데이터를 디스크에 저장하기 때문에 데이터 유실 위험이 memcached에 비해 적다.
* 클러스터링을 지원하지 않는다.
* 샤딩을 통해 확장한다.
* 데이터를 저장하는 방식
  * snapshotting
    * save
      * 순간적으로 redis를 정지시키고, 그 때의 스냅샷을 디스크에 저장한다.
    * blocking
      * 별도의 프로세스를 띄운후, 그 당시의 메모리 스냅샷을 디스크에 저장하며 redis는 정상적으로 작동한다.
  * AOF(Append On File)
    * redis의 모든 write/update 연산 자체를 모두 log 파일에 기록해 두었다가 서버가 재 시작될 때 기록된 로그를 통해 데이터를 복구한다.
    * 기본적으로 non-blocking call이다.

### Datasocurce

* 커넥션 풀의 커넥션을 관리하기 위한 객체이다.
* 데이터소스 객체를 통해서 필요한 커넥션을 획득, 반납한다.
* JNDI(Java Naming and Directory Interface) Server를 통해 이용된다.

### HikariCP

* 스프링 부트 2.0의 기본 jdbc cp이다.

## Java

### Java 8

* 인터페이스에 디폴트 메소드 추가
* 스트림 api
* 람다
* 함수형 프로그래밍

### 인터페이스

### 다형성

### 제네릭

### 뮤터블 & 이뮤터블

### OOP

### 리플렉션

### 접근 제어 지시자

## JVM(Java Virtual Machine)

- 자바 가상 머신
- 자바 애플리케션을 클래스 로더를 통해 읽어 들어 자바 API와 함께 실행하는 것이다.
- 자바 바이트 코드를 실행할 수 있는 주체이다.
- 인터프리터나 JIT컴파일 방식으로 다른 컴퓨터 위에서 자바 바이트 코드를 실행할 수 있도록 구현한다.
- 자바 바이트 코드는 플랫폼에 독립적이다.
- 이론적으로 모든 자바 프로그램은 CPU나 운영체제의 종류와 무관하게 동작할 것을 보장한다.
- 스택 기반, 대다수의 명령어가 스택 선두에서 피연산자를 택하고 다시 스택에 넣는다.
- 포인터를 지원하지만, C처럼 주소 값을 임의로 조작이 가능한 포인터 연산을 지원하지 않는다.
- GC를 사용해 자원을 관리한다.
- 메모리 관리가 가능하다.

### Class Loader

* 각 소스코드 파일을 오브젝트 파일로 변환시켜주는 역할을 한다.
* Loader(로더)
  * 목적 프로그램(기계어 파일)을 실행 가능한 파일로 변환하기 위해 컴퓨터의 주 기억 장소를 할당(Allocation)하거나, 여러개의 목적 프로그램을 연계 편집하여 CPU가 처리할 수 있는 프로그램으로 변환시킨다.
  * 프로그램을 실행하기 위하여 프로그램을 보조 기억 장치로부터 컴퓨터의 주 기억 장치에 올려 놓는 작업을 수행
  * 목적 프로그램들 끼지 연결(Linking)시키거나, 주 기억 장치를 재배치(Relocation)하는 증 포괄적인 작업을 수행
  * 할당(Allocation) -> 연결(Linking) -> 재배치(Relocation) -> 적재(Loading) 순으로 진행
* Linker : 모든 오브젝트 파일들을 하나의 오브젝트 파일로 합친다.
* JVM내로 Class(.class)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다.
* Runtime 시점에 Dynamic으로 Class를 Load한다.
* jar 파일 내 저장된 Class들을 JVM 위에 올리고, 사용하지 않는 Class들은 메모리에서 삭제한다.
* class의 instance를 생성하면 Class Loader를 통해 메모리에 Load하게 된다.
* Java는 Dynamic Code
* Compiletime이 아니라 Runtime시에 참조한다. 즉 Class를 처음 참조할 때, 해당 Class를 Load하고 Link한다.

### Runtime Data Areas

* JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당 받은 메모리 공간을 의미한다.
* PC Register
  * CPU는 명령어를 수행하는 동안 필요한 정보를 Register라고 하는 CPU내의 기억장치를 사용한다.
  * Stack - Base로 동작한다.
  * 각 쓰레드별로 하나씩 존재하며, 현재 수행중인 JVM 명령어의 주소를 가지게 된다.
  * 만약 Native Method를 수행한다면 PC Register는 undefined 상태가 된다.
  * PC Register에 저장되는 명령어의 주소는 Native Pointer 일 수도 있고, Method ByteCode일 수도 있다.
  * Native Method를 수행할 때에서는 JVM을 거치지 않고 API를 통해 바로 수행하게 된다.
* JVM Stack
  * JVM Stack은 Thread의 수행 정보를 Frame를 통해서 저장하게 된다.
  * Thread가 시작될 때 생성되며, 각 쓰레드별로 생성이 되기 때문에 다른 쓰레드에는 접근할 수 없다.
  * 메소드가 호출되면 메소드와 메소드 정보는 스택에 쌓이게 되며 메소드 호출이 종료될 때 스택 point에서 제거된다.
  * 메소드 정보는 해당 메소드의 매개변수, 지역변수, 임시변수, 주소(메소드 호출한 주소) 등을 저장하고, 메소드 종료시 메모리 공간이 사라진다.
* Native Method Stack
  * Java외의 언어로 작성된 Native Code들을 위한 스택, 즉 JNI(Java Native Interface)를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 Stack
* Method Area(Class Area, Static Area)

  * 모든 쓰레드가 공유하는 메모리 영역이다.
  * Method Area는 Class, Interface, Method, Field, Static 변수 등의 Byte Code 등을 보관한다.

* Heap

  * 인스턴스 또는 객체를 저장하는 공간이다.
  * 프로그램상에서 런타임시 다이나믹으로 할당하여 사용하는 공간이다.
  * .class를 이용해 인스턴스를 생성하면 Heap에 저장된다.
  * new 연산자로 생성된 인스턴스를 저장한다.
  * 초기 heap size는 64m이고 최대 heap size는 256m이다.
  * GC의 대상이 된다.
  * 성능 이슈를 일으키는 공간이다.
    * Permanent Generation
      * 생성된 인스턴스의 주소값이 저장된 공간이다.
    * New/Young Area
      * eden : 인스턴스들이 최초로 생성되는 공간
      * survivor 0/1 : eden에서 참조되는 인스턴스들이 저장되는 공간
    * Old
      * new Area에서 일정 시간 참조되고 살아남은 객체들이 저장되는 공간
      * eden 영역에서 인스턴스가 가득차게 되면 첫번째 GC가 발생한다.
      * eden 영역에 있는 값들을 Survivor 1영역에 복사하고, 이 영역을 제외한 나머지 영역의 객체를 삭제한다.
* Runtime Constant Pool

  * 각 클래스와 인터페이스의 상수, 메소드와 필드에 대한 모든 레퍼런스를 담고 있는 테이블이다.
  * 메소드나 필드의 실제 메모리상 주소를 찾을땐 해당 풀을 참조한다.

### Execution Engine

* Load된 Class의 바이트코드를 실행하는 Runtime Module이다.
* Class Loader를 통해 JVM내의 Runtime Data Areas에 배치된 ByteCode는 Execution Engine에 의해 실행되며, 실행 엔진은 자바 바이트코드를 명령어 단위로 읽어서 실행한다.
* 최초의 JVM은 인터프리터 방식이기 때문에 속도가 느린 단점이 있었지만, JIT 컴파일러 방식을 통해 이 점을 보완했다.
* JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준을 넘어서면 JIT 컴파일러 방식으로 실행한다.

### 실행 과정

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다. JVM은 이 메모리를 용도에 따라 여러 영역(Rumtime Data Areas)으로 나누어 관리한다.
2. Java Compiler(Javac)가 Java SourceCode(.java)를 읽어들여 Java ByteCode(.class)로 변환한다.
3. Class Loader를 통해 class 파일들을 JVM으로 Loading한다.
4. Loading된 class 파일들은 Execution Engine을 통해 해석된다.
5. 해석된 ByteCode는 Runtime Data Areas에 배치되며 실질적인 수행이 이루어지게 된다. 이러한 과정속에서 필요에 따라 Thread Synchronization과 같은 GC 작업을 수행한다.

### JIT(Just In Time) Compiler

1. Interpreter 방식의 단접을 보완하기 위해 도입된 컴파일러
2. 인터프리터 방식으로 실행하다가 일정한 기준을 넘어서면 바이트코드 전체를 컴파일하여 네이티브 코드로 변환한다.
3. 이후에는 더이상 인터프리터 방식으로 컴파일하지 않고, 네이티브 코드로 직접 실행한다.
4. NativeCode는 Cache에 보관하기 때문에 한번 Compile된 Code는 빠르게 수행된다.
5. JIT Compiler가 Compile하는 과정은 ByteCode를 Interpreting하는 방식보다 훨씬 오래 걸리므로 한 번만 실행되는 Code라면 Interpreter방식이 적절하다.

## GC

## Spring

### Spring

### Spring Boot

### Spring Bean Life Cycle

### IoC Container

### IoC(Inversion of Control, 제어의 역전)

* 

### DI(Dependency Injection, 의존성 주입)

* 

### AOP

### Servlet

### Spring MVC

### JPA

### MyBatis

### 생성자 주입

### Hystrix

## Web

### Tomcat

### WAS

### Web Server

### URL 입력시 동작 방식

### OAuth

### CORS

### CSRF

### XSS

### 쿠키

### 로컬 스토리지

### 세션

### 토큰

### HTTP

### HTTP2

### HTTPS

### Payload

### URI

### URL

### REST

### AJAX

## 자료구조

### 스택

### 큐

### Linked List

### 힙

### HashTable

### HashMap

### 이진 트리

## 알고리즘

### 정렬 알고리즘

### 바이너리 서치

### 캐시 알고리즘

### DFS & BFS

### 암호화 알고리즘

## Network

### OSI 7계층

### L4, L7

### TCP

### UDP

### 로드밸런싱

### 클러스터링

### 게이트웨이

## OS

### 데드락

### 뮤텍스

### 세마포어

### 페이징

### 스케쥴링

### 교착상태

### 경쟁상태

### fork

### 프로세스

### 쓰레드

## Others

### Git

### IO

### NIO

### 동기

### 비동기

### Blocking

### NonBlocking

### Docker

### CI

### 함수형 프로그래밍