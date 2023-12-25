### **JPA란?(Java Persistence API)**

JPA는 자바에서 사용하는 ORM(Object-Relational Mapping) 기술 표준이다.

JPA는 자바 애플리케이션과 JDBC 사이에서 동작하며, 자바 인터페이스로 정의되어 있다.

**ORM: Object-Relational Mapping(객체 관계 매핑)**

- 객체와 관계형 데이터베이스의 데이터를 매핑하는 기술
- ORM 프레임워크가 객체와 데이터베이스 중간에서 매핑
- 객체와 테이블을 매핑하여 **패러다임 불일치 문제**를 해결

### **JPA의 동작**

JPA는 애플리케이션과 JDBC 사이에서 동작한다.

</br>
      <img width="600" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/141606477/b7b7b584-dd3d-4f1a-b845-cf7988f2b95d">
<br/>
</br>

JPA는 JDBC API를 사용하여 데이터베이스와 데이터를 주고받게 된다.

**저장**

</br>
      <img width="600" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/141606477/394f3f1a-8cd2-476d-a76a-250f98a5304c">
<br/>
</br>

- MemberDAO 클래스를 통해 persist()를 실행하면, JPA가 Entity 객체를 분석하여 SQL문을 생성한다.
- JDBC API를 사용하여 DB에 생성된 INSERT SQL을 보내게 된다.
- 이 과정에서 JPA는 객체와 데이터베이스 테이블의 패러다임 불일치를 해결한다.

**조회**

</br>
      <img width="600" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/141606477/bb401d62-06e3-49e3-9c18-5b929e078fbf">
<br/>
</br>

MemberDAO 클래스를 통해 find(id)를 실행하면, JPA는 SELECT SQL을 생성한다.

- JDBC API를 사용하여 생성된 SELECT SQL을 보낸다.
- DB에서 반환된 정보를 ResultSet 매핑을 통해 객체로 변환해 준다.
- 이 과정에서도 패러다임 불일치 문제를 해결해 준다.

### **JPA를 사용하는 이유**

기존의 개발 방식은 SQL 중심적인 개발이었다.

JPA를 사용하면 객체 중심으로 애플리케이션 개발이 가능하다.

**생산성(CRUD)**

JPA를 사용하면 기본적으로 생산성이 높아진다. JDBC 방식의 경우 SQL 쿼리문을 직접 작성해야 데이터베이스에 접근할 수 있다.

하지만, JPA는 쿼리문을 별도로 작성할 필요가 없다.

다음과 같이 간단한 메서드를 통해 CRUD가 가능하다.

- 저장 : jpa.persist(member)
- 조회 : Member member = jpa.find(memberId)
- 수정 : member.setName(”변경할 이름”)
- 삭제 : jpa.remove(member)

**유지보수**

기존에는 엔티티 클래스의 필드가 변경되면 모든 SQL을 수정해야 했다.

JPA에서는 쿼리를 직접 작성하지 않기 때문에 필드가 변경되더라도 매핑 정보만 잘 연결하면 SQL문은 자동으로 작성된다.

**패러다임의 불일치 문제 해결**

상속, 연관관계, 객체 그래프 탐색, 비교 등의 설계 차이로 인해 발생하는 패러다임 불일치 문제를 해결한다.

- 객체는 상속 구조를 만들 수 있으며, 다형성 구현이 가능하지만, 관계형 데이터베이스의 테이블은 상속이라는 개념이 존재하지 않는다.
- 객체는 참조를 통해 관계를 표현하며 방향을 가지고 있으나, 관계형 데이터베이스는 외래 키를 통해 관계를 표현하며, 방향이 존재하지 않는다. 또한, 다대다 관계 문제를 해결하기 위해 조인을 사용한다.
- 이 외에도 다양한 패러다임 불일치 문제가 존재

이와 같이 객체와 데이터베이스는 서로 다른 목적을 가지고 설계되었기 때문에 매핑하는 데 있어 여러 문제가 발생한다. 하지만 JPA를 사용하면 이러한 문제를 모두 해결할 수 있다.


**참고자료**

https://ittrue.tistory.com/253

인프런 강의- '자바 ORM 표준 JPA 프로그래밍 - 기본편', 김영한
