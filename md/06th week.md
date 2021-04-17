6주차 : 자바의 상속에 대해 학습하세요.
=======
🎯 **목표** 
- 자바 상속의 특징
- super 키워드
- 메소드 오버라이딩
- 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)
- 추상 클래스
- final 키워드
- Object 클래스
--------------------------------------------------------------
## 1. 자바 상속(Inheritance)의 특징
객체 지향 프로그램에서도 부모 클래스의 멤버를 자식 클래스에게 물려줄 수 있다.    
이를 상속이라고 한다.   
  부모 클래스 = 상위 클래스
  자식 클래스 = 하위 클래스 = 파생 클래스   
  
**특징**   
- 이미 잘 개발된 클래스 재사용 -> 코드 중복을 줄여줌
- 클래스 수정을 최소화 -> 유지 보수 시간 최소화
- extends뒤에는 단 하나의 부모 클래스만 올 수 있다.-> 다중 상속을 허용x

```java
/* 부모 클래스 정의 */
public class Phone {
	String model, color;  // 필드
	
	// 생성자x-> 기본생성자 자동생성될것임
	// 메서드
	void powerOn() {System.out.println("전원-ON");}
	void powerOFF() {System.out.println("전원-OFF");}
	void ring() {System.out.println("고전 벨소리");}
}
```
```java
/* 자식 클래스 정의 */
public class MyPhone extends Phone{	//Phone클래스 상속
	boolean film; // 필드
	// 생성자
	MyPhone (String model, String color, boolean film){
		this.model = model;
		this.color = color;
		this.film = film;
	}
	// 메소드 재정의
	@Override
	void ring() {
		System.out.println("빈지노의 Break!");
	}
}
```
```java
/* 실행 클래스 */
public class PhoneExample {
	public static void main(String[] args) {
		MyPhone mine = new MyPhone("s10e","white",true);
		
		// Phone으로 부터 상속 받은 필드
		System.out.println("모델: " + mine.model);
		System.out.println("색상: " + mine.color);
		// myPhone의 필드
		System.out.println("필름 부착: " + mine.film);
		
		// 재정의한 메소드
		mine.ring();
	}
}
```

## 2. super 키워드
자식 객체를 생성하면, 부모 객체가 먼저 생성되고 자식 객체가 그 다음에 생성된다.
부모 객체를 생성하기 위해 부모 생성자를 호출하는데, 이는 자식 생성자의 맨 첫 줄에서 호출된다. 이것이 super()이다.
자식 생성자가 명시적으로 선언되지 않았다면 컴파일러는 super();을 자동으로 추가하여 부모의 기본 생성자를 호출한다.

- super(매개값, ...)는 매개값의 타입과 일치하는 부모 생성자를 호출한다.
  (만약 매개값의 타입과 일치하는 부모 생성자가 없을 경우 컴파일 오류 발생)
- super()은 반드시 자식 생성자 첫 줄에 위치해야한다.

```java
스/* 부모 클래스*/
public class Airplane {
	public void fly() {
		System.out.println("일반 비행");
	}
}
```
```java
/* 자식 클래스*/
public class SupersonicAirplane extends Airplane{
	public static final int NORMAL = 1
							,SUPERSONIC = 2;
	public int flyMode = NORMAL;
	@Override
	public void fly() {
		if (flyMode == SUPERSONIC) {
			System.out.println("초고속 비행");
		} else {
			// Airplane 객체의 fly()메소드 호출
			super.fly();
		}
	}
}
```
```java
/* 실행 클래스*/
public class AirplaneExample {
	public static void main(String[] args) {
		SupersonicAirplane sa = new SupersonicAirplane();
		
		sa.fly();
		sa.flyMode = SupersonicAirplane.NORMAL;
		sa.fly();
	}
}
```

## 3. 메소드 오버라이딩
오버라이딩(Overriding)은 상속된 메소드의 내용이 자식 클래스에 맞지 않을 경우, 자식 클래스에서 동일한 메소드를 재정의 하는 것을 말한다.   
자식 객체에서 메소드를 호출하면, 오버라이딩된 자식 메소드가 호출된다.

- 부모의 메소드와 동일한 시그니처(리턴타입, 메소드 이름, 매개변수 리스트)를 가져야한다.
- 접근 제한을 더 강하게 오버라이딩할 수 없다.
- 새로운 예외(Exception)를 throws할 수 없다.
- @Override어노테이션은 생략가능

## 4. 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)
### 디스패치
디스패치란 어떤 메서드를 호출할 것인가를 결정하는 과정을 말한다.   
즉, 메서드의 의존성을 결정하는 과정이라고 할 수 있다.
- 정적 디스패치
- 동적 디스패치
   
#### 정적 디스패치
정적 디스패치는 런타임이 아닌 **컴파일 타임** 에서 컴파일러, 사용자, 바이트코드 모두 어떤 메서드가 실행될 지 아는 것이다.
min객체의 두 greet메서드 중 어떤 메서드를 실행할 지 **컴파일 타임에 결정** 한다.
```java
public class Person {
	private int age;
	
	public void greet() {
		System.out.println("Hello");
	}
	
	public void greet(String str) {
		System.out.println(str);
	}
	
	public void printJob() {
	}
	
	public static void main(String[] args) {
		Person min = new Person();
		min.greet();
		min.greet("Long time no see!");
	}
}
```
**동적 디스패치**
추상 클래스인 Job클래스를 상속받는 Student,Teacher클래스가 있다.
이 두 클래스를 Job타입 객체로 만들고, 오버라이딩한 printJob()을 실행시킨다.

Job이라는 abstract클래스의 메서드로 접근하는데, 컴파일타임에 하위 타입의 printJob()의 구현을 고려할 수 있을까? 
컴파일 타임에 어떤 클래스의 printJob을 실행해야하는지 알 수 없다.

어떤 메서드를 실행시킬까는 런타임에 결정하게 되는것이다.
이를 동적 디스패치라고 한다.


```java
/* 추상 클래스 Job */
abstract class Job {
	abstract void printJob();
}
/* 추상 클래스 Job의 자식 Student */
class Student extends Job{
	@Override
	public void printJob() {
		System.out.println("I'm Student");
	}
}
/* 추상 클래스 Job의 자식 Teacher */
class Teacher extends Job{
	@Override
	public void printJob() {
		System.out.println("I'm Teacher");
	}
}
```
```java
/* 클래스 실행 */
public class Person2 {
	public static void main(String[] args) {
		Job student = new Student();
		Job teacher = new Teacher();
		
		student.printJob();
		teacher.printJob();
	}
}

```
   
## 5. 추상 클래스
사전적 의미로 추상(abstract)은 실체 간에 공통되는 특성을 추출한것을 말한다.
구체적인 실체라기보다는 실체들의 공통되는 특성을 가직 있는 추상적인 것.
**목적**   
- 실체 클래스들의 공통된 필드와 메소드의 이름을 통일하기 위함
- 실체 클래스를 작성할 때 시간을 절약하기 위함
**특징**
- 클래스 선언에 abstract 키워드를 붙여야 한다.
- 추상클래스는 new연산자를 사용해 인스턴스를 생성시키지 못한다.
```java
/* 추상 클래스 */
public abstract class Animal {
	public String kind;
	
	public void breath() {
		System.out.println("숨을 쉰다");
	}
	public abstract void sound();	//추상 메서드 선언
}
```
```java
/* 자식 클래스, 추상 메소드 오버라이딩*/
public class Dog extends Animal{
	public Dog() {
		this.kind = "포유류";
	}

	// 추상 메소드 재정의
	@Override
	public void sound() {
		System.out.println("멍멍\n");
	}
}

```
```java
/* 실행 클래스 */
public class AnimalExample {
	public static void main(String[] args) {
		Dog dog = new Dog();
		System.out.println("-- Dog변수로 호출하기 --");
		dog.sound();

		System.out.println("-- 부모 변수로 타입 변환 후 호출하기 --");
		Animal animal = null;
		animal = new Dog();
		animal.sound();
		
		System.out.println("-- 부모타입 매개변수 자식 객체 대입,(다형성 적용) --");
		animalSound(new Dog());
	}
	public static void animalSound(Animal animal) {
		animal.sound();
	}
}
```
## 6. final 키워드
- final키워드는 해당 선언이 최종상태이고, 결코 수정될 수 없음을 뜻함
- 클래스, 필드, 메소드 선언 시 사용가능.

클래스와 메소드 선언 시에 final키워드가 지정되면 상속과 관련이 있다.

#### class
클래스를 선언할 때 final키워드를 class앞에 붙이게 되면 상속할 수 없는 클래스가 된다. 
final클래스는 부모 클래스가 될 수 없어 자식 클래스를 만들 수 없다.
#### 메소드
오버라이딩할 수 없는 메소드가 된다. 
즉, 부모 클래스를 상속해서 자식 클래스를 선언할 때 부모 클래스에 선언된 final메소드는 자식 클래스에서 재정의 할 수 없다.

## 7. Object 클래스 
java의 모든 클래스의 최상위 부모이다.   
자동으로 java.lang.Object클래스를 상속받으므로 extends역시 필요 없다.
   
**주요 메소드**  
| 반환형 | 메소드명 | 설명 |
|---|---|---|
| boolean| equals(Object o) | 객체 간에 동일여부를 나타냄 | 
| int | hashCode() | 객체의 해쉬값 | 
| String | toString() | 객체를 string으로 나타냄, 기본적으로 클래스명@16진수hashCode값 | 
| Object | clone() | 객체를 복사해서 리턴 | 
| void | finalize() | 객체가 GC처리 되기전에 호출 | 
| Class | getClass() | 객체의 runtime class를 리턴 |
> instanceof연산자와 getClass()   
> instanceof연산자는 상위 부모클래스에소 true를 반환한다.   
> if문으로 분기처리할 경우, getClass()로 정확히 처리하는 것이 낫다.
