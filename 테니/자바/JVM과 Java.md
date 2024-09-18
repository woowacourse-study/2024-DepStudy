## JVM의 구조와 Java의 실행방식을 설명해주세요.

**JVM 구조**
- Class Loader
- Execution Engine
- Garbage Collector
- Runtime Data Area
    - Heap
    - Method
    - Stack
    - PC register
    - Native Method Stack


**자바 실행방식**

1. 프로그램을 실행하면 JVM은 OS로 부터 프로그램이 필요로 하는 메모리를 할당받음
2. 자바 컴파일러가 자바 소스코드(.java)를 읽어 바이트 코드(.class)로 변환
3. Execution Engine이 필요한 바이트 코드 Class Loader에 요청
4. `Class Loader`를 통해 바이트 코드들을 JVM으로 로딩
5. 클래스 로더는 동적 로딩을 통해 필요한 클래스들을 로딩 및 링크하여 `Runtime Data Area`에 배치
6. Execution Engine이 메모리(`Runtime Data Area`)에 올라온 바이트 코드를 실행
7. Garbage Collector가 힙 메모리 영역 중 참조되지 않은 객체 제거
