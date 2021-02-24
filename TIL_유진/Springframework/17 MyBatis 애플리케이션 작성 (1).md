

## 17 MyBatis 애플리케이션 작성 (1)

### 학습 목표

- MyBatis 및 MyBatis-Spring 설치
- Mapping 파일 작성 및 MyBatis 설정
- DAO 클래스 작성 및 테스트

### 1 MyBatis 및 MyBatis-Spring 설치

~~~
	   <dependency>
      	   <groupId>org.mybatis</groupId>
    	   <artifactId>mybatis-spring</artifactId>
     	   <version>2.0.6</version>
	   </dependency>
	   <dependency>
	       <groupId>org.mybatis</groupId>
	       <artifactId>mybatis</artifactId>
	       <version>3.5.6</version>
	   </dependency>
~~~

### 2 Mapping 파일 작성 및 MyBatis 설정

#### Mapping 파일 작성(SqlMapConfig.xml)

~~~
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<typeAliases>
		<typeAlias alias="User" type="myspring.user.vo.UserVO" />
	</typeAliases>
</configuration>
~~~

#### User.xml

~~~
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="userNS">
	
    <!-- userid를 통해 user 조회 -->
	<select id="selectUserById" parameterType="string" resultType="User">
		select * from users where userid=#{value}
	</select>
	
    <!-- 모든 user 조회 userid로 정렬 -->
	<select id="selectUserList" resultType="User">
		select * from users order by userid
	</select>
    
    <!-- 값을 받아와 user 테이블에 추가 -->
	<insert id="insertUser" parameterType="User">
		insert into users values( #{userId},#{name},#{gender},#{city} )
	</insert>
    
	<!-- 값을 받아와 특정 userid를 가진 레코드 수정 -->
	<update id="updateUser" parameterType="User">
		update users set
		name = #{name},
		gender = #{gender},
		city = #{city}
		where userid = #{userId}
	</update>
	
    <!-- 특정 userid의 레코드 삭제 -->
	<delete id="deleteUser" parameterType="string">
		delete from users where userid = #{value}
	</delete>
</mapper>
~~~

- 매핑 파일에 네임스페이스 설정 필요

  - 매핑 파일이 여러개라면, 아이디가 같을때에 파일이 같은 메모리를 사용하게 되는데, 충돌발생 -> 이떄 namespace 통해 태그의 id 같을 때에도 구분이 가능해지면 충돌 안 일어남

- ~~~
  <select resultType="User" parameterType="string" id="selectUserById">select * from users where userid=#{id} </select>
  ~~~

  - 한 레코드가 반환되어 resultType이 User이므로, UserVO객체에 담아주는 것을 MyBatis가 해줌

- ~~~
  insert into users values( #{userId},#{name},#{gender},#{city} )
  ~~~

  - 입력 데이터가 자동으로 매핑됨

- 테이블 컬럼명과 VO클래스의 변수명을 동일하게 해 주는 것이 중요

#### SpringBean 설정 파일(beans.xml)

~~~
<!-- SqlSessionFactoryBean 설정 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="configLocation" value="classpath:config/SqlMapConfig.xml" />
		<property name="mapperLocations">
			<list>
				<value>classpath:config/User.xml</value>
			</list>
		</property>
	</bean>

	<!-- SqlSessionTemplate 설정 -->
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg ref="sqlSessionFactory"/>
	</bean>
~~~

- SqlSessinFactory와 SqlSession 파일 설정
- 매핑 xml이 늘어나면 value테이블 추가하면 됨

#### UserDaoImplJDBC.java에서 @repository 주석처리

- 주석 처리하면 아래와 같은 오류 생김
- 주석 해제

#### UserClient.java

- 테스트 1

~~~
@Test
	public void configTest() {
	    SqlSession session = context.getBean("sqlSession", SqlSession.class);
	    System.out.println(session.getClass().getName());
	}	
~~~

- 결과 : org.mybatis.spring.SqlSessionTemplate
- SqlSession을 구현한 하위 클래스명이 출력됨

- 테스트 2

~~~
	@Test
	public void configTest() {
	    SqlSession session = context.getBean("sqlSession", SqlSession.class);
	    System.out.println(session.getClass().getName());
	    
	    UserVO vo = session.selectOne("userNS.selectUserById", "gildong");
	    System.out.println(vo);
	}
~~~

- 출력

  org.mybatis.spring.SqlSessionTemplate
  User [userId=gildong, name=홍길동3, gender=남3, city=서울3]

### 3 DAO 클래스 작성 및 테스트

#### DAO 클래스 작성

##### UserDaoImpl.java

```
package myspring.user.dao;

import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import myspring.user.vo.UserVO;
@Repository("userDao")
public class UserDaoImpl implements UserDao {

	@Autowired
    private SqlSession session;
	
	@Override
	public UserVO read(String id) {
		UserVO user  = session.selectOne("userNS.selectUserById", id);
		return user;
	}

	public List<UserVO> readAll() {
		List<UserVO> userList = session.selectList("userNS.selectUserList");
		return userList;
	}
	
	public void insert(UserVO user) {
		session.update("userNS.insertUser", user);
		System.out.println("등록된 Record UserId=" + user.getUserId() + " Name=" + user.getName());
	}

	@Override
	public void update(UserVO user) {
		session.update("userNS.updateUser", user);
	}

	@Override
	public void delete(String id) {
		session.delete("userNS.deleteUser", id);
		System.out.println("삭제된 Record with ID = " + id ); 
	}
}
```

- test전에 오류 나지 않으려면  UserDaoImplJDBC.java에서 @repository 주석처리

#### 테스트 1

```
@Test
public void daoTest() {
    UserDao dao = context.getBean("userDao", UserDao.class);
    List<UserVO> list = dao.readAll();
    for (UserVO userVO : list) {
    	System.out.println(userVO);
    }
}
```

> **실행 결과**
>
> User [userId=gildong, name=홍길동3, gender=남3, city=서울3]



#### 테스트 2

```
	public void daoTest() {
	    UserDao dao = context.getBean("userDao", UserDao.class);
	    // 수정
	    dao.update(new UserVO("dooli", "둘리리", "여", "부산"));
	    // 조회
	    List<UserVO> list = dao.readAll(); // readAll은 전체 메서드 다 읽어옴
	    
	    for (UserVO userVO : list) {
	    	System.out.println(userVO);
	    }
	}
	
```

> **실행 결과**
>
> [ update ] 아규먼트 User [userId=dooli, name=둘리리, gender=여, city=부산]

### 트러블 이슈

- UserClient.java congfigTest에서

  ~~~
  java.lang.IllegalStateException: Failed to load ApplicationContext
  	at ...
  Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userService': Unsatisfied dependency expressed through field 'userdao'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'myspring.user.dao.UserDao' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
  ~~~

  - 해결 : UserDaoImplJDBC.java에서 @repository 주석처리
    - 주석 처리하면 아래와 같은 오류 생김
    - 주석 해제
