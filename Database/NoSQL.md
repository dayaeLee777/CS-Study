### 💡 NoSQL이란?

---

- 단순히 기존 관계형 DBMS가 갖고 있는 특성뿐만 아니라, 다른 특성들을 부가적으로 지원함
- NoSQL은 No SQL, Not Only SQL, Non-Relational Operational Database SQL 등 다양한 의견이 있지만 대다수가 `Not Only SQL`로 풀어 설명함
- NoSQL 데이터베이스 또는 비관계형 데이터베이스는 관계 데이터를 저장하지 않는다고 오해하지만, `NoSQL 데이터베이스는 관계형 데이터베이스와 방식은 다르지만 관계 데이터를 저장할 수 있음`
- NoSQL 데이터베이스에 있는 관계 데이터를 모델링하는 것이 SQL 데이터베이스보다 쉽다는 것을 알 수 있는데, 이는 관련 데이터를 테이블 간에 분할할 필요가 없음

### 💡 NoSQL 배경

- 1998년 카를로 스트로찌(Carlo Strozzi)라는 엔지니어가 공개한 표준 SQL 인터페이스를 채용하지 않은 자신의 경량 Open Source 관계형 데이터베이스를 NoSQL이라고 명명한데서 유래
- 이후 2009년에는 요한 오스칼손(Johan Oskarsson)이라는 엔지니어가 Open Source기반의 분산 데이터베이스 관련 행사를 준비하면서 NoSQL이라는 용어를 사용
- RDBMS가 꾸준히 강세를 보였으나, 2000년 후반 인터넷이 활성화되고, SNS 등이 등장하면서 비정형데이터라는 것을 보다 쉽게 담아서 저장하고 처리할 수 있는 NoSQL이 각광을 받게 됨
- 또한, 2000년대 말에 스토리지 비용이 크게 하락하면서 주목받음

### 💡 관계형 데이터베이스와의 차이점

- 관계형 모델을 사용하지 않으며 테이블간의 조인 기능 없음
- 직접 프로그래밍을 하는 등의 비SQL 인터페이스를 통한 데이터 액세스
- 대부분 여러 대의 데이터베이스 서버를 묶어서(클러스터링) 하나의 데이터베이스를 구성
- 관계형 데이터베이스에서는 지원하는 Data처리 완결성(Transaction ACID 지원) 미보장
- 데이터의 스키마와 속성들을 다양하게 수용 및 동적 정의 (Schema-less)
- 데이터베이스의 중단 없는 서비스와 자동 복구 기능지원
- 다수가 Open Source로 제공
- 확장성, 가용성, 높은 성능

### 💡 NoSQL의 종류

- **Document DB**
  - JSON(JavaScript Object Notation) 객체와 비슷한 문서에 데이터를 저장
  - 각 문서에는 필드와 값의 쌍이 포함됨
  - MongoDB
- **Key-Value DB**
  - Key와 Value의 쌍으로 데이터가 저장되는 가장 단순한 형태
  - 일반적인 사용자 선호도 저장 또는 캐싱에서 사용
  - Redis, DynanoDB
- **Wide Columnar Store**
  - 테이블, 행 및 동적 열에 데이터를 저장
  - 대량의 데이터를 저장해야 할 때 적합하며, 쿼리 패턴을 예측가능
  - Cassandra, HBase
- **Graph DB**
  - 노드와 에지에 데이터를 저장
  - 노드에는 일반적으로 사람, 장소 및 사물에 대한 정보가 저장되고, 에지에는 노드 간의 관계에 대한 정보가 저장
  - Neo4j, JanusGraph

### 💡 참고자료

---

[NoSQL이란 무엇입니까? NoSQL Databases 설명](https://www.mongodb.com/ko-kr/nosql-explained)

[NoSQL이란 무엇인가? 대량데이터 동시처리위한 DBMS 종류와 특징](https://www.samsungsds.com/kr/insights/1232564_4627.html)
