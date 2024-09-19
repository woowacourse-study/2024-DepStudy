# Spring DI/IoC는 어떻게 동작하나요?

### Spring DI/IoC (Dependency Injection/Inversion of Control)의 동작 원리

**스프링의 DI(Dependency Injection)** 와 **IoC(Inversion of Control)** 는 스프링 프레임워크의 핵심 개념입니다. 이 두 가지는 객체 간의 **의존성 관리**를 간편하게 하고, 코드의 **유연성**과 **테스트 용이성**을 높이는 데 기여합니다.

#### 1. **IoC(Inversion of Control, 제어의 역전)**

IoC는 객체의 생성 및 의존성 주입을 **개발자가 아닌 프레임워크가 관리**한다는 개념입니다. 전통적인 방식에서는 객체가 스스로 필요한 의존성을 생성하거나 가져오는 반면, 스프링에서는 객체의 의존성을 외부에서 주입해주는 방식으로 동작합니다. 이를 통해 객체 간의 결합도를 낮추고, 코드의 유연성을 높일 수 있습니다.

#### 2. **DI(Dependency Injection, 의존성 주입)**

DI는 IoC의 구현 방식 중 하나로, 객체가 다른 객체에 의존할 때, 그 의존 객체를 **직접 생성하는 것이 아니라 외부에서 주입받는** 방법입니다. 스프링은 의존성 주입을 통해 객체 간의 결합도를 줄이고 테스트 용이성을 높입니다.

### Spring에서 DI/IoC의 동작 과정

1. **빈(Bean) 등록**: 
   - 스프링 컨테이너는 애플리케이션에서 사용할 **빈(Bean)**을 생성하고 관리합니다. 빈은 주로 애너테이션(`@Component`, `@Service`, `@Repository`, `@Controller`) 또는 XML 설정 파일을 통해 등록됩니다.
   - 빈 등록은 컨테이너에 의해 자동으로 이루어지며, 개발자는 빈을 직접 생성하거나 관리할 필요가 없습니다.

2. **빈의 의존성 설정**: 
   - 등록된 빈들이 서로 의존성이 있을 때, 스프링 컨테이너는 필요한 의존성을 주입해 줍니다. 이때 의존성 주입은 **생성자 주입**, **세터 주입**, 또는 **필드 주입** 방식으로 이루어집니다.
   - 예를 들어, 클래스 A가 클래스 B에 의존하고 있을 때, 스프링이 클래스 B의 인스턴스를 A에게 주입해 줍니다.

3. **빈 초기화 및 라이프사이클 관리**: 
   - 스프링 컨테이너는 각 빈을 생성하고 의존성을 주입한 후, 빈의 초기화 작업을 수행합니다(`@PostConstruct` 메서드).
   - 빈은 컨테이너에 의해 **싱글톤**으로 관리되며, 애플리케이션이 종료될 때 컨테이너는 빈을 소멸시킵니다(`@PreDestroy`).

### 주요 의존성 주입 방식

1. **생성자 주입(Constructor Injection)**:
   - 의존성을 주입할 때, 주 생성자를 통해 필요한 의존성을 전달합니다. 생성자 주입은 의존성이 반드시 필요한 경우에 유용하며, 불변성을 유지할 수 있습니다.
   - 예시:
     ```java
     @Component
     public class ServiceA {
         private final RepositoryB repositoryB;

         // 생성자 주입
         @Autowired
         public ServiceA(RepositoryB repositoryB) {
             this.repositoryB = repositoryB;
         }
     }
     ```

2. **세터 주입(Setter Injection)**:
   - 의존성을 세터 메서드를 통해 주입하는 방식입니다. 선택적으로 의존성을 주입할 때 유용하지만, 불변성이 약해질 수 있습니다.
   - 예시:
     ```java
     @Component
     public class ServiceA {
         private RepositoryB repositoryB;

         // 세터 주입
         @Autowired
         public void setRepositoryB(RepositoryB repositoryB) {
             this.repositoryB = repositoryB;
         }
     }
     ```

3. **필드 주입(Field Injection)**:
   - 의존성을 클래스의 필드에 직접 주입하는 방식입니다. 코드가 간결해지는 장점이 있지만, 테스트가 어렵고 의존성 주입이 지연될 가능성이 있어 권장되지 않습니다.
   - 예시:
     ```java
     @Component
     public class ServiceA {
         @Autowired
         private RepositoryB repositoryB;
     }
     ```

### **DI/IoC의 장점**

1. **유연한 코드 구조**: 의존성이 외부에서 주입되므로, 코드의 결합도가 낮아지고, 다양한 환경에서 유연하게 동작할 수 있습니다.
2. **테스트 용이성**: 의존성 주입 덕분에, 테스트할 때 **Mock 객체**나 **테스트용 객체**를 쉽게 주입할 수 있어 단위 테스트 작성이 용이합니다.
3. **재사용성**: 의존성이 주입되므로, 객체의 재사용성이 높아집니다. 객체는 자신의 의존성을 알 필요가 없기 때문에 다양한 곳에서 재사용할 수 있습니다.

### **스프링 컨테이너의 역할**

스프링에서는 **ApplicationContext**가 컨테이너 역할을 합니다. 컨테이너는 빈의 생성, 초기화, 의존성 주입, 소멸까지의 전체 생명 주기를 관리합니다. 이를 통해 개발자는 객체 생성과 관리에 신경 쓰지 않고, 비즈니스 로직에 집중할 수 있습니다.

- **BeanFactory**: 가장 기본적인 스프링 컨테이너로, 빈의 생성 및 관리를 담당합니다. 일반적으로 자주 사용되지는 않지만, 매우 가벼운 컨테이너가 필요할 때 사용합니다.
- **ApplicationContext**: `BeanFactory`를 확장한 인터페이스로, 빈 관리 외에도 애플리케이션 이벤트, 메시지 리소스 처리 등을 지원합니다. 실무에서는 주로 `ApplicationContext`를 사용합니다.

---

### **IoC/DI의 예시**

#### 클래스 간 의존성

```java
@Component
public class ServiceA {
    private final RepositoryB repositoryB;

    // 생성자 주입
    @Autowired
    public ServiceA(RepositoryB repositoryB) {
        this.repositoryB = repositoryB;
    }

    public void performAction() {
        repositoryB.doSomething();
    }
}

@Component
public class RepositoryB {
    public void doSomething() {
        System.out.println("RepositoryB is doing something!");
    }
}
```

#### 스프링이 관리하는 컨텍스트에서 의존성 주입

```java
@SpringBootApplication
public class SpringApp {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(SpringApp.class, args);

        ServiceA serviceA = context.getBean(ServiceA.class);
        serviceA.performAction(); // RepositoryB의 doSomething()을 호출
    }
}
```

이 예시에서는 **ServiceA**가 **RepositoryB**에 의존하고 있으며, 스프링 컨테이너가 `@Autowired` 애너테이션을 통해 **RepositoryB**의 인스턴스를 **ServiceA**에 주입해줍니다.

---

### **요약**

- **IoC(Inversion of Control)**: 객체의 생성과 의존성 주입을 개발자가 아닌 프레임워크가 관리하는 방식입니다.
- **DI(Dependency Injection)**: 의존성 주입 방식으로, 생성자 주입, 세터 주입, 필드 주입이 있습니다.
- 스프링 컨테이너가 빈을 관리하고, 필요한 의존성을 주입함으로써 코드의 결합도를 낮추고 유연성을 높입니다.
- DI를 통해 테스트 용이성, 재사용성, 코드 유연성 등 많은 이점을 얻을 수 있습니다.

# 실제로 프로젝트를 진행하며 ioc 와 di 의 장점을 느낀 부분이 무엇이었나요? 어떻게 활용했나요?

이 질문에 답할 때는 **실제 경험**을 바탕으로 IoC와 DI의 **구체적인 장점**을 설명하고, **어떻게 활용했는지**를 이야기하는 것이 중요합니다. 예시로 답변을 구성해보면 다음과 같습니다:

---

**"프로젝트에서 IoC와 DI를 활용하며 크게 두 가지 측면에서 장점을 느꼈습니다.**

1. **유연한 설계와 테스트 용이성**: 
   - 프로젝트에서 여러 서비스 클래스들이 서로 의존성이 많았는데, DI를 사용해 이러한 의존성 주입을 생성자 기반으로 관리했습니다. 이를 통해 실제 비즈니스 로직에 필요한 클래스들 간의 결합도를 크게 낮출 수 있었고, 특히 **단위 테스트**를 작성할 때 **Mock 객체**를 쉽게 주입할 수 있었습니다. 덕분에 **비즈니스 로직만을 독립적으로 테스트**할 수 있어, 테스트 작성과 유지보수가 훨씬 쉬워졌습니다.

2. **확장성 있는 구조**: 
   - 프로젝트에서 사용하던 일부 서비스 로직이 다양한 구현체로 대체되어야 했을 때, 인터페이스를 통해 DI를 활용한 덕분에 구현체를 쉽게 교체할 수 있었습니다. 예를 들어, 특정 기능에서 기존에는 API 호출을 통해 데이터를 처리했지만, 요구사항이 변경되어 데이터베이스를 활용해야 하는 경우가 있었습니다. 이때 IoC 컨테이너를 통해 새로운 구현체를 주입함으로써, 코드 수정 없이 **유연하게 기능을 확장**할 수 있었습니다.