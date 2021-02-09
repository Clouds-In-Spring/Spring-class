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