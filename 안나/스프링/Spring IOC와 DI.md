# Spring IoC와 DI란? 

IoC(Inversion Of Control)은 제어의 역전. 클래스가 가지고 있는 인스턴스 변수의 생명주기에 대한 제어를 외부로 위임하는 것이다.

DI(Dependency Injection)은 의존성 주입. 내부에서 객체를 생성하는 것이 아니라 외부에서 생성하여 주입하는 것이다. 

AnimalService 클래스는 메서드에서 직접 Dog 객체를 생성하고 있다. 

```java
public class AnimalService {
	public void bark() {
		Dog dog = new Dog();
		dog.bark();
	}
}

public class Dog {
	public void bark() {
		System.out.in.println("멍멍");
	}
}
``` 

생성 주기에 대한 제어권을 외부로 넘기면 어떻게 될까? 

```java
public class AnimalService {
	private final Dog dog;

	public AnimalService(Dog dog) {
		this.dog = dog;
	}

	public void bark() {
		dog.bark();
	}
}

public class Dog {
	public void bark() {
		System.out.in.println("멍멍");
	}
}
```

위와 같이 클래스를 인스턴스 변수로 옮긴 것이 DI이다. 이제 AnimalService는 Dog를 직접 생성할 필요 없이 외부에서 그대로 받아서 쓰면 된다. 

> 의문점
> 
> DI 만으로 코드를 유연하게 만들 수 있는가? 직접 코드를 작성해보니 DI 자체만으로는 장점이 느껴지지 않는다.

여기서 IoC를 사용해서 변경에 더욱 용이하게 만들어보자. 만약 추가로 Cat 클래스가 생성되었다고 할 때, AnimalService가 Cat의 `bark()` 메서드를 호출하려면 구현체를 직접 바꾸어야 할 것이다. 

```java
public class AnimalService {
	private final Dog dog; 
	
	public AnimalService(Dog dog) {
		this.dog = dog;
	}
	
	public void bark() {
		Dog dog = new Dog();
		dog.bark();
	}
}

public class Dog {
	public void bark() {
		System.out.in.println("멍멍");
	}
}

public class Cat {
	public void bark() {
		System.out.in.println("야옹");
	}
}
```

이 경우 DIP (Dependency Inversion Principal, 의존 역전 원칙)을 사용하여 상위 모듈 (`AnimalService`)이 하위 모듈 (`Dog`, `Cat`) 을 참고하지 않도록 한다.

추상화를 사용하는 것이 핵심이다. 
```java
public class AnimalService {
	private final Animal animal; 
	
	public AnimalService(Animal animal) {
		this.animal = animal;
	}
	
	public void bark() {
		Dog dog = new Dog();
		dog.bark();
	}
}

public interface Animal {
	public void bark();
}

public class Dog implements Animal {
	@Override
	public void bark() {
		System.out.in.println("멍멍");
	}
}

public class Cat implements Animal {
	@Override
	public void bark() {
		System.out.in.println("야옹");
	}
}
```

이렇게 인터페이스를 사용하여 런타임 시점에 실제 구현 클래스를 주입받을 때 IoC가 적용되었다고 할 수 있다. 
어떤 객체를 생성할 지, 객체의 생명주기의 관리 등의 부분을 외부로 위임하기 때문이다.


> 의문점
> 
> 인터페이스를 사용해 추상화해야만 IoC를 사용한 것인가? 
> Spring IoC는 컨트롤러 클래스를 만들고 서비스 클래스를 외부로 주입받았을 때 (인터페이스가 아니더라도) 자동으로 호출해서 주입해주는데, 이것도 IoC라고 하지않나?
