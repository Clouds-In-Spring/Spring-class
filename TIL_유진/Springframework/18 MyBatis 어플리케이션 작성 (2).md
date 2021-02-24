# 18 MyBatis 어플리케이션 작성 (2)

### 학습 목표

1. Mapper 인터페이스 개념
2. Mapper 인터페이스 작성 및 설정
3. 여러 개의 Mapper 인퍼테이스 설정

### 1 Mapper 인터페이스 개념

#### Mapper 인터페이스

- sql 호출하기 위한 인터페이스

#### Mapper 인터페이스 사용하지 않았을 때

~~~
session.selectOne("userNS.selectUserByID", userid);

<mapper namespace="userNS">
	<select id = "selectUserById" parameterType= "String">
~~~

- SQL을 호출하는 프로그램은 SqlSession의 메서드의 아규먼트에 문자열로 

  **네임스페이스+"."+SQL ID**로 지정

- 문자열로 지정하기 때문에 오타에 의해 버그가 숨어있거나, IDE에서 제공하는 code assist를 사용할 수 없음

#### Mapper 인터페이스 사용했을 때

~~~
<mapper namespace= "myspring.user.dao.UserMapper">  패키지명.인터페이스이름
	<select id = "selectUserById" parameterType = "String"> id는 매핑할 메서드 이름
~~~

- UserMapper 인터페이스는 개발자가 작성
- 해당되는 메서드명은 SQL ID와 동일하도록 작성
- **패키지 이름 + "."+ 인터페이스 이름 + "."+ 메서드 이름**이 네임스페이스+"."+SQL의 ID가 되도록 네임스페이스와 SQL ID 설정해야 함
- 네임스페이스 속성에는 패키지를 포함한 Mapper 인터페이스 이름
- SQL ID에서는 매핑하는 메서드 이름 지정

### 2 Mapper 인터페이스 작성 및 설정

- 순서 : Mapper 인터페이스 작성 ->  매핑 파일 수정 -> DAO 클래스 수정 -> MapperFactoryBean의 설정 -> 

Mapper 인터페이스의 사용 테스트 

- MapperFactoryBean의 설정
  - MapperFactoryBean은 UserMapper를 구현하는 프록시 클래스 생성하고 그것을 어플리케이션에 주입
  - 프록시는 런타임시 생성되므로 Mapper는 인터페이스여야
  - sqlSessionFactory나 sqlSessionTemplate를 필요로 함

- **UserMapper.java**

  -  반드시 인터페이스로 선언
  - sql 아이디와 메서드명 동일하게 선언
  - 레코드 한건을 조회/ 여러개 조회

  ~~~
  public interface UserMapper {
  	UserVO selectUserById(String id);
  	List<UserVO> selectUserList();
  	void insertUser(UserVO userVO);
  	void updateUser(UserVO userVO);
  	void deleteUser(String id);
  }
  ~~~

- **User.xml 수정**

  ~~~
  <mapper namespace="userNS"> -> <mapper namespace="myspring.user.dao.UserMapper">
  ~~~

- ##### **UserDaoImplMapper.java**

  - UserSession이아니라 UserMapper에 의해 레코드 받아옴

  ~~~
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
  ~~~

- **beans.xml**

  - ref 속성은 아래의 sqlSessionTemplate의 id명임

  ~~~
  <!-- Mapper 설정 -->
  <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
  	<property name="mapperInterface" value="myspring.user.dao.UserMapper" />
  	<property name="sqlSessionTemplate" ref="sqlSession"/>
  </bean>
  ~~~

- 테스트시, 동일한 아이디가 있는데 insert하게 되면 오류 생기니까 아이디 바꿔서 ㄱㄱ

### 3 여러 개의 Mapper 인퍼테이스 설정

#### MapperScannerConfigurer의 사용

- MapperFactoryBean을 이용해 Mapper 인터페이스를 등록할 때, Mapper 인터페이스의 개수가 많아지게 되면 일일이 정의하는데 시간이 많이 걸림
- Mapper 인터페이스의 수가 많아지면 MapperScannerConfigurer를 이용하여 Mapper 인터페이스의 객체를 한 번에 등록하는 것이 편리
- MapperScannerConfigurer를 이용하면 지정한 패키지 아래 모든 인터페이스가 Mapper 인터페이스로 간주되어 Mapper 인터페이스의 객체가 DI 컨테이너에 등록되는 것

#### MapperScannerConfigurer의 설정

- beans.xml

  ~~~
  <bean>
  	<property name="basePackage" value="myspring.user.dao" />
  </bean>
  ~~~

- bawsePackage에서Package(Mapper 인터페이스를 검색할 대상)를 지정 
- myspring.user.dao 아래의 인터페이스들은 모두 Mapper 인터페이스에 대응하여 Mapper 객체가 생성된다는 점

#### Marker 인터페이스와 Marker 어노테이션의 사용

- 검색의 대상이 되는 Package 아래의 인터페이스들 중 Mapper로서 작성한 인터페이스로만 좁히려면 

  Marker 인터페이스와 Marker 어노테이션을 작성하여  MapperScannerConfigure에 설정하면 됨

- beans.xml
  - org.mybatis.spring.mapper.MapperScannerConfigurer : 컴포넌트 스캔을 통해 Mapper 찾기
  - basePackage : Mapper 찾는 베이스 패키지
  - annotationClass : Mapper 지정하는 어노테이션 클래스

~~~
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="myspring.user.dao" />
    <property name="annotationClass" value="myspring.user.dao.MyMapper" />
</bean>
~~~

- MyMapper.java

  ~~~
  package myspring.user.dao;
  
  public @interface MyMapper {
  
  }
  ~~~

- UserMapper.java

  ~~~
  package myspring.user.dao;
  import java.util.List;
  import myspring.user.vo.UserVO;
  @ MyMapper
  public interface UserMapper {
  	UserVO selectUserById(String id);
  	List<UserVO> selectUserList();
  	void insertUser(UserVO userVO);
  	void updateUser(UserVO userVO);
  	void deleteUser(String id);
  }
  ~~~

- UserClient에서 출력 -> 



### 트러블 슈팅

- 맨 마지막에서 오류

- Configuration problem: Unnamed bean definition specifies neither 'class' nor 'parent' nor 'factory-bean' - can't generate bean name 에러 발생

  - beans.xml에서 중복된 코드 지우고 추가한 코드로 처리

- ~~~
   available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations 
   
   No qualifying bean of type 'myspring.user.dao.UserMapper' 
  ~~~

  - 첫번째: autowire할 bean이 적어도 1개 필요, 
  - 두번째 : 종속에 적합한 bean이 없다
  - 해결1 : 어노테이션이 잘 붙어있는가 ? @Autowired @Service / @Repository 
  - 해결 2 : 패키지명 base-package
  - 아직 해결 못함 나중에 다시
