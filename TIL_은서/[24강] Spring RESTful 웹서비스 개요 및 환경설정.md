# RESTful 웹서비스 개요

#### Open API(Application Programming Interface)란?

* API는 응용프로그램을 개발할 때 사용하는 인터페이스라는 의미
* 프로그래밍에서 사용할 수 있는 개방되어 있는 상태의 인터페이스를 말함
* 대부분의 Open API는 REST 방식으로 지원되고 있다.



#### REST(REpresentational Safe Transfer)란?

* **HTTP URI + HTTP Method**

  HTTP URI를 통해 제어할 자원을 명시하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍처

| HTTP Method | CRUD             |
| ----------- | ---------------- |
| POST        | Create(Insert)   |
| GET         | READ(Select)     |
| PUT         | Update or Create |
| DELETE      | Delete           |



#### RESTful API란?

* **REST의 원리를 따르는 시스템**을 RESETful이란 용어로 지칭함

* 기존의 웹 접근 방식과의 차이점

  * **기존 방식**

    GET/list.do?no=510&name=java		➡ **쿼리스트링**으로 제어하려는 자원 표현

    GET과 POST만으로 자원에 대한 CRUD 처리(**글읽기-GET / 글등록-POST / 글삭제-GET / 글수정-POST**)

  * **RESTful 방식**

    GET/bbs/java/510	➡  **URI**로 제어하려는 자원 표현

    4가지 메서드 모두 사용(**글읽기-GET / 글등록-POST / 글삭제-DELETE / 글수정-PUT**)





# JSON과 XML

#### JSON(JavaScript Object Notation)이란?

* 사람과 기계 모두 이해하기 쉬우며 용량이 작아 XML을 대체하여 데이터 전송 등에 많이 사용
* 대부분의 프로그래밍 언어에서 JSON 포맷의 데이터를 핸들링 할 수 있는 라이브러리 제공



#### JSON 형식

* name-value 형식의 쌍

  ```json
  {
  	"firstName": "Brett",
  	"lastName": "McLaughlin",
  	"email": "brett@newInstance.com"
  }
  ```

* 값들의 순서화된 리스트 형식

  ```json
  {
  	"hobby": ["puzzles", "swimming"]
  }
  ```



#### JSON 라이브러리 - Jackson

* Jackson은 JSON 형태를 Java 객체로, Java객체를 JSON 형태로 변환해주는 **Java용 JSON 라이브러리**
* 가장 많이 사용하는 JSON 라이브러리





#### XML(eXtensible Markup Language)란?

* 데이터를 저장하고 전달하기 위한 언어

* 인간/기계 모두에게 읽기 편한 언어이며 데이터의 구조와 의미를 설명한다

* HTML과 같은 Markup Language

  * HTML

    : Data를 **표현**하는 것에 포커스, 미리 정의된 Tag만 사용 가능

  * XML

    : Data를 **전달**하는 것에 포커스, 사용자가 마음대로 Tag 정의 가능



#### XML의 Tree 구조

* XML문서는 "root"에서 시작해서 "leaves"로 뻗어가는 트리 구조

* XML 버전과 문자 인코딩을 정의하는 선언부 존재

  XML은 어떠한 데이터를 설명하기 위해 임의로 작성한 Tag로 데이터를 감싼다

```xml
< ?xml version="1.0" encoding="UTF-8"?>
<customer>
    <name>홍길동</name>
    <addr>서울</addr>
</customer>

```





# Spring MVC 기반 RESTful 웹서비스 환경설정

#### 1. Jackson mapper Library 설치

pom.xml에 추가



#### 2. web.xml의 DispatcherServlet url-pattern 변경

*.do➡/

앞에서 이미 했음



#### 3. beans-web.xml 설정

```xml
<mvc:annotation-driven />
```

> **Spring MVC에 필요한 Bean들을 자동으로 등록해주는 태그**



```
<mvc:default-servlet-handler/>
```

> DispatcherServlet의 변경된 url-pattern때문에 필요
>
> url-pattern을 "/"로 설정하게되면 tomcat의 server.xml에 정의되어있는 url-pattern "/"을 무시하게된다.
>
> 이로 인해 DefaultServlet이 동작할 수 없게 된다.
>
> 이를 해결하기 위함임





#### 앞으로의 구현 절차

1. RESTful 웹서비스를 처리할 RestulController 클래스 작성, Spring Bean으로 등록

2. 요청을 처리할 메서드에 @RequestMapping, @RequestBody, @ResponseBody 어노테이션 선언

   > **@RequestMapping**
   >
   > : 메서드와 URI 매핑
   >
   >  **@RequestBody**
   >
   > : 클라이언트 요청에서 들어오는 JSON을 java 객체로 변환
   >
   > **@ResponseBody**
   >
   > : java 객체를 JSON 포맷으로 변환

3. REST Client Tool(**Postman**)을 사용하여 각각의 메서드 테스트

4. Ajax 통신을 통해 RESTful 웹서비스를 호출하는 HTML 페이지 작성