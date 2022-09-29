## 💡 GC(Garbage Collection)란?
- 사용중이거나 사용중이지 않은 객체를 힙 메모리 상에서 식별하는 과정
- 시스템에 있는 모든 객체의 수명을 정확히 몰라도 런타임이 대신 객체를 추척하며 쓸모없는 객체를 알아서 제거하는 것
- 자동 회수된 메모리는 비우는 재활용됨
- C 언어의 경우, 메모리 할당/할당해제를 수동으로 작성하지만, 자바의 경우 GC에 의해 자동으로 처리됨

### 기본원칙
1. 알고리즘은 반드시 모든 가바지를 수집해야 한다.
2. 살아있는 객체는 절대로 수집해선 안 된다.
<br/>

## 💡 GC 과정
1. Marking
GC는 메모리 조각이 사용중인지 사용중이 아닌지를 식별한다.

|                              Marking                      |
| :---------------------------------------------------------------------------: |
|  ![image](https://user-images.githubusercontent.com/8343301/156578978-a4609952-5658-4612-862c-b0c51389fbe6.png) |

2. Normal Deletion
일반삭제는 참조하지 않는 객체를 삭제하여 참조된 객체와 포인터를 여유 공간으로 남김

|                              Normal Deletion                      |
| :---------------------------------------------------------------------------: |
|  ![image](https://user-images.githubusercontent.com/8343301/156579644-389c226c-1a4e-410e-9734-e90ab5dec8dc.png) |

2a. 
성능을 더욱 향상시키기 위해 참조되지 않은 객체를 삭제하고 참조된 나머지 객체를 압축함. 참조된 객체를 함께 이동하면 새 메모리 할당이 훨씬 쉽고 빨라짐

|                              Normal Deletion                      |
| :---------------------------------------------------------------------------: |
|  ![image](https://user-images.githubusercontent.com/8343301/156580155-137dd73f-9d60-48ad-bcd5-fd0693059287.png) |


## 💡 JVM Generations
힙 부분은 `Young Generation`, `Old or Tenured Generation`, `Permanent Generation` 입니다.


|                              Hostpot Heap Structure                      |
| :---------------------------------------------------------------------------: |
|  ![image](https://user-images.githubusercontent.com/8343301/156581501-7a58e3f1-d141-4fa1-a461-68868bf51202.png) |

1. Young Generation
    - 모든 새로운 객체가 할당되고 에이징되는 곳
    - Young Generation 가득 차면 minor garbage collection 수집 발생
    - 높은 개체 사망률을 가정하면 minor garbage collection을 최적화 가능 
    - 죽은 물건으로 가득 찬 젊은 세대는 매우 빠르게 수집
    - 살아남은 일부 개체는 노화되어 결국 구세대로 이동

2. Stop the World Event
    - 모든 `minor garbage collection`는 "Stop the World" 이벤트
    - 이는 작업이 완료될 때까지 모든 애플리케이션 스레드가 중지됨을 의미

3. Old Generation 
    - 오래 살아남은 개체를 저장하는 데 사용
    - 일반적으로 Young Generation 개체에 대해 임계값이 설정되고 해당 에이지가 충족되면 개체가 이전 세대로 이동
    - 결국 구세대를 수집해야 함
    - 이 이벤트를 `major garbage collection` 이라고 함
    - major garbage collection도 Stop World Event 입니다. 
    - 종종 major garbage collection은 모든 살아있는 개체를 포함하기 때문에 훨씬 느립니다. 
    - 따라서 반응형 애플리케이션의 경우 major garbage collection을 최소화해야 합니다. 
    - 또한 major garbage collection에 대한 Stop World Event의 길이는 이전 세대 공간에 사용되는 가비지 수집기의 종류에 따라 영향을 받습니다.

4. Permanent generation
    - Permanent generation 에는 응용 프로그램에서 사용되는 클래스와 메서드를 설명하기 위해 JVM이 필요로 하는 메타데이터가 포함됩니다. 
    - Permanent generation은 애플리케이션에서 사용 중인 클래스를 기반으로 런타임 시 JVM에 의해 채워집니다. 
    - 또한 Java SE 라이브러리 클래스 및 메서드를 여기에 저장할 수 있습니다.

<br/>

## 가비지 컬렉션 과정

- `stop-the-world` : GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것으로, GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 작업을 모두 멈춤
- GC 작업을 완료한 이후, 중단했던 작업을 다시 시작함
- 보통 GC 튜닝이란 `stop-the-world` 를 줄이는 것임
- **Java는 프로그램 코드에서 메모리를 명시적으로 지정하여 해제하지 않음 → GC가 더이상 필요없는 (쓰레기) 객체를 찾아 지워야 함**
- 가비지 컬렉터의 전제 조건 `weak generational hypothesis`
    - 대부분의 객체는 금방 접근 불가능한 상태(unreachable)가 된다.
    - 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.
- 이 가설의 장점을 최대한 살리기 위해 HotSpot VM 에서는 크게 `Old`, `Young` 2개로 물리적 공간을 나눔
    - Young 영역(Young Generation 영역) : 새롭게 생성한 객체의 대부분이 위치함. 대부분의 객체가 금방 접근 불가능한 상태가 되기 때문에 매우 많은 객체가 Young 영역에서 생성되었다가 사라짐. 여기서 객체가 사라질때 Minor GC가 발생한다고 함
    - Old 영역(Old Generation 영역) : 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사됨. 대부분 Young 영역보다 크게 할당되기 때문에 Young 영역보다 GC가 적게 발생함. 여기서 객체가 사라질때 Major GC(혹은 Full GC)가 발생한다고 함
    
    ![image](https://user-images.githubusercontent.com/8343301/193070026-1c26d028-24c5-4c07-aad7-51bd81d4f61c.png)
    
    GC 영역 및 데이터 흐름도
    
    - Permanent Generation 영역 : Method Area 라고 함. 객체나 억류(intern)된 문자열 정보를 저장하는 곳으로 Old 영역에서 살아남은 객체가 영원히 남는 것은 아님. 이 영역에서 GC가 발생할 수 있으며  발생한다면 Major GC 횟수에 포함됨
- Old 영역에 있는 객체가 Young 객체를 참조하는 경우에는?
    
    Old 영역에 있는 512바이트의 덩어리(chunk)로 되어있는 카드 테이블에 Old 객체가 Young 객체를 참조할 때마다 정보를 표시함
    
    Young 영역의 GC를 실행할 때에는 Old 영역에 있는 모든 객체의 참조를 확인하지 않고 카드 테이블을 뒤져서 GC 대상인지 식별함
    
    카드 테이블은 write barrier 을 사용하여 관리하며, 이는 Minor GC를 빠르게 할 수 있도록 하는 장치임. write barrier로 약간의 오베헤드가 발생할 수 있지만 전반적인 GC 시간은 줄어듬

![카드 테이블 구조](https://user-images.githubusercontent.com/8343301/193070257-a75918c6-c0dd-4921-961e-d76e1fd477f0.png)

카드 테이블 구조

<br/>

## Young 영역의 구성

- Young 영역은 3개의 영역으로 나뉜다
    
    1) **Eden 영역**
    
    2) **Survivor 영역(2개)**
    
- **각 영역의 처리 절차**
    1. 새로 생성한 대부분의 객체는 `Eden 영역` 에 위치함
    2. `Eden 영역` 에서 GC가 한번 발생한 후 살아남은 객체는 `Survivor 영역` 중 하나로 이동
    3. `Eden 영역` 에서 GC가 발생하면 이미 살아남은 객체가 존재하는  `Survivor 영역` 으로 객체가 계속 쌓임
    4. 하나의 `Survivor 영역` 이 가득차게 되면 그 중에서 살아남은 객체를 다른 `Survivor 영역` 으로 이동시킴. 가득찬 `Survivor 영역` 은 아무 데이터가 없는 상태가 됨
    5. 이 과정을 반복하며 계속해서 살아남은 객체는 `Old 영역` 으로 이동
- `Survivor 영역` 중 하나는 반드시 비어있는 상태여야 함

![GC 전과 후 비교](https://user-images.githubusercontent.com/8343301/193070428-b13fb533-910b-46de-a704-ecc23a03d4ff.png)

GC 전과 후 비교

- Young 영역은 `mark-copy` 알고리즘을 사용함

<br/>

## Old 영역에 대한 GC

- Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행함
- **GC 방식**
    - Serial GC
    - Parallel GC
    - Parallel Old GC(Parallel Compacting GC)
    - Concurrent Mark & Sweep GC(이하 CMS)
    - G1(Garbage First) GC
    - ZGC

### **Serial GC (-XX:+UseSerialGC)**

- 운영서버에서 절대로 사용하면 안되는 방식으로 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해 만든 방식임. Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어짐
- 적은 메모리(대략 100MB까지)와 CPU 코어 개수가 적을때 적합함
- Old 영역은 `mark-sweep-compact` 알고리즘을 사용함
    - mark : Old 영역에 살아 있는 객체를 식별(Mark)
    - sweep : 힙(heap)의 앞 부분부터 확인하여 살아 있는 것만 남김(Sweep)
    - compact : 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눔(Compaction)

### **Parallel GC (-XX:+UseParallelGC)**

- Parallel GC는 Serial GC와 기본적인 알고리즘은 같음
- 단, Parallel GC는 처리하는 쓰레드가 여러개이기 때문에 Serial GC보다 빠르게 객체처리 가능
- 메모리가 충분하고 코어의 개수가 많을 때 유리함
- Throughput GC 라고도 불림

![Serial GC와 Parallel GC의 차이](https://user-images.githubusercontent.com/8343301/193070576-7e1b30da-ee87-409c-a2c9-0b5952cee89f.png)

Serial GC와 Parallel GC의 차이

### **Parallel Old GC(-XX:+UseParallelOldGC)**

- JDK 5 update 6부터 제공한 GC 방식
- Old 영역의 GC 알고리즘만 다르며, `Mark-Summary-Compaction` 단계를 거침
    - Summary : 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별

### **CMS GC (-XX:+UseConcMarkSweepGC)**

![Serial GC와 CMS GC](https://user-images.githubusercontent.com/8343301/193070635-dad1d705-a11c-45bb-a40a-fd28224f8791.png)

Serial GC와 CMS GC

- 절차
    - Initial Mark : 클래스 로더에서 가장 가까운 객체 중 살아있는 객체만 찾는 것으로 끝남 → 멈추는 시간이 매우 짧음
    - Concurrent Mark : 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인함. 다른 스레드가 실행 중인 상태에서 동시에 진행된다는 것
    - Remark : Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인함
    - Concurrent Sweep : 쓰레기를 정리하는 작업을 실행. 다른 스레드가 실행되고 있는 상황에서 진행함
- `stop-the-world` 시간이 매우 짧음
- 모든 애플리케이션의 응답 속도가 매우 중요할때 사용하며, Low Latency GC 라고도 부름
- 단점
    - 다른 GC 방식보다 메모리와 CPU를 더 많이 사용
    - Compaction 단계가 기본적으로 제공되지 않음
- 조각난 메모리가 많아 Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 더 길기 때문에 Compaction 작업이 얼마나 자주, 오랫동안 수행되는지 확인 필요

### G1(Garbage First) GC(XX:+UseG1G)

- 큰 메모리를 가진 멀티프로세서 머신을 위해 만들어짐
- 바둑판의 각 영역에 객체를 할당하고 GC를 실행함
- 해당 영역이 꽉차면 다른 영역에서 객체를 할당하고 GC를 실행함
- 지금까지 설명한 Young의 세 가지 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식과 같음
- CMS GC를 대체하기 위해 만들어짐

![GC의 레이아웃](https://user-images.githubusercontent.com/8343301/193070743-b09ad9ac-c710-40e4-8195-bdb74f8b9f5e.png)

GC의 레이아웃

- 장점 : 성능, 빠름
- JDK 6에서는 early access 라고 부르며 시험삼아 사용하였으며 JDK 7에서 정식으로 G1 GC를 포함하였음

### ZGC(XX:+UseZGC)

- 대기시간이 낮은 확장 가능한 Low latency GC임
- 자바 스레드가 실행되는 동안 모든 작업을 완료함
- GC가 애플리케이션의 응답시간에 영향을 주는 것으로 제한함

> Java 8에서 default로 사용되는 gc는 ParallelGC이고 Java 9에서부터 default는 G1GC 임
> ZGC는 JDK 11부터 실험적으로 도입되었으며 JDK 15에서 Production Ready 로 선언됨
