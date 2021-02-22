# 19 Spring MVC 개요

### 학습 목표

1. MVC 패턴의 개념과 모델2 아키텍처
2. Spring MVC 개념
3. Spring MVC 기반 웹 애플리케이션 개발

### 1. MVC 패턴의 개념과 모델2 아키텍처

#### MVC(Model-View-Controller) 패턴의 개념

- Business logic과 Presentation logic을 분리하기 위한 아키텍처 패턴
- MVC 패턴을 사용하면, 시각적 요소와 로직을 분리하여 서로 영향X -> 수정이 쉬워짐
  - Model : 애플리케이션의 정보 (데이터, 비지니스 로직)
  - View : 사용자에게 제공할 화면(프레젠테이션 로직)
  - Controller : model과 view 사이를 연동, 관리

- MVC 패턴 흐름

     ![image](https://user-images.githubusercontent.com/38436013/108628423-2f2c0180-749e-11eb-8c91-5212aee59901.png)

  - 컨틀롤러 : 요청처리 및 흐름제어 
  - 모델 : 비즈니스 로직 및 데이터 처리
  - 뷰 : 결과 화면 생성

  1. 클라이언트(브라우저)가 컨트롤러에게 요청 보냄
  2. 컨트롤러는 모델을 호출
  3. 모델릉 요청 처리 결과를 컨트롤러에게 전달
  4. 컨트롤러는 뷰에게 화면생성 요청을 보냄
  5. 뷰는 결과 화면을 컨트롤러에게 돌려줌
  6. 컨트롤러는 클라이언트에게 겨로가를 돌려줌

#### 각각의 MVC 컴포넌트의 역할

- 모델 컴포넌트
  - DB와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 일
  - 여러 개의 데이터 변경 작업을 하나의 작업으로 묶는 트랜잭션 다루는 일도 함
  - DAO 클래스, service 클래스에 해당
- 뷰 컴포넌트
  - 사용자에게 출력할 화면 만드는 일
  - 화면은 웹 브라우저가 출력, 뷰 컴포넌트는 html과 css, java script를 사용하여 웹 브라우저가 출력할 ui 만듦
  - html과 JSP 사용하여 작성
- 컨트롤러 컴포넌트
  - 클라이언트의 요청을 받았을 때, 요청에 대한 업무를 수행하는 모델 컴포넌트 호출하는 역할
  - 클라이언트가 보낸 데이터가 있다면, 모델을 호출할 때 전달하기 쉽게 가공
  - 모델이 업무 완료하면, 화면 생성할 수 있도록 뷰에게 전달
  - Servlet과 JSP를 사용하여 작성 가능

#### 모델2 아키텍처 개념

- 모델 1 아키텍처 : 컨트롤러 역할을 JSP가 담당

  단점 : JSP 페이지에 자바코드가 많이 들어가게 되어 복잡해짐

- 모델 2 아키텍처 : 컨트롤러 역할을 Servlet이 담당

  -> 최근에 많이 사용

  -> Spring MVC 도 모델 2 아케틱처 기반의 웹 프레임워크

- 모델 2 

  ![image](https://user-images.githubusercontent.com/38436013/108628407-0dcb1580-749e-11eb-9984-bbb53e76cbfd.png)

  1. 클라이언트가 Controller(Servlet)에게 Model 요청 보냄
  2.  Controller(Servlet)는 모델 호출
  3. 모델은 db와 연동하여 sql 문을 실행하고 결과 받음
  4. 모델은 받은 결과값을 vo 객체로 저장하고 저장된 VO 객체를 Controller(Servlet)에게 전달
  5. Controller(Servlet)는 VO 객체를 View(JSP)에게 전달
  6. View(JSP)는 전달받은 VO 객체를 참조하고 결과 화면을 Controller(Servlet)에게 전달
  7. Controller(Servlet)은 결과 화면을 클라이언트에게 전달

#### 모델 2 호출 순서

![image](https://user-images.githubusercontent.com/38436013/108628353-c93f7a00-749d-11eb-8b04-337ae9943169.png)

1. 웹 브라우저가 웹 애플리케이션 실행 요청하면, 웹 서버가 그 요청 받아 서블릿 컨테이너(톰캣서버)에 넘겨줌

   서블릿 컨테이너는 URL을 확인하여 그 요청을 처리할 서블릿 찾아 실행

2. 서블릿은 업무 처리하는 모델 자바 객체의 메서드 호출

   만약, 웹 브라우저가 보낸 데이터를 저장하거나 변경해야 한다면 그 데이터를 가공하여 VO 객체를 생성하고, 모델 객체의 메서드를 호출할 때 인자 값으로 넘긴다

   모델 객체는 엔터프라이즈 자바빈(EJB : **Enterprise JavaBeans**)일 수도 있고, 일반 자바 객체(POJO로 된 Service 또는 DAO object)일 수 있다

3. 모델 객체는 JDBC를 사용하여 매개변수로 넘어온 값 객체를 DB에 저장하거나, DB로부터 질의 결과를 가져와서 VO 객체로 만들어 반환

5. JSP는 서블릿으로 부터 값 객체를 참조하여  화면을 만들고, 웹 브라우저에 출력

   ![image](https://user-images.githubusercontent.com/38436013/108628577-25ef6480-749f-11eb-8cdd-d51a1358be88.png)

6. 웹 브라우저는 서버로부터 받은 응답 내용을 화면에 출력

#### Frontend Controller 패턴 아키텍처

- Frontend Controller 프로세스

  ![image](https://user-images.githubusercontent.com/38436013/108628670-98f8db00-749f-11eb-81da-6b607650b6b1.png)

  - Front Controller는 클라이언트가 보낸 요청을 받아 공통적인 작업 먼저 수행
  - Front Controller는 적절한 세부 Controller에게 작업을 위임
  - 각각의 애플리케이션 Controller는 클라이언트에게 보낼 뷰를 선택해서 최종 결과 생성
  - Front Controller패턴은 공통처리로직 있을 경우, 클라이언트의 요청을 중앙 집중 관리할 때 사용

### 2 Spring MVC 개념

#### Spring MVC의 특징

- Spring은 DI나 AOP같은 기능 뿐 아니라, 서블릿 기반의 웹 개발을 위한 MVC 프레임워크를 제공
- Spring MVC는 모델 2 아키텍처와 Front Controller 패턴을 프레임워크 차원에서 제공
- Spring MVC 프레임워크는 Spring을 기반으로 하고 있기 때문에, Spring이 제공하는 트랜잭션 처리나 DI 및 AOP등을 손쇱게 사용

#### Spring MVC와 Front Controller 패턴

- 대부분의 MVC 프레임워크들은 Front Controller  패턴을 적용해서 구현

- Spring MVC도 Front Controller 역할을 하는 **DispatcherServlet이라는 클래스**를 계층 맨 앞단에 놓고,

  **서버로 들어오는 모든 요청을 받아서 처리하도록 구성**

- 예외가 발생했을 때 일관된 방식으로 처리하도록 하는 것도 Front Controller의 역할

#### DispatcherServlet 클래스

- Front Controller 패턴 적용
- web.xml에 설정
- 클라이언트로부터의 모든 요청을 전달 받음
- Controller나 View와 같은 Spring MVC의 구성요소를 이용하여 클라이언트에게 서비스 제공

- 호출 순서
  1. DispatcherServlet이 들어오는 요청을 맨 앞단에서 받아 Controller(개발자가 작성하는 클래스)에게 요청 위임
  2. Controller 클래스가 요청 핸들링, DB 연동의 결과물을 모델 객체에 담아 DispatcherServlet에게 전달
  3. DispatcherServlet은 받은 모델 객체를 View에게 전달
  4. View 템플릿은 화면을 그리고 결과를 DispatcherServlet에게 전달
  5. DispatcherServlet은 브라우저에게 응답 결과를 전달

#### Spring MVC의 주요 구성 요소 (*** 개발자가 주로 사용하게 되는 클래스)

- DispatcherServlet ***

  : 클라이언트의 요청을 받아 Controller, View에게 알맞게 전달

- HandlerMapping

  : 요청 URL, 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정,

  DispatcherServlet은 하나 이상의 HandlerMapping을 가질 수 있음

- Controller ***

  : 클라이언트의 요청 처리 후 Model 호출, 그 결과를 DispatcherServlet에게 전달

- ModelAndView ***

  : Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체

- View ***

  : Controller의 처리 결과 **화면에 대한 정보**를 보유

- ViewResolver

  : Controller가 리턴한 View 이름을 기반으로 어떤 View를 실행할지 결정해주는 역할

#### Spring MVC 주요 구성 요소의 요청 처리 과정

1. 클라이언트의 요청이 DispatcherServlet에게 전달
2. DispatcherServlet은 HandlerMapping를 통해 클라이언트의 요청을 처리할 Controller를 알아냄
3. DispatcherServlet은 Controller 객체를 이용하여 클라이언트의 요청 처리
4. Controller는 **요청 처리 결과, View 페이지 정보를 담은 ModelAndView 객체** 반환
5. DispatcherServlet은 ViewResolver를 통해 응답 결과를 생성할 View 객체를 알아냄
6. View는 클라이언트에게 전송할 응답 생성하여 DispatcherServlet에게 반환

### 3 Spring MVC 기반 웹 애플리케이션 개발

- 작성 절차
  1. 클라이언트의 요청을 받는 DispatcherServlet을 web.xml에 설정
  2. 클라이언트의 요청을 처리할 컨트롤러 작성
  3. Spring Bean으로 컨트롤러 등록
  4. JSP를 이용해 View 영역의 코드 작성
  5. 브라우저상에서 JSP 실행
