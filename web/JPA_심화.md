## 영속성 컨텍스트
- JPA를 이해하는데 가장 중요한 용어
- "엔티티를 영구 저장하는 환경"이라는 뜻
- EntityManager.persist(entity);

## 엔티티 매니저 VS 영속성 컨텍스트
- 영속성 컨텍스트는 논리적인 개념
- 눈에 보이지 않음
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근

## 영속성 컨텍스트의 이점
- 1차 캐시에서 조회(쓰레드 하나 쓰는 동안 사용)
- 동일성(identity) 보장
- 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
- 변경 감지(Dirty Checking)
- 지연 로딩(Lazy Loading)

## 스프링 데이터 JPA
- 지루하게 반복되는 CRUD 문제를 세련된 방법으로 해결
- 개발자는 인터페이스만 작성
- 스프링 데이터 JPA가 구현 객체를 동적으로 생성해서 주입

## Optional 개념
- NullPointException을 방지하는 클래스로 Java8부터 지원
  - Optional<T> : null이 올 수 있는 값을 감싸는 Wrapper 클래스
  
- get() : Optional 객체에 저장된 값에 접근
  - Optional 객체에 저장된 값이 null 이면 NoSuchElementException 예외 발생
    - 우리가 Test 시 syso 이 출력되지 않았던 이유!!!!
  -  get() 메소드를 호출하기 전에 isPresent() 메소드를 사용하여 Optional 객체에 저장된 값이 null인지 아닌지를 먼저 확인한 후 호출하는 것을 권장!
      ```
      if(opt.isPresent()) {
        System.out.println(opt.get());
      }
      ```
  - orElse() vs orElseGet()
    - orElse() : Optional의 값이 null이든 아니든 항상 호출
    - orElseGet() : Optional의 값이 null일 경우에만 호출
