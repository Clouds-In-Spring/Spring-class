# 21 Spring MVC 어플리케이션 작성 1

### 학습  목표

- EL과 JSTL
- Hello 컨트롤러 작성
- 사용자 목록 조회 컨트롤러 작성

### 1 EL과 JSTL

- EL과 JSTL(java standard tag library)을 사용하면 <% %>와 같은 스크립팅 태그를 JSP에서 없앨 수 있다.

#### EL(Expression Language)

- EL 표현식은 중괄호({})로 묶고 앞에 달러($)기호를 붙이며, 도트 연산자를 사용

- EL은 저장 객체의 출력을 단순화 하는 용도로 사용되므로, 저장 객체를 출력할 때도 스크립팅을 전혀 쓰지 않는다.

  ~~~
  예를 들어, <% =request.getParameter("name")%> 대신에 ${param.name} 구문을 사용
  ~~~

- Scope(page, request, session, application)의 객체에 접근하여 출력 처리

- EL에서는 해당값이 null이거나 공백일 경우에는 아무 내용도 표시하지 않고 에러도 발생하지 않는다
- JSP에서 기본으로 지원, JSTL은 따로 설치해야 한다

#### EL과 스크립팅 태그 비교

1. `${param.name}`

   = `<%=request.getParameter("name")%>`

2. `${greet}`

   = `<% String value = (String)request.getAttribute("greet"); out.println(value); %>`

3. `${user}`

   = `<% UserVO user = (UserVO)request.getAttribute("user"); out.println(user); %>`

4. `${user.name}`

   = `<% UserVO user = (UserVO)request.getAttribute("user"); out.println(user.getName()); %>`

5. `${sessionScope.user.name}`

   = `<% UserVO user = (UserVO)session.getAttribute("user"); out.println(user.getName()); %>`

#### JSTL 개요

- JSP 표준 라이브러리
-  JSP에서 스크립팅 사용하지 않으면서, 루프를 돌리거나 조건물을 실행할 수 있도록 해줌
- JSP에서 자주 사용하는 기능을 구현해 좋은 Custom Tag Library 모음
- EL을 사용하여 표현
- EL과 다르게 따로 설치 

- 5개의 라이브러리

  | 라이브러리 | 기능                                        | URL                                    | 접두어 |
  | ---------- | ------------------------------------------- | -------------------------------------- | ------ |
  | 코어       | 변수선언, 조건/제어/반복문 기능             | http://java.sun.com/jsp/jstl/core      | C      |
  | 포맷팅     | 숫자 날짜 시간을 포맷팅, 국제화, 다국어지원 | http://java.sun.com/jsp/jstl/fmt       | fmt    |
  | 함수       | 문자열 처리                                 | http://java.sun.com/jsp/jstl/functions | fn     |
  | db         | 입력 수정 삭제 삭제                         | http://java.sun.com/jsp/jstl/sql       | sql    |
  | xml        | xml 문서 처리할때 필요한 기능 제공          | http://java.sun.com/jsp/jstl/xml       | x      |

- 사용법

JSP 페이지의 맨 위에 선언부에 **접두어**와 **URL 식별자**로 라이브러리 선언 후 사용

```
<!-- 선언부 -->
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"  %>

<!-- 사용 -->
<c:set var="hello" value="Hello" />
${hello}
```

#### JSTL Core 태그

- JSTL 태그 라이브러리중 가장 많이 사용 
- `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`로 선언
- 변수지원
  - set : JSP에서 사용될 변수 설정
  - remove : 설정할 변수 제거
- 흐름제어
  - if : 조건에 따라 내부 코드 수행
  - choose : 다중 조건 처리할 때 사용
  - forEach : 컬렉션이나 Map의 각 항목을 처리할 때 
  - forTokens : 구분자로 분리된 각각의 토큰을 처리할 때
- URL 처리
  - import : URL을 사용하여 다른 자원의 결과를 삽입
  - redirect : 지정한 경로로 리다이렉트
  - url : URL 재작성
- 기타
  - catch : 익셉션 처리 사용
  - out : JspWriter에 내용을 알맞게 처리한 후 출력

### 2 Hello 컨트롤러 작성

#### Controller와 JSP 구현 절차

1. 클라이언트의 요청을 처리할 POJO 형태의 HelloController 클래스 작성
2. Controller클래스에 @Controller 어노테이션 선언
3. 요청을 처리할 메서드 작성하고 @RequestMapping 어노테이션 선언
4. JSP을 이용한 View 영역의 코드 작성
5. Browser 상에서 JSP 실행

#### Controller와 JSP   호출 순서

1. 브라우저에서 localhost:8080/SpringWebPrj/hello.do 요청

2. DispatcherServlet이 호출된다(*.do)

3. DispatcherServlet가 설정파일에 설정된 Controller를 호출해준다

   (Controller와 의존관계에 있는(@Autowired) 메서드도 함께 호출됨)

4. 반환된 값은 View에서 사용될 수 있게하기 위해 Model클래스를 통해 저장한다

5. Controller는 jsp페이지를 호출하고 jsp페이지는 Model에 저장된 데이터값을 불러온다

#### View에 데이터를 전달하는 Model 클래스

- Controller에서 Service를 **호출한 결과를 받아서 View에 전달하기 위해**, 전달받은 결과를 Model 객체에 저장
- `addAttribute(string name, Object value)` : value 객체를 name이라는 이름으로 저장하고, View에서 name으로 지정한 이름을 통해 value를 사용한다

#### Controller와 JSP 구현

##### HelloController.java

```
@Controller
public class HelloController {
	@Autowired
    private Hello helloBean;
    
	@RequestMapping("/hello.do")
	public String hello(Model model) {
		String msg = helloBean.sayHello();
		model.addAttribute("greet", msg);
		return "hello.jsp";
	}
}
```

- `@RequestMapping("/hello.do")` : hello.do라는 url로 요청이 들어오면 아래의 메서드로 매핑이 된다
- sayHello() 메서드를 호출하고 리턴 받은 문자열을 Model에 "greet"라는 이름으로 저장
- 결과를 보여줄 jsp페이지 명을 return

##### hello.jsp

```
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	${greet}
</body>
</html>
```

- controller에서 model에 저장한 attribute의 이름은 "greet"

