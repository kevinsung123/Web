### 웹로직(WebLogic Server)이란?
1. **웹로직서버**
	1.  오라클의 WebLogic Server
		- WAS제품의 한 종류로서, 제우스, 웹스피어처럼 상용되어 판매 모두 J2EE를 표준 채택
	2.  왜 웰로직?
		- WAS 는 산재해 있는 여러 애플리케이션과 리소르를 관리하는 하나의 창구
		- ex) top-down 방식의 중앙집중형 관리도구
2. **웹로직 용어정리**
	1. Domain
		- 도메인은 하나의 논리적인 관리단위
		- 같은 도메인내에서는 웹로직을 사용하기 위해 필요한 start, stop 스크립트 등이 모여있는 환경설정을 저장하는 하나의 **도메인 디렉터리(환경저장소)** 가 존재
		- 도메인내의 서버는 클러스터를 형성
	2. Configuration Repository
		- 도메인내에서 운영되고 있는 모든 서버들의 환경을 저장하는 장소로써 중앙 집중적인 관리를 가능
	3. Administartion Server
		- 도메인내에는 Configuration Repsoitory관리와 loggin를 담당하는 하나의 Administration Server가 존재하여 실제 관리는 내부의 MBean에의 해 수행
	4. Machine
	    - 물리적으로 분리된 장비
	5. Node Manager
		- 각 머신에 하나씩 존재하며, Managed Server를 관리
	6. Managed Server
	   - 실제 서비스를 제공하는 서버로서, 비지니스 로직을 포함하고 있는 어플리케이션과 컴포넌트들을 거의 대부분 Managed Server에 배포

3. **Server**
	1. Server란 ?
		- HW 상관없이 JVM상에서 실행되는 하나의 Weblogic server 인스턴스
	2.. Server 형태
		1. Admin Server
			 - 전체도메인을 관리하기 위한 서버
			 - Managed server를 설정/관리하기 위한 WebLogic Server 인스턴스
		2. Managed Server란
			- 도메인내에서 실제 서비스되는 애플리케이션이 실행되는 서버
			-  애플리케이션이 필요로하는 서비스나 리소스를 제공하는 서버
4. CLuster
	- WebLogic Cluster
		- 여러대의 웹 로직 서버 인스턴스를 묶어 부하분산과 장애 복구 기능을 제공하여 Client에게 중단 없는 서비스를 제공하는 기능
		- Cluster는 사용자에게 하나의 서버로 인식
		- Failover/Load Balancing을 위해서는 L4 또는 웹서버등 프록시 서버를 함꼐구성

	※  J2EE(Java 2 Enterprise Edition) : java로 기업환경에 애플리케이션을 만드는데 필요한 스펙들을 모아둔 스펙집합. 
		- Servlet : client가 보내는 HTTP요청을 처리하는 서버측 자바프로그램 Servlet엔진이 필요
		- JSP(Java Server Pages) : HTML이나 Java코드를 써서 사용자에게 정보를 보여줌. 처음 실행될때, Servlet엔진이 Servlet으로 컴파일 시켜서 내부적으로는 Servlet으로 동작
		-  EJB(Enterpris Java Beans) : java에서 제공하는 분산 컴포넌트 기술로 비즈니스 로직이나, 데이터, 메시지를 처리 
		- RMI(Remote Method Invocation) : 프록시를 써서 원격에 있는 Java 객체의 메소드를 실행시키기 위한 기술
		- JNDI(Java Naming Directory Interface) : Java기술로 만들어진 개체에 이름을 붙여 찾을 수 있도록 단일 interface 제공
		- JDBC(Java Database Connection) : 여러종류의 데이터베이스 시스템에 접근하는 단일 interface 제공 각각의 DB에 맞는 JDBC 드라이버가 있어야한다
		- JCA(Java Connector Architecture) : 이기종 플랫폼을 통합할 수 있도록 플랫폼 독립적인 interface 제공
		- JMS(Java Message Service) : 여러가지 메시징 시스템에 대한 플랫폼 독립적인 interface 제공
		
![enter image description here](https://t1.daumcdn.net/cfile/tistory/267BCB4556C84AE82E)


![enter image description here](https://t1.daumcdn.net/cfile/tistory/2275783E56C84AE919)

	
