# 5강

> 1. IoC(Inversion of Control)
> 2. DI(Dependency Injection)
> 3. Spring 컨테이너    

<br>
<br>

# IoC(Inversion of Control)의 개념
* 제어권의 역전
* 모든 객체에 대한 제어권이 프레임워크 컨테이너가 가진다
* 컴포넌트 의존관계 결정(component dependency resolution), 설정(configuration) 및 생명주기(lifecycle)를 해결하기 위한 디자인 패턴

<br>

## IoC 컨테이너
* 객체에 대한 생성 및 생명주기를 관리, 의존성 관리
* POJO(Plain Old Java Object)특정 규약에 제한되지 않는 일반적인 자바객체. 의 생성, 초기화, 서비스, 소멸에 대한 권한을 가짐
* 개발자들이 직접 POJO 생성 가능하지만 컨테이너에게 맡김

<br>

## IoC의 분류
DL은 특정 라이브러리에 종속되기 때문에 DI를 많이 쓴다.

* IoC
    - DL(Dependency Lookup)
        - EJB(Enterprise Java Beans)
        - Spring
    - DI(Dependency Injection)
        - Spring
            - Setter Injection
            - Constructor Injection
            - Method Injection
        - PicoContainer

<br>

## DL과 DI
|DL(의존성 검색)|DI(의존성 주입)|
|:---:|:---:|
|저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup하는 것|각 클래스간의 의존관계를 빈 설정(Bean Definition)정보를 바탕으로 컨테이너가 자동으로 연결해줌|

<br>

# DI의 개념
각 클래스간의 의존관계를 빈 설정정보를 바탕으로 컨테이너가 자동으로 연결
* 개발자는 빈 설정 파일에서 의존관계가 필요하다는 정보를 추가
* 실행시에 동적으로 의존관계가 생성됨
* 컨테이너가 흐름의 주체가 되어 애플리케이션 코드에 의존관계를 주입

<br>

* 장점
    - 코드가 단순해짐
    - 컴포넌트 간의 결합도가 제거됨

<br>
<br>

## DI의 유형
- Setter Injection
- Constructor Injection
- Method Injection

<br>

## DI를 이용한 클래스 호출 방식
![DI를이용한클래스호출방식구조](https://user-images.githubusercontent.com/37285946/106128956-2dbf3000-61a3-11eb-903c-a636296b505f.png)

<br>
<br>
<br>

# Spring DI 컨테이너   

* Spring DI 컨테이너의 개념
    - bean
    - bean들을 관리한다는 의미로 컨테이너를 빈 팩토리(BeanFactory)라고 부른다
    * 객체의 생성과 객체 사이의 런타임 관계를 DI 관점에서 볼 때는 컨테이너를 BeanFactory라고 한다.
    * BeanFacotry에 여러가지 컨테이너 기능을 추가하여 애플리케이션 컨텍스트(ApplicationContext)라고 부름


    |BeanFactory|Application|
    |:---|:---|
    |- 빈을 등록, 생성, 조회, 반환, 관리함<br>- 보통은 빈팩토리를 바로 사용하지 않고 이를 확장한 ApplicationContext를 사용함<br>-getBean() 메서드가 정의되어 있음|-Bean을 등록, 생성, 조회, 반환 관리하는 기능은 BeanFactory와 같음<br>-Spring의 각종 부가 서비스를 추가로 제공함<br>-Spring이 제공하는 ApplicationContext 구현 클래스가 여러가지 종류로 있음|