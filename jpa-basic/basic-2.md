## 기본 키 매핑 전략
기본 키를 애플리케이션에서 직접 할당하는 것이 아니라면 DB 벤더사 별로 DB에서 지원하는 기본 키 생성 방식은<br>
다르다. <br/> 
ex) 오라클(시퀀스), MySQL(Auto_Increment)

<br/>

## JPA의 기본 키 생성 전략 종류
1. 직접 할당 : 애플리케이션 내부에서 기본키를 직접 할당
2. 자동 생성
    - IDENTITY : 기본 키 생성을 DB에 위임
    - SEQUENCE : DB의 시퀀스를 이용해 키 할당
    - TABLE : 키 생성 전용 채번 테이블을 사용
    - AUTO : DB 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 자동으로 1가지를 선택(default)
 
**자동 생성 전략 사용 시 사용하는 애노테이션** : @GeneratedValue
 
 <br/>
 
 **직접 할당 전략**<br/>
 @Id 적용 가능한 자바의 자료형 타입 종류가 다음과 같음
 - 자바 프리미티브 자료형
 - 자바 래퍼형
 - String
 - java.util.Date
 - java.sql.Date
 - java.math.BigDecimal
 - java.math.BigInteger
 
 <br/>
 
 **IDENTITY 전략**<br/>
 기본 키 생성을 DB에 위임하는 전략, MySQL Auto_Increment 같은 DB가 자동으로 생성해주는 키를 이용<br/>
 엔티티가 영속 상태가 되려면 식별자가 반드시 필요한데, IDENTITY 전략은 엔티티를 DB에 저장해야 식별자를<br/>
 구할 수 있다. 그래서 엔티티 매니저는 em.persist() 호출하는 즉시 INSERT SQL이 DB에 전달된다.<br/>
 __주의점 : 이 전략은 트랜잭션이 지원하는 쓰기 전략 지연이 동작하지 않음__
 
 <br/>
 
 **SEQUENCE 전략**<br/>
 DB의 Sequence를 사용해서 기본 키를 할당하는 전략. Sequence 방식은 내부 동작이 persist()를 호출할 때 <br/>
 먼저 DB의 sequence를 사용해 Key를 조회하고 엔티티에 Key값을 할당하여 해당 엔티티를 영속성 컨텍스트에 저장<br/>
 이후 트랜잭션이 커밋될 때 flush가 일어나면서 엔티티를 DB에 저장한다.
 __주의점 : sequence 전략의 allocationSize는 성능 최적를 위해서 50이 default 값이다.__
 
 <br/>
 

## 권장하는 식별자 선택 전략
식별자는 다음과 같은 3가지 조건을 만족해야함.
1. Null은 허용하지 않는다
2. 유일해야 한다
3. 불변, 변하지 않아야 한다

## 테이블 기본 키 전략 2가지
- 자연 키
  - 비즈니스에 의미가 있는 키 ex) 주민번호, 이메일, 전화번호
- 대리 키
  - 비즈니스와 관련 없는 임의로 만들어지 키, 대체 키로도 불림 ex) 시퀀스, auto_increment, 채번 테이블

## 실무에서 유의 사항
1. 자연 키 사용보다 대리 키를 권장
     - 현실에서 비즈니스 규칙은 쉽게 변할 수 있기 때문
2. JPA는 모든 엔티티에 일관된 방식으로 대리 키 사용을 권장

## 영속성 컨텍스트와 식별자
영속성 컨텍스트는 엔티티를 보관할 때 엔티티의 식별자(id)를 Key로 사용한다. 엔티티를 구분할 때 이 식별자를   
사용해서 구분을 하고, 식별자를 구분하기 위해서 equals와 hashCode를 사용해 동등성을 비교한다.