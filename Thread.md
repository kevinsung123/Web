### Thread
- WEB 장애가 발생했을 떄나 WAS가 느리게 동작할 떄, Thread Dump를 분석해야한다
	- 스레드 덤프 획득방법
	- 스레드 덤프 해석 방법 
#### Java 그리도 스레드
- 웹서버에서는 많은 수 의 동시 사용자를 처리하기 위해 수십-수백개 정도의 스레드를 사용
-  두개 이사의 스레드가 같은 자원을 이용할 대는 필연적으로 스레드간에 경합(Contention)이 발생 경우에 따라서 데드락(DeadLock)발생
- **경합(Contention)** : 어떤 스레드가 다른 스레드가 획득하고 있는 lock이 해제되기를 기다리는 상태
- WAS에서 여러 스데르가 공유자원에 접근하는 일은 매우 빈번 
- ``` ex) 로그 기록하는 것도 로그를 기록하는 스레드가 락을 획득하고 공유자원에 접근 ```
- **데드락(Deadlock)** : 경합의 특별한경우
	- 두 개 이상의 스레드에서 작업을 완료하기 위해 상대의 작업이 끝나야하는 상황
	- 스레드 경합때문에 다양한 문제 발생가능
	- 이 문제를 분석하기 위해 스레드 덤프(Thread Dump)이용
#### Java 스레드 배경지식
##### 스레드 동기화
- 스레드는 다른 스레드와 동시 실행가능
- 정합성을 보장하려면 스레드로 동기화로 한번에 하나의 스데르만 공유자원에 접근해야함
- Java에서는 Monitor를 이요해 스레드를 동기화
-모든 Java객체는 하나의 Monitor를 가지고 있음
- Monitor는 하나의 스레드만 소유 가능 
- 어떤 스레드가 소유한 Monitor를 다른 스레드가 획득하려면 해당 Monitor를 쇼유하고 있는 스레드가 Monitor를 해제할때 까지 wait Queue에서 대기 
-
```
※ 모니터
어떤 공유 데이터에 대해 모니터를 지정해놓으면, 프로세스는 그 데이터를 접근하기 위해 모니터에 들어가야만 한다. 즉, 모니터 내부에 들어간 프로세스에게만 공유 데이터를 접근할 수 있는 기능을 제공하는 것이다. 또한 프로세스가 모니터에 들어가고자 할 때 다른 프로세스가 모니터 내부에 있다면 입장 큐에서 기다려야 한다.
```

##### 스레드 상태
- 스레드의 상태는 java.lang.Thread 클래스 내부에 State라는 이ㅇ름을 가진 Enumberated Types(열거형) 선언
-![enter image description here](https://d2.naver.com/content/images/2015/06/helloworld-10963-1.png)
- NEW : 스레드가 생성되었지만, 아직 실행되지 않은 상태
- RUNNABLE : 현재 CPU를 점유하고 작업을 수행 중인 상태. OS의 자원분배로 WAITING상태가 될 수 도 있다
- BLOCKED : Monitor를 획득하기 위해 다른 스레드가 lock를 해제하기를 기다리는 상태
- WAITING : wait()메서드, join()메서드, park()메서드를 이용해 대기하고 있는 상태
- TIMED_WAITING : sleep()메서드, wait()메서드, join()메서드 , park()메서드 등을 이용해 대기하고 있는 상태 WAITING 상태와 차이점은 메서드의 인수로 최대 대기 시간을 명시할 수 있어 외부적인 변화 뿐만 아니라 시간에 의해서도 WAITING상태가 해제 가능

#### 스레드의 종류
- 데몬 스레드(Daemon Thread)
	- 데몬 스레드는 다른 비데몬 스레드가 없다면 동작을 중지
	- 사용자가 직접 스레드를 생성하지 않더라도 , Java앱에서 기본적으로 여러개의 스레드를 생성 
	- 대부분이 데몬스레드인데 가비지컬렉션(GC)나, JMX등의 작업을 처리하기 위한 것 
	- static void main(String p] args) 메서드가 실행되는 스레드는 비데몬 스레드로 생성되고, 이 스레드가 동작을 중지하면 다른 데몬 스레드도 같이 동작을 중지하게 되는것 
	- 이 main 메서드가 실행되는 스레드를 HotSpot VM에서는 VM Thread라고 부름
#### 스레드 덤프회득
- 스레드의 상태변화를 확인하려면 5초정의 간격으로 5-10회정도 획득하는 것이 좋다
1.	j**stack을 이용하는 방법**
- JDK1.6이상을 사용한다면 Windows에서도 jstack을 이용해 스레드 덤프를 획득 가능
- 먼저 수행중인 Java애플리케이션의 PID를 확인 
```
# 1. java앱의 pid확인
$jps -v 

# 2. jps로 추출한 pid를 인수로 넣어 jstack을 실행하면 스레드 덤프 획득가능
$jstack [pid] > [file]
```

▶ Jstack?
- SYNOPIS
	- jstack [option] pid
	- jstack [option] executable core
	- jstack [option] [server-id]@remote-hostname or IP
- PARAMETER
	- pid : 프로세스id (stack trace가출력될) . 반드시 자바프로세스 (jps사용)
	- executable : core dump가 생성될 java executable 
	- core : stack trace가 출력될 core file
- DESCRIPTION
	- jstack을 Java 스레드들의 Java stack trace들을 출력(Java 프로세스 또는 core file 또는 remote debug server)
	- Java Frame, class name, method name, bci(byte code index), line number 출력
	- NOTE
		- JDK future 버전 지원 x
		- window에서는 dbgeng.dll 설치되어있어야함
		- PATH에 jvm.dll이 포함되어야함
		- PATH=<jdk>\jre\bin\client
[jstack Java SE Doc]
(https://docs.oracle.com/javase/7/docs/technotes/tools/share/jstack.html)

2.  Java Visual VM 이용방법
3.  Kill 방법
- linux환경에서는 kill명령어로 획득가능
- 인수로 사용할 PID 
	> $ps -ef | grep java
 - kill명렁의 인수인 -3(-QUIT | -SIGOUT)함꼐 사용하여 스레드 덤프를 회득
 > $kill -3 [pid]
 
 ▶kill옵션
- kill -9(KILL) : 하드웨어적 종료(가장강력)
- kill -15(TERM)  : 소프트웨어적 종료
- kill -1(HUP) : 데몬의 경우 종료 후, 다시 시작(프로세스 종료가 아님 코드 및 데이터 refresh 역할)
- kill -2 : foreground에서 ctrl+c<작업취소>
- kill -3 : foreground에서 ctrl+|
- signal종류 별로 signal hanlder를 지정할 수 있느네 유일하게 지정할 수 없는 시그널은 SIGKILL(9), SIGSTOP(19) 두개의 시그널 
- 정상적으로 작성된 프로그램이면 종료 의미로 사용하는 signal(INT,HUP,TERM)등 받으면 resource를 정리하는 cleanup코드를 실행하고 종료
- 스크립트
	- $ps -eaf [process] | grep -v grep | awk '{print "kill -TERM"$2}' | sh -x
	- 
- 참고
	 - [kill 옵션 ](https://bigsun84.tistory.com/355)


#### 스레드 덤프 정보
```
1)pool-1-thread-13
2)prio=6 
3)tid=0x000000000729a000 nid=0x2fb4 
4)runnable [0x0000000007f0f000]
5)java.lang.Thread.State: RUNNABLE
```
1. 스레드이름 
	- 스레드의 고유이름
	- java.lang.Thread클래스 이용해 스레드 생성시, Thread(Number)형식으로 이름 생성
2. 우선순위 : 스레드의 우선순위
3. 스레드ID : 스레드ID. 해당 정보를 이용해 스레드의 CPU사용, 메모리사용 등 유용한 정보 얻을 수 있음
4. 스레드 상태 : 스레드의 상태
5. 스레드 콜스택 : Call Stack 정보

#### 스레드 덤프 유형별 패턴
- BLOCKED : lock을 획득하지 못 하는 경우 
	- 한 스레드가 락을 소유하고 있어 다른 스레드가 락을 획득하지 못해 애플리케이션의 전체적인 성능이 느려짐
```
다음 스레드 덤프 예에서는 BLOCKED_TEST pool-1-thread-1 스레드가 <0x0000000780a000b0> 락을 소유한 상태에서 동작하고 있어, BLOCKED_TEST pool-1-thread-2 스레드와 BLOCKED_TEST pool-1-thread-3 스레드는 <0x0000000780a000b0> 락을 획득하기 위해 대기하고 있는 상태이다.
```

![enter image description here](https://d2.naver.com/content/images/2015/06/helloworld-10963-3.png)
- 데드락인경우
	-  스레드A가 작업을 계속하려면 스레드B가 소유한 락을 획득해야하고, 스레드가 B가 작업을 계속하려면 스레드A가 사용한 락을 획득해야 해야해서 Deadlock발생
```
다음 스레드 덤프 예에서 DEADLOCK_TEST-1 스레드는 <0x00000007d58f5e48> 락을 소유하고 있으며, DEADLOCK_TEST-2 스레드가 소유한 <0x00000007d58f5e60> 락을 획득하려 한다. 한편 DEADLOCK_TEST-2 스레드는 <0x00000007d58f5e60> 락을 소유하고 있으며, DEADLOCK_TEST-3 스레드가 소유한 <0x00000007d58f5e78> 락을 획득하려 한다. 그리고 DEADLOCK_TEST-3 스레드는 <0x00000007d58f5e78> 락을 수유하고 있으며, DEADLOCK_TEST-1 스레드가 소유한 <0x00000007d58f5e48> 락을 획득하려 한다.
```
![enter image description here](https://d2.naver.com/content/images/2015/06/helloworld-10963-4.png)
	- 위와 같이 상대가 소유하고 있는 락을 획득하기 위해 대기하기 때문에 한 스레드가 자신의 락을 해제하기 전 까지 데으락 사태가 해제되지않음!
	- lock? 내가 리소스를 사용하고 있는 동안에는 문을 걸어잠궈나서 아무도 들어오지 못하게 하는것 
	- [락개념참고](https://jhnyang.tistory.com/36)

- 원격서버로부터 수신을 받기 위해 계속 대기하는 경우
```
다음 스레드 덤프 예에서는 스레드가 계속 RUNNABLE 상태에 있어 문제가 될 만한 부분이 없는 것처럼 보인다. 하지만 스레드 덤프를 시간순으로 나열하면, socketReadThread 스레드가 계속 소켓을 읽으려 무한정으로 대기하고 있는 상태이다.
```
#### 스레드 덤프를 이요한 문제해결
1. 상황 : CPU사용률이 비정상적으로 높을떄
- 먼저 다음과 같이 CPU를 가장 많이 점유하는 스레드가 무엇인지 추출한다.
- 추출한 결과에서 CPU를 가장 많이 사용하는 LWP(light weight process)와 고유 번호를 확인한다. 고유 번호를 16진수로 변환하면 NID를 얻을 수 있다. 다음 예에서 CPU를 가장 많이 사용하는 LWP의 고유 번호는 10039이고, 이 번호를 16진수 변환한 숫자는 0x2737이다.
```

[user@linux ~]$ ps -mo pid,lwp,stime,time,cpu -C java
PID LWP STIME TIME %CPU

10029 - Dec07 00:02:02 99.5

- 10039 Dec07 00:00:00 0.1

- 10040 Dec07 00:00:00 95.5
```
-  그 다음으로 스레드 덤프를 얻어 스레드의 동작을 확인한다. 다음은 PID가 10029인 애플리케이션의 스레드 덤프이다 .이 스레드 덤프에서 NID가 0x2737인 스레드를 찾아 스레드의 동작을 확인한다.
2. 수행 성능이 비정상적으로 느릴때
- 애플리케이션의 수행 성능이 비정상적으로 느릴 때에는 BLOCKED 상태인 스레드가 원인인 경우가 많다. 이 때에는 스레드 덤프를 여러 번 얻은 다음 BLOCKED 상태인 스레드 목록을 찾고 BLOCKED 상태인 스레드가 획득하려는 락과 관계된 스레드를 추출해 본다.
- 아래 스레드 덤프 예에서는 **<0xe0375410>** 락을 획득하지 못해 계속 BLCOKED 상태에 있다. 해당 락을 획득하고 있는 스레드의 스택 트레이스(Stack Trace)를 분석하면 문제를 해결할 수 있다.
-  위의 패턴은 DBMS다루는 애플리케이션에서 자주 나타남
	- 원인 
		-  첫쨰,스레드가 계속 동작하고 있지만 DBCP 등의 설정이 적절하지 못해 충분한 성능을 내지 못하는 경우이다. 이 경우에는 스레드 덤프를 여러 번 얻어서 비교하면 BLOCKED 상태에 있던 스레드 중 몇 개는 다른 상태인 경우가 많을 것이다.
		- 둘째, DBMS와 연결이 비정상적인 상태라 계속 타임아웃(timeout) 시간까지 대기하는 경우이다. 이 경우에는 스레드 덤프를 여러 번 추출해 비교해도 DBMS와 관련된 스레드는 계속해서 BLOCKED 상태에 있는 것을 확인할 수 있다. 타임아웃 값 등을 적절하게 변경해서 문제 발생 시간을 줄일 수 있다.

- 출처 
	- [Dzone Performance Zone](https://dzone.com/articles/how-analyze-java-thread-dumps)
	- [NAVER D2](https://d2.naver.com/helloworld/10963)
