목표
=======
자바가 제공하는 제어문을 학습하세요.

학습할 것(필수)
=======
- 선택문   
- 반복문

과제 (옵션)
=======
- 과제 0. JUnit 5 학습하세요.   
- 과제 1. live-study 대시 보드를 만드는 코드를 작성하세요.
- 과제 2. LinkedList를 구현하세요.
- 과제 3. Stack을 구현하세요.
- 과제 4. 앞서 만든 ListNode를 사용해서 Stack을 구현하세요.
- 과제 5. Queue를 구현하세요.
--------------------------------------------------------------
week4: 제어문
=======
## 1. 선택문 
조건을 걸고 해당 조건에만 실행되는 로직이 있는 경우 사용한다.

### if else문
조건문을 하나 더 추가하는 경우는 `else if`를 사용할 수 있다.
```java
if (조건문1) {
   //조건문1과 부합할 경우 실행
} else if (조건문2){
   //조건문2과 부합하지 않을 경우 실행
} else {
   //조건문1,2과 부합하지 않을 경우 실행
}
```
### switch case문
switch문에서는 변수나 조건문의 결과가 case문에서 지정한 값과 같은지 동등비교만 가능하다.    
break키워드는 로직 실행을 종료할 수 있다. 로직은 switch문이 종료되거나 break키워드를 만날때 까지 실행된다.

```java
switch(변수or조건문) {
   case 값1:
      //값1과 부합할 경우 실행;
      break;
   case 값2:
      //값2과 부합할 경우 실행;
      break;
   default:
      //값1,2모두 부합하지 않을경우 실행;
      breka;
}
```
## 2. 반복문
로직을 조건문에 맞게 반복해주는 기능을 한다.

### for문
statement 1 : 초기화식, 제일 먼저 로직이 실행되기 전 한 번 실행.    
statement 2 : 조건식, 로직 실행되는 조건.   
statement 3 : 증감식, 로직이 실행되고 매번 실행된다.   
```java
for(statement1 ; statement2 ; statement 3) {
  // 실행할 로직
}
```
### while문
조건문 판단 후 조건문이 true인 경우 로직을 실행한다.
```java
while(조건문) {
  // 실행할 로직
}
```
### do-while문
로직을 먼저 실행 후 조건문을 판단 한다. 조건문이 true인 경우 do 블럭을 실행한다.    
따라서 조건문과 상관없이 로직은 `반드시 한 번 이상 실행`된다.
```java
do{
  // 실행할 로직
} while(조건문);
```
### for each문
배열을 인덱스가 아닌 `요소`로 접근가능하다.
```java
for(타입 변수명: 배열) {
  // 실행할 로직
}
```
* * *
## 과제
### 과제 0. JUnit 5 학습하세요.
- 인텔리J, 이클립스, VS Code에서 JUnit 5로 테스트 코드 작성하는 방법에 익숙해 질 것.
- 이미 JUnit 알고 계신분들은 다른 것 아무거나!
- 더 자바, 테스트 강의도 있으니 참고하세요~
   
### 과제 1. live-study 대시 보드를 만드는 코드를 작성하세요.
- 깃헙 이슈 1번부터 18번까지 댓글을 순회하며 댓글을 남긴 사용자를 체크 할 것.
- 참여율을 계산하세요. 총 18회에 중에 몇 %를 참여했는지 소숫점 두자리가지 보여줄 것.
- Github 자바 라이브러리를 사용하면 편리합니다.
- 깃헙 API를 익명으로 호출하는데 제한이 있기 때문에 본인의 깃헙 프로젝트에 이슈를 만들고 테스트를 하시면 더 자주 테스트할 수 있습니다.
   
### 과제 2. LinkedList를 구현하세요.
- LinkedList에 대해 공부하세요.
- 정수를 저장하는 ListNode 클래스를 구현하세요.
- ListNode add(ListNode head, ListNode nodeToAdd, int position)를 구현하세요.
- ListNode remove(ListNode head, int positionToRemove)를 구현하세요.
- boolean contains(ListNode head, ListNode nodeTocheck)를 구현하세요.
   
### 과제 3. Stack을 구현하세요.
- int 배열을 사용해서 정수를 저장하는 Stack을 구현하세요.
- void push(int data)를 구현하세요.
- int pop()을 구현하세요.   
   
### 과제 4. 앞서 만든 ListNode를 사용해서 Stack을 구현하세요.
- ListNode head를 가지고 있는 ListNodeStack 클래스를 구현하세요.
- void push(int data)를 구현하세요.
- int pop()을 구현하세요.   
   
### 과제 5. Queue를 구현하세요.
- 배열을 사용해서 한번
- ListNode를 사용해서 한번.
