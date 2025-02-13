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
