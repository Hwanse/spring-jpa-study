## JPQL
JPQL은 SQL처럼 SELECT, UPDATE, DELETE 문을 사용할 수 있다. 그러나 엔티티를 저장할 때 EntityManager 에서     
제공하는 persist()를 통해 영속성컨텍스트가 Insert를 수행해주므로  JPQL에서 INSERT 문은 없다.

- 대소문자 구분    
엔티티와 속성은 대소문자를 구분한다. 그러나 JPQL에서 사용하는 SELECT, FROM, WHERE 같은 문법 구문들은    
대소문자를 구분하지 않는다.    
- 엔티티 이름     
JPQL를 사용할 때 FROM 절에 Member와 같이 명시되는 것들은 클래스 명이 아닌 엔티티명이다. 즉, 기본적으로 연관관계    
매핑 시 클래스 명으로 자동으로 엔티티 등록을 해주지만 엔티티 이름을 상황에 때라 변경이 가능하기 때문에 클래스명과     
혼동해서 사용해서는 안된다는 뜻이다. 그래도 보통 클래스 명을 엔티티 명으로 사용하는 것을 권장
- 별칭은 필수다     
Member와 같은 엔티티를 JPQL에서 사용한다고 가정했을 때, 반드시 `Member As m` or `Member m` 같이 별칭을 필수로   
사용해야 한다. 별칭을 사용하지 않으면 문법 오류가 발생한다.

<br>

### Type Query, Query
JPQL을 실행하려면 쿼리 객체를 만들어야 한다. 쿼리 객체로 TypeQuery, Query가 있다.

- TypeQuery : 반환 타입이 명확하게 있는 쿼리     
ex) `select m from Member m` 와 같은 쿼리면 em.createQuery(쿼리문장, 반환 타입) 처럼 사용
- Query : 반환 타입이 명확하지 않은 쿼리
ex) `select m.username, m.age from Member m` 와 같이 반환 타입이 명확하지 않을 때 사용    
 반환 결과가 1개이면 Object로 반환되고, 둘 이상이면 Object[] 로 반환
 
결과적으로는 개발 사용성 측면에서는 TypeQuery가 더 용이하다.

#### 결과 조회
- query.getResultList() : 결과를 List로 반환하며, 결과가 없으면 빈 컬렉션을 반환함.
- query.getSingleResult() : 결과 데이터 건수가 정확히 하나일 때 사용     
결과가 1건도 없을 땐 javax.persistence.NoResultException 예외 발생    
결과 데이터가 1건보다 많으면 javax.persistence.NonUniqueResultException 예외 발생

### 파라미터 바인딩
- 이름을 기준으로 한 파라미터 바인딩     
파라미터를 이름으로 구분하는 방식, 이름 기준 파라미터 사용 시 이름앞에 `:`를 붙여서 사용한다.
- 위치 기준 파라미터     
파라미터의 위치(index)를 기준으로 바인딩하는 방식, `?`를 위치값 앞에 붙여서 사용    

=> 결론은 이름을 기준으로 한 파라미터 바인딩 방식을 사용하는 것이 여러모로 장점이 많다.
- SQL을 위 방식들을 사용하는 것이 아닌 직접 문자열에 더해 만들어 사용하게 된다면 SQL 인젝션 공격 위험 있음
- 성능 적인 이점, JPA는 JPQL을 SQL로 파싱한 결과를 재사용이 가능해짐
- DB에서도 내부에서 실행한 SQL을 파싱해서 재사용하기 때문에 겨로가 적으로 애플리케이션, DB 단 양쪽에서 쿼리 파싱    
결과를 재사용이 가능

<br>

### 프로젝션
SELECT 절에 조회할 대상을 지정하는 것을 프로젝션이라 한다. 프로젝션 대상으로 엔티티, 임베디드 타입, 스칼라 타입    
등이 있다. 여기서 스칼라 타입은 숫자, 문자와 같은 기본형 타입을 말함.

- 엔티티 프로젝션     
조회한 엔티티는 영속성 컨텍스트에서 관리됨
- 임베디드 프로젝션    
임베디드 타입은 엔티티 타입이 아닌 값 타입으로, 띠라서 영속성 컨텍스트에서 관리되지 않는다.
- 스칼라 타입 프로젝션
숫자, 문자, 날짜와 같은 기본 데이터 타입들을 조회할 때
- new 명령어
DTO와 같은 의미있는 객체로 변환이 가능, SELECT 절에서 new 키워드를 사용해서 클래스의 FCQN 값으로 생성자를 호출     
이 가능함. 

<br/>

#### 서브쿼리
- JPQL도 SQL 처럼 서브 쿼리를 지원함
- WHERE 절 서브쿼리 예시    
`select m from Member m where m.age > (select avg(m2.age) from Member m2)`
- 서브쿼리 함수 종류    
`EXISTS`, `[ALL|ANY|SOME]`, `IN`

#### JPA 서브 쿼리 한계
- JPA는 WHERE, HAVING 절에서만 서브쿼리가 사용이 가능하다
- SELECT 절도 가능하지만 JPA 표준이 아닌 하이버네이트에서 지원해줌
- FROM 절의 서브 쿼리는 현재 JPQL에서 불가능하다
     - Join 으로 풀어서 할 수 있으면 Join