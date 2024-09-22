## 스프링 Bean의 생성 과정을 설명해주세요.

### 등록 방법
1. `@Component`을 활용한 자동등록 방식
2. `@Configuration` + `@Bean`을 활용한 수동등록 방식
   - `@Configuration`이 아닌 `@Component` + `@Bean` 방식은 객체를 새로 생성

Bean은 자동/수동방식 공통적으로 `@ComponentScan`이 특정 classpath 이하에 있는 컴포넌트들을 등록하기 위해 스캔하여 빈으로 등록함

### 빈의 생명주기
  - Bean은 Spring Container에 의해 관리된다.
  - 그래서 해당 application으로 요청이 올 때마다 객체를 생성하는 것이 아닌, Spring Container 생성 시점에 생성된 Bean객체를 활용하기 때문에 성능상 이점이 있다.

1. 스프링 컨테이너가 초기화될 때 **빈 객체를 생성**하며 
2. **의존 관계를 설정**한 뒤 
3. 빈 **초기화**를 진행
4. **객체를 사용**하고
5. 컨테이너가 종료될 때 **빈 소멸**

- 스프링 컨테이너 생성 시점에 빈 객체를 생성하는 이유

IoC 컨테이너는 bean이 실제로 생성된 후에야 실질적인 의존관계를 형성하며, 이 과정 중에 bean 설정에 대한 예외가 발생했는지 파악한다.
**만약 bean이 필요시에만 만들어진다면, 해당 bean 생성 요청을 하기 전 까지는 bean 설정에 대한 예외가 발생했는지 파악할 수 없다.**
이러한 이슈는 ApplicationContext(IoC container)에서 bean을 구현할 때 pre-instantiate singleton을 기본으로 선택한 이유이다.

### 빈 생명주기 콜백 3가지
  - 구성
    - 초기화 콜백 (의존관계 주입이 끝나면 호출)
    - 소멸 전 콜백 (메모리 반납, 연결 종료와 같은 과정)
1. 인터페이스 (InitializingBean, DisposableBean)
2. 설정 정보에 초기화 메소드, 종료 메소드 지정
3. `@PostConstruct`, `@PreDestroy` 어노테이션 지원
 
