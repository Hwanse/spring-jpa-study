## 객체지향 쿼리
SQL문은 데이터 베이스 테이블을 대상으로 데이터 중심의 쿼리라면, ORM을 사용하면 데이터 중심이 아닌 엔티티를    
대상으로 개발하므로 엔티티 객체를 대상으로 한 쿼리가 필요하다. JPA에서는 이러한 문제를 해결하기 위해서 JPQL    
을 만들었다. JPQL은 JPA가 JPQL의 쿼리문을 분석하여 적절한 SQL을 만들어 DB를 조회하고 결과로 엔티티 객체를    
생성해서 반환한다. 

<br>

### JPA가 지원하는 다양한 기능
- JPQL (Java Persistence Query Language)
- Criteia 쿼리 (Criteria Query)
- 네이티브 SQL (Native SQL)

JPA가 공식 지원하지는 않지만 함께 사용할 수 있는 것들
- QueryDSL
- JDBC를 직접 사용하는 SQL 매퍼
 
<br>

#### JPQL 소개
- 객체를 대상으로 검색하는 객체지향 쿼리
- SQL을 추상화한 특정 데이터베이스 SQL에 의존하지 않는 쿼리

<br>

#### Criteria 소개
- JPQL을 생성하는 빌더 클래스
- 문자가 아닌 query.select(m).where(...) 처럼 프로그래밍 코드로 JPQL을 작성 가능
- 컴파일 시점에 오류를 발견할 수 있음
- IDE의 코드 자동 완성 기능과 같은 지원을 받을 수 있음
- 동적 쿼리 작성이 편리    

But 이러한 장점들이 있으나 이러한 모든 장점들을 상쇄할 정도로 코드가 복잡해지고 장황해진다. 이로인하여 사용성     
측면에서 붎편하고 Criteria로 작성된 코드는 가독성도 떨어진다. 

<br>

#### QueryDSL 소개
JPA 표준이 아닌 오픈소스 프레임워크다. Criteria 같이 JPQL 빌더 역할을 하지만 Criteria 보다 단순해서 사용성    
측면에서 쉽고 QueryDSL로 작성한 코드는 JPQL과 비슷하여 가독성이 좋다. 
현재 Criteria 보다는 편리하기 때문에 이 기능을 사용할 것을 권장하고 Spring-Data-Jpa 프로젝트에서도 지원

<br>

#### 네이티브 SQL
JPA에서 SQL을 직접 사용할 수 있는 기능, 보통 특정 DB에 의존하고 있는 기능을 사용할 필요성이 있을 때, 네이티브   
SQL을 사용하여 기능 구현을 한다. 예로 Oracle의 Start With .. Connect By 와 같은 특정 기능사항.     
그러나 특정 DB를 의존하는 기능이기 때문에 DB가 변경되면 해당 쿼리도 교체된 DB로 마이그레이션을 해야한다.

<br>

    

