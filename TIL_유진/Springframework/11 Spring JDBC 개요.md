## 11 Spring JDBC 개요

#### 학습 목표

1. 데이터 액세스 공통 개념에 대하여 이해
2. Spring JDBC 개요에 대하여 이해
3. Spring JDBC의 JdbcTemplate 클래스에 대하여 이해

#### 1. 데이터 액세스 공통 개념

###### DAO(Data Access Object) 패턴

- 데이터 액세스 계층은 DAO 패턴을 적용하여 **비즈니스 로직과 데이터 액세스 로직을 분리**하는 것이 원칙
  - DAO 패턴은 서비스 계층에 영향 주지 않고 데이터 액세스 기술 변경
- 비즈니스 로직이 없거나 단순하면 DAO와 서비스 계층 통합 가능

###### DataSource

- 컨넥션 풀링 지원

  - 컨넥션 풀링 -> connection 재사용 하는 기법

    : 미리 정해진 개수만큼의 DB connection을 풀(pool)에 준비해두고, 애플리케이션이 요청할 때마다 풀에서 꺼내 하나씩 할당하고 다시 돌려받아 풀에 넣는 기법 

  - 다중 사용자를 갖는 엔터프라이즈 시스템이라면 반드시 connection 풀링을 지원하는 DataSource 사용해야 함
  - Spring에서는 DataSource를 공유 가능한 Bean으로 등록하여 사용

###### DataSource - 클래스 종류

- 테스트용 DataSource

  - SimpleDriverDataSource
    - Spring이 제공하는 가장 단순한 DataSource 구현 클래스
    - getConnection()을 호출할 때 마다 **매번 DB 커넥션을 새로 만들고** 따로 **풀 관리를 하지 않으므로**(➡풀링 기법 사용X) 단순한 테스트용으로만 사용해야 한다

  - **SimpleConnectionDriverDataSource**
    - 순차적으로 진행되는 통합테스트에서는 사용 가능 but 멀티쓰레드로 진행되는 통합테스트에는 적합X
    - 매번 DB커넥션을 생성하지 않으므로 **SimpleDriverDataSource보다 빠르게 동작**
  - 오픈소스 DataSource (오픈소스 형태로 제공되는 DBConnection pool library)
    - **Apache Commons DBCP** ➡ 가장 유명함
    - **c3p0 JDBC/DataSource Resource Pool**
    - 두 가지 모두 setter 메서드를 제공하므로 bean으로 등록하여 사용하기 편리

#### 2. Spring JDBC 개요

###### JDBC

- 모든 자바의 데이터엑세스 기술의 근간이 된다
- 최신 ORM 기술(MyBatis, Hibername 등)도 내부적으로는 DB와의 연동을 위해 JDBC를 이용

###### Spring JDBC

- Spring JDBC를 사용하려면 먼저 DB 커넥션을 가져오는 DataSource를 Bean으로 등록해야 한다

- JDBC에서 해주지 않았던 작업들을 해줌

  - Connection 열기와 닫기 : 필요한 시점에 알아서 열고 알아서 닫는다. 예외가 발생했을 때도 열린 모든 connection 객체를 닫아줌

  - Statement 준비와 닫기 : SQL정보가 담긴 Statement 또는 PreparedStatement를 생성하고 필요한 준비 작업을 해줌

  - Statement 실행 : SQL이 담긴 Statement를 실행, 실행 결과를 다양한 형태의 객체로 가져옴

  - ResultSet Loop 처리 : 쿼리 실행 결과가 한 건 이상이면 ResultSet 루프를 만들어 반복해줌

  - Exception 처리와 반환 : JDBC 작업 중 발생하는 모든 예외를 Spring JDBC 예외 변환기가 처리 as 런타임 예외인 DataAccessException타입으로 변환

    > Checked Exception : 개발자가 알아서 try, catch 문으로 잡아줘야함
    >
    > Runtime Exception : try, catch 필요X

  - Transaction 처리 : Spring JDBC를 쓰면 transaction과 관련된 모든 작업(commit, rollback)에 대해 신경 안써도됨

#### 3. Spring JDBC 개요 및 JdbcTemplate 클래스

###### Spring JDBC의 JdbcTemplate 클래스

- JDBC의 모든 기능을 최대한 활용할 수 있는 유연성을 제공하는 클래스
- 실행, 조회, 배치의 세 가지 작업에 대한 기능 제공
  - 실행 : DB 데이터에 변경이 일어나는 쿼리 수행하는 작업 ex) Insert, Update
  - 조회 : 데이터 조회 ex) Select
  - 배치 : 여러 개의 쿼리를 한 번에 수행해야 하는 작업

###### update() 메서드

- Insert, Update, Delete와 같은 SQL 실행할 때 사용

- `int update(String sql, [SQL 파라미터])`

  리턴 타입이 int인 이유 = SQL 실행으로 영향을 받은 레코드의 개수 리턴

  ex) `int result = jdbcTemplate.update("password=?, name=?", user.getpassword(), user.getId());`

###### queryForObjec() 메서드

- **여러개의 column, 한 개의 row**로 반환되는 SQL 구문을 처리하는 메서드
- `<T>T queryForObject(String sql, [SQL 파라미터], RowMapper<T> rm)`
- T는 VO(Value Object) 객체 타입 중 하나
- 여러개의 column을 가진 한 개의 row를 **RowMapper 콜백을 이용하여 VO 객체로 매핑**해줌

###### query() 메서드

- **여러개의 column, 여러개의 row** 형태로 반환되는 SQL 구문을 처리하는 메서드

- `<T> List<T> query(String sql, [SQL 파라미터], RowMapper<T> rm)`

- 결과 값은 여러개의 row를(VO 객체) 담아야 하므로 List형태로 받는다

  ➡ List의 각 요소가 하나의 row

