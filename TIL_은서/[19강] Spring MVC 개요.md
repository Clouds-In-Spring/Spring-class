# MVC 패턴의 개념과 모델2 아키텍처

####  MVC(Model-View-Controller) 패턴의 개념

* 목적은 Business logic과 Presentation logic을 분리하기 위함
* 사용자 인터페이스로부터 비즈니스 로직을 분리하여 애플리케이션의 시각적 요소와 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있음

> **Model : 애플리케이션의 정보 (데이터, 비즈니스 로직 포함)**
>
> **View : 사용자에게 제공할 화면 (Presentation Logic)**
>
> **Controller : Model과 View 사이의 상호작용을 관리**

* MVC 패턴의 흐름
  1. 클라이언트(browser)가 Controller에게 요청을 보냄
  2. Controller는 Model을 호출
  3. Model은 요청 처리 결과를 Controller에게 전달
  4. Controller는 View에게 화면생성 요청을 보냄
  5. View는 결과 화면을 Controller에게 돌려줌
  6. Controller는 클라이언트에게 결과를 돌려줌



#### 각각 MVC 컴포넌트의 역할

##### Model 컴포넌트

* DB와 연동하여 사용자가 입력한 데이터나 출력할 데이터를 다루는 일을
* 여러 개의 변경 작업(추가, 변경, 삭제)을 하나의 작업으로 묶는 트랜잭션을 다루는 일
* **DAO 클래스, Service 클래스가 해당됨**



##### View 컴포넌트

* 모델이 처리한 데이터나 작업 결과를 가지고 **사용자에게 출력할 화면**을 만드는 일
* 생성된 화면은 웹 브라우저가 출력, 뷰 컴포넌트는 HTML/CSS/JavaScript를 사용하여 출력할 UI를 만듦
* **Html과 JSP를 사용하여 작성**



##### Controller 컴포넌트

* 클라이언트의 요청을 받았을 때 그 요청에 대해 실제 업무를 수행하는 **Model 컴포넌트를 호출**하는 일
* 클라이언트가 보낸 데이터가 있다면, Model을 호출할 때 전달하기 쉽도록 **데이터를 적절하게 가공**하는 일
* Model이 업무 수행을 완료하면 그 결과를 가지고 화면을 생성하도록 **View에게 전달**(요청에 대한 모델, 뷰를 결정하여 전달)

* **Servlet과 JSP를 사용하여 작성**



#### 모델2 아키텍처의 개념

* 모델1 아키텍처 : Controller 역할을 **JSP**가 담당

  * 단점: JSP 페이지에 자바코드가 많이 들어가게되어 복잡해짐

* 모델2 아키텍처: Controller 역할을 **Servlet**이 담당

  ➡ 최근에는 **모델2 아키텍처를 많이 사용**함

  ​	 Spring MVC도 모델2 아키텍처 기반의 웹프레임워크

* 흐름

  1. 클라이언트가 Controller(Servlet)에게 Model 요청을 보낸다
  2. Controller(Servlet)는 Model 객체 호출
  3. Model은 DB와 연동하여 SQL문을 실행하고 결과를 받음
  4. Model은 받은 결과값을 VO객체로 저장하고 저장된 VO 객체를 Controller(Servlet)에게 전달
  5. Controller(Servlet)는 VO 객체를 View(JSP)에게 전달
  6. View(JSP)는 전달받은 VO 객체를 참조하고 결과 화면을 Controller(Servlet)에게 전달
  7. Controller(Servlet)은 결과 화면을 클라이언트에게 전달

* 호출 순서

  1. 웹 브라우저가 웹 앱 실행을 요청하면, 웹 서버가 그 요청을 받아서 **서블릿 컨테이너(톰캣 서버)**에 넘겨준다

     서블릿 컨테이너는 URL을 확인하여 그 요청을 처리할 서블릿을 찾아 실행

  2. 서블릿은 실제 업무를 처리할 모델 자바 객체의 메서드 호출

     모델 객체는 **엔터프라이즈 자바빈(EJB: Enterprise JavaBeans)** 또는 **일반 자바객체(POJO로 된 Service 또는 DAO object)**일 수 있다

  3. 모델 객체는 JDBC를 사용하여 매개변수로 넘어온 값을 DB에 저장하거나 질의 결과를 가져와 VO 객체로 변환한다

  4. 서블릿은 모델 객체로부터 받은 VO 값을 JSP에 전달

  5. JSP는 서블릿으로부터 받은 값을 참조하여 결과화면을 만들어 전달

  6. 웹 브라우저는 서버로부터 받은 응답 내용을 화면에 출력



#### Front Controller 패턴 아키텍처

* Front Controller : 맨 앞 단에서 모든 요청을 받아 처리하는 역할(Servlet 또는 JSP로 개발)
* Front Controller가 클라이언트에서 보낸 요청을 받아 공통적인 작업을 먼저 수행한 후 **이후 애플리케이션 Controller에게 작업을 위임**
* 애플리케이션 Controller는 클라이언트에게 보낼 뷰를 선택하여 최종 결과 생성하는 작업을 함
* **인증이나 권한체크**처럼 모든 요청에 대하여 공통적으로 처리해야 하는 로직이 있을 경우 사용 (클라이언트의 요청을 집중적으로 관리하고자 할 때)



# Spring MVC 개념

#### Spring MVC의 특징

* 서블릿 기반의 웹 개발을 위한 MVC 프레임워크 제공
* 모델2 아키텍처, Front Controller 패턴을 제공
* Spring이 제공하는 트랜젝션 처리, DI, AOP 등을 손쉽게 사용할 수 있음



#### Spring MVC와 Front Controller 패턴

* 대부분의 MVC 프레임워크들은 Front Controller 패턴 적용하여 구현

* Spring MVC도 Front Controller 역할을 하는 **DispatcherServlet이라는 클래스**를 계층 맨 앞단에 놓고,

  **서버로 들어오는 모든 요청을 받아서 처리하도록 구성**

* 예외가 발생했을 때 일관된 방식으로 처리하도록 하는 것도 Front Controller의 역할



#### DispatcherServlet 클래스

* Front Controller 패턴이 적용된 클래스
* web.xml에 설정 필요
* 클라이언트로부터 **모든 요청**을 전달받음
* Controller나 View같은 Spring MVC 구성요소를 이용해 클라이언트에게 서비스 제공

* 호출 순서
  1. DispatcherServlet이 들어오는 요청을 맨 앞단에서 받아 Controller(개발자가 작성하는 클래스)에게 요청을 위임
  2. Controller클래스가 요청 핸들링, DB 연동의 결과물을 모델 객체에 담아 DispatcherServlet에게 전달
  3. DispatcherServlet은 받은 모델 객체를 View에게 전달
  4. View 템플릿은 화면을 그리고 결과를 DispatcherServlet에게 전달
  5. DispatcherServlet은 브라우저에게 응답 결과를 전달



#### Spring MVC의 주요 구성 요소 (🚨: 개발자가 주로 사용하게 되는 클래스)

* DispatcherServlet 🚨

  : 클라이언트의 요청을 받아 Controller, View에게 알맞게 전달

* HandlerMapping

  : 요청 URL, 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정,

    DispatcherServlet은 하나 이상의 HandlerMapping을 가질 수 있음

* Controller 🚨

  : 클라이언트의 요청 처리 후 Model 호출, 그 결과를 DispatcherServlet에게 전달

* ModelAndView 🚨

  : Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체

* View 🚨

  : Controller의 처리 결과 **화면에 대한 정보**를 보유

* ViewResolver

  : Controller가 리턴한 View 이름을 기반으로 어떤 View를 실행할지 결정해주는 역할



#### Spring MVC 주요 구성 요소의 요청 처리 과정 

1. 클라이언트의 요청이 DispatcherServlet에게 전달
2. DispatcherServlet은 HandlerMapping를 통해 클라이언트의 요청을 처리할 Controller를 알아냄
3. DispatcherServlet은 Controller 객체를 이용하여 클라이언트의 요청 처리
4. Controller는 **요청 처리 결과, View 페이지 정보를 담은 ModelAndView 객체** 반환
5. DispatcherServlet은 ViewResolver를 통해 응답 결과를 생성할 View 객체를 알아냄
6. View는 클라이언트에게 전송할 응답 생성하여 DispatcherServlet에게 반환



# Spring MVC 기반 웹 애플리케이션 개발

#### 앞으로의 작성 절차

1. 클라이언트의 요청을 받는 DispatcherServlet을 web.xml에 설정
2. 클라이언트의 요청을 처리할 Controller 작성
3. Spring Bean으로 Controller 등록
4. JSP를 이용해 View 영역의 코드 작성
5. Browser상에서 JSP 실행



