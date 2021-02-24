# 사용자 관리 RESTful 웹서비스 개요

#### 사용자 관리 RESTful 웹서비스 URI와 Method

* 사용자 목록(/users) - GET
* 사용자 보기(/users/{id}) - GET
* 사용자 등록(/users) - POST
* 사용자 수정(/users) - PUT
* 사용자 삭제(/users/{id}) - DELETE



#### RESTful Controller를 위한 핵심 어노테이션

* 클라이언트에서 전송한 XML이나 JSON 데이터를 Controller <-> Java 객체로 변환하여 주고 받을 수 있는 기능 제공

  **@RequestBody** - **HTTP Request Body(요청 몸체)를 Java 객체로** 전달받을 수 있음

  **@ResponseBody** - **Java 객체를 HTTP Response Body(응답 몸체)로** 전송할 수 있음

  

> 🚨 @ResponseBody가 있는 getByIdInJSON() (ID값을 통해 데이터를 JSON형태로 받는 메서드) 메서드
>
> : MappingJacksonHttpMessageConverter가 리턴값인 UserModel 객체를 JSON으로 변환하는 작업 처리
>
> 
>
> 🚨 @ResponseBody가 없는 경우
>
> : ViewResolver에 의해 선택된 /user.jsp가 포워드되어지고, user.jsp에서 UserModel 객체 참조





# 사용자 정보 조회 및 등록 기능 구현

#### 1. 사용자 정보 조회 : 사용자 목록 조회 메서드 구현

##### RestfulUserController.java

```java
@RequestMapping(value="/users", method=RequestMethod.GET)
@ResponseBody
public Map getUserList() {
    List<UserVO> userList = userService.getUserList();
    Map result = new HashMap();
    result.put("result", Boolean.TRUE);
    result.put("data", userList);
    return result;
}
```

* UserService를 호출해서 받은 결과값(list 객체)을 "data"라는 이름으로 Map에 저장
* @ResponseBody를 통해 리턴시킨 Java객체(Map)를 JSON 포맷으로 변환



##### POSTMAN으로 Body 확인

**🚨 500 내부서버 오류 발생**

**➡ 의존성에 Jackson Databind 추가**

이후 JSON형식의 데이터 잘 넘어오는 것 확인



#### 2. 사용자 정보 조회 : 특정 사용자 조회 메서드 구현

##### RestfulUserController.java

```java
@RequestMapping(value="/users/{id}", method=RequestMethod.GET)
@ResponseBody
public Map getUser(@PathVariable String id) {
    UserVO user = userService.getUser(id);
    Map result = new HashMap();
    result.put("result", Boolean.TRUE);
    result.put("data", user);
    return result;
}
```



##### POSTMAN으로 Body 확인

http://localhost:8000/SpringWebPrj/users/dooli



#### 3. 사용자 정보 등록 : 사용자 등록 메서드 구현

##### RestfulUserController.java

```java
@RequestMapping(value="/users", method=RequestMethod.POST,
                headers= {"Content-type=application/json"})
@ResponseBody
public Map insertUser(@RequestBody UserVO user) {
    if(user!=null) {
        userService.insertUser(user);
    }
    Map result = new HashMap();
    result.put("result", Boolean.TRUE);
    return result;
}
```

* 입력을 통해 들어온 JSON 값을 @RequestBody를 통해 Java 객체로 변환
* 변환한 값을 @ResponseBody를 통해 JSON 객체로 변환





# 사용자 정보 수정 및 삭제 기능 구현

#### 사용자 정보 수정 : 사용자 수정 메서드 구현

##### RestfulUserController.java

```java
@RequestMapping(value="/usrs", method=RequestMethod.PUT, 
                headers= {"content-Type=application/json"})
@ResponseBody
public Map updateUser(@RequestBody UserVO user) {
    if(user!=null) {
        userService.updateUser(user);
    }
    Map result = new HashMap();
    result.put("result", Boolean.TRUE);
    return result;
}
```

* **PUT 메서드 이용**

  > HTTP의 PUT은 리소스의 위치가 지정되었을 때 **생성 또는 업데이트**를 위해 사용
  >
  > POST와 다른 점은 같은 연산을 연속해서 수행하면 **POST는 매번 다른 곳에 새로운 리소스를 생성**하는 반면 PUT은 생성 또는 업데이트를 시킨다



#### 사용자 정보 삭제 : 사용자 삭제 메서드 구현

##### RestfulUserController.java

```java
@RequestMapping(value="/users/{id}", method=RequestMethod.DELETE)
@ResponseBody
public Map deleteUser(@PathVariable String id) {
    userService.deleteUser(id);

    Map result = new HashMap();
    result.put("result", Boolean.TRUE);
    return result;
}
```

