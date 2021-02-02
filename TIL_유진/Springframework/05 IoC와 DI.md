# 05 IoC와 DI

### 1. IoC

- IoC 개념

  - 제어권의 역전
  - 프레임워크의 컨테이너가 객체 생성, 생명 주기를 관리한다는 의미
  - 컴포넌트 의존관계 결정, 설정, 생명주기 해결 위한 디자인 패턴

  - IOC 아닌 경우 : 개발자가 객체 생성

  - IOC 인 경우 : 프레임워크의 컨테이너가 객체 생성해서 개발자 코드에 주입시킨다

    

- IoC 컨테이너
  - 제어권이 있음
  - 객체 생성 및 생명 주기 관리
  - POJO(일반적 자바 객체)를 컨테이너가 생성 할 수 있음
  - POJO의 생성 초기ㅗ하 서비스 소멸에 대한 권한 지님



- IoC의 분류

  - DI >> DL

    ![image](https://user-images.githubusercontent.com/38436013/106430566-5733d080-64af-11eb-8401-95eff726defc.png)

    

- DL(dependency lookup)과 DI(dependency injection)

  ![image](https://user-images.githubusercontent.com/38436013/106430703-88140580-64af-11eb-84c7-b1a75a616a08.png)



### 2. DI(dependency injection)

- DI의 개념

  - 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해 주는 것

  - 의존관계 있다는 것을 XML, annofation (빈 설정 정보- 개발자가 관여)방식으로 설정해 놓으면 컨테이너가 이것을 읽어서 의존관계를 자동으로 연결

  - 개발자는 빈 설정파일에 의존관계 정보만 추가

  - 객체 레퍼런스를 컨테이너로부터 주입 받아서, 실행 시(런타임)에 동적으로 의존관계 생성

  - 컨테이너가 흐름의 주체

  - 장점

    - 코드 단순
    - 컴포넌트 간의 결합도가 제거됨

    

  - DI의 유형

    ![image](https://user-images.githubusercontent.com/38436013/106431370-80089580-64b0-11eb-945e-3cd0682fbe04.png)

- DI를 이용한 클래스 호출 방식

  - 왼쪽 그림에서, 클래스가 구현클래스 사용한다 가정, 클래스가 구현 클래스를 의존하는 것이 아니라

    인터페이스만 사용

  - beans.xml은 hello라는 클래스가 언제 string printer쓰고, console printer쓸지 개발자가 설정해둠

  - 다시 말해서, hello가 string printer 객체를 직접 생성하지는 않는다

    ![image](https://user-images.githubusercontent.com/38436013/106432037-7b90ac80-64b1-11eb-84a6-f17bd8a40b0d.png)

    

- Setter Injection

  - Hello가 String Printer를 의존

  - 하지만, 오른쪽에 new String Printer() 의존관계를 명시 하지는 않음

  - 오른쪽 class 안에 Printer printer라는 멤버 변수 설정, 그 밑에 개발자는 setter 메소드 설정

  - beans에 hello와 String Printer의 의존관계를 개발자가 명시

  - bean.Hello가 bean.StringPrinter와 의존관계가 있다는 것은 오른쪽 setter메서드에 명시

  - propertry 태그가 오른쪽 setter와 매핑됨

  - setter injection은 한번에 한개씩만 의존관계 주입

    ![image](https://user-images.githubusercontent.com/38436013/106432930-c6f78a80-64b2-11eb-9867-91dbcf0c1ad4.png)

- Constructor Injection

  - setter  injection은 한번에 한개씩만 의존관계 주입 => 이것을 개선
  - 오른쪽 클래스안에 두가지 인자 받는 생성자를 선언
  - beans설정파일에 index로 구분해서 설정해주면 됨

  ![image](https://user-images.githubusercontent.com/38436013/106433104-0d4ce980-64b3-11eb-98bb-4a9a321bdd77.png)

### 3. Spring DI 컨테이너

- 스프링 DI 컨테이너가 관리하는 객체 -> 빈(BEAN)

- 빈들을 관리하는 컨테이너 = 빈 팩토리 

- 빈 팩토리에 여러가지 컨테이너 기능 추가 = 애플리케이션 컨텍스트(ApplicationContext)

  ![image](https://user-images.githubusercontent.com/38436013/106433597-c14e7480-64b3-11eb-8c21-6eec7a891b73.png)

  ![image](https://user-images.githubusercontent.com/38436013/106433828-0bcff100-64b4-11eb-95b3-d94234df5d11.png)

  

