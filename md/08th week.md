8주차 : 자바의 인터페이스에 대해 학습하세요.
=======

🎯 **목표** 
- 인터페이스 정의하는 방법
- 인터페이스 구현하는 방법
- 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 인터페이스 상속
- 인터페이스의 기본 메소드 (Default Method), 자바 8
- 인터페이스의 static 메소드, 자바 8
- 인터페이스의 private 메소드, 자바 9
-------------------------------------------------------------- 
## 인터페이스란?
객체의 사용 방법을 정의한 타입이다. 개발 코드와 객체가 서로 통신하는 접점 역할을 한다.   
개발 코드 -> 인터페이스의 메소드를 호출 -> 인터페이스 -> 객체의 메소드 호출과 같이 동작하게된다.   
그렇기 때문에, 개발 코드는 객체의 **내부 구조를 알 필요가 없고,**  **인터페이스의 메소드만** 알고있으면 된다.

**장점**
- 코드 변경 없이 실행 내용과 리턴값을 다양화할 수 있다.
   
## 1.인터페이스 정의하는 방법 
물리적으로는 클래스와 동일하다.
`xx.java`형태의 소스 파일로 작성되고, 컴파일러(javac.exe)를 통해 `xx.class`형태로 컴파일 된다.
```java
[public] interface 인터페이스명 { ... }
```
- 인터페이스 이름은 클래스 이름을 작성하는 방법과 동일하다.
- 상수와 메소드만 구성 멤버로 가진다.
- 객체로 생성할 수 없기 때문에 생성자를 가질 수 없다.
- 자바7이전까지는 메소드는 추상 메소드로만 선언 가능
- 자바8부터는 디폴트 메소드, 정적 메소드 선언 가능

```java
/* 인터페이스 선언 */
interface 인터페이스명 {
	타입 상수명 = 값;			// 상수, 반드시 초기값 대입필요.
	타입 메소드명(매개변수, ...);	// 추상 메소드
	default 타입 메소드명(매개변수, ...) { ... };	 // 디폴트 메소드
	static 타입 메소드명(매개변수) { ... };		 	 // 정적 메소드
}

/* 인터페이스 작성 예 */
public interface RemoteControl {
	public int MAX_VOLUM = 10,
				MIN_VOLUME = 0;	// 상수
	
	// 추상 메소드(선언부만 작성)
	public void turnOn();
	public void turnOff();
	public void setVolume(int volume);
	
	// 디폴트 메소드
	default void setMute(boolean mute) {
		if (mute) System.out.println("무음처리 합니다."); 
		else System.out.println("무음 해제 합니다.");
	}
	
	// 정적 메소드
	static void changeBattery() {
		System.out.println("건전지를 교환합니다.");
	}	
}
```
   
## 2. 인터페이스 구현하는 방법
클래스 선언부에 **implements** 키워드를 추가 후 인터페이스 명을 명시한다.   
구현 클래스에서 인터페이스의 추상메소드들을 오버라이딩할때,    
기본적으로 public접근 제한을 갖기 때문에 public보다 더 낮은 접근 제헌으로 작성할 수 없다.
```java
/* 인터페이스 구현 */
public class 구현클래스명 implements 인터페이스명 {
	// 인터페이스에 선언된 추상 메소드의 실체 메소드 선언
}

/* 인터페이스 구현 예 */
public class Tv implements RemoteControl{

	private int volume; //필드

	// 추상 메소드  오버라이딩
	public void turnOn() {
		System.out.println("TV 전원 킴");
	}
	public void turnOff() {
		System.out.println("TV 전원 끔");
	}
	// 추상 메소드  오버라이딩_ 인터페이스 상수를 이용한 값 제한
	public void setVolume(int volume) {
		if (volume > RemoteControl.MAX_VOLUME) {
			this.volume = RemoteControl.MAX_VOLUME;
		} else if (volume < RemoteControl.MIN_VOLUME) {
			this.volume = RemoteControl.MIN_VOLUME;
		} else {
			this.volume = volume;
		}
		System.out.println("현재 볼륨 : " + volume);
	}
}
```
인터페이스에 선언된 추상 메소드를 구현클래스에서 오버라이딩하지않을 시 -> 구현 클래스는 자동적으로 추상 클래스가 됨   
-> 그렇기 때문에 클래스 선언부에 abstract 키워드를 추가해야함.
```java
public abstract class Tv implements RemoteControl {
	public void turnOn() { ... } //구현
	public void turnOn() { ... } //구현
	// setVolume()실체 메소드가 없음	
}
```
   
## 3. 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
인터페이스로 구현 객체를 사용하려면, 인터페이스 변수를 선언하고 구현 객체를 대입해야 한다.
```java
인터페이스 변수 = 구현객체;
```
```java
/* 인터페이스 변수에 구현 객체 대입 */
public class RemoteControlExample {
	public static void main(String[] args) {
		RemoteControl rc; 	//인터페이스 변수(참조type)
		rc = new Tv(); 		//구현 객체의 번지
	}
}

/* 익명 구현 클래스 */
public class RemoteControlExample {
	public static void main(String[] args) {
		RemoteControl rc = new RemoteControl() {
			public void turnOn() {/*실행부*/}
			public void turnOff() {/*실행부*/}
			public void setVolume(int volume) {/*실행부*/}
		};
	}
}
```
   
## 4. 인터페이스 상속
- 인터페이스가 다른 인터페이스 상속 가능
- (클래스와 달리)다중 상속 허용
```java
public interface 하위인터페이스 extends 상위인터페이스1,상위인터페이스2 {...}
``` 
하위 인터페이스를 구현하는 클래스는 하위 인터페이스의 메소드뿐만 아니라,    
상위 인터페이스의 모든 추상 메소드에 대한 실체 메소드를 가지고있어야한다.
```java
/* 부모 인터페이스 (InterfaceA.java)*/
public interface InterfaceA {
	public void methodA();
}
/* 부모 인터페이스 (InterfaceB.java)*/
public interface InterfaceB {
	public void methodB();
}
/* 하위 인터페이스 (InterfaceC.java) */
public interface interfaceC extends InterfaceA, InterfaceB {
	public void methodC();
}
```
```java
/* 하위 인터페이스 구현 (ImplementationC.java)*/
public class ImplementationC implements InterfaceC {
	public void methodA() {
		System.out.println("methodA 실행");
	}
	public void methodB() {
		System.out.println("methodB 실행");
	}
	public void methodC() {
		System.out.println("methodC 실행");
	}
}
```
```java
/* 호출 가능 메소드 (Example.java)*/
public class Example {
	public static void main(String[] args){
		ImplementationC impl = new ImplementationC();
		ImplementationA ia = impl;
		ia.methodA();
		
		ImplementationB ib = impl;
		ib.methodB();
		
		ImplementationC ic = impl;
		ic.methodA();
		ic.methodB();
		ic.methodC();
	}
}
```
   
## 5. 인터페이스의 기본 메소드 (Default Method), 자바 8
자바 8에서 추가된 인터페이스의 새로운 멤버이다. default키워드가 리턴 타입 앞에 붙는다.
```java
[public] defulat 리턴타입 메소드명(매개변수, ...) {...}

/* 작성 예 */
default void setMute(boolean mute) {
	if (mute) System.out.println("무음처리 합니다."); 
	else System.out.println("무음 해제 합니다.");
}
``` 
인터페이스에 선언되지만 사실은 객체(구현 객체)가 가지고 있는 인스턴스 메소드라고 생각해야한다.   
추상 메소드가 아닌 인스턴스 메소드이므로, 구현 객체가 있어야 사용가능하다.    
**기존 인터페이스를 확장**해서 새로운 기능을 **추가**하기 위해 허용되었다.    
public특성을 갖기 때문에 public을 생략하더라도 자동적으로 컴파일 과정에서 붙게된다.

### Default 메소드 사용
```java
RemoteControl.setMute(ture);  // (x)RemoteControl의 구현 객체가 필요

RemoteControl rc = new Tv();
rc.setMute(true);   // (o)
```
   
## 6. 인터페이스의 static 메소드, 자바 8
자바8에서 추가되었다. 디폴트 메소드와 달리 객체가 없어도 인터페이스만으로 호출이 가능하다.
```java
[public] static 리턴타입 메소드명(매개변수, ...) {...}

/* 작성 예 */
static void changeBattery() {
	System.out.println("건전지를 교환합니다.");
}
```
   
### static 메소드 사용
```java
/* 작성 예 */
public class RemoteControlExample{
	public static void main(String[] args) {
		RemoteControl.changeBattery();
	}
}
```
   
## 7. 인터페이스의 private 메소드, 자바 9
자바9에서 추가된 메서드로, 인터페이스 내에서 사용하기 위해 구현하는 메서드이다.  
인터페이스 내부에 코드를 구현할 수 있게 되면서 외부 구현체에서 필요한 메소드가 아닌 내부에서만 작동되기를 원하는 메소드도 노출 되는 경우 발생하였다.
이를 해결하기 위해 private 메소드 지원하게되었다.
코드 중복 피하고 내부에서 작동하는 메소드에 대해 캡슐화 유지 가능하다.
즉, default메서드나 static메서드 내에서 사용하기 위하여 구현한다.   
구현하는 클래스에서 재정의하여 사용할 수 없으며, 자식 인터페이스에서도 상속 불가능하다.  
