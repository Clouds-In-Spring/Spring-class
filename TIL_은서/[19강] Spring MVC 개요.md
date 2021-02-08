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

11분 41초