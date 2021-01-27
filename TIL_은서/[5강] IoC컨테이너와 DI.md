# 5강 IoC 컨테이너와 DI

#### IoC(Inversion of Control: 제어권의 역전)

* 컴포넌트간 의존관계를 결정해주고, 설정 및 생명주기를 해결하기 위한 디자인 패턴



#### IoC 컨테이너

* 객체 생성, 의존성 관리
* POJO의 생성, 초기화, 서비스, 소멸에 대한 권한 가짐 (개발자들이 컨테이너에게 POJO객체를 맡김)



#### DL과 DI

* **DL(Dependency Lookup: 의존성 검색)**: 저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup
  * 컨테이너가 제공하는 API를 사용하므로 컨테이너에 **종속적**
* **DI(Dependency Injection: 의존성 주입)**: 클래스간 의존관계를 **Bean 설정정보**를 바탕으로 컨테이너가 자동으로 연결해주는 것

➡ Spring은 DL이기도 DI이기도 함

➡ DL 사용시 컨테이너 종속성이 증가하여 주로 DI 사용



#### DI

* Bean 설정정보 바탕(XML, Annotation)
* 개발자들은 간단히 Bean 설정파일에서 의존관계가 필요하다는 정보를 추가하면 된다
* 실행시 동적으로 의존관계 생성된다
* 컨테이너가 제어권의 흐름의 주체가 되어 애플리케이션 코드에 의존관계 주입해주는 것
* 코드가 단순해지며 컴포넌트간 결합도 제거된다는 장점이 있음



#### DI의 유형

1. Setter Injection

   : 의존성 입력받는 setter 메서드를 만들어 이를 통해 의존성 주입

2. Constructor Injection

   : 필요한 의존성을 포함하는 클래스의 생성자를 만들고 이를 통해 의존성 주입

3. Method Injection

   : 의존성 입력받는 일반 메서드를 만들고 이를 통해 의존성 주입



#### Spring DI 컨테이너의 개념

* Spring DI 컨테이너가 관리하는 객체를 **빈(Bean)**이라고 한다

* 빈을 관리한다는 의미로 컨테이너를 **빈 팩토리(BeanFactory)**라고 부른다

  * Bean을 등록, 생성, 조회, 반환 관리

  * Application Context는 BeanFactory를 확장한 것

    BeanFactory역할 + Spring의 각종 부가서비스 제공