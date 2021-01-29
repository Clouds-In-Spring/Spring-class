## 03 Spring Framework 개요

#### 1. Spring Framework란?

- 자바 개발 편하게해주는 **경량급 애플리케이션 프레임워크**
- 애플리케이션 -> 범용적, 경량급 -> 웹컨테이너에서도 고급기술 사용 가능
- 엔터프라이즈 개발 용이
  - 개발자는 low level 에 신경x

- 오픈소스 
  - 오픈소스의 단점과 한계 극복 - vmware 가 spring opensource 사들임(오픈소스 부분 지원)

- spring framework 전략

  - spring 삼각형 : 엔터프라이즈 개발의 복잡함을 4가지로 상대
  - ![image](https://user-images.githubusercontent.com/38436013/106222354-b204c800-6222-11eb-828e-a57017db7f6f.png)
  - portable Service Abstraction
    - 트랜잭션 추상화, OXM(object xml mapping) 추상화, ... 
    - 복잡한 기술은 low level 기술과 기술 사용하는 인터페이스로 분리
    - 개발자는 인터페이스 참고

  - 객체지향과 DI(dependency injection)
    - 의존만계주입 ??
    - DI는 유연하게 확장 가능한 객체를 만들어 두고, 그 관계는 외부에서 다이내믹하게 설정

  - AOP(Aspect Oriented Programming)
    - 관점지향프로그래밍
    -  로직에 있는 기술 관련 코드를 분리해서 별도의 모듈로 관리

  - pojo

    - 자바 오브젝트

      

#### 2. Spring Framework 특징

- 컨테이너 역할 : 자바 객체의 라이프사이클 관리, 스프링 컨테이너로부터 필요한 객체 가져와 사용 가능
- DI 지원 : XML , component 통해 객체가느이 의존관계 설정
- AOP 지원 : 업무 로직에서 로깅, 보안 같은 공통 모듈을 분리해서 개발
- POJO 지원 : 특정 인터페이스 구현하거나 클래스 상속 받지 않아도 된다
- 트랜잭션 처리 위한 일관된 방법 지원 :  COMMIT, ROLLBACK 하겠다는 것을 프로그램 안에 코드를 작성하는 것이 일반적이지만, 스프링은 코드가 아니라 XML, ANNOTAION 같은 설정을 하여 지원
- 영속성 : Mybatis, Hibernate 같은 db 지원

### 3. Spring Framework 기능 요소

![image](https://user-images.githubusercontent.com/38436013/106223845-88996b80-6225-11eb-951c-04f1e75b808e.png)

- Spring Core 
  - 기본 기능(컨테이너), 
  - BeanFactory 는 스프링의 기본 컨테이너이면서 DI의 기반

- Spring AOP
  - Aspect 지향 프로그래밍
  - 업무 로직과 공통 로직을 분리

- Spring ORM
  - Mybatis, Hibernate 같은 ORM과 연동

- Spring DAO
  - JDBC 코딩이나 예외처리 하는 부분을 간편화 시킴
  - AOP 모듈을 이용해 트랜잭션 관리 서비스도 제공

- Spring Web
  - 브라우저 기반 개발 기능 제공
  - 다른 웹애플리케이션 프레임워크와 통합 기능 제공

- Spring Context
  - spring core 개념을 확장, 국제화 기능(여러 언어 지원), 라이플사이클, 유효성 검증 등을 지원

- Spring WebMVC

  - 인터페이스가 애플리케이션 로직과 분리되는 웹애플리케이션을 만드는 경우

  
