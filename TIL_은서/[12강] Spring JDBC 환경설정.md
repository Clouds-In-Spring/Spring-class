# Spring JDBC 설치 및 DataSource 설정

#### pom.xml에 Oracle Jdbc Driver 의존성 추가 후 Spring JDBC 추가



#### DataSource 설정: DataSource를 Bean으로 등록하여 사용

##### value.properties

```properties
db.driverClass=oracle.jdbc.OracleDriver
db.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
db.username= 
db.password= 
```



##### beans.xml

```xml
<!-- DataSource 설정 -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
<property name="driverClass" value="${db.driverClass}"/>
<property name="url" value="${db.url}" />
<property name="username" value="${db.username}" />
<property name="password" value="${db.password}" />
</bean>
```

```xml
<!-- user 패키지를 scan하도록 설정 -->
<context:component-scan base-package="myspring.user" />
```



#### DataSource 설정 테스트

##### UserClient.java

```java
@Test
public void dataSourceTest() {
	DataSource ds = (DataSource) context.getBean("dataSource");
	try {
		System.out.println(ds.getConnection());
	} catch (SQLException e) {
		e.printStackTrace();
	}
}
```
> e.printStackTrace() : 에러 메시지의 발생 근원지를 찾아서 단계별로 에러 출력



#### 사용자 조회 테스트

##### UserClient.java

```java
@Autowired
UserService service;

@Test
public void getUserTest() {
    UserVO user = service.getUser("gildong");
    System.out.println(user);
    assertEquals("홍길동", user.getName());
}
```

> @Autowired를 통해 UserService를 자동으로 주입 받고 UserService의 getUser를 통해 "gildong"이라는 id를 가진 값을 리턴받는다
>
> UserVO 클래스의 getName() 메서드를 통해 userName을 리턴받는다



#### 사용자 등록 및 목록조회 테스트

##### UserClient.java

```java
@Test
public void insertUserTest() {
    service.insertUser(new UserVO("dooly", "둘리", "남", "경기"));
	
    // user 테이블 출력
    for (UserVO user : service.getUserList()) {
    	System.out.println(user);
    }
}
```

> **Service**의 insertUser() 메서드는 **UserDao**의 insert()메서드를 사용한다
>
> **Service**의 getUserList() 메서드는 **UserDao**의 readAll()메서드를 사용한다
>
> **➡ 서비스 계층은 다른 계층들과 통신하기 위한 인터페이스 제공**



#### 사용자 정보수정 테스트

##### UserClient.java

```java
@Test
public void updateUserTest() {
    service.updateUser(new UserVO("dooly", "김둘리", "여", "부산"));

    UserVO user = service.getUser("dooly");
    System.out.println(user);
}
```

>  **Service**의 updateUser() 메서드는 **UserDao**의 update()메서드를 사용한다



#### 사용자 정보삭제 테스트

```java
@Test
public void deleteUserTest() {
	service.deleteUser("dooly");
	
	// user 테이블 출력
    for (UserVO user : service.getUserList()) {
    System.out.println(user);
    }
}
```

