# 11강
>데이터 액세스 공통 개념<br>
Spring JDBC 설치 및 DataSource 설정<br>
Spring JDBC 개요 및 JdbcTemplate 클래스

<br><br>

## 데이터 액세스 공통 개념
1. DAO(Data Access Object) 패턴
    * 데이터 액세스 계층은 DAO패턴을 적용하여 비즈니스 로직과 데이터 액세스 로직을 분리하는 것이 원칙
    * 서비스 계층에 영향을 주지 않고 데이터 액세스 기술을 변경할 수 있다.

2. 컨넥션 풀링을 지원하는 DataSource
    >미리 정해진 개수만큼의 DB 컨넥션을 풀(Pool)에 준비해두고 어플리케이션이 요청할 때마다 풀에서 꺼내서 하나씩 할당해주고 다시 돌려받아서 풀에 넣는 식의 기법
    * 다중 사용자를 갖는 엔터프라이즈 시스템에서라면 반드시 DB 컨넥션 풀링 기능을 지원하는 DataSource를 사용해야함
    * Spring에서는 DataSource를 공유 가능한 Spring Bean으로 등록해주어 사용할 수 있도록 해줌

3. DataSource 구현 클래스 종류
    - 테스트 환경을 위한 DataSource
        * SimpleDriverDataSource
            - 스프링이 제공하는 가장 단순한 형태의 데이터소스 구현 클래스.
            - 단순히 테스트용으로만 사용해야함

        * SingleConnectionDriverDataSource
            - 순차적으로 진행되는 통합 테스트에서는 사용가능하나 멀티스레드로 진행되는 테스트에는 적합하지 않음
            - 매번 DB 컨넥션을 생성하지 않기 때문에 SimleDriverDataSource보다 빠르게 동작함
    - 오픈소스 DataSource
        * Apache Commons DBCP
            - 가장 유명한 오픈소스 DB 커넥션 풀 라이브러리
            - Apache의 Commons 프로젝트(http://commons.apache.org/dbcp/)
        
        * c3p0 JDBC/DataSource Resource Pool
            - JDBC 3.0 스펙을 준수하는 Connection과 Statement풀을 제공하는 라이브러리
            - http://www.mchange.com/projects/c3p0/
        * 두 가지 모두 수정자(setter) 메서드를 제공하므로 Spring Bean으로 등록해서 사용하기 편리



<br><br>

## Spring JDBC 설치 및 DataSource 설정
1. JDBC란?
    >모든 자바의 데이터 액세스 기술의 근간.<br>엔티티 클래스와 어노테이션을 이용하는 최신 ORM 기술(마이바티스, 하이버네이트)도 내부적으로는 JDBC를 이용
    * 안정적이고 유연한 기술이지만 로우 레벨 기술로 인식됨
    * 중복된 코드가 반복적으로 사용되며 DB에 따라 일관성 없는 정보를 가진채로 Checked Exception으로 처리한다.
        - 단점) Connection과 같은 공유 리소스를 제대로 릴리즈 해주지 않으면 시스템의 자원이 바닥나는 버그를 발생시킴
        - 장점) 별도의 학습없이 개발이 가능

2. Spring JDBC란?
    >JDBC의 장점과 단순성을 그대로 유지하면서도 기존 JDBC의 단점을 극복할 수 있게 해주고 간결한 형태의 API사용법을 제공, JDBC API에서 지원되지않는 편리한 기능 제공

    * Spring JDBC는 반복적으로 해야하는 많은 작업들을 대신 해줌
    * 실행할 sql이 뭔지 정의하고 이게 실행될 때 바인딩될 파라미터 정보와 실행결과를 어떤 객체에 넘겨받을지를 지정만 하면 됨
    * DB컨넥션을 가져오는 DataSource를 Bean으로 등록 후 사용해야한다

3. Spring JDBC가 해주는 작업
    - Connection 열기/닫기
        * 예외 발생시에도 열린 모든 Connection객체를 닫아줌으로써 리소스를 반납한다.
    - Statement 준비와 닫기
        * Statement 또는 PreparedStatement 를 생성하고 필요한 준비 작업을 해줌
        * Connection처럼 다 쓰고나면 닫기도 해줌
    - Statement 실행
    - ResultSet Loop 처리
        * ResultSet에 담긴 쿼리 실행결과가 한 건 이상이면 루프를 만들어 반복해줌
    - Exception 처리와 반환
        * Spring JDBC 예외 변환기가 처리
        * 체크 예외인 SQLException을 런타임예뢰인 DataAccessException 타입으로 변환한다. (체크 예외 - 개발자가 직접 try, catch구문을 통해 처리를 해줘야함 런타임예외는 이 일을 안해도 됨)
    - Transaction 처리
        * 트랜잭션에 관련된 모든 작업(commit, rollback등)에 대해서는 신경쓰지 않아도 됨
<br><br>

## Spring JDBC 개요 및 JdbcTemplate 클래스
1. JdbcTemplate 클래스
    >Spring JDBC의 모든 기능을 최대한 활용할 수 있는 유연성을 제공하는 클래스
    * 제공하는 기능
        - 실행
            * Insert와 Update와 같이 DB의 데이터에 변경이 일어나는 쿼리를 수행하는 작업
        - 조회
            * Select를 이용해 데이터를 조회
        - 배치
            * 여러개의 쿼리를 한 번에 수행해야하는 작업
2. 생성 방법
    ```java
    JdbcTemplate template = new JdbcTemplate(dataSource);
    ```

    - DataSource는 보통 Bean으로 등록해서 사용하므로 JdbcTemplate이 필요한 DAO 클래스에서 DataSource Bean을 DI받아서 JdbcTemplate 생성할 때 인자로 넘겨준다
    - 멀티스레드 환경에서도 안전하게 공유해서 쓸 수 있기 때문에 DAO 클래스의 인스턴스 변수에 저장해놓고 사용할수도 있다.

3. JdbcTemplate 클래스 생성 코드
* DataSource에 대한 수정자 메서드(setter)에서 직접 JdbcTemplate 객체를 생성해준다.

4. JdbcTemplate 클래스 update()메서드
    * INSERT, UPDATE, DELETE와 같은 SQL을 실행할 때는 JdbcTemplate 의 update() 메서드 사용
    ```java
    int update(String sql, [SQL 파라미터])
    ```
    * update() 메서드를 호출할 때는 SQL과 함께 바인딩 할 파라미터는 Object 타입 가변인자(Object ... args) 사용 가능(여러개 가능)
    * update() 메서드의 리턴되는 값은 SQL 실행으로 영향을 받은 레코드의 개수를 리턴

5. JdbcTemplate 클래스의 queryForObject() 메서드
    * SELECT SQL을 실행하는 메서드
        - queryForObject()
            * 여러개의 컬럼
            * 한개의 Row 형태로 반환
        - query()
            * 여러개의 컬럼
            * 여러개의 Row 반환
        

    > 하나의 Row를 가져올 때는 JdbcTemplate 클래스의 
    queryForObject()메서드 사용

    ```java
    <T> T queryForObject(String sql, [SQL 파라미터], RowMapper<T> rm)
    ```
    * SQL 실행 결과는 여러개의 칼럼을 가진 하나의 로우를 처리하는 메서드
    * T는 VO 객체의 타입에 해당됨
        - 칼럼이 여러개고 로우가 하나인 결과를 받을 때 씀
    * RowMapper는 VO객체로 매핑해주는 작업을 해줌

6. JdbcTemplate 클래스의 queryForObject() 메서드 Code
     
    ```java
    물음표에 바인딩될 파라미터

    박스로 처리된 부분이 세번째 파라미터(RowMapper)
    ```

7. JdbcTemplate 클래스의 query() 메서드
    
    ```java
    <T> List<T> query(String sql, [SQL 파라미터], RowMapper<T> rm)
    ```
    * 여러개의 Row를 가져올 때 사용
    * 여러개의 로우를 받기 위해 리스트형태로 받는다
