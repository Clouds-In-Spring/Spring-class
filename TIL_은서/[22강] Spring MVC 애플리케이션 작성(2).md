# 특정 사용자 조회 기능 구현

#### ViewResolver 설정

* Controller의 실행 결과를 **어떤 View에서 보여줄 것인지를 결정**하는 기능
* **InternalResourceViewResolver**는 JSP를 사용하여 View를 생성해주는 클래스

```xml
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/" />
    <property name="suffix" value=".jsp" />
</bean>
```

* Controller에서 포워딩 되는 .jsp 확장자를 생략할 수 있음

* prefix는 **Controller가 리턴한 View 이름 앞**에 붙을 접두어

  suffix는 **Controller가 리턴한 View 이름 뒤**에 붙을 확장자

* Controller가 처리 결과를 보여줄 View의 이름으로 "hello"를 사용했다면 InternalResourceViewResolver에 의해 사용되는 View는 "/hello.jsp"가 된다



##### UserController.java

```java
@RequestMapping("/getUserList.do")
	public String getUserList(Model model) {
		List<UserVO> userList = userService.getUserList();
		model.addAttribute("userList", userList);
		return "userList";		✔.jsp 제거
	}
```





#### Controller를 위한 핵심 어노테이션 @RequestParam

##### @RequestParam은 HTTP 요청에 포함된 **파라미터 참조 시 사용**된다



##### userList.jsp

```jsp
<a href="getUser.do?id=${user.userId}">${user.userId}</a>
```



##### UserController.java

```java
@RequestMapping("/getUser.do")
	public ModelAndView getUser(@RequestParam String id) {
		UserVO user = userService.getUser(id);
		return new ModelAndView("userInfo", "user", user);
	}
```

* Controller의 @RequestParam 변수의 이름과 jsp의 요청 url의 id값이 동일해야 한다

> ModelAndView 🚨
>
> : Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체



##### userInfo.jsp

```jsp
<body>
	<div class="container">
		<h2 class="text-center">사용자 상세정보</h2>
		<table class="table table-bordered table table-hover"> 
			<tr><td>아이디 :</td><td>${user.userId}</td></tr>
			<tr><td>이름 :</td><td>${user.name}</td></tr>
			<tr><td>성별 :</td><td>${user.gender}</td></tr>
			<tr><td>거주지 :</td><td>${user.city}</td></tr>
		</table>
	</div>
</body>
```



#### View에 데이터와 화면정보를 전달하는 ModelAndView 클래스

`ModelAndView(viewName: String, modelName: String, modelObject: Object)`

`ModelAndView(viewName: String)`

* Controller에서 **Service를 호출한 결과를 받아 View에 전달하기 위해**, 전달 받은 **데이터와 화면정보**를 ModelAndView 객체에 저장한다
* ModelAndView 클래스의 생성자나 setViewName()클래스를 이용하여 View 이름을 지정할 수 있다
* addObject(String name, Object value) 메서드를 이용해 View에 전달할 데이터를 저장할 수 있다
* 위에서 작성한 **UserController.java**에서 `ModelAndView("userInfo", "user", user);`는 `userInfo` View에 VO객체인 user를 `user` Model에 저장하여 전달하겠다는 뜻





#### Controller와 JSP 호출 순서

1. userList.jsp의 \<a>태그의 링크를 클릭하면 UserController의 getUser.do라는 요청 URL과 매핑되는 getUser()라는 메서드가 호출된다
2. getUser() 메서드는 Service의 getUser()메서드를 호출하고 해당 메서드는 DB와 연동하여 특정 사용자의  정보를 UserVO객체에 담아 반환한다
3. Controller가 해당 객체를 받아 모델에 저장하고 ModelAndView를 통해 전달할 데이터와 화면정보를 저장
4. userInfo.jsp는 Controller에서 반환된 ModelAndView 정보를 읽어 화면 출력





# 사용자 등록 - 화면 구현

#### Controller와 JSP 호출 순서

1. userLIst.jsp에서 \<a>태그의 링크를 클릭하면 UserController의 insertUserForm.do라는 요청 URL과 매핑되는 insertUserForm() 메서드 호출
2. 화면정보와 화면에서 쓰일 데이터를 ModelAndView로 저장
3. userInsert.jsp로 포워딩되고 ModelAndView로 데이터를 읽어온다



#### Controller와 JSP 구현

##### UserController.java

```java
@RequestMapping("/insertUserForm.do")
	public ModelAndView insertUserForm() {
		List<String> genderList = new ArrayList<String>();
		genderList.add("남");
		genderList.add("여");
		
		List<String> cityList = new ArrayList<String>();
		cityList.add("서울");
		cityList.add("경기");
		cityList.add("부산");
		cityList.add("대구");
		cityList.add("제주");
		
		Map<String, List<String>> map = new HashMap<>();
		map.put("genderList", genderList);
		map.put("cityList", cityList);
		
		return new ModelAndView("userInsert", "map", map);
	}
```

* map을 출력해보면 **{cityList=[서울, 경기, 부산, 대구, 제주], genderList=[남, 여]}**

> 🚨 **Map**
>
> key-value 형식, 파이썬의 dict와 비슷
>
> 
>
> 🤔 **HashMap**
>
> Map은 인터페이스로 선언되어있고 Map을 구현한 여러 클래스들이 존재한다
>
> **HashMap은 Map의 클래스 중 하나**
>
> (Map에서 가장 많이 쓰이는 클래스는 HashMap, TreeMap, LinkedHashMap)
>
> HashMap은 key 또는 value에 null값 저장 가능



##### userInsert.jsp

```jsp
<body>
	<div class="container">
		<h2 class="text-center">사용자 등록</h2>
		<table class="table table-bordered table table-hover"> 
			<tr>
				<td>성별 :</td>
				<td><c:forEach var="genderName" items="${map.genderList}">
					<input type="radio" name="gender" value="${genderName}">${genderName}
				</c:forEach></td>
			</tr>
			<tr>
				<td>거주지 :</td>
				<td><select name="city">
					<c:forEach var="cityName" items="${map.cityList}">
						<option value="${cityName}">${cityName}</option>
					</c:forEach>
				</select></td>
			</tr>
		</table>
	</div>
</body>
```

* `<c:forEach var="genderName" items="${map.genderList}">` 

  `${map.genderList}`의 값들을 하나씩 꺼내와 `genderName` 변수에 넣는다





# 사용자 등록 - 기능 구현

#### Controller를 위한 핵심 어노테이션 @ModelAttribute

* HTTP 요청에 포함된 파라미터를 모델 객체로 바인딩

  사용자가 폼에서 입력한 값(요청 파라미터)을 Controller의 UserVO 객체로 **자동으로** 변환, 저장

  🚨 **\<input>태그의 이름과 UserVO의 변수명이 일치해야 한다** 

##### 

#### Controller와 JSP 호출 순서

1. userInsert.jsp의 등록버튼 클릭
2. Controller의 insertUser() 메서드 호출된 후 UserService의 insertUser()메서드 호출
3. return된 결과값을 userList.jsp로 표시



##### userInsert.jsp - \<form> 태그 추가

```jsp
<body>
	<div class="container">
		<h2 class="text-center">사용자 등록</h2>
		<form method="post" action="insertUser.do">
			<table class="table table-bordered table table-hover"> 
				<tr>
					<td>아이디:</td>
					<td><input type="text" name="userId"  /></td>
				</tr> 
				<tr>
					<td>이름:</td>
					<td><input type="text" name="name" /></td>
				</tr>
				<tr>
					<td>성별 :</td>
					<td><c:forEach var="genderName" items="${map.genderList}">
						<input type="radio" name="gender" value="${genderName}">${genderName}
					</c:forEach></td>
				</tr>
				<tr>
					<td>거주지 :</td>
					<td><select name="city">
						<c:forEach var="cityName" items="${map.cityList}">
							<option value="${cityName}">${cityName}</option>
						</c:forEach>
					</select></td>
				</tr>
				<tr>
					<td colspan="2"  class="text-center">
						<input type="submit" value="등록" /></td>
					</tr>
					<tr>					
						<td colspan="2" class="text-center"><a href="getUserList.do">사용자 목록보기</a></td>
					</tr>
			</table>
		</form>
	</div>
</body>
```



##### UserController.java

```java
@RequestMapping("/insertUser.do")
	public String insertUser(@ModelAttribute UserVO user) {
		if (user != null) {
			userService.insertUser(user);
		}
		return "redirect:/getUserList.do";
	}
```





#### CharacterEncodingFilter 클래스 설정

요청 데이터를 인코딩해주는 Filter 클래스



##### web.xml

```xml
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>EUC-KR</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>*.do</url-pattern>
</filter-mapping>
```



**🚨 설정 후 서버 재시작 필요**