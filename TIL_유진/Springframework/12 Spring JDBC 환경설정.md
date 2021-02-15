## 12 Spring JDBC 환경설정

#### 1 Spring JDBC 설치 및 DataSource 설정

###### pom.xml에 Oracle Jdbc Driver 의존성 추가 후 Spring JDBC 추가

~~~
        <dependency>
            <groupId>com.hynnet</groupId>
            <artifactId>oracle-driver-ojdbc6</artifactId>
            <version>12.1.0.1</version>
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
  
  ~~~

  - e.printStackTrace() : 에러 메시지으 발생 근원지를 찾아서 단계별로 에러 출력

###### 사용자 조회 테스트

- UserClient.java

  ~~~
  
  ~~~

  - @Autowired를 통해 UserService를 자동으로 주입받고 UserService의 getUser를 통해 "gildong" 이라는 id를 가진 값을 리턴받는다
  - UserVO 클래스의 getName() 메서드를 통해 userName을 리턴받는다

###### 사용자 등록 및 목록조회 테스트

- UserClient.java

  ~~~
  
  ~~~

  - Service의 insertUser() 메서드는 UserDao의 insert() 메서드를 사용
  - Service의 getUserList() 메서드는 UserDao의 readAll() 메서드를 사용

###### 사용자 정보수정 테스트

- UserClient.java

  ~~~
  
  ~~~

  - Service의 updateUser() 메서드는 UserDao의 update() 메서드를 사용

###### 사용자 정보삭제 테스트	

```

```

### 에러

~~~
	<repositories>
		<repository>
        <id>codelds</id>
        <url>https://code.lds.org/nexus/content/groups/main-repo</url>
    </repository>
	</repositories>
	
		<repositories>
		<repository>
        <id>codelds</id>
        <url>https://code.lds.org/nexus/content/groups/main-repo</url>
    </repository>
	</repositories>	
~~~

