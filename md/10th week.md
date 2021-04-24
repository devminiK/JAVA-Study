10주차 : 자바의 멀티쓰레드 프로그래밍에 대해 학습하세요.
=======

🎯 **목표** 
- Thread 클래스와 Runnable 인터페이스
- 쓰레드의 상태
- 쓰레드의 우선순위
- Main 쓰레드
- 동기화
- 데드락
-------------------------------------------------------------- 
## 1. Thread 클래스와 Runnable 인터페이스
### Thread
- *`프로세스`를 이루는 코드의 실행 흐름. 시작점과 종료점을 가짐
- 어떠한 프로그램내에서 특히 프로세스 내에서 실행되는 흐름의 단위.
- 한번 종료한 thread는 다시 시작시킬 수 없다(스레드 객체를 다시 생성해야함)
- run()메소드가 종료하면 스레드는 종료된다
- 한 스레드에서 다른 스레드를 강제 종료할 수 있다
- 실행 중에 멈출 수 있으며, 동시 수행 가능하다

> 운영체제에서 실행중인 하나의 어플리케이션을 프로세스라고 한다.   
> 운영체제로부터 실행에 필요한 메모리를 할당받아 어플리케이션의 코드를 실행한다.
   
### Thread 클래스
JDK에서 지원하는 java.lang.Thread을 제공한다. Thread클래스를 상속받아 사용한다.    
public void run()메소드를 오버라이드하고 start()메소드를 호출시켜 이용한다.

**Thread 이름 정의**
- 스레드는 각각 이름이 있다
- 디버깅할때 어떤 스레드가 에러를 발생하는지 조사할 목적으로 사용된다고 한다
- 이름을 부여하지 않은 스레드는 "Thread-n"이라는 이름으로 설정
- setName("이름")으로 이름 설정가능, getName()으로 이름 반환가능

**JAVA에서 Thread를 작성하는 2 가지 방법**
- [1] Thread 클래스를 상속 받아 확장
- [2] Runnable 인터페이스 구현하여 생성

**[1] Thread클래스 이용**
```java
class ThreadTest extends Thread{
	public void run(){
		//실행할 코드
	}
}
/* 실제 사용 예*/
ThreadTest = TT = new ThreadTest();
TT.start();

```
**예) 1~10 사이 랜덤 숫자 출력하기**
```java
/* Thread 상속 후 run오버라딩 클래스*/
import java.util.Random;

public class ThreadTest extends Thread {
	Random rand = new Random();
    public void run() {
        // 인터럽트 됬을때 예외처리
        try {
            for(int i = 0 ; i < 10 ; i++) {
                Thread.sleep(500);	 // 스레드 0.5초동안 대기
                System.out.println("Thread : " + (rand.nextInt(10)+1));
            }
        } catch(InterruptedException e) {
            System.out.println(e);
        }
    }
}
```
```java
/* Thread실행 클래스 */
public class ThreadRun {
	public static void main(String[] args) {
		ThreadTest th1 = new ThreadTest();
		ThreadTest th2 = new ThreadTest();
		
		/* 동시에 출력(한번에 2개의 숫자 씩) */
		th1.start();
		th2.start();
		
		/* 번갈아서 출력(한번에 1개의 숫자씩 )*/
//		th1.run();
//		th2.run();
	}
}
```
**[2] Runnable 인터페이스 구현하여 생성**   
현재의 클래스가 이미 다른 클래스로부터 상속 받고 있다면 Runnable 인터페이스를 이용하여 스레드를 생성가능.      
Runnable 인터페이스는 JDK 라이브러리 인터페이스이고 run()메소드만 정의되어 있다.
```java
/* Runnable구현 클래스 */
public class RunnableTest implements Runnable{
	Random rand = new Random();
    public void run() {
        // 인터럽트 됬을때 예외처리
        try {
            for(int i = 0 ; i < 10 ; i++) {
               
                Thread.sleep(500);	 // 스레드 0.5초동안 대기
                System.out.println("Thread : " + (rand.nextInt(10)+1));
            }
        } catch(InterruptedException e) {
            System.out.println(e);
        }
    }
}
```
```java
/* 실행 클래스 */
public class RunnableRun {
    public static void main(String args[]) {
        // Runnable 인터페이스 객체생성
        RunnableTest Obj1 = new RunnableTest();
        RunnableTest Obj2 = new RunnableTest();
        
        // Runnable 객체를 매개변수로 하여 스레드 객체 th생성
        Thread th1 = new Thread(Obj1);
        Thread th2 = new Thread(Obj2);
        
        th1.start();
        th2.start();
        
        th1.run();
        th2.run();
    }
}
```
   
## 2. 쓰레드의 상태
스레드의 상태는 JVM에 의해 기록관리 된다.   
**Thread의 상태 6가지**
1) **NEW**: 스레드가 생성되었지만, 스레드가 아직 실행할 준비가 되지 않음
2) **RUNNABLE**: 스레드가 실행되고 있거나, 실행준비되어 스케쥴링을 기다린느 상태
3) **WAITING**: 다른 스레드가 notify(), notifyAll()을 불러주기 기다리고 있는 상태(동기화)
4) **TIMED_WAITING**: 스레드가 sleep(n)호출로 인해 n밀리초동안 잠을 자고 있는 상태
5) **BLOCK**: 스레드가 I/O작업을 요청하면 자동으로 스레드를 BLOCK상태로 만듦
6) **TERMINATED**: 스레드가 종료한 상태
   
## 3. 쓰레드의 우선순위
2개 이상의 스레드가 동작 중일 때 우선 순위를 부여하여 우선 순위가 높은 스레드에게 실행의 우선권을 부여할 수 있다.

**우선 순위를 지정하기 위한 상수 제공**
- **static final int MAX_PRIORITY** : 우선순위 10 - 가장 높은 우선 순위
- **static final int MIN_PRIORITY**  :  우선순위 1 - 가장 낮은 우선 순위
- **static final int NORM_PRIORITY** : 우선순위 5 - 보통의 우선 순위

**특징**
- **스레드 우선 순위는 변경 가능함**
	- void setPriority(int priority)
	- int getPriority()
- **main()스레드의 우선 순위 값은 초기값이 5이다**
- **JVM의 스케쥴링 규칙에 따른다**
	- 철저한 우선 순위 기반이다
	- 가장 높은 우선 순위의 스레드가 우선적으로 스케쥴링
	- 동일한 우선 순위의 스레드는 돌아가면서 스케쥴링(라운드 로빈)
```java
/* 스레드 상속 클래스 */
import java.util.Random;

public class PriorityTest extends Thread {
	// 출력 생성자
	PriorityTest(String str) {
		super(str);
	}
	
	Random rand = new Random();
    public void run() {
        // 인터럽트 됬을때 예외처리
        try {
            for(int i = 0 ; i < 10 ; i++) {   
                Thread.sleep(500);	 // 스레드 0.5초동안 대기
                System.out.println(getName()  +": " + (rand.nextInt(10)+1));
            }
        } catch(InterruptedException e) {
            System.out.println(e);
        }
    }
}
```
```java
/* 실행 클래스 */
public class PriorityRun {
    public static void main(String args[]) {
        // Runnable 인터페이스 객체생성
        PriorityTest th1 = new PriorityTest("우선순위 MAX-Thread");
        PriorityTest th2 = new PriorityTest("우선순위 MIN-Thread");
        
        th1.setPriority(Thread.MAX_PRIORITY);	//최대 우선순위 지정
        th2.setPriority(Thread.MIN_PRIORITY);	//최소 우선순위 지정
      
        System.out.println("th1의 우선순위= "+ th1.getPriority());
        System.out.println("th2의 우선순위= "+ th2.getPriority());
        
        th1.start();
		th2.start();

//        th1.run();
//        th2.run();
    }
}
```
   
## 4. Main 쓰레드
자바 어플리케이션은 메인스레드가 main메소드를 실행하면서 시작한다.
   
## 5. 동기화(Synchronization)
스레드는 자원을 공유하기 때문에, 동시에 여러 스레드가 실행되면 공유자원에 대해 동기화가 발생할 수 있다.
실행 흐름을 순차적으로 진행되지만, 자원은 한정적인데 여러 스레드가 동시 접근해서 사용하려고 하면 문제가 발생하게 된다.
공유 메모리에 대해서 한번에 한 스레드만 공유자원에 접근할 수 있도록 만들어 동기화 문제를 해결할 수 있다.
- **임계영역(critical section)**   
    멀티 스레드 프로그램에서 단 하나의 스레드만 실행할 수 있는 코드 영역    
자바는 임계영역을 지정하기 위해 `동기화 메소드`와 `동기화 블록`을 제공한다.
동기화 메소드 만드는 방법은 메소드 선언에 `synchronized` 키워드를 붙이면 된다.   
(인스턴스, 정적 메소드 어디든 가능)
```java
public synchronized void method(){
	임계 영역 //단 하나의 스레드만 실행
}
```
동기화 메소드는 전체 내용이 임계영역이므로  해당 메소드 실행 죽시 객체 잠금이 일어나고, 종료시 잠금이 해제된다.
메소드 전체 내용이 아니라, 일부 내용만 임계 영역으로 만들고 싶다면 동기화 블록을 이용한다.
```java
public void method() {
	//여러 스레드 실행 가능 영역
	synchronized(공유 객체){
		임계 영역//단 하나의 스레드만 실행
	}
	//여러 스레드 실행 가능 영역
}
```
   
## 6. 데드락(DeadLock)
두 개 이상의 작업이 하나씩 자원을 소지(hold)하고 있으면서 상대방이 가진 자원을 서로 원하고(need) 있는 상태.   
어떤 작업도 실행되지 못하고 계속 서로 상대방의 작업이 끝나기만 바라는 무한정 대기하는 비정상적 상태이다.   
하나의 쓰레드를 끊어주거나, 모두 다 깨워주는 방법으로 교착상태를 해결한다.

### DeakLock 발생 조건
- **상호 배제 (Mutual Exclusion)** : 한 자원에 대해 여러 쓰레드 동시 접근 불가
- **점유와 대기 (Hold and Wait)** : 자원을 가지고 있는 상태에서 다른 쓰레드가 사용하고 있는 자원 반납을 기다리는 것
- **비선점 (Non Preemptive)** : 다른 쓰레드의 자원을 실행 중간에 강제로 가져올 수 없음
- **환형대기 (Circle Wait)** : 각 쓰레드가 순환적으로 다음 쓰레드가 요구하는 자원을 가지고 있는 것
