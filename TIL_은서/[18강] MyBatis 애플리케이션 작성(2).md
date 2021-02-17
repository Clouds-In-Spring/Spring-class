# Mapper 인터페이스 개념

* Mapping 파일에 기재된 SQL을 호출하기 위한 인터페이스

* Mapper 인터페이스를 사용하지 않으면 SQL을 호출하는 프로그램은 SqlSession의 메서드의 아규먼트에 **문자열로** 네임스페이스+"."+SQL ID로 지정해야 한다

  ➡ 문자열로 지정하기 때문에 오타에 의해 버그가 숨어있거나 IDE에서 사용하는 Code assist를 사용할 수 없음

* UserMapper 인터페이스는 개발자가 작성한다

  ex) \<mapper namespace="myspring.user.dao.UserMapper"> ➡ (패키지명).(인터페이스 이름)

  ​	  \<select id="selectUserById" parameterType="String"> ➡ id는 매핑할 메서드의 이름을 담고있음





# Mapper 인터페이스 작성 및 설정

 #### Mapper 인터페이스 작성

##### UserMapper.java

```java
public interface UserMapper {
	UserVO selectUserById(String id);
	List<UserVO> selectUserList();
	void insertUser(UserVO userVO);
	void updateUser(UserVO userVO);
	void deleteUser(String id);
}
```



##### User.xml 수정

```xml
<mapper namespace="userNS">
		⬇⬇⬇⬇
<mapper namespace="myspring.user.dao.UserMapper">
```



##### UserDaoImplMapper.java

```java
@Repository("userDao")
public class UserDaoImplMapper implements UserDao {
	@Autowired
	private UserMapper userMapper;	
	
	@Override
	public UserVO read(String id) {
		UserVO user  = userMapper.selectUserById(id);
		return user;
	}
	
	public List<UserVO> readAll() {
		List<UserVO> userList = userMapper.selectUserList();
		return userList;
	}

	public void insert(UserVO user) {
		userMapper.insertUser(user);
		System.out.println("등록된 Record UserId=" + user.getUserId() + 
				" Name=" + user.getName());
	}

	@Override
	public void update(UserVO user) {
		userMapper.updateUser(user);
	}
	
	@Override
	public void delete(String id) {
		userMapper.deleteUser(id);
		System.out.println("삭제된 Record with ID = " + id ); 
	}
}
```



##### beans.xml

```xml
<!-- Mapper 설정 -->
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
	<property name="mapperInterface" value="myspring.user.dao.UserMapper" />
	<property name="sqlSessionTemplate" ref="sqlSession"/>
</bean>
```





 #### 테스트

##### UserClient.java의 daoTest() 이용

insert➡update➡delete순으로 테스트



# 여러 개의 Mapper 인터페이스 설정

#### MapperScannerConfigurer의 사용

* MapperFactoryBean을 이용하면 Mapper 인터페이스의 개수가 많아지게 되면 일일이 정의하는데 시간이 많이 걸린다

  ➡ 그래서 MapperScannerConfigurer를 이용하여 객체를 한 번에 등록하는 것이 편리함

* MapperScannerConfigurer를 이용하면 **지정한 패키지 아래 모든 인터페이스가 Mapper 인터페이스로 간주되어** Mapper 인터페이스의 객체가 DI 컨테이너에 등록된다



#### MapperScannerConfigurer의 설정

##### beans.xml

```xml
<bean>
	<property name="basePackage" value="myspring.user.dao" />
</bean>
```

* basePackage 속성에서 Mapper 인터페이스를 검색할 대상이 되는 패키지 지정



#### Marker 인터페이스와 Marker 어노테이션의 사용

* 검색의 대상이 되는 패키지 아래의 인터페이스들 중 **Mapper로 작성한 인터페이스로만 범위를 좁히기 위해** 사용

##### 1. New File > Annotation > MyMapper.java 생성 (생성만 하면 됨)

##### 2. UserMapper.java에 @MyMapper 어노테이션 지정

##### 3. beans.xml 파일에 내용 추가

```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="myspring.user.dao" />
    <property name="annotationClass" value="myspring.user.dao.MyMapper" />
</bean>
```

* annotationClass : Mapper를 지정하는 어노테이션 클래스





