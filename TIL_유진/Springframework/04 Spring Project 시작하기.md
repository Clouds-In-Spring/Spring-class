## Spring Project 시작하기

#### 1 STS (SpringSource Tool Suite) 소개

- 이클립스 확장판

- 스프링 개발에 최적화 되도록 만들어진 IDE

- STS 제공 기능

  - Bean 클래스 이름 자동완성 - 클래스 목록 불러옴
  - 설정파일 생성 위저드 - xml 설정할때 네임스페이스와 스키마 버전 선택
  - Bean 의존관계 그래프 
    - xml 읽어서 자동으로 그래프 그려줌
    - 각 빈이 어떻게 참조되고 , 어떤 속성 갖는지 

  - AOP 적용 대상 표시 - 공통기능(AOP 적용 대상)을 분리해냄

    

#### 2. Maven과 Library 관리

- Maven

  - 라이브러리 관리 + 빌드 툴

  - 사용하는 이유

    - 편리한 Dependency + Library 관리 - Dependency Management
    - 여러 프로젝트에서 프로젝트 정보나 jar 파일들을 공유하기 쉬움
    - 모든 프로젝트의 빌드 프로세스를 일관되게 가져갈 수 있음

    - ![image](https://user-images.githubusercontent.com/38436013/106227991-b84c7180-622d-11eb-9c8f-5fdc3a66bc0d.png)

    - ![image](https://user-images.githubusercontent.com/38436013/106228035-d87c3080-622d-11eb-9d9d-701d63d3b9ac.png)

    - pom.xml
      - maven 프로젝트 생성하면 이파일 생성됨
      - project object model 정보를 담고 있음

    - pom.xml 의존관계 추가
      - 의존관계 - 라이브러리를 말함
      - 스프링 프레임워크 설치

    

#### 3. Spring Project 시작

- sts 시작 -> 스프링프로젝트 생성 및 설치
- java project -> convert to maven -> add spring project nature
