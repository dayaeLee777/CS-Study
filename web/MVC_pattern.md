## 💡MVC(Model-View-Controller) Pattern 이란?
JSP를 이용하여 구성할 수 있는 Web Application Architecture는 Model1과 Model2로 나뉜다. 
1. Model1 : JSP가 client 요청에 대한 로직 처리와 view(response page)에 대한 처리를 모두 수행
2. Model2 : JSP가 view(response page)에 대한 처리만 수행
 
여기서 **Model2 구조는 MVC(Model-View-Controller) Pattern을 웹 개발에 도입한 구조이다.** <br/><br/>


## 💡Model 1란?

### **구조**

model1은 view와 logic을 JSP 페이지 하나에서 처리하는 구조로, client로부터 요청이 들어오면 JSP 페이지는 java beans나 별도의 service class를 이용하여 작업을 처리, 결과를 client에 출력한다.

<img width="1139" alt="스크린샷 2022-01-06 오전 1 15 46" src="https://user-images.githubusercontent.com/8343301/148360110-ac48454a-06e8-4ecd-85d0-7bce3e08802d.png">


### **장단점**

1.  **장점**
    -   구조가 단순하여 직관적이다.
    -   개발시간이 비교적 짧아 개발비용이 감소한다.
2.  **단점**
    -   view 코드와 로직처리를 위한 java 코드가 섞여있어 JSP 코드 자체가 복잡하다.
    -   JSP 코드에 Back-End와 Front-End가 혼재되어있어 분업이 힘들다.
    -   프로젝트 규모가 커지면 코드가 복잡해져 유지보수가 어렵다.
    -   확장성이 나쁘다. <br/> <br/>


## 💡Model 2란?

### **구조**

Model2 구조는 MVC 패턴을 웹개발에 도입한 구조이다.

client 요청에 대한 처리는 Servlet이, logic 처리는 java class(Service, Dao...), client에게 출력하는 response page는 JSP가 담당한다.

| **Model2** | **MVC 패턴** | **설명** |
| --- | --- | --- |
| Service, Dao,   Java Beans | **Model** | **Logic 처리**   Controller로 부터 넘어온 data를 이용하여 수행하고 그에 대한 결과를 다시 Controller로 return |
| JSP | **View** | **모든 화면 처리 담당   **Client 요청에 대한 결과뿐 아니라 controller에 요청을 보내는 화면단도 jsp에서 처리 |
| Servlet | **Controller** | **Client 요청을 분석하여 Logic 처리를 위한 Model 호출**   필요에 따라 request, session 등에 결과 data를 저장하고, redirect 또는 forward 방식으로 jsp(view) page를 이용하여 출력 |

<img width="1120" alt="스크린샷 2022-01-06 오전 1 55 06" src="https://user-images.githubusercontent.com/8343301/148360301-94c9220f-f0e8-4b9b-85dc-d444926e6e53.png">

### **장단점**

1.  **장점**
    -   view 코드와 로직처리 코드가 분리되어있어 JSP는 Model1에 비해 복잡하지 않다.
    -   Back-End와 Front-End가 분리되어 분업이 용이하다.
    -   기능에 따라 코드가 분리되었기 때문에 유지보수가 쉬워졌다.
    -   확장성이 뛰어나다.
2.  **단점**
    -   구조가 복잡하여 초기진입이 어렵다.
    -   개발시간의 증가로 개발 비용이 증가한다.
