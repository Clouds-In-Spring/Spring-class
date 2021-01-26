# 3강 Spring Framework 개요

#### Spring Framework의 4가지 구성요소

1. Portable Service Abstraction (서비스 추상화)

   * 추상화 계층을 사용하여 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공해주는 것
   * 사용하는 기술(프로그램의 api)이 바뀌더라도 비즈니스 로직에 변화가 없도록 해줌

2. DI (Dependency Injection: 의존성 주입)

   * 객체 자체가 아니라 프레임워크에 의해 객체의 의존성이 주입되는 설계 패턴
   * 프레임워크에 의해 동적으로 주입된다

3. AOP (Aspect Oriented Programming: 관점 지향 프로그래밍)

4. POJO (Plain Old Java Object: 일반적인 자바 오브젝트(?))

   * 특정 환경이나 규약에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 객체

   * ex) 서블릿 파일로 HttpServlet을 상속받은 MyServlet을 만들었다. 여기서 이 MyServlet은 POJO라고 볼 수 없다

     왜냐면 HttpServlet을 상속받았기 때문 = 특정 환경 컨테이너가 필요한 환경에서 만들어졌기 때문



#### Spring Framework 특징

1. 컨테이너 역할

   : Java 객체의 LifeCycle을 관리

2. DI 지원

   : 설정파일이나 어노테이션을 통해 객체 간 의존관계 설정 가능

3. AOP 지원

   : 트랜잭션, 로깅, 보안 등과 같이 공통적으로 필요로 하는 모듈들을 실제 핵심 모듈(업무로직)과 **분리**해서 적용할 수 있다

4. POJO 지원

   : Spring 컨테이너에 저장되는 Java 객체는 특정한 인터페이스를 구현하거나, **특정 클래스를 상속받지 않아도 된다**

5. 트랜잭션 처리를 위한 일관된 방법을 지원

   : JDBC, JTA 등 어떤 트랜잭션을 사용하던 **코드가 아닌 설정(XML, Annotation)을 통해 정보를 관리**하므로 트랜잭션 구현에 상관없이 동일한 코드 사용 가능

6. 영속성과 관련된 다양한 API 지원

   : MyBatis, Hibernate 등 DB 처리를 위한 ORM(Object Relational Mapping) 프레임워크들과의 연동 지원



#### Spring Framework를 구성하는 기능 요소

1. Spring Core
   * 기본기능 제공
   * 이 모듈의 BeanFactory는 Spring의 기본 컨테이너면서 스프링 DI의 기반이다
2. Spring AOP
   * 업무로직, 공통로직 분리 => Aspect 지향 프로그래밍 기반 제공
3. Spring ORM
   * MyBatis, Hibernate, JPA 등 ORM 프레임워크와 연동 API 제공
4. Spring DAO
   * JDBC(Java DB Connectivity)에 대한 추상화 계층, AOP 모듈 이용하여 트랜잭션 관리 서비스 제공 	
5. Spring Web
   * 웹 어플리케이션 개발에 필요한 기본 기능 제공
6. Spring Context
   * Spring Core의 개념 확장
   * 국제화 기능(다양한 언어 제공), 생명주기 이벤트, 유효성 검증 등 지원
7. Spring Web MVC
   * 사용자 인터페이스, 애플리케이션 로직 분리
