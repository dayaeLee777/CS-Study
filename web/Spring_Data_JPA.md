## 💡 Spring Data Jpa란?

- Spring Data JPA는 JPA(Java Persistence API)에 대한 저장소(repository) 지원 제공
- `JPA data sourse`에 대한 접근이 필요한 어플리케이션 개발을 용이하게 함

## 💡 Spring Data Repositories로 작업하기

- Spring Data repository 추상화의 목표는 다양한 영속성 저장소에 대한 데이터 액세스 계층을 구현하는 필요한 상용구 코드(boilerplate code*)의 양을 크게 줄이는 것이다.  
  - boilerplate code : sections of code that are repeated in multiple places with little to no variation

</aside>

## 💡 핵심개념

- Spring Data repository 추상화의 중심 인터페이스는 `Repository`  임
- 관리할 `도메인 클래`스와 도메인 클래스의 `ID 타입`을 타입 인자로 사용함
- `CrudRepository` 인터페이스는 관리되는 Entity 클래스를 위해 정교화된 CRUD 기능을 제공함
    
    ```java
    public interface CrudRepository<T, ID> extends Repository<T, ID> {
    
    	// Saves the given entity
      <S extends T> S save(S entity);      
    
    	// Returns the entity identified by the given ID.
      Optional<T> findById(ID primaryKey); 
    
    	// Returns all entities
      Iterable<T> findAll();               
    
    	// Returns the number of entities
      long count();                        
    
    	// Deletes the given entity
      void delete(T entity);               
    
    	// Indicates whether an entity with the given ID exists
      boolean existsById(ID primaryKey);   
    
      // … more functionality omitted.
    }
    ```
    
- 영속성 추상화로 `JpaRepository`, `MongoRepository` 도 있으며, 이러한 인터페이스들은 `CrudRepository`  를 상속받아 사용함

## 💡 Query 메소드 정의

- Repository 프록시에는 메소드 이름에서 query를 파생시키는 2가지 방법이 있음
    1. Method 이름에서 직접 쿼리 파생
    2. 수동으로 정의된 쿼리 사용

- Method 이름에서 직접 쿼리 파생
    
    ```java
    interface PersonRepository extends Repository<Person, Long> {
    
      List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
    
      // Enables the distinct flag for the query
      List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
      List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);
    
      // Enabling ignoring case for an individual property
      List<Person> findByLastnameIgnoreCase(String lastname);
      // Enabling ignoring case for all suitable properties
      List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
    
      // Enabling static ORDER BY for a query
      List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
      List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
    }
    ```
    
    - `find…By`, `exists…By` 는 Query의 주제를 나타냄
    - 두 번째 부분은 술어를 형성
    - `find…By` 에서는 `Distinct` 키워드를 사용 가능
    - Entity 속성에 대한 조건을 정의하고 `And`,  `Or` 를 이용하여 연결 가능
    - `Between`, `LessThan`, `GreaterThan`,  `Like` 와 같은 연산자 지원
    - `IgnoreCase`  또는 `AllIgnoreCase` 를 이용하여 대소문자 구분 무시 가능
    - `OrderBy` 와 정렬방식((`Asc` 또는 `Desc`)을 이용한 정적 정렬 지원

## 💡 Special parameter 핸들링

- `Pageable` , `Sort` 과 같은 매개변수를 이용하여 동적쿼리 생성

```java
Page<User> findByLastname(String lastname, Pageable pageable);

Slice<User> findByLastname(String lastname, Pageable pageable);

List<User> findByLastname(String lastname, Sort sort);

List<User> findByLastname(String lastname, Pageable pageable);
```

## 💡 페이징 및 정렬

- 정렬 표현 정의
    
    ```java
    Sort sort = Sort.by("firstname").ascending()
      .and(Sort.by("lastname").descending());
    ```
    

- type-safe API 를 사용한 정렬 표현 정의
    
    ```java
    TypedSort<Person> person = Sort.sort(Person.class);
    
    Sort sort = person.by(Person::getFirstname).ascending()
      .and(person.by(Person::getLastname).descending());
    ```
    

- Querydsl AP를 이용한 정렬 표현 정의
    
    ```java
    QSort sort = QSort.by(QPerson.firstname.asc())
      .and(QSort.by(QPerson.lastname.desc()));
    ```
    

## 💡쿼리 결과 제한

- ****`Top` **,**  `First` 를 이용한 결과 제한
    - 반환할 최대 결과 크기 지정을 위해  `Top` **,**  `First`  뒤에 숫자 값을 붙일 수 있음
    - 숫자를 생략하면 결과 크기를 1로 간주함
    
    ```java
    User findFirstByOrderByLastnameAsc();
    
    User findTopByOrderByAgeDesc();
    
    Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);
    
    Slice<User> findTop3ByLastname(String lastname, Pageable pageable);
    
    List<User> findFirst10ByLastname(String lastname, Sort sort);
    
    List<User> findTop10ByLastname(String lastname, Pageable pageable);
    ```
    

## 💡 Repository Methods의 return

- 다양한 결과값을 반환하는 쿼리 메소드는 standard Java인 `Iterable`, `List`, `Set` 등을 사용
- Spring Data JPA는 Spring Data의 `Streamable` , `Iterable` 의 사용자 확장을 return으로 지원
    - `Streamable` 을 사용하여 쿼리 메소드 결과 결합
    
    ```java
    interface PersonRepository extends Repository<Person, Long> {
      Streamable<Person> findByFirstnameContaining(String firstname);
      Streamable<Person> findByLastnameContaining(String lastname);
    }
    
    Streamable<Person> result = repository.findByFirstnameContaining("av")
      .and(repository.findByLastnameContaining("ea"));
    ```
    

## 💡 **Querydsl 확장**

- Querydsl은 정적으로 작성된 SQL과 유사한 쿼리를 구성할 수 있는 프레임워크
- 여러 Spring Data 모듈은 `QuerydslPredicateExecutor` QuerydslPredicateExecutor interface
    
    ```java
    public interface QuerydslPredicateExecutor<T> {
    
      Optional<T> findById(Predicate predicate);  
    
      Iterable<T> findAll(Predicate predicate);   
    
      long count(Predicate predicate);            
    
      boolean exists(Predicate predicate);        
    
      // … more functionality omitted.
    }
    ```
    
    ```java
    Predicate predicate = user.firstname.equalsIgnoreCase("dave")
    	.and(user.lastname.startsWithIgnoreCase("mathews"));
    
    userRepository.findAll(predicate);
    ```
    

출처 : [https://docs.spring.io/spring-data/jpa/docs/2.4.5/reference/html/#reference](https://docs.spring.io/spring-data/jpa/docs/2.4.5/reference/html/#reference)
