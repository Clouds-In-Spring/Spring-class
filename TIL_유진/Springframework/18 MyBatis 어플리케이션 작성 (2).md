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

6분부터 정리하자

