# MyBatis
- 데이터베이스를 쉽게 다룰 수 있도록 해주는 SQL 매핑 프레임워크
- JDBC로 처리하는 상당부분의 코드와 파라미터 설정 및 결과 처리를 대신 처리해준다.
- Map 인터페이스 그리고 자바 POJO를 설정 데이터베이스와 매핑해서 사용할 수 있다.
- **동적 퀴리 작성이 가능하다**

# MyBatis 사용 목적
- 데이터베이스 쿼리 <-> 프로그래밍 언어 코드를 분리하여 유지보수성과 생산성을 높인다

# MyBatis 장점
- 유연성 : SQL 쿼리 직접 작성할 수 있고 동적 쿼리 작성 가능하다.
- 간결성 : 쿼리와 프로그래밍 언어 코드를 분리하기 때문에 코드가 간결하고 유지보수에 용이하다 
- 성능 : 캐시 기능을 제공하여 데이터베이스 연산 속도를 높일 수 있따.
- 다양한 데이터베이스 지원

# MyBatis-Spring 모듈
- MyBatis와 Spring를 간편하게 연동하도록 해주는 모듈
- Spring framework와 MyBatis 간에 연관성은 없다. 우리가 편하게 쓰려고 연동하는 것.
- 해당 모듈은 MyBatis로 하여금 Spring transaction에 쉽게 연동되도록 처리한다.

# MySQL
```mysql
DROP DATABASE IF EXISTS ssafy_board;
CREATE DATABASE ssafy_board DEFAULT CHARACTER SET utf8mb4;

USE ssafy_board;

CREATE TABLE board (
	id INT AUTO_INCREMENT,
    writer VARCHAR(20) NOT NULL,
    title VARCHAR(50) NOT NULL,
    content TEXT,
    view_cnt INT DEFAULT 0,
    reg_date TIMESTAMP DEFAULT now(),
    PRIMARY KEY(id)
);

INSERT INTO board(title, writer, content) 
VALUES ("BackEnd 너두 마슷허","양씨","너도 할 수 있어"),
        ("누르지마시오", "따봉맨", "아무내용없음"),
        ("대답잘하는 법", "구루마", "채팅 잘치면됨 네(필쑤)");
```

# DB 설정
- application.properties에 작성
- DB 연결 정보(DataSource) 등록
```
# dataSource
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/ssafy_board?serverTimezone=UTC
spring.datasource.username=ssafy
spring.datasource.password=ssafy

# mybatis
mybatis.mapper-locations=classpath*:mappers/*.xml
mybatis.type-aliases-package=com.ssafy.board.model.dto
```

# MyBatis와 Spring Boot 연동 설정
- mybatis-x.x.x.jar 파일을 프로젝트에 추가
- maven 프로젝트를 사용한다면 pom.xml에 의존성 추가
```xml
<!--MyBatis-Spring boot 연동 -->
<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.3.1</version>
</dependency>
<!--MySQL 연동-->
<dependency>
			<groupId>com.mysql</groupId>
			<artifactId>mysql-connector-j</artifactId>
			<scope>runtime</scope>
</dependency>
```

# DBConfig 클래스
```java
package com.ssafy.board.config;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan(basePackages = "com.ssafy.board.model.dao")
public class DBConfig {

}
```
### @Mapper
- 매퍼 등록을 위한 인터페이스에 선언하여 사용
### @MapperScan
- 패키지 경로에 위치한 인터페이스를 모두 매퍼로 사용

# DAO 작성
```java
package com.ssafy.board.model.dao;

import java.util.List;

import com.ssafy.board.model.dto.Board;
import com.ssafy.board.model.dto.SearchCondition;

public interface BoardDao {
	// 전체글 조회
	public List<Board> selectAll();
}
```
# Mapper 작성
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ssafy.board.model.dao.BoardDao">
	<!-- 전체글 조회 -->
	<select id="selectAll" resultType="Board">
		SELECT id, content, writer, title, view_cnt as viewCnt, date_format(reg_date, '%Y-%M-%d') as regDate 
		FROM board;
	</select>
</mapper>
```
