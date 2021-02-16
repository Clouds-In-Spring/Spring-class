## 12 Spring JDBC 환경설정

#### 1 Spring JDBC 설치 및 DataSource 설정

###### pom.xml에 Oracle Jdbc Driver 의존성 추가 후 Spring JDBC 추가

~~~
       <!-- oracle jdbc -->
		<dependency>
			<groupId>com.oracle</groupId>
			<artifactId>ojdbc6</artifactId>
			<version>11.2.0.3</version>
		</dependency>
			
		<dependency>
		    <groupId>org.springframework</groupId>
		    <artifactId>spring-jdbc</artifactId>
		    <version>3.2.17.RELEASE</version>
		</dependency>
~~~

###### DataSource 설정 : DataSource를 Bean으로 등록하여 사용

- value.properties

  ~~~
  db.driverClass=oracle.jdbc.OracleDriver
  db.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
  db.username= scott
  db.password= tiger
  
  ~~~

- beans.xml

  ~~~
  	<!-- DataSource 설정 -->
  	<bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
  		<property name="driverClass" value="${db.driverClass}"/>
  		<property name="url" value="${db.url}" />
  		<property name="username" value="${db.username}" />
  		<property name="password" value="${db.password}" />
  	</bean>
  
  ~~~

#### 2 사용자 관리 프로젝트 실행

###### 사용자 관리 프로젝트의 Bean 등록 및 의존관계 설정

- <context:component-scan > 태그 사용
- @Service, @Repository 어노테이션을 선언한 클래스들과 @Autowired 어노테이션을 선언하여 의존관계를 설정한 클래스들이 위치한 패키지를 SCAN하기 위한 설정을 XML에 해주어야 한다

~~~
<!-- user 패키지를 scan하도록 설정 -->
<context:component-scan base-package="myspring.user" />
~~~

###### DataSource 설정 테스트

- UserClient.java

  ~~~
      @Test
      public void dataSourceTest() {
          DataSource ds = (DataSource) context.getBean("dataSource");
          try {
              System.out.println(ds.getConnection());
          } catch (SQLException e) {
              e.printStackTrace();
          }
      }
  ~~~

  - e.printStackTrace() : 에러 메시지의 발생 근원지를 찾아서 단계별로 에러 출력

###### 사용자 조회 테스트

- UserClient.java

  ~~~
  	@Test
  	@Ignore
  	public void dataSourceTest() {
  		DataSource ds = (DataSource) context.getBean("dataSource");
  		try {
  			System.out.println(ds.getConnection());
  		} catch (SQLException e) {
  			e.printStackTrace();
  		}
  	} // oracle.jdbc.driver.T4CConnection@134d26af 결과 도출
  ~~~

  - @Autowired를 통해 UserService를 자동으로 주입받고 UserService의 getUser를 통해 "gildong" 이라는 id를 가진 값을 리턴받는다
  - UserVO 클래스의 getName() 메서드를 통해 userName을 리턴받는다

###### 사용자 등록 및 목록조회 테스트

- UserClient.java

  ~~~
  	// 사용자 정보 조회
  	@Test
  	// @Ignore
  	public void getUserTest() { 
  		UserVO user = service.getUser("gildong");
  		System.out.println(user);
  		assertEquals("홍길동", user.getName());
  	} // User [userId=gildong, name=홍길동, gender=남, city=서울]
  	
  	
  	// 사용자 정보 삽입
  	@Test
  	// @Ignore
  	public void insertUserTest() {
  		service.insertUser(new UserVO("dooly", "둘리", "남", "경기"));
  
  		for (UserVO user : service.getUserList()) {
  			System.out.println(user);
  		}
  	}
  	/*
  	 	등록된 Record UserId=dooly Name=둘리
  		User [userId=gildong, name=홍길동, gender=남, city=서울]
  		User [userId=dooly, name=둘리, gender=남, city=경기]
  	 */
  ~~~

  - Service의 insertUser() 메서드는 UserDao의 insert() 메서드를 사용
  - Service의 getUserList() 메서드는 UserDao의 readAll() 메서드를 사용

###### 사용자 정보수정 테스트

- UserClient.java

  ~~~
  	@Test
  	// @Ignore
  	public void updateUserTest() {
  		service.updateUser(new UserVO("gildong", "홍길동2", "남2", "서울2"));
  
  		UserVO user = service.getUser("gildong");
  		System.out.println(user);
  	}
  	// 갱신된 Record with ID = gildong
  	// User [userId=gildong, name=홍길동2, gender=남2, city=서울2]
  ~~~

  - Service의 updateUser() 메서드는 UserDao의 update() 메서드를 사용

###### 사용자 정보삭제 테스트	

```
	@Test
	// @Ignore
	public void deleteUserTest() {
		service.deleteUser("dooly");

		for (UserVO user : service.getUserList()) {
			System.out.println(user);
		}
	}
	// 삭제된 Record with ID = dooly
	// User [userId=gildong, name=홍길동2, gender=남2, city=서울2]
```

#### 참고

- UserDaoImplJDBC.java
- UserServiceImpl.java

### 에러

##### 1. Oracle Jdbc Driver 의존성 추가

- Oracle Jdbc Driver 의존성 추가에서, Missing artifact com.hynnet:oracle-driver-ojdbc6:jar:12.1.0.1 에러가 남

- 과정1 : 다른 ojdbc 넣어보았지만 같은 에러

- 과정2 : repository 삽입하여 의존성 추가해 보았지만, url이 유효하지 않음

- 해결 : 수동으로 jar파일 넣어줌

  :  [ojdbc6-11.2.0.3.jar](https://www.datanucleus.org/downloads/maven2/oracle/ojdbc6/11.2.0.3/) 를 C:\Users\yujin\.m2\repository\com\oracle\ojdbc6\11.2.0.3에 넣어줌 -> pom.xml에 아래와 같이 추가해줌 -> 이클립스에서 실행해보면 오류 안뜸

~~~
		<!-- oracle jdbc -->
		<dependency>
			<groupId>com.oracle</groupId>
			<artifactId>ojdbc6</artifactId>
			<version>11.2.0.3</version>
		</dependency>
~~~

##### 2.  클래스 못 찾는 에러

- java.lang.NoClassDefFoundError: Could not initialize class org.springframework.jdbc.core.StatementCreatorUtils at org.springframework.jdbc.core.ArgumentPreparedStatementSetter.cleanupParameters(ArgumentPreparedStatementSetter.java:70) at org.springframework.jdbc.core.JdbcTemplate$1.doInPreparedStatement(JdbcTemplate.java:656) at org.springframework.jdbc.core.JdbcTemplate.execute(JdbcTemplate.java:589) at org.springframework.jdbc.core.JdbcTemplate.query(JdbcTemplate.java:639) at org.springframework.jdbc.core.JdbcTemplate.query(JdbcTemplate.java:668) at org.springframework.jdbc.core.JdbcTemplate.query(JdbcTemplate.java:676) at org.springframework.jdbc.core.JdbcTemplate.queryForObject(JdbcTemplate.java:731) at myspring.user.dao.UserDaoImplJDBC. ...
- 해결 : 아래추가

~~~
		<dependency>
        	<groupId>org.springframework</groupId>
        	<artifactId>spring-context</artifactId>
        	<version>4.0.2.RELEASE</version>
   		</dependency>
~~~



