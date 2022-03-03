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
