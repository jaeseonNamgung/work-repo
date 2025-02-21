# SQL 실행 순서
1. FROM : 쿼리의 대상이 되는 테이블을 선택
2. WHERE : 특정 조건을 만족하는 행만 선택
3. GROUP BY : 특정 열을 기준으로 그룹을 만듬
4. HAVING: 그룹에 대한 조건을 지정
5. SELECT: 조회할 열을 선택
6. DISTINCT: 중복된 결과를 제거
7. ORDER BY: 결과를 정렬
8. LIMIT / OFFSET: 결과를 제한하거나 오프셋을 지정

# NVL, NVL2 함수

- NVL: 값이 `NULL` 인 경우 지정 값을 출력하고, `NULL` 이 아닌 경우 원래 값을 그대로 출력한다.
  - 예: `NVL('값', '지정 값')`
- NVL2: `NULL` 이 아닌 경우 지정 **값1**을 출력하고, `NULL` 인 경우 지정 **값2**를 출력한다.
    - 예: `NVL2('값', '지정 값1', '지정 값2')`

# ROW_NUMBER 함수

- 각 PARTITION 내에서 ORDER BY 절에  의해 정렬된 순서를 기준으로 고유한 값을 반환하는 함수
- 윈도우 함수(Window Function)로 그룹 내 순위 함수이다.

### 문법

```sql
ROW_NUMBER() OVER(PARTITION BY [그룹핑할 컬럼] ORDER BY [정렬할 컬럼]) 
# PARTITION BY는 선택, ORDER BY는 필수
```

# 파티션(Partition)

## 파티션 이란?

- 테이블이나 인덱스 테이블를 파티션 단위로 나누어 저장하는 것을 말한다.
- 논리적으로는 하나의 테이블이나 인덱스를 여러 물리적 저장 공간에 저장하는 것이다.
- 대용량의 데이터를 관리하고 성능을 향상 시킬 수 있다.

## 파티션을 사용하는 이유

- 관리적 측면:
    - 파티션 단위의 data 작업이 수월하다.
- 성능적 측면:
    - 파티션 단위의 DML 수행의 효율 증가, 부하 분산에 있다.

## 관련 키워드

### Partition Key

- 파티션을 나누어 기준이 되는 Column을 Partition Key라고 한다.
- 파티션의 종류에 따라 Partition Key는 단일 또는 복합으로 구성될 수 있다.

### 파티션 종류

- Range Partition: 파티션 키를 기준으로 삼는 범위로 파티션을 구분
- List Partition:  특정 값으로 파티션을 구분
- Hash Partition: 해쉬 값을 통한 파티션 구분

### Partition Index

- Local Index: 파티션의 키 값을 기준으로 생성된 인덱스
- Global Index: 파티션의 키 값과는 별개의 인덱스

# Mybatis에서 select  조회 시 DB VARCHAR 타입을 JAVA  Date 타입으로 값 매핑하는 법

```sql
<resultMap id="userResultMap" type="com.example.model.User">
    <result column="reg_date" property="regDate" javaType="java.util.Date"/>
</resultMap>
```

MyBatis의 `resultMap`에서 **`javaType`을 지정**하면 MyBatis가 알맞은 JAVA 타입으로 자동 변환해준다.

# SimpleDateFormat

### **`SimpleDateFormat` (`java.util.Date`)**

- `SimpleDateFormat`은 Java의 `java.util.Date`를 기반으로 동작
- `java.util.Date`는 시스템의 현재 시간을 사용하며, **타임존**과 관련된 설정을 사용할 수 있다.
- 기본적으로 `SimpleDateFormat`은 시스템에서 설정된 **로컬 타임존**을 기준으로 시간을 출력

# SYSTIMESTAMP

### **`TO_CHAR(SYSTIMESTAMP, 'YYYYMMDD')` (Oracle)**

- `SYSTIMESTAMP`는 **Oracle**의 함수로, 시스템의 **현재 타임스탬프**를 반환한다.
- 이 타임스탬프는 **타임존을 포함한** 값으로 반환된다. 따라서 Oracle DB에서 **서버의 타임존**을 기준으로 값을 반환한다.
- 
# Mybatis Date, LocalDateTime 지원 여부
- Mybatis 3.3.4 이후 버전 부터 Date, LocalDateTime 을 지원한다
- Mybatis 3.3.4 이전 버전은 직접 TypeHandler 구현이 필요하다.

# JDBC Type 오류

```bash
org.mybatis.spring.MyBatisSystemException: nested exception is 
org.apache.ibatis.type.TypeException: Could not set parameters for mapping: 
ParameterMapping{property='address', mode=IN, javaType=class java.lang.Object, 
jdbcType=null, numericScale=null, resultMapId='null', 
jdbcTypeName='null', expression='null'}. 

Cause: org.apache.ibatis.type.TypeException: 
Error setting null for parameter #6 with JdbcType OTHER . 

Try setting a different JdbcType for this parameter or 
a different jdbcTypeForNull configuration property. 

Cause: java.sql.SQLException: 부적합한 열 유형: 1111
```

이 오류는 Null 값을 입력하면서 JdbcType을 다른 것으로 지정했기 때문에 **org.apache.ibatis.type.TypeException** 예외가 발생한 것이다.

### 해결 방법

파라미터에 다른 JdbcType을 설정하거나, Configuration 속성에 jdbcTypeForNull을 설정하면 된다.

1. 해당 컬럼에 JDBC Type 명시

```sql
<insert id="insertData">
    INSERT INTO mytable (컬럼명)
    VALUE(#{컬럼명, jdbcType=컬럼_데이터타입})
</insert>
```

1. **mybatis 설정 파일에 Null 처리를 하도록 명시**

```sql
 <settings>
     <setting name="jdbcTypeForNull" value="NULL" />
 </settings>
```
# **SqlSessionFactory**

SqlSessionFactory는 데이터베이스와의 연결과 SQL의 실행에 대한 모든 것을 가진 가장 중요한 객체다.

이 객체가 DataSource를 참조하여 MyBatis와 Mysql 서버를 연동시켜준다.

SqlSessionFactory를 생성해주는 SqlSessionFactoryBean 객체를 먼저 설정하여야 한다.

root-context.xml

```sql
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

# Mybatis Transaction

공식 문서 마이바티스 스프링 연동모듈을 사용하는 중요한 이유중 하나는 마이바티스가 스프링 트랜잭션에 자연스럽게 연동될수 있다는 것이다. 마이바티스에 종속되는 새로운 트랜잭션 관리를 만드는 것보다는 마이바티스 스프링 연동모듈이 스프링의 `DataSourceTransactionManager`과 융합되는 것이 좋다. 스프링 트랜잭션 관리자를 한번 설정하면, 대개의 경우처럼 스프링에서 트랜잭션을 설정할 수 있다. `@Transactional` 애노테이션과 AOP스타일의 설정 모두 지원한다. 하나의 `SqlSession`객체가 생성되고 트랜잭션이 동작하는 동안 지속적으로 사용될것이다. 세션은 트랜잭션이 완료되면 적절히 커밋이 되거나 롤백될것이다. 마이바티스 스프링 연동모듈은 한번 셋업되면 트랜잭션을 투명하게 관리한다. DAO클래스에 어떠한 추가적인 코드를 넣을 필요가 없다.

## Transaction **설정**

스프링 트랜잭션을 가능하게 하려면, 스프링 설정파일에 `DataSourceTransactionManager`를 생성해줘야 한다.

```java
@Configuration
public class DataSourceConfig {
  @Bean
  public DataSourceTransactionManager transactionManager() {
    return new DataSourceTransactionManager(dataSource());
  }
}
```

명시된 `DataSource`는 스프링을 사용할때 일반적으로 사용한다면 어떠한 JDBC `DataSource`도 될수 있다. JNDI룩업을 통해 얻어진 `DataSource`뿐 아니라 커넥션 풀링 기능도 포함한다.

트랜잭션 관리자에 명시된 `DataSource`가 `SqlSessionFactoryBean`을 생성할때 사용된 것과 **반드시** 동일한 것이어야 한다.  그렇지 않으면 트랜잭션 관리가 제대로 되지 않는다.
