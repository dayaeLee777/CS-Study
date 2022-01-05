### 💡 REST란?
- REST는 웹의 장점을 최대한 활용할 수 있는 아키텍처로, Representational State Transfer의 약어입니다.
 
> Representational State Transfer

- 즉, 하나의 URI는 하나의 고유한 리소스(resource)를 대표하도록 설계된다는 개념에서 파생되었으며 <br/>
전송방식을 결합해서 원하는 작업을 지정하는 것을 말합니다.

<img width="696" alt="스크린샷 2022-01-05 오후 4 41 07" src="https://user-images.githubusercontent.com/8343301/148189476-02014184-7f89-4b94-beb3-a1aca9805d6d.png"><br/><br/>
 
 
### 💡 REST의 필요성 
1. 기존 Service
   - 요청에 대한 처리 후, 가동된 데이터를 특정 플랫폼에 적합한 형탱의 View로 만들어서 반환
2. REST Service
   - 데이터만 처리하거나 처리 후 반환될 데이터가 있으면 JSON이나 XML 형식으로 전달하기 때문에 View에 대해서 신경쓸 필요가 없다. 즉, 멀티플렛폼에 대한 지원이 가능하다.
   - 이러한 이유로, OPEN API에서 많이 쓰임.<br/><br/>


### 💡 REST 구성
1. 자원(Resource) - URI
    - 예시 : /devdange/user
 
2. 행위(Verb) - HTTP Method

| **Method** | **의미** | **SQL** |
| --- | --- | --- |
| POST | Create | Insert |
| GET | Read | Select |
| PUT | Update | Update |
| DELETE | Delete | Delete |
 
3. 표현(Representations) 
    - Client와 Server가 데이터를 주고 받는 형태로, JSON, XML과 같은 언어로 표현합니다.
 
 
#### 👉 잘 표현된 HTTP URI로 자원(Resouce)를 정의하고, HTTP Method로 행위(Verb)를 정의합니다.<br/><br/>
 

### 💡 REST 특징
1.  Uniform (유니폼 인터페이스)
2.  Stateless (무상태성)
3.  Cacheable (캐시 가능)
4.  Self-descriptiveness (자체 표현 구조)
5.  Client - Server 구조
6.  계층형 구조
