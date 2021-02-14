# EL과 JSTL

##### EL과 JSTL을 사용하면 `<% %>` 같은 스크립팅 태그를 JSP에서 없앨 수 있다.

> <% %>는 JSP에서 Java 코드를 기술할 때 사용



#### EL(Expression Language)의 개요

* `${ }` 사용

* 저장 객체의 출력을 단순화 하는 용도로 사용

  ex) `<%=request.getParameter("name")%>` 대신 `${param.name}` 사용

* 4가지 Scope(page, request, session, application) **객체에 접근하여 출력 처리**
* 해당값이 null이거나 공백일 경우에는 아무 내용도 표시하지 않으며 에러도 발생시키지 않는다
* JSP에서 기본적으로 지원



#### 예시를 통한 EL 과 스크립팅 태그 비교

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



#### JSTL(Java Standard Tag Library)의 개요

* JSP 표준 라이브러리
* 스크립팅을 사용하지 않으면서 **루프를 돌리거나 조건문을 실행할 수 있도록** 해줌
* EL을 사용하여 표현
* EL과 다르게 따로 설치 필요
* 5개의 라이브러리를 가짐
  1. 코어(접두어 c) : **변수 선언, 조건/제어/반복문** 기능제공
  2. 포맷팅(접두어 fmt) : 숫자, 날짜, 시간을 포맷팅하는 기능
  3. 함수(접두어 fn) : 문자열 처리하는 함수 제공
  4. 데이터베이스(접두어 sql) : 별로 안쓰임, DB를 입력/수정/삭제/조회하는 기능 제공
  5. XML처리(접두어 x) : XML 문서를 처리할 때 필요한 기능 제공

* 사용법

  JSP 페이지의 맨 위에 선언부에 **접두어**와 **URL 식별자**로 라이브러리 선언 후 사용

  ```jsp
  <!-- 선언부 -->
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"  %>
  
  <!-- 사용 -->
  <c:set var="hello" value="Hello" />
  ${hello}
  ```

  

#### JSTL Core 태그

* JSTL 태그 라이브러리 중 가장 많이 사용하는 태그
* `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`로 선언

* 변수지원 태그
  * set : JSP에서 사용될 변수 설정
  * remove : 설정한 변수를 제거

* 흐름제어 태그
  * if, choose, forEach(컬렉션이나 Map의 각 항목을 loop 처리할 때 사용), forTokens(구분자로 분리된 각각의 토큰을 처리할 때 사용)

* URL처리 태그
  * import : URL 사용하여 다른 자원(이미지, 페이지 등)의 결과를 삽입
  * redirect : 지정한 경로로 리다이렉트
  * url : URL을 재작성
* 기타 태그
  * catch : exception 처리
  * out : 문자열, 변수, 함수 등을 출력할 때 사용



# 컨트롤러 작성

#### Controller와 JSP 호출 순서

1. 브라우저에서 localhost:8080/SpringWebPrj/hello.do 요청

2. DispatcherServlet이 호출된다(*.do)

3. DispatcherServlet가 설정파일에 설정된 Controller를 호출해준다

   (Controller와 의존관계에 있는(@Autowired) 메서드도 함께 호출됨)

4. 반환된 값은 View에서 사용될 수 있게하기 위해 Model클래스를 통해 저장한다

5. Controller는 jsp페이지를 호출하고 jsp페이지는 Model에 저장된 데이터값을 불러온다



#### View에 데이터를 전달하는 Model 클래스

* Controller에서 Service를 **호출한 결과를 받아서 View에 전달하기 위해**, 전달받은 결과를 Model 객체에 저장
* `addAttribute(string name, Object value)` : value 객체를 name이라는 이름으로 젖아하고, View에서 name으로 지정한 이름을 통해 value를 사용한다



#### Controller와 JSP 구현

##### HelloController.java

```java
@Controller
public class HelloController {
	
	@RequestMapping("/hello.do")
	public String hello(Model model) {
		String msg = helloBean.sayHello();
		model.addAttribute("greet", msg);
		return "hello.jsp";
	}
}
```



##### hello.jsp

```

```

