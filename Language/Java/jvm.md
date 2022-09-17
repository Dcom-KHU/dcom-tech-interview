# JVM의 메모리 구조

## **JVM (Java Virtual Machine; 자바 가상 머신)**

자바가 아닌 C나 C++등으로 작성한 코드를 컴파일하면, 각기 다른 운영체제에서 실행할 수 있는 어셈블리어로 번역된 결과물을 얻을 수 있다. 즉, 컴파일 과정이 운영체제에 종속적이다, 그러나 자바에서는 컴파일 과정이나 컴파일 결과물이 아닌, 이러한 결과물을 실행하는 JVM(Java Virtual Machine)이 운영체제에 종속적이다.

자바 언어로 프로그램 코드를 작성한 뒤 (`java` 파일), JAVAC 컴파일러를 이용해 컴파일하면 바이트 코드 파일(class 파일)을 생성할 수 있다. 맥이든 윈도우든 `class` 파일만 있다면 각기 다른 운영체제의 JVM이 이를 기계어로 번역해 실행시켜준다. 따라서 프로그래머가 프로그램을 실행하는 운영체제를 신경 쓰지 않고 프로그래밍할수 있다는 장점이 있다.

---

## **JVM의 메모리 구조 (Runtime Data Area)**

JVM이 시작되면 운영체제로부터 메모리 영역을 할당받는데, 아래와 같이 5개의 영역으로 나눌 수 있다.

![Java Runtime Data Area](https://velog.velcdn.com/images/bambookim/post/53106efb-7605-460c-b7a8-225a45804656/image.png)

### 스레드가 공유하는 영역

- **메소드 영역 (Method Area)**<br>
    
    클래스별로 런타임 상수풀(runtime constant pool), 필드 데이터, 메소드 데이터, 메소드 코드, 생성자 코드 등을 저장한다. `static` 변수에 관한 정보도 메소드 영역에 저장된다.
    한마디로, 클래스의 바이트 코드들이 저장된다. 
    
  
- **힙 영역 (Heap Area)**<br>
    
    힙 영역은 객체와 배열이 생성되는 영역이다. (참고로 C/C++과는 다르게, 자바에서는 배열 또한 객체로 존재한다.) `new` 연산자를 통해 객체 또는 배열을 생성하면 이 영역에 저장된다. JVM 스택 영역의 변수나 다른 객체의 필드에서 객체를 참조한다면, 바로 이 Heap 영역에 존재하는 객체를 참조하는 것이다.<br>
    
    C에서는 `malloc`과 `free`, C++에서는 `new`와 `delete`를 이용해 객체를 동적 할당하고 필요 없는 객체는 동적 할당을 해제한다. 그러나 Java에서는 프로그래머가 직접 메모리 영역에 손을 댈 수 없기 때문에 할당을 해제할 수 없다. 대신 JVM의 Garbage Collector가 자동으로 필요 없는 객체를 메모리에서 대신 제거해 준다. 참조되지 않는 객체를 쓰레기로 간주하고 힙 영역에서 제거한다.
    

### 스레드마다 존재하는 영역

- **JVM 스택 영역 (Stack Area)**<br>
    
    각 스레드마다 하나씩 존재한다. 각 스레드에서 메소드를 호출할 때마다 프레임(Frame)을 추가하고, 메소드 실행이 종료되면 해당 프레임을 제거한다. 프레임 내부에는 로컬 변수 스택이 있다. 로컬 변수 스택에는 기본 타입(primitive type) 변수와 참조 타입(reference type) 변수가 존재한다. 기본 타입 변수는 스택 영역에 직접 값을 가지고 있다(`char`, `int`형 등등). 반면에 참조 타입 변수는 힙 영역에 존재하는 객체의 주소값을 이용해 객체를 참조한다(`String`, `Integer` 등등).
    

- **PC (Program Counter) Register**<br>자바 스레드마다 존재하는 프로그램 카운터 값을 저장한다. 현재 실행 중인 JVM 명령어의 주소를 저장한다.
    

- **Native Method Stack**<br>
    
    자바가 아닌, C/C++ 등 다른 언어로 작성된 코드를 실행하기 위한 영역이다.
    

---

## 참고자료

- 이것이 자바다, 신용권 저, 한빛미디어
- [JVM 구조와 자바 런타임 메모리 구조 (자바 애플리케이션이 실행될 때 JVM에서 일어나는 일, 과정을 설명해줄 수 있나요?)](https://jeong-pro.tistory.com/148)
- [JAVA :: 자바의 메모리 구조 - 1. 메소드 영역(Method Area)](https://blog.wanzargen.me/16)