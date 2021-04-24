12주차 : 자바의 애노테이션에 대해 학습하세요.
=======

🎯 **목표** 
- 애노테이션 정의하는 방법
- @retention
- @target
- @documented
- 애노테이션 프로세서
-------------------------------------------------------------- 
## 1. 애노테이션 정의하는 방법
사전적 의미로는 '주석'.    
Java에서 사용될 때는 코드 사이에 주석처럼 쓰여지는 특별한 의미, 기능을 수행하도록 하는 기술로,   
프로그램에게 추가적인 정보를 제공해주는 메타데이터(meta data:데이터를 위한 데이터)이다.    

**용도**   
- 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공
- 소프트웨어 개발툴이 빌드나 배치시 코드를 자동으로 생성할 수 있도록 정보 제공
- 실행시(런타임시)특정 기능을 실행하도록 정보를 제공

**사용 순서**   
- 1.어노테이션 정의
- 2.클래스에 어노테이션을 배치
- 3.코드가 실행되는 중에 Reflection을 이용하여 추가정보를 획득하여 기능실시
   
## 2. @retention
자바 컴파일러가 어노테이션을 다루는 방법을 기술하며, 특정 시점까지 영향을 미치는지를 결정한다.  
보통 어노테이션은 Runtime시에 많이 사용하므로 대부분의 어노테이션의 Retention 값은 RUNTIME으로 되어있다.
   
**종류**    
- **RetentionPolicy.SOURCE**    
	: 소스상에서만 어노테이션 정보를 유지. 소스코드를 분석할 때만 의미가 있으며, 바이트코드 파일에는 정보가 남지 않는다.
	컴파일 전까지만 유효(컴파일 이후에는 사라짐)
- **RetentionPolicy.CLASS**
	: 바이트 코드 파일까지 어노테이션 정보를 유지한다.   
	하지만 리플렉션을 이용해서 어노테이션 정보를 얻을 수는 없다.
	컴파일러가 클래스를 참조할 때까지 유효
- **RetentionPolicy.RUNTIME** 
	: 바이트 코드 파일까지 어노테이션 정보를 유지하면서 
	리플렉션을 이용해서 런타임에 어노테이션 정보를 얻을 수 있다.
	컴파일 이후에도 JVM에 의해 계속 참조 가능(리플렉션 사용)
```java
import java.lang.annotation.Retention; 
import java.lang.annotation.RetentionPolicy; 
@Retention(RetentionPolicy.RUNTIME) 
@interface MyAnnotation { 
	String value() default ""; 
}
```
   
## 3. @target
@Target는 적용할 위치(ex : 클래스, 필드, 메서드 ...)를 선택한다.    

**위치**   
- **PACKAGE** : 패키지 선언
- **TYPE : 타입 선언 (클래스, 인터페이스, 열거 타입)
- **ANNOTATION_TYPE** : 어노테이션 타입 선언
- **CONSTRUCTOR** : 생성자 선언
- **FIELD** : 멤버 변수 선언
- **LOCAL_VARIABLE** : 로컬변수 선언
- **METHOD** : 메소드 선언
- **PARAMETER** : 전달인자 선언
- **TYPE_PARAMETER** : 전달인자 타입 선언
- **TYPE_USE** : 타입 선언

```java
import java.lang.annotation.ElementType; 
import java.lang.annotation.Target; 

@Target({ElementType.METHOD}) 
public @interface MyAnnotation { 
	String value(); 
}
```
   
## 4. @documented
@Documented 어노테이션이 지정된 대상의 JavaDoc에 이 어노테이션의 존재를 표기하도록 지정한다.
```java
java.lang.annotation.Documented 

@Documented public @interface MyAnnotation { }
```
```java
@MyAnnotation public class MySuperClass { ... }
```

## 5. 애노테이션 프로세서(Annotation Processor)
일반적으로 애노테이션에 대한 코드베이스를 검사, 수정 또는 생성하는데 사용된다.  
본질적으로 애노테이션 프로세서는 java 컴파일러의 플러그인의 일종이다.   
애노테이션 프로세서를 적재적소에 잘 사용한다면 개발자의 코드를 단순화 할 수 있다.   
   
**장점**   
컴파일 시점에 조작하기 때문에 런타임 비용이 제로   
**단점**    
기존의 코드를 고치는 방법 -> 현재로써는 public 한 api가 존재하지 않는다.
