# 12강
>DB설정 및 JDBC Driver 설치<br>
Spring JDBC 설치 및 DataSource 설정<br>
사용자 관리 프로젝트 테스트

<br><br>

## DB설정 및 JDBC Driver 설치
1. DB users 테이블 생성

```sql

SQL*Plus: Release 11.2.0.2.0 Production on 금 2월 5 17:09:43 2021

Copyright (c) 1982, 2014, Oracle.  All rights reserved.

# db권한으로 접속
SQL> conn sys as sysdba;
Enter password:
Connected.

# 사용자계정 생성
SQL> create user scott identified by tiger default tablespace users
  2  temporary tablespace temp;

User created.

# 사용자에게 권한 부여
SQL> grant connect, resource to scott;

Grant succeeded.

# 사용자계정으로 접속
SQL> conn scott/tiger;
Connected.

# user.sql 실행 -> USERS 테이블을 만들고 홍길동 insert
SQL> start c:\SPRING\springframework\user.sql;

Table created.


1 row created.


Commit complete.


# 잘 됐는지 확인 
SQL> select * from users;

USERID
------------------------------------------------------------
NAME
--------------------------------------------------------------------------------
GENDER
--------------------
CITY
------------------------------------------------------------
gildong
홍길동
남
서울
```

<br>

2. Oracle Jdbc Driver 라이브러리 검색 및 설치
    * 1 ) http://mvnrepository.com
    * 2 ) oracle ojdbc6 검색
    * 3 ) oracle jdbc 12.1.0.1 -> pom.xml에 추가


<br>
<br>

## Spring JDBC 설치
1. Spring JDBC 설치
    * 1 ) http://mvnrepository.com
    * 2 ) spring jdbc 검색
    * 3 ) spring jdbc 3.2.17 -> pom.xml에 추가

2. DataSource 설정
    * DataSource를 Spring Bean으로 등록하여 사용
    * url, username, password등은 운영환경으로 가게되면 변경될수 있기 때문에 value.properties에 따로 빼두고 쓰자

    value.properties
    ```java
    db.driverClass=oracle.jdbc.OracleDriver
    db.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
    db.username=scott
    db.password=tiger
    ```

    beans.xml
    ```xml
    <!-- DataSource 설정 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
		<property name="driverClass" value="${db.driverClass}" />
		<property name="url" value="${db.url}" />
		<property name="username" value="${db.username}" />
		<property name="password" value="${db.password}" />
	</bean>
    ```

<br><br>

## DataSource 설정
사용자관리 프로젝트의 Bean 등록 및 의존관계 설정
1. \<context:component-scan> 태그 사용  
    beans.xml
    ```xml
    <!-- component scan 설정 -->
        <context:component-scan base-package="myspring.user" />
    ```
2. DataSource 설정 테스트
    - 데이터소스 빈이 spring bean으로 설정되었는지 확인

    src/myspring.user.test/UserClient.java
    ```java
                                :
        @Test
        public void dataSourceTest() {
            DataSource ds = (DataSource) context.getBean("dataSource");
            try {
                System.out.println(ds.getConnection());
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
                                :

    ```
    <br>
    결과) 해당되는 컨넥션 클래스 이름 출력

    ```
    oracle.jdbc.driver.T4CConnection@2dbf4cbd
    ```
    
<br><br>

## 사용자 관리 프로젝트 테스트
1. 사용자 조회 테스트
    - UserService의 getUser() 메서드 사용

    UserClient.java
    ```java
        @Autowired
        UserService service;


                        :
        @Test
        public void getUserTest() {
            UserVO user = service.getUser("gildong");
            System.out.println(user);
            assertEquals("홍길동", user.getName());
        }
    ```

    결과
    ```
    User [userId=gildong, name=홍길동, gender=남, city=서울]
    ```
2. 사용자 등록 및 목록 조회 테스트
    - UserService의 insertUserTest() 메서드 사용
3. 사용자 갱신
    - updateUserTest()
4. 사용자 삭제
    - deleteUserTest()

