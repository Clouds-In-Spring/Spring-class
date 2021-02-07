# Mapping 파일 작성 및 MyBatis 설정

#### 1. MyBatis Config 파일 작성

##### SqlMapConfig.xml ➡ VO클래스에 대한 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<typeAliases>
		<typeAlias alias="User" type="myspring.user.vo.UserVO" />
	</typeAliases>
</configuration>
```



##### User.xml ➡ SQL문 mapping 파일

```xml
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
```

* mapping파일에 **namespace**를 설정해주는 것이 중요

  ➡ mapping 파일이 여러 개 있을 경우 namespace를 통해 태그의 id가 같을 때에도 구분이 가능해지며 충돌이 안남

* `<select id="selectUserById" parameterType="string" resultType="User">`

  ➡ SQL문을 실행한 결과를 VO객체에 담아주는 것을 MyBatis가 내부적으로 해준다

* `insert into users values( #{userId},#{name},#{gender},#{city} )`

  ➡ User 타입의 입력데이터를 UserVO객체의 getUserId, getName 등의 메서드를 이용함

  **🚨 테이블의 칼럼명과 VO클래스의 변수명을 동일하게 해 주는 것이 중요**



##### beans.xml

```xml
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
```

* mapping 파일이 추가적으로 더 늘어날 수 있으므로 \<list>와 \<value> 태그 사용



#### 2. 테스트(1)

##### UserClient.java

```java
@Test
public void configTest() {
    SqlSession session = context.getBean("sqlSession", SqlSession.class);
    System.out.println(session.getClass().getName());
}
```

> **실행 결과** : SqlSession을 구현한 하위 클래스명이 출력됨
>
> org.mybatis.spring.SqlSessionTemplate



#### 3. 테스트(2)

##### UserClient.java

```java
@Test
public void configTest() {
    SqlSession session = context.getBean("sqlSession", SqlSession.class);
    System.out.println(session.getClass().getName());

    UserVO vo = session.selectOne("userNS.selectUserById", "gildong");
    System.out.println(vo);
}
```

> **실행 결과**
>
> org.mybatis.spring.SqlSessionTemplate
> User [userId=gildong, name=김둘리, gender=여, city=부산]





# DAO 클래스 작성 및 테스트

#### DAO 클래스 작성

##### UserDaoImpl.java

```java
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



#### 테스트(1)

```java
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
> User [userId=gildong, name=김둘리, gender=여, city=부산]



#### 테스트(2)

```java
@Test
public void daoTest() {
    UserDao dao = context.getBean("userDao", UserDao.class);
    // 등록
    dao.insert(new UserVO("dooli", "둘리", "남", "서울"));
    
    // 조회
    List<UserVO> list = dao.readAll();
    
    for (UserVO userVO : list) {
        System.out.println(userVO);
    }
}
```

> **실행 결과**
>
> User [userId=dooli, name=둘리, gender=남, city=서울]
> User [userId=gildong, name=김둘리, gender=여, city=부산]



#### 테스트(3)

```java
@Test
public void daoTest() {
    UserDao dao = context.getBean("userDao", UserDao.class);
    // 수정
    dao.update(new UserVO("dooli", "또치", "여", "경기"));
    // 조회
    List<UserVO> list = dao.readAll();
    
    for (UserVO userVO : list) {
    	System.out.println(userVO);
    }
}
```

> **실행 결과**
>
> User [userId=dooli, name=또치, gender=여, city=경기]
> User [userId=gildong, name=김둘리, gender=여, city=부산]



#### 테스트(3)

```java
@Test
public void daoTest() {
    UserDao dao = context.getBean("userDao", UserDao.class);
    // 삭제
	dao.delete("dooli");
    // 조회
    List<UserVO> list = dao.readAll();
    
    for (UserVO userVO : list) {
    	System.out.println(userVO);
    }
}
```

> **실행 결과**
>
> User [userId=gildong, name=김둘리, gender=여, city=부산]