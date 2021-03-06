목표
=======
자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.

학습할 것
=======

* JVM이란 무엇인가
* 컴파일 하는 방법
* 실행하는 방법
* 바이트코드란 무엇인가
* JIT 컴파일러란 무엇이며 어떻게 동작하는지
* JVM 구성 요소
* JDK와 JRE의 차이

--------------------------------------------------------------
week1: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가
=======

## 1. JVM이란 무엇인가
JVM은 Java Virtual Machine의 약자로 **자바 가상 기계**이다.     
운영체제는 자바 프로그램을 바로 실행할 수 없다. 그 이유는 자바 프로그램은 완전한 기계어가 아닌, 중간 단계의 바이트 코드이기 때문이다.     
*이것을 해석하고 실행할 수 있는 가상의 운영체제가 필요한데, 이것을 자바 가상 기계, JVM*이라고 한다.     
    
JVM은 실 운영체제를 대신해서 자바 프로그램을 실행하는 가상의 운영체제 역할을 하며, 운영체제에 종속적이다.    
자바 프로그램을 운영체제가 이해하는 기계어로 번역해서 실행해야 하므로 JVM은 운영체제에 맞게 설치되어야 한다.   
JVM은 JDK또는 JRE를 설치하면 자동으로 설치되는데, JDK와 JRE가 운영체제 별로 제공된다.

## 2. 컴파일 하는 방법
**Java 코드**
<pre><code>public class Hello {
    public static void main(String[] args) {
    System.out.println("Hello, Welcome to the java world!");
    }
}</code></pre>

**컴파일 & 컴파일 후 결과**
<pre><code>> javac 파일명.java</code></pre>
<img src="../img/w1_compile_result.png" width="450px" height="300px" alt="java_run_process"></img><br/>

## 3. 실행하는 방법
<pre><code>> java 파일명</code></pre>
<img src="../img/w1_run_result.png" width="200px" height="100px" alt="java_run_process"></img><br/>

1. .java소스 파일 작성 
2. 컴파일러(javac.exe)로 바이트 코드 파일(.class) 생성
3. JVM 구동 명령어(java.exe)로 실행

## 4. 바이트코드란 무엇인가
자바 가상머신이 이해할 수 있는 언어로 변환된 자바 소스 코드를 의미한다.
자바 컴파일러에 의해 변환되는 코드의 명령어 크기가 1바이트라서 자바 바이트코드라고 부른다.
자바 바이트 코드의 확장자는 .class이다.
자바 바이트 코드는 JVM만 설치되어 있으면, 어떤 운영체제에서라도 실행될 수 있다.

## 5. JIT 컴파일러란 무엇이며 어떻게 동작하는지
Java에서 컴파일을 하면 바이트코드로 변환된다. 
이 바이트코드는 기계가 바로 읽을 수 있는 형태가 아니므로 실제 실행될 때 다시 한번 기계가 읽을 수 있는 형태(native code)로 interpreter를 통해 해석이 되어야한다.
이때 쓰이는 것이 JIT컴파일러이다.

*이렇게 한번에 기계어로 변환하지 않고 두번의 과정을 밟는 이유는 Java의 최대 장점 중 하나인 machine-independent특성 때문이다.
platform independent라고도 하는데, 한번 작성으로 어떤 기계에서든 돌릴 수 있다는 특성을 말한다.

JIT 컴파일러(Just-In-Time compiler)란 프로그램이 실행 중인 런타임에 실제 기계어로 변환해 주는 컴파일러를 의미한다.
동적 번역(dynamic translation)이라고도 불리는 이 기법은 프로그램의 실행 속도를 향상시키기 위해 개발되었다.
JIT 컴파일러는 같은 코드를 매번 해석하지 않고 실행할 때 컴파일을 하면서 해당 코드를 캐싱한다. 
이후엔 바뀐 부분만 컴파일 하고 나머지는 캐싱된 코드를 사용하므로 인터프리터의 속도를 개선 할 수 있는것이다.
즉, JIT 컴파일러는 자바 컴파일러가 생성한 자바 바이트 코드를 런타임에 바로 기계어로 변환하는 데 사용한다.

## 6. JVM 구성 요소
1. **자바 인터프리터(interpreter)**    
 : 자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 한다.
2. **클래스 로더(class loader)**    
 : 자바는 동적으로 클래스를 읽어오므로, 프로그램이 실행 중인 런타임에서야 모든 코드가 JVM과 연결된다. 이렇게 동적으로 클래스를 로딩해주는 역할을 하는것
3. **JIT컴파일러(Just-In-Time compiler)**   
 : 실행 중인 런타임에 실제 기계어로 변환해 주는 컴파일러를 의미한다.
 동적 번역(dynamic translation)이라고도 불리는 이 기법은 프로그램의 실행 속도를 향상시키기 위해 개발되었다.
 즉, JIT컴파일러는 자바 컴파일러가 생성한 자바 바이트 코드를 런타임에 바로 기계어로 변환하는데 사용된다.
4. **가비지 컬렉터(garbage collector)**   
 : 더는 사용하지 않는 메모리를 자동으로 회수해주는 기능을 한다.따라서 개발자가 따로 메모리를 관리하지 않아도 된다.

## 7. JDK와 JRE의 차이
자바 자바 개발 키트(JDK: Java Development Kit)는 프로그램 개발에 필요한 자바 가상 기계(JVM), 라이브러리 API, 컴파일러 등의 개발 도구가 포함되어 있다.
자바 실행 환경(JRE:Java Runtime Enviroment)는 프로그램 실행에 필요한 자바 가상 기계(JVM), 라이브러리 API만 포함되어 있다. 
자바 프로그램을 개발하고자 하는것이 아니고, 이미 개발된 프로그램만 실행한다면 JRE만 설치하면 된다.

* **JRE = JVM + 표준 클래스 라이브러리**
* **JDK = JRE + 개발에 필요한 도구**

