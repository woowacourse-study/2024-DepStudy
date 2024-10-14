SOLID는 객체 지향 프로그래밍에서 소프트웨어 디자인 원칙을 나타내는 약어로, 유지보수성과 확장성을 높이기 위해 설계된 다섯 가지 원칙을 포함합니다. 이 원칙들은 다음과 같습니다:

### 1. **S - Single Responsibility Principle (SRP) - 단일 책임 원칙**
- **정의**: 클래스는 하나의 책임만 가져야 하며, 그 책임을 완수하는 데 필요한 모든 기능을 포함해야 합니다.
- **설명**: 즉, 클래스는 변경되는 이유가 하나여야 합니다. 만약 클래스가 두 가지 이상의 책임을 가진다면, 그 중 하나의 변경이 다른 책임에 영향을 미칠 수 있습니다.
- **예시**: 
  ```java
  class User {
      private String name;
      private String email;

      // 유저의 정보 출력
      public void printUserInfo() {
          System.out.println("Name: " + name + ", Email: " + email);
      }
  }

  class UserRepository {
      // 데이터베이스에 유저를 저장
      public void saveUser(User user) {
          // save logic
      }
  }
  ```

### 2. **O - Open/Closed Principle (OCP) - 개방/폐쇄 원칙**
- **정의**: 소프트웨어 요소는 확장에 대해서는 개방적이어야 하고, 수정에 대해서는 폐쇄적이어야 합니다.
- **설명**: 즉, 기존 코드를 수정하지 않고도 기능을 확장할 수 있어야 하며, 새로운 기능을 추가할 때 기존 코드를 변경하지 않는 것이 이상적입니다.
- **예시**: 
  ```java
  abstract class Shape {
      abstract double area();
  }

  class Circle extends Shape {
      private double radius;

      @Override
      double area() {
          return Math.PI * radius * radius;
      }
  }

  class Rectangle extends Shape {
      private double width, height;

      @Override
      double area() {
          return width * height;
      }
  }
  ```

### 3. **L - Liskov Substitution Principle (LSP) - 리스코프 치환 원칙**
- **정의**: 서브타입은 언제나 자신의 기반 타입으로 대체할 수 있어야 하며, 프로그램의 정확성을 유지해야 합니다.
- **설명**: 즉, 자식 클래스는 부모 클래스의 기대를 충족해야 하며, 자식 클래스를 부모 클래스로 대체할 때 문제가 발생해서는 안 됩니다.
- **예시**: 
  ```java
  class Bird {
      void fly() { /* flying logic */ }
  }

  class Sparrow extends Bird {
      @Override
      void fly() { /* sparrow flying logic */ }
  }

  class Ostrich extends Bird {
      @Override
      void fly() {
          throw new UnsupportedOperationException("Ostrich can't fly");
      }
  }
  ```

### 4. **I - Interface Segregation Principle (ISP) - 인터페이스 분리 원칙**
- **정의**: 클라이언트는 자신이 사용하지 않는 인터페이스에 의존하지 않아야 하며, 인터페이스는 특정 클라이언트를 위한 기능에 맞춰 분리되어야 합니다.
- **설명**: 즉, 큰 인터페이스보다는 여러 개의 작은 인터페이스를 사용하는 것이 좋습니다. 이를 통해 클라이언트가 필요 없는 메소드를 구현하는 것을 방지할 수 있습니다.
- **예시**: 
  ```java
  interface Printer {
      void print();
  }

  interface Scanner {
      void scan();
  }

  class MultiFunctionPrinter implements Printer, Scanner {
      public void print() { /* print logic */ }
      public void scan() { /* scan logic */ }
  }

  class SimplePrinter implements Printer {
      public void print() { /* print logic */ }
  }
  ```

### 5. **D - Dependency Inversion Principle (DIP) - 의존성 역전 원칙**
- **정의**: 고수준 모듈은 저수준 모듈에 의존해서는 안 되며, 둘 다 추상화에 의존해야 합니다.
- **설명**: 즉, 구체적인 구현에 의존하기보다는 인터페이스나 추상 클래스에 의존해야 합니다. 이를 통해 코드의 유연성과 테스트 용이성을 높일 수 있습니다.
- **예시**: 
  ```java
  interface Database {
      void save();
  }

  class MySQLDatabase implements Database {
      public void save() { /* save logic for MySQL */ }
  }

  class UserService {
      private Database database;

      public UserService(Database database) {
          this.database = database;
      }

      public void saveUser() {
          database.save();
      }
  }
  ```

### 요약
- **SRP**: 클래스는 하나의 책임만 가져야 한다.
- **OCP**: 소프트웨어 요소는 확장에 대해서 개방적, 수정에 대해서 폐쇄적이어야 한다.
- **LSP**: 서브타입은 기반 타입으로 대체할 수 있어야 한다.
- **ISP**: 클라이언트는 사용하지 않는 인터페이스에 의존하지 않아야 한다.
- **DIP**: 고수준 모듈과 저수준 모듈은 추상화에 의존해야 한다.

이 원칙들을 잘 적용하면 객체 지향 설계의 품질을 높이고, 유지보수와 확장성이 뛰어난 소프트웨어를 개발할 수 있습니다.