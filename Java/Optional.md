## 💡 Optional 개념

- NullPointException을 방지하는 클래스로 Java8부터 지원
    - Optional<T> : null이 올 수 있는 값을 감싸는 Wrapper 클래스
- Spring Data JPA 사용 시 Repository에서 리턴 타입을 Optional로 바로 받을 수 있도록 지원

## 💡 메소드

- get() : Optional 객체에 저장된 값에 접근
    - Optional 객체에 저장된 값이 null 이면 NoSuchElementException 예외 발생
    - get() 메소드를 호출하기 전에 isPresent() 메소드를 사용하여 Optional 객체에 저장된 값이 null인지 아닌지를 먼저 확인한 후 호출하는 것을 권장
        
        **→ *Optional* 의 목표에 어긋나며 향후 릴리스에서 더 이상 사용되지 않을 것😱**
        
        ```java
        if(opt.isPresent()) {
        	return opt.get();
        } else {
        	return "default";
        }
        ```
        

- **orElse() vs orElseGet()**
    - orElse(...) : Optional의 값이 null이든 아니든 항상 호출
        
        → 새 객체 생성이나 새로운 연산을 유발하지 않고 이미 생성되었거나 이미 계산된 값일 때만 사용해야 함
        
        ```java
        return opt.orElse("default");
        ```
        
    - orElseGet(...) : Optional의 값이 null일 경우에만 호출
        
        → `Optional`에 값이 없을 때만 새 객체를 생성하거나 새 연산을 수행하므로 불필요한 오버헤드가 없음
        
        ```java
        String defaultStr = "Default String";
        
        return opt.orElseGet(this::defaultStr);
        ```
        
    

출처 : [https://www.baeldung.com/java-optional](https://www.baeldung.com/java-optional)
