# SpringBoot로 인별그램 만들기

## REST API

> REST 기반으로 서비스 API를 구현한 것이다.

_API란? 데이터와 기능의 집을 제공하여 컴퓨터 프로그램 간 상호작용을 촉진하고, 서로 정보 교환이 가능하도록하는 것이다._

<br>

### REST(Representational State Transfer)란?

> HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.

CRUD Operation

- Create (POST) : 생성
- Read (GET) : 조회
- Update (PUT) : 수정
- Delete (DELETE) : 삭제

<br>

## 아키텍처

<img width="812" alt="아키텍처" src="https://user-images.githubusercontent.com/63037344/147459716-b60a0841-7372-4c28-be63-e81245a4bb02.png">

> 사용자들은 디바이스에 있는 웹 브라우저(크롬) 등을 통해서 AWS내에 있는 SpringBoot 서버로 통신하고, DB와의 CRUD를 통해서 다시 클라이언트로 결과값을 전해준다.

<br>

## 프로젝트 생성 및 API 설계하기

### 기능

- 조회 : DB에 있는 상위 1000개의 데이터들을 가져와서 보여준다.
- 등록 : 사진과 글을 등록한다.
- 수정 : 사진과 글을 수정한다. 글 작성 시 입력 비밀번호를 받아서 맞지 않으면 수정되지 않게 한다.
- 삭제 : 해당 건의 글을 삭제한다. 입력 비밀번호를 받아서 맞지 않으면 삭제되지 않게 한다.

<br>

### Dependencies

   <img width="300" alt="springboot dependencies" src="https://user-images.githubusercontent.com/63037344/147020324-82c6ef1a-91be-418f-ad97-12051642996c.png">

- Spring Web : Rest 형태
- H2 Database : 컴퓨터에 내장된 램(RAM)메모리에 의존하는 데이터베이스, localhost:8080/h2-console
- Spring Data JPA : JPA라는 ORM 객체 릴레이션 맵핑 라이브러리를 이용해서 객체를 저장
- DebTools : 소스가 바뀌면 동적으로 서버가 재기동 되는 것을 도와준다.
- Lombok : 소스의 간결화 및 가용성을 돕는다.

_Lombok? Java 라이브러리이다._

> 반복되는 getter, setter, toString 등의 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리
> <br>

_JPA(Java Persistence API)?_

> 자바 진영에서 ORM 기술 표준으로 사용되는 인터페이스의 모음이다. 즉, 실제적으로 구현된 것이 아닌 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크이다. ex) Hibernate

- 사용이유 : 신규 개발/신규 컬럼 추가 시 CRUD SQL을 반복적으로 사용하기 때문에, 객체와 테이블을 매핑시켜주는 ORM을 사용하기 위해 JPA를 사용한다.
  <br>

### 프로젝트 구조

<img width="249" alt="프로젝트 구조" src="https://user-images.githubusercontent.com/63037344/147460316-491be11f-2311-4310-8db1-e7fd0d49bdf1.png">
 
<br>

### JpaRepository 표준 방법

> 인터페이스에 미리 메소드를 정의해 두면, 메소드를 호출하는 것만으로 데이터 검색을 할 수 있다.

- 저장 : persist(엔티티)
- 조회 : find("ID")
- findAll()
- getOne("ID")
- 수정 : setName("이름")
- 삭제 : delete(엔티티)
- count()

### JpaRepository의 메소드 명명 규칙

> 메소드 이름 작성 방법을 숙지하면, 필요한 메소드를 빠르게 쓰고 추가할 수 있다.

- findByXX
- Like / NotLike
- StartingWith / EndingWith
- IsNull / IsNotNull
- True / False
- Between
  <br>

## 구현

### 사진 업로드 POST

❗️사진 크기가 커서 오류 발생  
💡 <b>application.properties</b>에 다음 두 줄 추가

- spring.servlet.multipart.max-file-size=10MB
- spring.servlet.multipart.max-request-size=10MB
