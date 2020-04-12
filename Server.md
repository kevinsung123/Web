### WebServer
- 클라이언트 요청을 받아 HTMl이나 오브젝트를 HTTP프로토콜을 이용해 전송
- 클라이언트에서 요청이 올때 **가장앞에서**요청에 대한 처리
- 사용자가 클라이언트로 요청을 보내면 그 명령에대한 처리를 실행하고 다시 사용자에게 답변
- 사용자가 요청한 것들 중에 웹서버 자체적으로 처리할 수 없는것들은 tomcat과 같은 WAS 서버에 넘겨 처리 결과를 받아와서 넘겨줌
-  웹서버만 구축된 서버는 웹페이지,이미지 정적인페이지만 처리, JSP 컨데이너가 포홤된 WAS는 JSP페이지를 컴파일해 동적인 페이지 생성
- 웹서버는 웹문서를 WAS는 JSP페이지등을 양분하여 서버 부담줄임
- Apache, IIS,WebtoB, Nginx

### WAS(Web Application Server)
- 웹서버 + 웹컨테이너
- 웹상에서 사용하는 컴포넌트들을 올련호고 사용하게되는 서버
- EJB와 같은 빈들이 올라가게 되면 서버에 따라 필요한 많은기능 포함
- BEA사의 Web Logic, IBM사의 Web Sphere , TMAX사의 Jeus, Tomcat..
- 동작 프로세스
	1. 웹서버로부터 요청이 오면 컨테이너가 받아서 처리
	2. 컨테이너는 web.xml을 참조하여 해당 서블릿에 대한 쓰레드 생성하고 httpServletRequest와 httpServletResponse 객체를 생성하여 전달한다.
	3. 컨테이너는 서블릿을 호출한다.
	4. 호출된 서블릿의 작업을 담당하게 된 쓰레드(2번에서 만든 쓰레드)는 doPost()또는 doGet()을 호출한다.
	5. 호출된 doPost(), doGet() 메소드는 생성된 동적 페이지를 Response객체에 담아 컨테이너에 전달한다.
	6. 컨테이너는 전달받은 Response객체를 HTTPResponse형태로 바꿔 웹서버에 전달하고 생성되었던 쓰레드를 종료하고 httpServletRequest, httpServletResponse 객체를 소멸시킨다.
	- 대표 : Tomcat, Jeus, JBoss

  

### WEB/WAS차이점
- 동적서버컨텐츠를 수행하는가 여부
- WAS: 동적인 처리담당 서버
- 분산트랜잭션, 보안, 메시징, 쓰레디 츠러ㅣ 등 분산환경
- 주로 DB서버와 같이 수행
- WEB: 정적인 HTML이나 이미지제공서버
- DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server

- ![](https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png)
![](https://t1.daumcdn.net/cfile/tistory/156A50404F93CDE817)
