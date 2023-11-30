# 트랜잭션(Traonaction)이란?
- 여러 작업을 진행하다가 문제가 생겼을 경우 이전 상태로 롤백하기 위해 사용되는 것
- 더 이상 쪼갤 수 없는 최소 작업 단위
     - 커밋: 작업이 마무리 됨
     - 롤백: 작업을 취소하고 이전의 상태로 돌림

# Transactional 방법 2가지
1. XML
```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
     <property name="dataSource" ref="dataSource"/>
</bean>
<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
```
2. 어노테이션
- 클래스 또는 메서드에 선언
```java
// BoardRestController
@Transactional
@Override
public void modifyBoard(Board board) {
     boardDao.updateBoard(board);
}
```

# 동작 원리
- SpringAOP를 통해 구현
```java
import org.springframework.transaction.annotation.Transactional;
```
- 클래스, 메소드에 @Transactional이 선언되면 해당 클래스에 트랜잭션이 적용된 프록시 객체 생성
- 프록시 객체는 @Transactional이 포함된 메서드가 호출될 경우, 트랜잭션을 시작하고 Commit or Rollback을 수행
- CheckedException or 예외가 없을 때는 Commit
- UncheckedException이 발생하면 Rollback

# 우선순위
> 클래스 메소드 > 클래스 > 인터페이스 메소드 > 인터페이스
- 자바 어노테이션은 인터페이스로부터 상속되지 않기 때문에 인터페이스 기반 프록시에서만 인식한다
