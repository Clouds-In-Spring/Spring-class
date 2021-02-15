## 13  AOP 개요

### 학습 목표

- AOP의 개요와 용어
- Spring AOP의 특징 및 구현방식에 대한 이해
- AspectJ와 Spring AOP 설치에 대한 이해

### 1 AOP 개요와 용어

###### 핵심기능 & 부가기능

- 핵심기능 : 업무 로직을 포함하는 기능
- 부가기능 : 핵심 기능 도와주는 로깅, 보안 기능
- 핵심기능에서 부가기능을 분리해서 모듈화 어려움 => AOP 적용하자

###### AOP(Aspect Oriented Programming : 관점 지향 프로그래밍)의 개요

- 애플리케이션에서의 기능의 분리
- 즉, 핵심기능에서 부가기능을 분리
- OOP를 적용해도 기능 분리 어려움 -> 애스팩트라는 독특한 모듈형태로 만들어서 개발
- 또한 애스팩트는 객체지향적 가치를 지킴

###### 애스펙트(Aspect)

- Advice(부가기능) + PointCut(어드바이스 어디에 적용할지) 

###### AOP 용어

- 타겟(target)

  - 핵심기능 모듈, 타켓은 부가기능을 부여할 대상

- 어드바이스(advice)

  - 부가기능 모듈

- 조인포인트(join point)

  - 어드바이스 적용될 위치
  - **Target 객체가 구현한 인터페이스의 모든 메서드**

- PointCut

  : Advice를 적용할 타겟의 메서드를 선별하는 정규표현식

- Aspect

  : AOP의 기본 모듈, 싱글톤 형태의 객체로 존재

- Advisor (=Aspect)

  : Advisor = Advice + PointCut, Spring AOP에서만 사용되는 특별한 용어

- Weaving

  : PointCut의해 결정된 Target의 JoinPoint에 Advice를 삽입하는 과정

    **➡ 핵심기능의 메서드에 부가기능을 추가하는 것**

  핵심기능의 코드에 영향을 주지 않으면서 필요한 부가기능을 추가할 수 있도록 해주는 핵심적인 처리과정

  ![image](https://user-images.githubusercontent.com/38436013/107918330-cd0a6280-6fac-11eb-8625-cefe479a4d5d.png)

| 핵심기능 |        부가기능         |
| :------: | :---------------------: |
|  Target  | Aspect, Advice, Advisor |

**🚨 Aspect는 Advice를 포함한다**

### 2 Spring AOP의 특징 및 구현 방식

###### Spring AOP의 특징

1. Spring은 프록시 기반 AOP를 지원한다

   * Target 객체(핵심기능)에 대한(객체를 감싼) 프록시를 만들어 제공
   * 프록시는 Advice(부가기능)를 Target 객체에 적용하는 시점에 생성되는 객체

2. 프록시가 호출을 가로챈다

   ![image](https://user-images.githubusercontent.com/38436013/107918905-dd6f0d00-6fad-11eb-9132-65ca8c533be8.png)

   * **전처리 Advice**

     프록시는 Caller의 요청시

     > Target 객체에 대한 호출을 가로챔 > **Advice의 부가기능 로직 수행** > Target의 핵심 기능 로직 호출

   * **후처리 Advice**

     >  핵심기능 메서드 호출 후 부가기능 을 수행하는 경우

3. Spring AOP는 메서드 조인 포인트만 지원한다

   * Target(핵심기능)의 메서드가 호출되는 **Runtime시점에만** Advice(부가기능)를 적용할 수 있다
   * **AspectJ(AOP 프레임워크)**를 사용하면 다양한 시점(객체 생성, 필드값 조회 및 조작, static 메서드 호출 및 초기화 등)에도 부가기능을 적용할 수 있다

###### Spring AOP의 구현 방식

1. **XML 기반의 POJO 클래스를 이용한 AOP 구현방식**

   * 부가기능을 제공하는 Advice 클래스 작성

   * XML 설정 파일에 Advice와 PointCut 설정

     ➡ \<aop.config>태그를 이용해 Aspect 설정

2. **@Aspect 어노테이션을 이용한 AOP 구현방식**

   * @Aspect를 이용해 부가기능을 제공하는 Aspect 클래스 작성

     Aspect 클래스는 Advice를 구현하는 메서드 + PointCut 포함

   * XML 설정파일에 \<aop:aspectj-autoproxy />를 설정

### 3 AspectJ와 Spring AOP 라이브러리 설치

