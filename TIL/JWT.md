### 💡 JWT(JSON Web Tocken)란?
---
> JSON 웹 토큰은 선택적 서명 및 선택적 암호화를 사용하여 데이터를 만들기 위한 인터넷 표준

### 💡 특징
---
- SPA(Single Page Application)
- 비교적 최신 트렌드
- 사용자 경험이 중요한 복잡한 Front-End 구현에 적합
- Vue, React, Anular 등
- 웹 브라우저 외 모바일, IoT 등 다양한 플랫폼 미 디바이스의 클러아인를 하나의 Back-End 서비스 API로 대응가능

### 💡 장점
---
- 멀티 플랫폼 서비스 대응에 유리
- 토큰 정보를 클라이언트가 보유
- 서버의 Scale-Up 부담이 상대적으로 낮음
- 클라이언트 세션 관리에 대한 서버 메로리 부담 감소
- 다양한 도메인 대응 부담 감소

### 💡 단점
---
- 클라이언트 토큰 정보 탈취로 보안적인 부분의 상대적 취약 가능성
- 토큰에 Signature와 같은 부가정보가 있어, 세션 기반 인증에 비해 Server-Client가 주고받는 데이터의 양 증가
- 토큰 인증을 위한 Database 조회 발생으로, DB 성능 부하 가능성


### 💡 JWT 구조
---
- 크게 3가지 1)Header, 2)Payload, 3)Verify Sinature 로 나누어져 있습니다.
 
- Encoded
  - 인코딩 데이터로 스켈레톤 코드에서 로그인 시, alter 창 표기되는 acesstoken과 같습니다.
  - 인코딩 데이터가 각기 다른 3가지 색으로 작성되어 있는 걸 볼 수 있다. 각 구성요소에 대해 Decoded에서 자세히 살펴보겠습니다.
- Decoded 
  1. Header : 알고리즘 & 토큰 타입으로, 알고리즘을 무엇으로 적용할지(HS256, RS384등), 토큰 Type(JWT 등)이 작성
  2. Payload : 넘겨줄 Claim set(쉽게 생각하면, 넘겨줄 data를 포함하여 토큰 옵션 지정) 작성. 단, 공식문서를 참고하여 필요한 claim을 사용해야 함
  3. Verify Sinature : 토큰 확인용 서명으로, 생성된 토큰이 다시 우리에게 전송되기 전에 클라이언트에서 변경하지 않았는지 확인하는 용도
  ![image](https://user-images.githubusercontent.com/8343301/149268352-48a2ed3c-d93b-4be7-be06-31c045528537.png)


### 💡 Why JWT?
---
JWT는 생성된 토큰은 서버에 저장하는 것이 아니라 브라우저에 저장을 한다. 
따라서 서버에 세션값을 저장한느 세션 방식과는 달리, 다른 서버에서도 동일한 토큰을 이용하여 인증을 구현할 수 있다.


#### 🎬 추천영상
[What Is JWT and Why Should You Use JWT](https://youtu.be/7Q17ubqLfaM)
