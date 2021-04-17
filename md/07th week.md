7주차 : 자바의 패키지에 대해 학습하세요.
=======

🎯 **목표** 
- package 키워드
- import 키워드
- 클래스패스
- CLASSPATH 환경변수
- -classpath 옵션
- 접근지시자
-------------------------------------------------------------- 
# 1. package 키워드
패키지는 비슷한 성격의 자바 클래스들을 모아 넣은 자바의 디렉토리이다.
```java
package house.kim;

public class MinKim {
}
```
```java
package house.kim;

public class OakKim {
}
```
   
## 2. import 키워드
한 패키지에서 다른 패키지의 클래스를 사용하려면 import키워드를 사용하여 포함시켜주어야한다.
또는, `*` 기호를 이용할 수도 있다.
```java
import house.kim.MinKim;
import house.*;
```
   
## 3. 클래스패스
## 4. CLASSPATH 환경변수
## 5. -classpath 옵션
## 6. 접근지시자
