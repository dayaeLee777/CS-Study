### 💡 JPA 탄생 배경?!
---
- 객체와 관계형 데이터베이스의 차이
  1. 상속
  2. 연관관계
  3. 데이터 타입
  4. 데이터 식별 방법
<br/>

### 💡 JPA(Java Persistence API)란?
---
- 자바 진영의 ORM 기술 표준
- 인터페이스의 모음
- 발전과정
  - EJB- 엔티티 빈(자바표준) ➔ 하이버네이트(오픈소스) ➔ JPA(자바 표준) 

- JPA의 필요성
  - SQL 중심적인 개발에서 객체 중심으로 개발 가능
  - 생산성 증가
  - 유지보수 용이
  - 패러다임의 불일치 해결
  - 성능 향상
  - 데이터 접근 추상화와 벤더 독립성
  - 표준
<br/>

### 💡 ORM이란?
---
- Objected-Relational Mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재
<br/>

### 💡 기본 매핑
---
- 스키마 자동 생성 옵션 지정 
  - create : 기존 테이블 삭제 후 다시 생성(DROP+CREATE)
  - create-drop : create와 같으나 종료시점에 테이블 DROP
  - update : 변경분만 반영
  - validate : 엔티티와 테이블이 정상 매핑되었는지만 확인
  - none : 사용하지 않음
  
 > 👉 스켈레톤 프로젝트에서는 `application.properties` 25번째줄 `spring.jpa.hibernate.ddl-auto=update` 에 작성되어있음
   


- 객체 매핑하기
  - @Entity : JPA가 관리할 객체
  - @Id : DB PK와 매핑할 필드 
  ```
  @Entity	
  public class User{
    @Id
    String userId;
    String name;
  ```


- 매핑 어노테이션(DB에 매핑되는 정보)
  - @Column
   - 컬럼 관련 어노테이션으로 가장 많이 사용됨
     - name : 필드와 매핑할 테이블의 컬럼 이름
     - insertable, updatable : 읽기 전용
     - nullable : 허용여부 결정, DDL 생성시 사용
     - unique : 유니크 제약 조건, DDL 생성시 사용
      ```
      @Column(nullable = false) // not null
      String name;
      @Column(name="user_id") // table에 "user_id"로 컬럼생성
      String userId;
      ```
      
  - @Temporal
    - 날짜 타입 매핑
    ```
    @Temporal(TemporalType.DATE)
    Date date;  //날짜
    @Temporal(TemporalType.TIME)
    Date time;  //시간
    @Temporal(TemporalType.TIMESTAMP)
    Date timeStamp; //날짜와 시간
    ```
  - @Enumerated : 열거형 매핑
  - @Lob : 컨텐츠 길이가 길때 byte로 저장
  - @Transient
    - 이 필드는 매핑하지 않는다.
    - 애플리케이션에서 DB에 저장하지 않는 필드


- 식별자 매핑 어노테이션
  - @Id : 직접매핑
  - @GeneratedValue
    - IDENTITY : 데이터베이스에 위임. MySQL
    - SEQUENCE : 데이터베이스 시퀀스 오브젝트 사용. Oracle
      - @SequenceGenerator 필요
    - TABLE :  키생성용 테이블 사용
      - @TableGenerator 필요
    - AUTO : 방언에 따라 자동지정, 기본값
  ```
  //import javax.persistence.Id;
    @Id // Table DB PK와 매핑할 필드
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id = null;
  ```
 
 
- 주의
  - 엔티티 매니저 팩토리는 하나만 생성해서 애플리케이션 전체에서 공유
  - 엔티티 매니저는 쓰레드간에 공유하면 안된다.
  - JPA의 모든 데이터 변경은 트랜잭선 안에서 실행 

<br/>

### 💡 연관관계 매핑
---
- 단방향 매핑
```
//스켈레톤 예시 UserConference -> User
@Entity
@Getter
@Setter
public class UserConference extends BaseEntity{
	int conferenceId;
  //int userId; // JoinColumn 컬럼에서 작성되기 때문에 삭제
	// 연관관계작성
	@ManyToOne(fetch = FetchType.LAZY)	// 조인된 객체가 아닌 단일 객체만 가져오기
  //@ManyToOne(fetch = FetchType.EAGER) // default -> 조인된 값을 가져옴
	@JoinColumn(name="user_id")
	User user;
```

- 양방향 매핑
  - 반대방향으로 객체 그래프   
```
//스켈레톤 예시 UserConference <-> User
// 단방향 코드도 작성되어야함

@Entity	
@Getter
@Setter
public class User extends BaseEntity{
	String department;
    String position;
    String name;
    String userId;
    
    @OneToMany(mappedBy = "user")
    List<UserConference> userConferences = new ArrayList<UserConference>();
    
    @JsonIgnore
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
    String password;
}
```

- 객체의 양방향 관계
  - 객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단방향 관계 2개이다.
  - 객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야한다.
  > A -> B(a.getB()) / B -> A(b.getA()) 

- 연관관계의 주인
  - 양방향 매핑 규칙
    - 객체의 두 관계 중 하나를 연관관계의 주인으로 지정
    - 연관관계의 주인만이 외래키를 관리(등록, 수정)
    - 주인이 아닌쪽은 읽기만 가능
    - 주인은 mapperBy 속상 사용X
    - 주인이 아니면 mapperBy 속성으로 주인 지정 
  - 주인 정하기
    - 외래키가 있는 곳을 주인으로 정함
  - 양방향 매핑시 연관관계의 주인에 값을 입력해야한다.
    - 순수한 객체관계를 고려하면 항상 양쪽다 값을 입력해야함 

- 양방향 매핑의 장점
  - 단방향 매핑만으로도 이미 연관관계 매핑은 완료
  - 양방향 매핑은 반대방향으로 조회(객체 그래프 탐색) 기능이 추가된 것 뿐
  - 단방향 매핑을 잘하고 양방향은 필요시 추가하면 됨(테이블에 영향을 주지않음)

- 연관관계 매핑 어노테이션
  - 다른 어노테이션은 제약조건이 많기 때문에 현업에선 아래 두 가지를 많이 사용하며  
    - 다대일(@ManyToOne)
    - 일대다(@OneToMany)
