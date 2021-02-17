# 사용자 수정화면 구현

#### Controller와 JSP 호출 순서

1. userList.jsp에서 수정하기 링크 클릭
2. 수정 전 DB에서 사용자 정보를 가져와야 하기 때문에 UserService의 getUser(id) 호출
3. 리턴된 UserVO객체와 함께 updateUserForm()메서드 호출
4. userUpdate.jsp 페이지 포워딩



#### 구현

##### userList.jsp

```jsp
<a href="updateUserForm.do?id=${user.userId}">수정</a>
```



##### UserController.java

```java
@RequestMapping("/updateUserForm.do")
	public ModelAndView updateUserForm(@RequestParam String id) {
		UserVO user = userService.getUser(id);
		List<String> genderList = new ArrayList<String>();
		genderList.add("남");
		genderList.add("여");
		
		List<String> cityList = new ArrayList<String>();
		cityList.add("서울");
		cityList.add("경기");
		cityList.add("부산");
		cityList.add("대구");
		cityList.add("제주");
		
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("genderList", genderList);
		map.put("cityList", cityList);
		map.put("user", user);
		return new ModelAndView("userUpdate", "map", map);
	}
```

 

##### userUpdate.jsp

```jsp
<body>
	<div class="container">
		<h2 class="text-center">사용자 정보 수정</h2>
		<form method="post" action="insertUser.do">
			<input type="hidden" name="userId" value="${map.user.userId}" />
			<table class="table table-bordered table table-hover"> 
				<tr>
					<td>아이디:</td>
					<td>${map.user.userId}</td>
				</tr> 
				<tr>
					<td>이름:</td>
					<td><input type="text" name="name" value="${map.user.name}" /></td>
				</tr>
				<tr>
					<td>성별 :</td>
					<td>
						<c:forEach var="genderName" items="${map.genderList}">
							<c:choose>
								<c:when test="${genderName eq map.user.gender}">
									<input type="radio" name="gender" value="${genderName}" checked="checked">${genderName}
								</c:when>
								<c:otherwise>
									<input type="radio" name="gender" value="${genderName}">${genderName}
								</c:otherwise>
							</c:choose>
						</c:forEach>
					</td>
				</tr>
				<tr>
					<td>거주지 :</td>
					<td>
						<select name="city">
							<c:forEach var="cityName" items="${map.cityList}">
								<c:choose>
									<c:when test="${cityName eq map.user.city}">
										<option value="${cityName}" selected>${cityName}</option>
									</c:when>
									<c:otherwise>
										<option value="${cityName}">${cityName}</option>
									</c:otherwise>
								</c:choose>
							</c:forEach>
						</select>
					</td>
				</tr>
				<tr>
					<td colspan="2"  class="text-center">
						<input type="submit" value="등록" /></td>
					</tr>
			</table>
		</form>
	</div>
</body>
```

> **\<c:choose>와 \<c:when>, \<c:otherwise>**
>
> 조건에 따른 분기, 일치하는 조건이 없다면 otherwise 블럭 수행
>
> - test: 비교연산자 입력





# 사용자 수정 및 삭제 기능 구현

#### 사용자 정보 수정: Controller와 JSP 호출 순서

1. userUpdate.jsp에서 수정할 내용 입력 후 수정 버튼 클릭
2. Controller의 updateUser()메서드 호출
3. updateUser()메서드는 값을 담아 UserService의 updateUser()호출
4. 업데이트 처리 후 리다이렉트를 위해 getUserList()호출되고 userList.jsp로 포워딩



#### 사용자 정보 수정: 구현

##### UserController.java

```java
@RequestMapping("/updateUser.do")
public String updateUser(@ModelAttribute UserVO user) {
    userService.updateUser(user);
    return "redirect:/getUserList.do";
}
```



#### 사용자 삭제

##### @PathVariable

* 파라미터를 쿼리스트링(?id=${userid})이 아닌 **URL 형식**(deleteUser.do/${user.userId})으로 받을 수 있도록 해줌
* **🚨🚨 RESTful API에 많이 사용되는 어노테이션 🚨🚨**



##### userList.jsp

```jsp
<a href="deleteUser.do/${user.userId}">삭제</a>
```



##### UserController.java

```java
@RequestMapping("/deleteUser.do/{id}")
public String deleteUser(@PathVariable String id) {
    userService.deleteUser(id);
    return "redirect:/getUserList.do";
}
```

> `RequestMapping("/deleteUser.do/{id}")`의 id는 자동으로 `deleteUser(@PathVariable String id)`의 id와 매핑이 된다



##### web.xml

##### @PathVariable 사용을 위해 DispatcherServlet의 url-patter 변경 필요

```xml
<servlet-mapping>
    <servlet-name>springDispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>	✔ *.do에서 /로 수정
</servlet-mapping>
```





# Spring MVC의 예외 처리

#### @ExceptionHandler 어노테이션 사용

* Controller의 메서드에서 예외가 발생했을 때 예외 처리를 할 수 있다
* 예외가 발생했을 때, **예외 Type, Message를 보여주는 JSP 페이지**(viewError.jsp) 작성 필요



##### viewError.jsp

```jsp
<%@ page isErrorPage = "true" %>			✔ 선언해주지 않으면 exception 사용 불가!
...

<body>
요청 처리 과정에서 에러가 발생했습니다.<br>
빠른 시간 내에 문제를 해결하도록 하겠습니다.
<p>
에러 타입: <%= exception.getClass().getName() %> <br>
에러 메시지: <b><%= exception.getMessage() %></b>
<hr>
<a href="${pageContext.request.contextPath}">Home</a>
</body>
```



##### 에러를 발생시켜보자

#### config > user.xml

```xml
<select id="selectUserById" parameterType="string" resultType="User">
    select * from users where userid1=#{value}	✔ 에러발생시키기위해 userid를 userid1로
</select>
```



이후 user 상세목록 클릭시 에러 발생하면서 viewError.jsp 뜬다



