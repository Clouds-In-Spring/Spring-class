# 16강
>MyBatis의 개요와 특징<br>
MyBatis와 MyBatis-Spring의 주요 컴포넌트<br>
MyBatis-Spring을 사용한 예제

<br><br>


## MyBatis의 개요와 특징
1. MyBatis의 개요
>자바 오브젝트와 SQL문 사이의 자동 매핑 기능을 지원하는 ORM 프레임워크

\* ORM: Object Relational Mapping

* MaBatis는 SQL을 별도의 파일로 분리해서 관리하게 해주며 객체-SQL 사이의 파라미터 Mapping작업을 자동으로 해줌

2. MyBatis의 특징
    * 쉬운 접근성과 코드의 간결함
        - 가장 간단한 퍼시스터스 프레임워크
        - XML 형태로 서술된 JDBC코드라고 생각해도 될만큼 JDBC의 모든 기능을 제공
        - 깔끔한 소스코드 유지
        - 수동적인 파라미터 설정과 쿼리 결과에 대한 맵핑구문 제거 가능
    * SQL문과 프로그래밍 코드의 분리
        - SQL 변경이 있을 때마다 자바코드를 수정or 컴파일 하지 않아도 됨
        - SQL 작성과 관리/검토를 DBA와 같은 같은 개발자가 아닌 다른 사람에게 맡길 수 있다.
    * 다양한 프로그래밍 언어로 구현 가능
        - Java, C#, .NET, Ruby



<br>

## MyBatis와 MyBatis-Spring의 주요 컴포넌트
1. MyBatis와 MyBatis-Spring을 사용한 DB 액세스 Architecture
2. MyBatis를 사용하는 데이터 액세스 계층
    presentation Layer
    Data AccessLayer에서 사용하는 프레임워크
3. MyBatis3의 주요 컴포넌트
4. MyBatis3의 주요 컴포넌트의 역할
    - MyBatis 설정파일(SqlMapConfig.xml)
        - 데이터 베이스의 접속 주소 정보/ Mapping 파일의 경로 등의 고정된 환경정보를 설정
    - SqlSessionFactoryBuilder
        - MyBatis 설정파일을 바탕으로 SqlSessionFactory 생성
    - SqlSessionFactory
        - SqlSession 생성
    - SqlSession
        - 핵심적인 역할을 하는 클래스로서 SQL 실행이나 트랜잭션 관리를 실행
        - SqlSession 오브젝트는 Thread-Safe하지 않으므로 thread마다 필요에 따라 생성
    - mapping 파일(user.xml)
        - SQL문과 OR Mapping 을 설정


5. MyBatis-Spring의 주요 컴포넌트
* 마이바티스와 스프링의 연동을 도와줌

<br>

## MyBatis-Spring을 사용한 예제
```
16강
	1.	마이바티스 개요와 특징
	1.	개요
	1.	MyBatis: 자바 오브젝트와 SQL문 사이의 자동 맵핑 기능을 지원하는 ORM 프레임워크이다. 
	1.	* orm: object relational mapping 
	2.	마이바티스는 sql을 별도의 파일로 분리해서 관리하게 해주며 객체와 sql사이의 파라미터 맵핑 작업을 자동으로 해주기 때문에 많은 인기를 얻고있는 기술
	2.	특징
	1.	쉬운 접근성과 코드의 간결함
	1.	가장 간단한 퍼시스턴스 프레임워크
	2.	xml형태로 서술된 JDBC코드라고 생각해도 될만큼 jdbc의 기능을 마이바티스가 제공함
	3.	복잡한 jdbc코드를 걷어내며 깔끔한 소스코드 유지 가능
	4.	수동적인 파라미터 설정과 쿼리 결과에대한 맵핑 구문을 마이바티스가 제공하므로 제거할 수있다
	2.	SQL문과 프로그래밍 코드의 분리
	1.	sql에 변경이 있을 때마다 자바 코드를 수정하거나 컴파일 하지않아도 됨
	2.	분리가 돼있으므로 개발 검토, 관리를 다른 사람에게 맡길 수 있다
	3.	다양한 프로그래밍 언어로 구현 가능
	2.	마이바티스와 MyBatis-Spring의 주요 컴포넌트
	1.	MyBatis/MyBatis-Spring을 사용한 DB액세스 아키텍쳐
	1.	퍼시스턴스 레이어에 데이터베이스
	2.	jdbc상에사 데이터베이스에 접속하기 위해 Datasource 사용
	3.	실제 내부적으로 DB vendor에서 제공하는 JDBC Driver
	4.	jdbc가 로우레벨의 api이기 때문에 얘를 감싸서 개발자가 편히하게 데이터베이스에 접근할수있도록 마이바티스3/마이바티스-스프링을 쓴다
	5.	서비스계층에서 DAO계층에서 맵퍼를 호출하면 마이바티스를 사용하게 되는 것
	2.	MyBatis를 사용하는 데이터 액세스 계층
	1.	presentation layer/service layer/db access layer중 DB레이어에서 마이바티스를 사용
	2.	프리젠테이션 콘트롤에서 서비스를 호출하면 Service Impl객체가 생성되고, 서비스가 DAO계층의 메서드를 호출하면 DAO imple객체가 생성됨, dao에서 마이바티스 프레임워크의 그림과 같은 컴포넌트들을 호출하게됨
	3.	그렇다면 마이바티스의 주요 컴포넌트들은 무엇일까?
	1.	SqlSessionFactoryBuilder
	1.	마이바티스 config파일을 자탕으로 sqlsessionfactory 생성
	2.	SqlSessionFactory
	1.	sqlsession생성
	3.	SqlSession
	1.	핵심 
	2.	맵핑파일의 sql실행
	3.	트랜잭션관리도 내부적으로 함
	4.	Thread-Safe하지 않기 때문에 thread마다 필요에 따라 실행
	4.	분이된 sql문을 포함한 Mapping File(개발자 작성)
	1.	sql문과 OR Mapping정보 설정
	2.	user.xml
	5.	MyBatis 환경설정에 필요한 xml파일
	1.	SqlMapConfig.xml: db 접속 주소 정보, sql문을 포함한 Mapping 파일(xml)의 경로, 고정된 환경정보(개발환경인지 운영환경인지)
	6.	순서
	1.	어플리케이션에서 빌더 호출, 빌더가 환경설정파일인 config file을 읽고 sqlsessionFactory를 생성한다, 그리고 다시 개발자가 어플리케이션에있는 디비 액세스하는 리드, 인서트 등의 호출함, 이 과정에서 sqlsessionfactory를 호출, 얘가 sqlsession 컴포넌트 생성하고 sqlsession을 개발자가 작성한 어플리케이션 코드에 리턴해줌. 리턴받아서 sqlsession에 있는 메스드 호출하면됨
	2.	핵심적인 클래스가 sqlsession임 얘가 개발자가 작성한 sql문을 호출해줌
	4.	MyBatis-Spring의 주요 컴포넌트
	1.	마이바티스와 스프링간 연동을 쉽게해주는 오픈소스
	2.	마이바티스 컴포넌트를 wrapping해서 개발자가 편리하게 작성할 수 있도록 제공해주는 요소들을 제공
	3.	SqlSessionFactoryBean과 SqlSessionTemplate이 그 역할을 함
	4.	순서
	1.	sql팩토리 빈이 sql세션팩토리 생성, 얘를 기반으로 sql세션템플릿이 만들어짐
	2.	개발자가 할 일: 팩토리 생성을 위한 빈을 스프링빈으로 등록해줘야함-> 내부적으로 ㅍㅐㄱ토리 생성해줌, 생성된 팩토리를 템플릿에 생성자에게 전달이 되도록 설정하면 템플릿 생성됨, 템플릿도 스프링빈으로 등록해줘야함
	1.	SqlSessionFactoryBean, SqlSessionTemplate을 Spring Bean으로 등록
	3.	sqlsession은 thread-safe하지않기 때문에 매번 요청이 올때마다 새롭게 생성하는 반면 SqlSessionTemplate은 Thread-Safe하기 때문에 멀티스레드 환경에서도 편리하게 사용 가능. SqlSessionTemplate도 sql문을 실행해줌
	5.	MyBatis-Spring의 주요 컴포넌트들의 역할
	1.	MyBatis 설정파일(sqlMappingConfig.xml): 마이바티스만 단독 사용 경우에는 오기에 디비 접속 정보, 맵핑파일위치를 설정하는데 스프링과 연동하는 경우에는 스프링빈 설정파일에 그와 같은 정보들을 등록해주고, sqlMappinfConfig 설정파일에는 VO(Value Object) 객체의 정보만 입력해주면 된다
	2.	SqlSessionFactoryBean: MyBatis 설정파일을 바탕으로 SqlSessionFactory 생성, Spring Bean으로 등록해야함
	3.	SqlSessionTemplate: SqlSession 인터페이스를 구현한 것, thread-safe함. 역할은 sqlsession과 같고 마찬가지로 spring bean으로 등록해야함
	4.	Mapping 파일(user.xml): SQL문과 OR Mapping 을 설정
	5.	Spring Bean 설정파일(beans.xml): 팩토리빈을 등록할 때 DataSource 정보와 MyBatis Config 파일정보, 맵핑파일의 정보를 함께 설정한다 , 템플릿을 빈으로 등록한다(앞에 썼던 내용들임)
	```