## SpringFramework의 정의
* * *
> Spring Framework란?
```
Java 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크이다.
```
|애플리케이션 프레임워크|경량급 프레임워크|엔터프라이즈 개발 용이|오픈소스|
|------|---|---|---|
|애플리케이션 전 영역을 포괄하는 범용적인 프레임워크|웹스피어, WAS, 웹로직과 같이 고가 장비가 있어야 돌아가는 것이 아니라 오픈소스 다운으로 사용가능한 웹컨테이너에서도 엔터프라이즈 개발의 고급기술을 대부분 사용할 수 있다|Lowlevel에 많이 신경쓰지 않으면서 Business Logic개발에 전념할 수 있다|오픈소스의 장점을 취하면서 오픈소스 제품의 한계 극복


> Spring Framework 전략
### Spring 삼각형

![spring삼각형](https://user-images.githubusercontent.com/37285946/106130621-eb96ee00-61a4-11eb-8e07-41e616556ae4.png)


1. Portable Service Abstraction(서비스 추상화)
    * Object XML Mapping(OXM) 추상화, 특정 DB 액세스의 예외 변환기능 등 기술적 복잡함은 추상화를 통해 Low Level로 기술 구현 / 기술을 사용하는 인터페이스로 분리
2. DI (의존관계주입)
    * 객체 지향에 충실한 설계 / 유연한 확장이 가능한 객체를 만들어두고 그 관계는 외부에서 설정
3. AOP (Aspect Oriented Programming : 관점지향 프로그래밍)
    * 어플리케이션 로직을 담당하는 코드에 남아있는 기술 관련 코드를 분리 -> 별도의 모듈로 관리하게 해주는 기술
4. POJO (Plain Pld Java Object)
    * 일반적인 Java Object
    * 특정환경이나 규약, 플랫폼에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 객체
    * ex) 서블릿 같은 경우 http servlet이라고 하는 클래스를 상속받아 MyServlet 클래스를 생성하였다. 웹컨테이너가 없으면 MyServlet을 사용할 수 없다 이런 경우 MyServlet을 POJO라고 볼 수 없다
<br>
<br>
<br>

## SpringFramework의 특징
* * *
1. 컨테이너 역할
2. DI(Dependency Injection) 지원
    * 어노테이션 통해서 객체간의 의존관계 설정
3. AOP 지원
    * 트랜잭션, 로깅, 보안과 같이 공통적으로 필요로 하는 모듈들을 실제 핵심 모듈에서 분리해서 적용할 수 있다
4. POJO 지원
    * Spring 컨테이너에 저장되는 Java 객체는 특정한 인터페이스를 구현하거나 특정 클래스를 상속받지 않아도 된다.
5. 트랜잭션 처리를 위한 일관된 방법 지원
    * commit, rollback과 같이 프로그램 안에 코드로 설정하는 것이 아니라 XML, 어노테이션과 같은 방법으로 설정해주기 때문에 동일한 코드 사용가능
6. 영속성과 관련된 다양한 API 지원
    * Spring은 MyBatis, Hibernate 등 DB처리를 위한 ORM(Object Relational Mapping) 프레임워크들과의 연동 지원

<br>
<br>
<br>

## SpringFramework의 기능요소
* * *
<br>


1. Core 컨테이너
    * 스프링 프레임워크의 기본기능 제공
    * BeanFactory는 스프링의 기본 컨테이너면서 스프링DI의 기본
2. AOP
    * 업무로직 / 공통로직 분리하여 공통 로직을 Aspect로 만들 수 있다.
    * Aspect 개발 지원

3. Spring ORM
    * MyBatis 등 ORM 제품과 연동 가능하게 하는 기능 제공
4. Spring DAO
    * JDBC 코딩이나 예외처리 간편화
    * AOP모듈을 이용해 트랜잭션 관리 서비스 제공
5. Web
    * 브라우저 기반으로 동작하는 웹애플리케이션 개발에 필요한 기본 기능 제공
    * Webwork, Struts와 같은 다른 웹애플리케이션 프레임워크와의 통합 지원
6. Context
    * 기존의 Spring Core부분을 확장 -> 국제화 기능 지원

7. WebMVC (Model/View/Controller)
    * 사용자 인터페이스가 애플리케이션 로직과 분리되는 웹 애플리케이션을 만드는 경우 일반적으로 사용되는 패러다임



