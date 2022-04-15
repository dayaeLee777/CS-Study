### 💡 특징

---

- Document
- BASE
- Open Source

### 💡 구조

---

- **RDBMS VS MongoDB**
  ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/93b8df09-9364-40be-a2da-1c0de3c5321a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220415%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220415T065959Z&X-Amz-Expires=86400&X-Amz-Signature=916867aae647b11cd33f100e60521eac471e7a65756fa247caa97ab3cf25d030&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
- **Document Database**
  - document 형식으로 JSON 형태와 유사함
  - `field` 와 `value` 로 구성됨
  - value에는 other documents, arrays, and arrays of documents 가 포함될 수 있음
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/efa60890-e326-4d04-a040-93b5de378f51/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220415%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220415T070022Z&X-Amz-Expires=86400&X-Amz-Signature=3c1037cdb4b2cda4683e166a8277dfabdbbe93f30557af6f3e1bca6e3d641100&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### 💡 Document의 장점

---

- 많은 프로그래밍 언어의 기본 데이터 타입에 대응함
- 포함된 문서와 배열은 조인의 필요성을 줄임
- 동적 스키마는 유연한 다형성을 지원함

### 💡 문법

---

#### Switch Database

```bash
# 현재 사용중인 DB 확인
>>> db
test

# DB 사용 명령어(mysql과 동일)
>>> use test
switch to db test
```

#### Insert

- **db.collection.insertMany()**

```bash
db.movies.insertMany([
   {
      title: 'Titanic',
      year: 1997,
      genres: [ 'Drama', 'Romance' ],
      rated: 'PG-13',
      languages: [ 'English', 'French', 'German', 'Swedish', 'Italian', 'Russian' ],
      released: ISODate("1997-12-19T00:00:00.000Z"),
      awards: {
         wins: 127,
         nominations: 63,
         text: 'Won 11 Oscars. Another 116 wins & 63 nominations.'
      },
      cast: [ 'Leonardo DiCaprio', 'Kate Winslet', 'Billy Zane', 'Kathy Bates' ],
      directors: [ 'James Cameron' ]
   },
   {
      title: 'The Dark Knight',
      year: 2008,
      genres: [ 'Action', 'Crime', 'Drama' ],
      rated: 'PG-13',
      languages: [ 'English', 'Mandarin' ],
      released: ISODate("2008-07-18T00:00:00.000Z"),
      awards: {
         wins: 144,
         nominations: 106,
         text: 'Won 2 Oscars. Another 142 wins & 106 nominations.'
      },
      cast: [ 'Christian Bale', 'Heath Ledger', 'Aaron Eckhart', 'Michael Caine' ],
      directors: [ 'Christopher Nolan' ]
   }
])
```

#### FindAll

- Document 조회(select All Documents)
- **db.collection.find()**

```bash
db.movies.find( { } )
```

#### Filter Data

- `<field>: <value>` 에 해당하는 값 조회
- **db.collection.find()**

```bash
# directors 가 Christopher Nolan 인 document 조회
db.movies.find( { "directors": "Christopher Nolan" } );

# ~ 이하 조회
db.movies.find( { "released": { $lt: ISODate("2000-01-01") } } );

# ~ 이상 조회
db.movies.find( { "awards.wins": { $gt: 100 } } );

# in 조회
db.movies.find( { "languages": { $in: [ "Japanese", "Mandarin" ] } } )
```

#### **Specify Fields to Return (Projection)**

- projection document로 특정 필드 return
- 전체 레코드를 처리하는 데 많은 시간이 걸리므로 선택한 데이터 만 처리시 사용
- **db.collection.find(<query document>, <projection document>)**

```bash
db.movies.find( { }, { "title": 1, "directors": 1, "year": 1 } );

db.movies.find( { }, { "_id": 0, "title": 1, "genres": 1 } );
```

#### **Aggregate Data($group)**

- 집계함수
- `$unwind` : 입력 문서에서 배열 필드를 분해하여 각 요소 에 대한 문서를 출력예제코드 :
  - 예제코드 : `genres`배열 의 각 요소에 대한 문서를 출력
- `genres` 로 그룹화되며 count 된 값은 `genreCount` 에 저장됨
- `genreCount` 필드별 내림차순으로 정렬

```bash
# 각 genre 값의 발생 횟수 집계
db.movies.aggregate( [
   { $unwind: "$genres" },
   {
     $group: {
       _id: "$genres",
       genreCount: { $count: { } }
     }
   },
   { $sort: { "genreCount": -1 } }
] )
```

### 💡 MongoDB 실습

[Getting Started](https://www.mongodb.com/docs/manual/tutorial/getting-started/)

### 💡 \***\*Spring Data MongoDB - Reference Documentation\*\***

[Spring Data MongoDB - Reference Documentation](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/)

### 💡 참고자료

[Introduction to MongoDB](https://www.mongodb.com/docs/manual/introduction/)

[MongoDB 이해하기](https://kciter.so/posts/about-mongodb)
