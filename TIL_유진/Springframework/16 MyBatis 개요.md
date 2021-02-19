## 16 MyBatis 개요

### 학습 목표

1. MyBatis 개요와 특징
2. MyBatis와 MyBatis-Spring의 주요 컴포넌트
3. MyBatis-Spring을 사용한 예제

### 1 MyBatis 개요와 특징

#### MyBatis의 개요

- **자바 객체와 SQL문 사이 자동 Mapping 기능을 지원**하는 ORM 프레임워크

  > ORM = Object Relational Mapping

- **SQL을 별도의 파일로 분리**하여 관리, 객체-SQL 사이 파라미터 Mapping 작업을 자동으로 해줌

#### MyBatis 특징

1. 쉬운 접근성, 코드의 간결함
   - XML 형태로 서술
   - JDBC의 거의 모든 기능을 제공
2. SQL문과 프로그래밍 코드의 분리
   - SQL에 변경이 있을 때마다 자바 코드 수정 x
3. 다양한 프로그래밍 언어로 구현 가능 (Java, C#, .NET, Ruby)

4. MyBatis-Spring은 오픈소스임

5. MyBatis는 [데이터 엑세스 계층](./[10강] 사용자 관리 프로젝트.md)에서 사용

MyBatis 프레임워크는 데이터 엑세스 계층과 DB 계층 사이에서 JDBC(Oracle 통신)로 통신

> **스프링 웹 계층**
>
> 프레젠테이션 계층 - 서비스 계층 - 데이터 엑세스 계층 - 데이터베이스 계층

### MyBatis와 MyBatis-Spring의 주요 컴포넌트

#### MyBatis와 MyBatis-Spring을 사용한 DB 액세스 Architecture

<img src="https://user-images.githubusercontent.com/38436013/108070709-2b683b80-70a8-11eb-902c-baa59666c455.png" alt="image" style="zoom:80%;" />

#### MyBatis를 사용하는 데이터 액세스 계층

![image](https://user-images.githubusercontent.com/38436013/108071158-bd704400-70a8-11eb-9244-63d37032f78a.png)

- presentation layer controller 서비스 호출 -> serviceImpl 생성 -> 서비스가 DAO 계층의 메서드 호출하면 DaoImpl 호출 -> MyBatis 컴포넌트 호출

#### MyBatis3의 주요 컴포넌트

![image](https://user-images.githubusercontent.com/38436013/108071490-21930800-70a9-11eb-842e-8d0189480fbc.png)

- 보라색 -> 개발자 
-  파란색 -> 마이바티스가 제공하는 클래스, 설정파일

#### MyBatis3 컴포넌트 실행 순서

1. Application에서 SqlSession Factory Builder라는 인터페이스를 호출
2. SqlSession Factory Builder는 MyBatis Config File을 읽고 SqlSession Factory를 생성
3. 개발자가 Application에서 DB로 Access하는 요청을 보냄(read 또는 insert, 기타등등)
4. 생성된 SqlSession Factory는 Application상에서 호출되고 SqlSession 컴포넌트를 생성한다
5. Application은 SqlSession Factory를 리턴받아 SqlSession에 있는 메서드를 호출한다
6. SqlSession은 개발자가 작성한 SQL문이 있는 mapping File을 호출한다

#### MyBatis3의 주요 컴포넌트의 역할

##### MyBatis3에 의해 제공되는

- MyBatis Config File

  (SqlMapConfig.xml)

  - MyBatis 환경설정에 필요한 XML 파일
  - DB의 접속 주소 정보, Mapping 파일(sql문 포함하는)의 경로 등 고정된 환경정보(개발환경 or 운영환경인지)

- SqlSession Factory Builder
  - MyBatis 설정 파일을 바탕으로 SqlSession Factory 생성
- SqlSession Factory
  - SqlSession을 생성
- SqlSession
  - 핵심적인 역할
  - 개발자가 작성한 mapping File의 SQL문을 호출, 트랜잭션 관리
  - thread마다 필요에 따라 생성됨 ➡ **thread-safe하지 않다**

##### 개발자에 의해 만들어지는

- mapping File

  : SQL문과 OR Mapping 설정하는 파일

- Application

#### MyBatis- Spring 의 주요 컴포넌트

- mybatis와 spring 연동을 쉽게 하는 오픈소스
  -  ->  SqlSessionFactory과 SqlSession이 그 역할을 함

- SqlSessionFactory bean을 기반으로 SqlSessionTemplate이 만들어짐
- 개발자는 SqlSessionFactory를 생성해주는 SqlSessionFactory bean을 SpringBean 설정파일에 등록해줌
-  SqlSessionTemplate도 Spring Bean으로 등록해주어야 함
- 결론 : SpringBean : SqlSessionFactory, SqlSessionTemplate 빈으로 설정해주어야 함
-  SqlSession은 thread-unsafe 하지만  SqlSessionTemplate safe하므로 멀티쓰레드 환경에서 유리

#### MyBatis- Spring 의 주요 컴포넌트 역할

**MyBatis 컴포넌트들을 wrapping하여 개발자가 편리하게 작성할 수 있는 요소제공**

- MyBatis 설정 파일 (SqlMapConfig.xml)
  - 스프링과 연동하는 경우 VO 객체의 정보만 설정

- SqlSessionFactoryBean
  - 설정파일을 바탕으로 SqlSessionFactory 생성
  - **beans.xml에서 등록되어야 함**
- SqlSessionTemplate
  - SQL 실행이나 트랜잭션 관리 실행 ➡ SqlSession 역할과 동일
  - SqlSession 인터페이스 구현
  - 멀티쓰레드 환경에서도 편리하게 사용할 수 있다 ➡ **Thread-safe하다**
  - **beans.xml에서 등록되어야 함**
- Mapping 파일
  - Sql 문과 OR 매핑
- Spring Bean 설정파일(beans.xml)
  - SqlSessionFactoryBean, SqlSessionTemplate 을 Bean으로 등록
  - SqlSessionFactoryBean을 등록할 때 DataSource 정보, MyBatis Config 파일 정보, Mapping 파일 정보 설정
