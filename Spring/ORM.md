# ORM

## 1. 영속성(Persistence)

> 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성, 데이터베이스에 데이터를 저장하는 핵심 개념

> 영속성을 가지지 못 하면, 메모리(램)에서만 존재하기에 프로그램을 종료하면 사라진다.

> 데이터를 데이터베이스에 저장하는 2가지 방법

- JDBC API
- Persistence Framework( Mybatis, JPA - Hybernate )

> Persistence Framework는 2 종류로 다시 구분됩니다.

- SQL MAPPER : MyBatis
- ORM : JPA

## 2. JDBC API

> JDBC API란?

- JAVA의 데이터 연결 표준, DB 종류에 맞추어 맞춤형 Driver가 내장되어 있음 => 특정 DBMS에 종속되지 않음
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb27DeN%2FbtrcXkEZZlA%2FnKTyeCwdKjUejVdbOveKH1%2Fimg.png"/>

- 아래와 같은 작업을 직접 세팅해야 되기 때문에, 중복코드-예외처리 등의 문제가 발생
  <img src="https://github.com/backtony/blog-code/blob/master/interview/jdbc-sqlmapper-orm/img/jdbc-sqlmapper-orm-6.PNG?raw=true"/>
- JDBC의 문제를 해결하기 위해 Persistence Framework 등장

## 3. SQL MAPPER

- 객체와 SQL 문을 매핑하여 데이터를 객체화
- SQL MAPPER는 SQL 쿼리문 결과와 객체를 매핑해주는 것, 객체와 관계를 매핑해주는 ORM과 다름
- SQL 문을 직접 작성하여 데이터베이스의 데이터를 다룸(ORM과의 차이점)
- MySQL 같은 SQL 문법을 그대로 사용하기 때문에 새로운 문법을 배울 필요 없음(러닝커브 적음)

> # MyBatis

### [ 장점 ]

- JDBC API 대비 단순해진 코드
- SQL 쿼리들을 XML 파일에 작성하여 코드와 SQL을 분리하여 관리
- 코드의 간결성 및 유지보수가 쉬워짐
- 복잡한 동적 쿼리 작성이 쉬움

### [ 단점 ]

- 사용하는 쿼리 문법이나 데이터 타입에 따라 특정 DBMS에 종속될 수 있음(예약어 관련)
- 단순 CRUD에서는 여전히 반복적인 작업을 수행
- 테이블이 수정되면 전체 코드들을 수정해야 하는 문제점
- 코드상으로는 SQL과 JDBC를 분리하는데 성공했지만, 논리적으로는 아직 강한 의존성 <br/>따라서 여전히 SQL 의존적인 개발 의 문제점
  <br/> => 관계형 DB는 관계를 지향, 객체지향 프로그래밍은 객체를 지향하기 때문에 패러다임 불일치 문제

## 4. ORM

- Object-Relational Mapping(객체 관계 매핑), 객체와 데이터베이스의 데이터를 매핑하여 데이터를 객체화
- ORM 프레임워크가 객체와 데이터베이스를 자동으로 매핑해주기 때문에, 객체와 관계형 데이터베이스를 개별적으로 설계 가능
- 객체 간의 관계를 코드로 작성하면 이를 바탕으로 SQL 문을 자동으로 생성(SQL MAPPER와의 차이점)

> # JPA

### JPA 동작 과정

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnbMBs%2FbtrcMZCYSUM%2FNbytKvoXneDxsFN7mZ3BdK%2Fimg.png"/>

### [ 장점 ]

- Java 코드만 작성해도 자동으로 CRUD를 생성해주고, 테이블 변경 시에도 객체 수정시 JPA가 처리해주기 생산성이 높음
- JPA는 인터페이스이기 때문에 특정 DBMS 에 종속적이지 않다. 코드 세팅만 약간 수정해 주면 해결(SQL MAPPER 와의 차이점)

### [ 단점 ]

- 기존 SQL과 문법이 달라서 추가 학습이 필요하기에, 러닝커브가 MyBatis 대비 높음
- JPA 만의 동작원리를 제대로 이해하지 못한 채로 사용한다면 성능 문제가 발생 (N + 1 문제)
- JPA 만으로는 복잡한 쿼리 사용이 어려움, Query DSL 등의 추가적인 활용 필요

## 5. 코드 비교

> 코드를 통해 MyBatis, JPA 두 프레임워크의 차이를 비교해봅니다. 편의를 위해 어노테이션 방식으로 진행합니다.

## 조회(SELECT)

### MyBatis

```
public interface UserMapper {
  @Select("SELECT * FROM users WHERE id = #{id}")
  User findById(int id);
}
```

### JPA

```
public interface UserRepository extends JpaRepository<User, Integer> {
  Optional<User> findById(Integer id);
}
```

## 삽입(INSERT)

### MyBatis

```
public interface UserMapper {
  @Insert("INSERT INTO users (name, email) VALUES (#{name}, #{email})")
  @Options(useGeneratedKeys = true, keyProperty = "id")
  int insert(User user);
}
```

### JPA

```
public interface UserRepository extends JpaRepository<User, Integer> {
  User save(User user);
}
```

> # 코드를 통해 알 수 있는 점

- JPA가 더 간단하고 효율적으로 코드를 구성 할 수 있다.
- 단, MyBatis 대비 복잡한 기능을 구현하는 부분에서는 제한적일 수 있다.

## 6. 참고

- 10분 테코톡 https://www.youtube.com/watch?v=RWFtuQUx3fo
- ORM이란 https://gmlwjd9405.github.io/2019/02/01/orm.html
- JDBC vs SQL Mapper vs ORM https://sorjfkrh5078.tistory.com/296
