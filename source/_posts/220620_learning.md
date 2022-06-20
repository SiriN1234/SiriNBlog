---
title: "2022.06.20 수업내용과 실습"
author: "Hanjeongin"
date: 2022-06-20 18:09:00
tags: Java
---

## 지난 주 복습

- JDK(Java Development Kit)
- JRE(Java Runtime Environment)
- JVM(Java Virtual Machine)
- JAVAC(Java Compiler)



## 환경변수 설정

운영체제에서 JDK, JRE, CLASSPATH등의 경로를 참조하기 위해서 설정한다.
참조하지 않는다면 실행할 때 마다 그 파일이 있는 풀 경로를 적어줘야 되기 때문에 귀찮고 실수하기 쉽다.



## 변수

#### Variable : 변할 수 있는 수(자료)

```
데이터타입 변수명; // 선언. declare
데이터타입 변수명 = 값; // 초기화. Initialize
```

1. 값을 반복하여 사용할 때
2. 값에 이름을 붙여줄 때



## 자료형

#### DataType

- 원시형 데이터 타입(8개). call by value
  - 논리형 : boolean
  - 문자형 : char
  - 정수형 : byte, short, int, long
  - 실수형 : float, double

- 참조형 데이터 타입. call by reference
  - 원시형 데이터 타입을 제외한 모든 데이터 타입
  - new 생성자로 호출한다.



## 연산자

사칙연산(+-*/), %(나머지), **(제곱)
++, --
+= -=, *=, /=
||, &&



## 조건문

```
if(조건1) {
    조건1이 만족할 시 실행할 문장;
} else if(조건2) {
    조건1이 만족하지 않고 조건2가 만족할 때 실행할 문장;
} else if(조건3) {
    조건1, 조건2가 만족하지 않고 조건3가 만족할 때 실행할 문장;
} else {
    위의 모든 조건에 만족하지 않을 때 실행할 문장;
}
```



## 반복문

- for문

```
for(자료형 이름; 조건; 본문을 실행한 후 실행할 스텝) {
    본문;
}
```

- while문

```
while(조건) {
    조건을 만족하는 동안 반복해서 실행할 문장;
}
```

- do-while문

while과 같은데 실행을 먼저하고 조건 검사.

- 향상된 for문

```
for(자료형 이름 : 이터레이터) {
    모든 요소에 대해 실행할 문장;
}
```



## 배열

같은 자료형이 순서대로 늘어선 자료구조
인덱스는 0부터 시작한다.
길이(크기)는 초기화 된 것에서 바꿀 수 없다.
들어있는 요소의 크기가 아니라 초기화 된 크기가 저장된다.



## 메소드

특정 역할을 하는 코드를 반복해서 사용(호출) 할 때 사용

```
접근제한자 리턴타입 메소드명(파라미터타입 파라미터명, ...) {
    본문;
    return 값;
}
```



## 객체

현실에서 프로그래밍으로 옮길 대상
자바에서는 '클래스'라는 설계도가 객체를 만드는 것이다.
설계도대로 실제 물리적 객체를 구현한 것이 '인스턴스'다.
인스턴스가 되어야 비로소 메모리에 올라가고, 그 안의 변수와 메소드를 사용할 수 있다.
new 생성자(매개변수)를 통해서 인스턴스를 만든다.



## 생성자

메소드와 형태가 거의 비슷한데 리턴타입이 없고 이름이 클래스명과 같다.

- 목적
  - 인스턴스화 하기 위해 필요한 데이터를 명시
  - 데이터가 준비되지 않았다면 인스턴스화 할 수 없게 한다.



## 상속

부모의 클래스에 있는 변수와 메소드를 그대로 물려받아서 자식 클래스에는 소스가 없더라도 사용할 수 있게 되는 것.

```
자식클래스 extends 부모클래스
```



## 추상클래스

abstract 메소드가 하나 이상 있는 클래스
클래스 앞에도 abstract 키워드를 붙여줘야 한다.
추상클래스로는 인스턴스를 만들 수 없다.
자식 클래스에 공통적으로 갖고있는 코드를 중복하지 않기 위해 부모에 묶은 개념이다.



## 인터페이스

어떤 함수가 있어야 하는지에 대한 악속.
리턴타입, 함수이름, 파라미터 타입, 파라미터 갯수를 약속 해놓고 구현체에서는 반드시 그 함수들을 오버라이드 해줘야 한다.

```
구현클래스명 implements 인터페이스
```



## 접근제한자

변수나 메소드를 해당 클래스 외부에 어느 정도로 노출 할 것인지에 대한 키워드

- private : 해당 클래스에서만
- default : 같은 패키지 내에서만
- protected : default + 상속받은 클래스
- public : 프로젝트 내 어디서나



## 오버라이드, 오버로드

override : 자식에서 덮어쓰는 것
overload : 파라미터가 다를 경우 다른 함수같이 작동되는 것



## 예외처리

프로그래머가 미리 예측하여 막을 수 있는 에러를 미리 처리 해놓는 것

```
try {
    예외가 발생 할 수도 있는 문장;
} catch(예외클래스 클래스명) {
    예외처리;
} finally {
    예외를 처리하든 못하든 반드시 처리 할 문장;
}
```



## Object 클래스

모든 객체의 최종 조상
toString, equals, clone 등의 함수가 있다.



## 상수, 

#### final 키워드

- final에 초기값을 줄수있는 경우는 2가지 뿐이다.
- 필드선언시, 생성자
- 초기화되지 않은 final 필드가 남아있으면 컴파일 에러
- 진짜 상수는 static final로 선언. 객체마다 저장할 필요가 없으며 다른값으로 바꿀수도 없기 때문.
- 명명규칙은 대문자에 언더바 조합



## enum

#### enum은 열거형(Enumeration). 상수의 그룹을 나타낸다.

주목적 : 우리만의 데이터 타입을 가지기 위해서
- 모든 enum들은 내부적으로 java.lang.Enum 클래스에 의해 상속된다.
- 만드는 문법  : enum 열거체이름 { 상수1이름, 상수2이름, ... }
        예) enum Rainbow { RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET }
- 사용문법 : 열거체이름.상수이름
        예) Rainbow.RED
- 메소드
  - values()	모든 상수 배열로 반환   예) Rainbow[] arr = Rainbow.values();
  - ordinal()	상수 인덱스 반환.   예)  int idx = Rainbow.YELLOW.ordinal();
  - valueOf()	상수 문자 값 반환   예) Rainbow rb = Rainbow.valueOf("GREEN");



## 쓰레드 thread

- 프로그램 : 어떤 작업을 위해 실행할 수 있는 파일
- 프로세스 : 실행되고 있는 프로그램.   실행되기 위해서는 메모리에 올라와야 한다. 프로세스는 운영체제로부터 자원을 할당받는다. 1개의 프로세스는 최소 1개의 스레드를 가지고있다.
  예) 크롬을 켜서 유튜브를 보고있는동시에 마우스를 움직이는 동시에 파일은 다운받아지고있다.
  예) 햄버거를 시키면 그릴에서 고기굽고, 음료수는 기계가, 감자튀김은 튀김기
- 쓰레드 : 하나의 프로세스내에서 돌아가는 병행적인 메소드
  쓰레드는 프로세스내에서 Stack영역만 따로 할당받고 Code, Data, Heap영역은 공유한다.  즉, 프로세스내의 여러자원들을 공유하면서 실행된다.
- 모든 자바어플리케이션은 Main Thread가 main()메소드를 실행하면서 시작한다.
- 멀티 쓰레드의 장점 : 시스템자원의 효율성 증대, 처리량 증가, 응답시간 단축, 통신의 부담 감소
- 멀티 쓰레드의 단점 : 주의깊은 설계, 까다로운 디버깅, 동기화문제, 하나의 쓰레드가 잘못되어도 프로그램이 멈춤
- 멀티 프로세스 대신 멀티쓰레드를 사용하는 이유
  위에서 말한 쓰레드의 장점.
- 생성방법 2가지
  1. Thread상속  (java.lang.Thread)
  2. Runnable 인터페이스를 구현
- 대표적인 메소드
  sleep, start, join, run 이 있다.
  호출시 실행하는 메소드는 **start**해주지만 쓰레드가 실제 돌아갈때는 오버라이드하는 run이 돌아간다.
  run을 통해 실행시키면 stack으로 쌓여서 병행처리가 안된다. 해당메소드가 종료되어야 다른 스레드가 실행된다.
  start를 통해 실행해야 정상적인 병행처리가 된다.
  쓰레드를 start하면 바로 실행하는것이 아니라 실행대기상태에 있따가 OS의 스케쥴러가 작성한 스케줄에 의해 순서대로 자기 차례에 실행되는 구조다.
- 쓰레드의 상태 6가지. getState()를 하면 알 수 있다.
  - NEW
  - RUNNABLE
  - BLOCKED
  - WAITING
  - TIMED_WAITING
  - TERMINATED
- 쓰레드의 종료
쓰레드는 자신의 run메소드가 모두 실행되면 자동적으로 종료된다.
그런데 run메소드의 끝까지 가기전에 '즉시 종료' 해야한다면 boolean으로 된 플래그를 사용하거나 interrupt()메소드를 이용하는 방법이 있다.
interrupt() 메소드는 InterruptedException 예외를 발생시키는데, 주의해야할점은 interrupt메소드를 이용하기위해서는 종료시키고 싶은 메소드가 일시정지상태일때 정지된다는것이다.
돌아가고있는 중에서는 정지가 안된다.

- 예제
```java
import java.text.SimpleDateFormat;
import java.util.Date;

public class ThreadTest {

    public static void main(String[] args) {
        Runnable d = new CheckThread("손흥민1");
        Runnable d1 = new CheckThread("이강인2");
        Runnable d2 = new CheckThread("황의조3");
        Runnable d3 = new CheckThread("황희찬4");

        Thread thread = new Thread(d);
        Thread thread1 = new Thread(d1);
        Thread thread2 = new Thread(d2);
        Thread thread3 = new Thread(d3);

        thread.start();
        thread1.start();
        thread2.start();
        thread3.start();

    }

}

class CheckThread implements Runnable{

    String name;

    CheckThread(String name){
        this.name = name;
    }

    @Override
    public void run() {
        try{

            for(int i=0;i<5;i++){
                System.out.println(name);
                Thread.sleep(1000);
            }

        }catch (Exception e){
            System.out.println(e.toString());
        }

        System.out.println("쓰레드 종료 : "+ name);
    }
}
```

: IllegalThreadStateException 에러
=> 하나의 Thread객체에 start()를 두번이상 호출할경우 발생한다.
기존에 생성된 쓰레드를 종료시키고 새로운 인스턴스를 만들어서 붙도록 하면된다.
:java.lang.InterruptedException: sleep interrupted
=> 일시 정지 상태에서 주어진 시간이 되기전에 interrupt() 메소드가 호출되면 InterruptedException이 발생하기 때문에 예외 처리가 필요하다.
interrupt함수는 종료시키는것이 아니라 예외를 발생시키는 함수. 정상종료의 표준이다.
유의할점은 즉시 예외가 발생하는것이 아니라 미래에 일시정지상태가 됐을때 발생한다는점이다.

: java.lang.InterruptedException: sleep interrupted 에러
=> 쓰레드를 외부에서 인터럽트 시켜서 interrupted=true일때 sleep명령어를 받은것
해결법 : try안에 sleep넣고 catch에 스레드 종료 유발

: 정확히 동시에 실행하는법
https://stackoverflow.com/questions/3376586/how-to-start-two-threads-at-exactly-the-same-time