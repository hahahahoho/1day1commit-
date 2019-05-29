스프링부트 적용에 앞서 그동안 js와 node만 쓰다보니... 환경설정과 java프로젝트에 대해서 아는것이 너무 없었다는 생각이 들었습니다. 그래서 프레임워크의 동작원리부터 어떤 특징을 갖고있는지 그리고 db와 연결하기 위해서는 어떠한 방법들이 있는지 찾아보고 공부하였습니다. DI, AOP의 개념을 적용시키기 위해 기본부터 다시 보았고 간단하게 요약해서 먼저 정리하였습니다. 내일부터는 조금 더 자세한 내용으로 찾아뵙겠습니다 ^^

# 1.Spring boot

###     - 특징

  		1. 의존주입
  		2. 관점지향				



###     - 모듈

- core, testing, dataAccess, webServlet, webReactive



# 2. JDBC(Java DataBase Connector)

#### 	1. JDBC load

#### 	2. DBMS 연결

#### 	3. SQL문 전송 및 결과 return (connection : 연결, statement : 결과)

```java
//순수자바 code
Connection con
class.forName("driverClassName");
con = DriverManager.getConnection(url, user,password);
Statement stat = con.createStatement();
String sql = "select * from test";
resultSet result = statement.excuteQuery(sql);
connection.close();
```

```java
//jdbcTemplate
@Autowired
JdbcTemplate jdbcTemplate
...
jdbcTemplate.excute("DROP TABLE test IF EXIST");
jdbcTemplate.batchUpdate("INSERT ~~");
jdbcTemplate.query("select ~~");
```

```java
//myBatis
xml을 통하여 sqml문을 작성하고 매핑. 
//JPA
Java ORM 기술표준. 관계형 데이터베이스에 접근하기 위한 ORM
 *ORM : 객체와 관계형DB를 매핑합낟. mybatis는 orm이 아님. 단지 sql구문을 매핑하여 실행하는 mapper
@Entity : 엔티티임을 정의
@Table : (name="테이블이름") - 매핑 테이블명/대소구분
@id : pk매핑
@column : 필드를 컬럼에 매핑
```





#### 