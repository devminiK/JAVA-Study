14주차 : 자바의 제네릭에 대해 학습하세요.
=======

🎯 **목표** 
- 제네릭 사용법
- 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
- 제네릭 메소드 만들기
- Erasure

-------------------------------------------------------------- 
### 제네릭이란(Generic)?
Java5부터 제네릭 타입이 새로 추가되었는데,    
제네릭 타입을 이용함으로써 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있게되었다. 
클래스와 인터페이스, 그리고 메소드를 정의할 때 타입(type)을 파라미터(parameter)로 사용할 수 있도록 한다.

**장점**
- 컴파일 시 강한 타입 체크 가능
	실행 시 에러가 나는 것보다는 컴파일 시에 미리 타입을 강하게 체크해서 에러를 사전에 방지하는 것이 좋다.
- 타입 변환(csting)을 제거한다
	비제네릭 코드는 불필요한 타입 변환을 하기 때문에 프로그램 성능에 악영향을 미친다.
```java
/* 비제네릭 코드 */
List list = new ArrayList();
list.add("hi");
String str = (String)list.get(0); //타입 변환을 해야한다.

/* 제네릭 코드 */
List<String> list2 = new ArrayList<String>();
list2.add("hi");
String str = list2.get(0); //타입 변환 불필요.
```

## 1. 제네릭 사용법
제네릭의 타입 파라미터는 일반적으로 알파벳 한 글자로 작성하고 아래와 같은 의미를 갖는다.
- `<T>` : Type
- `<E>` : Element
- `<K>` : Key
- `<N>` : Number
- `<V>` : Value
- `<R>` : Result

### 클래스 
```java
/* 제네릭 클래스 */
class Test<T>{
    private T t;
      
    public void set(T t) {
        this.t = t;
    }
  
    public T get() {
        return t;
    }
}
```
```java
/* 실행 */
public static void main(String[] args) {
    Test<String> test = new Test();
    test.set("test");
    System.out.println(test.get());
}
```
Test<T>의 T대신 String을 사용하였기 때문에 메소드 set(T)대신 set(String) 으로 사용되었고,
메소드 get()은 return T; 대신 return String;이 된것이다.

### 인터페이스 
```java
/* 제네릭 인터페이스 */
interface InterfaceSample<T1, T2> {
        T1 testMethod1(T2 t);
        T2 testMethod2(T1 t);
}
```
```java
/* 구현 클래스 */
class TestSampleInerface implements InterfaceSample<String, Integer> {
        @Override
        public String testMethod1(Integer t) {
               return null;
        }
  
        @Override
        public Integer testMethod2(String t) {
               return null;
        }
}
```
### 메소드
메소드 제네릭 타입은 Class에 Generic type을 선언하지 않고, 
각 메소드 마다 Generic type을 선언해 사용할 수 있다.

메소드의 파라미터의 T 이 선언되어 있다면, 리턴타입 바로앞에 <T> 제너릭 타입을 선언해주어야한다.
※ 메소드의 파라미터에 <T>가 선언되어 있다면, ReturnType 앞에 <T>를 선언한다.
```java
class TestMethod {
    public static <T> List<T> method(List<T> list, T item) {
        list.add(item);
        return list;
    }
}
```

## 2. 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
### 바운디드 타입
제네릭 바운디드 타입은 제네릭으로 사용되는 파라미터 타입을 제한할 수 있는것을 말한다.
제한된 타입 파라미터를 선언하려면, 타입 파라미터뒤에 extends키워드를 붙이고 상위 타입을 명시하면 된다.
상위 타입은 클래스, 인터페이스 모두 가능하다. 인터페이스라고해서 implements를 사용하진않는다.
```java
public <T extends 상위타입> 리턴타입 메소드(매개변수, ...){...}
```
```java
public class BufferReaderEx1 {
	public static void main(String[] args) {
		/* String은 Number 타입이 아님 */
//		String result = Util.compare(10, 20);
		
		int result1 = Util.compare(10, 20);
		System.out.println("결과: " + result1);
		
		int result2 = Util.compare(4.5, 3);
		System.out.println("결과: " + result2);
	
	}
	
	public static class Util{
		public static <T extends Number> int compare(T t1, T t2) {
			double v1 = t1.doubleValue();
			double v2 = t2.doubleValue();
			
			return Double.compare(v1, v2);
		}
	}
}
```

### 와일드 카드
코드에서 ?를 일반적으로 와일드 카드라고 부른다.
제네릭 타입을 매개값이나 리턴 타입으로 사용할 때 구체적인 타입 대신에 와일드카드를 사용할 수 있다.

- **제네릭타입<?>**    
	: 제한없음. 모든 객체 자료형, 내부적으로는 Object로 인식
- **제네릭타입<? extends 상위타입>**   
	: Upper Bounded Wildcards(상위 클래스 제한)
	타입 파라미터를 대치하는 구체적인 타입으로, 상위 타입이나 하위 타입만 올 수 있다.
- **제네릭타입<? super 하위타입>**       
	: Lower Bounded Wildcards(하위 클래스 제한)
	타입 파라미터를 대치하는 구체적인 타입으로 하위 타입이나 상위 타입이 올 수 있다.   
![w13_generic](../img/w13_generic.png)

위와 같이 클래스가 존재한다고 한다면   
**Course<?>**   
: 수강생은 모든 타입(Person,Worker,Student,HightStudent)이 될 수 있다.   
**Course<? extends Student>**   
: 수강생은 Student, HightStudent만 될 수 있다.   
**Course<? super Worker>**   
: 수강생은 Person,Worker만 될 수 있다.   

## 3. 제네릭 메소드 만들기
파라미터 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드를 제네릭 메소드라고 한다.
리턴 타입 앞에 `<>`를 추가하고 타입 파라미터를 기술하여 작성한다.

`public <타입 파라미터,...> 리턴타입 메소드명(파라미터,...)`형식으로 메소드를 작성하는데,   
일반적으로 타입 파라미터를 컴파일 시에 추론이 가능하기 때문에 앞의 <타입 파라미터> 부분을 생략할 수 있다.

```java
public T getName() {
    return name;
}
public T getGender() {
    return gender;
}
```
## 4. Erasure
제네릭은 타입의 안정성을 보장하며 실행시간에 오버헤드가 발생하지 않도록 하기 위해 추가 되었다.    
컴파일러는 컴파일 시점에 제네릭에 대하여 "type erasure(타입 이레이저)"라고 부르는 프로세스를 적용한다.   
타입 이레이저는 모든 타입 파라미터들을 제거하고나서 그 자리를 제한하고 있는 타입으로 변경하거나 타입 파라미터의 제한    
타입이 지정되지 않았을 경우에는 Object 로 대체한다. 따라서 컴파일 후에 바이트 코드는 새로운 타입이 생기지 않도록   
보장하는 일반 클래스들과 인터페이스, 메소드들만 포함한다.  Object 타입도 컴파일 시점에 적절한 캐스팅이 적용된다

```java
/* <T>인 경우 T는 Object로 치환한다. */
public <T> List<T> genericMethod(List<T> list) { 
	return list.stream().collect(Collectors.toList()); 
}


===>>>

// for illustration
public List<Object> withErasure(List<Object> list) { 
	return list.stream().collect(Collectors.toList()); 
}

// which in practice results in
public List withErasure(List list) {
    return list.stream().collect(Collectors.toList());
}
```

```java
/* 타입이 제한되어 있을 경우 그 타입은 컴파일시점에 제한된 타입으로 교체됨*/
public <T extends Building> void genericMethod(T t) { ... }

===>>>

public void genericMethod(Building t) { ... }
```
