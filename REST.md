#### REST란
- ```Represential State Transfer```의 약자
- 네트워크 아키텍처 원리의 일종
	- 리소스에대한 식별 및 접근 메시지를 통한 리소스의 조작
	- 어플리케이션의 어떤 상태인지에대한 정보
	- 
	- 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든것을 의미
	- 자원(resource)의 표현(representation)에 의한 상태 전달
		- 자원(resource)의 표현(representation)
			- 자원 : 해당 소프트웨어가 관리하는 모든것
			- 문서, 그림, 데이터, sw자체등
			- 자원의 표현
		- 상태(정보)전달
			- 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달
			- JSON 혹은 XML를 통해 통해 데이터를 주고 받음
- www와 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식
	- REST는 기본적으로 웹의 기존 기술과 HTTP프로토콜을 그대로 활용하기 떄문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일
	- Client와 Server의 통신 방식 중 하나
- REST의 구체적인 개념
	- **HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.**
	- 즉, REST는 자원 기반의 구조(ROA, Resource Oriented Architecture) 설계의 중심에 Resource가 있고 HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미한다.
	- 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.
	- CRUD Operation
		- Create : 생성(POST)
		- Read : 조회(GET)
		- Update : 수정(PUT)
		- Delete : 삭제(DELETE)
		- HEAD: header 정보 조회(HEAD)
https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
---
#### REST장단점
- 장점
	- HTTP프로토콜의 인프라를 그대로 사용하므로 REST API사용을 위한 별도의 인프라를 구축 할 필요가 없음
	- HTTP프로토콜 표준을 최대한 활용하여 여러 추가적인 장점을 가짐
	- HTTP프로토콜에 따르는 모든 플랫폼에서 사용가능
	- REST API메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악
	- 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화
	- 서버와 클라이언트의 역할을 명확하게 분리
- 단점
	- 표준이 존재 X
	- 사용메소드가 4가지 밖에 없다
	- 브라우저를 통해 테스트 할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header값이 왠지 어렵게 느껴짐
	- 구형 브라우저가 아직 제대로 지원해주지 못 하는 부분이 존재
---
#### REST 필요한이유
- 애플리케이션의 분리 및 통합
- 다양한 클라이언트 등장
- 멀티 플랫폼에대한 지원을 위해 서비스 자원에 아키텍처를 세우고 이용하는 방법을 모색한 결과 REST에 관심
---
#### REST 구성요소
1. 자원(Resource) :URL
	- 모든 자원에 고유한ID가 존재하고 이 자원은 Server에 존재
	- 자원을 구별하는 ID는 '/groups/:group_id'와 같은 HTTP URL
	- Client는 URL을 이용해서 자원을 지장하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청
2. 행위(Verb) : HTTP Method
	- HTTP프로토콜의 Method를 사용
	- GET/POST/PUT/DELETE 메서드 지원
3. 표현(Representation of Resource)
	- Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보냄
	- REST에서 하나의 자원은 ```JSON, XML, TEXT, RSS```등 여러 형태의 Representation으로 나타내어짐
---
#### REST 특징
1. Server-Client(서버-클라이언트 구조)
- 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다.
	- REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
	- Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
	서로 간 의존성이 줄어든다.
2. Stateless(무상태)
- HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다.
- Client의 context를 Server에 저장하지 않는다.
	- 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
- Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
	- 각 API 서버는 Client의 요청만을 단순 처리한다.
	- 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
	- 물론 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.
	- Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.
3. Cacheable(캐시 처리 가능)
	- 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
		- 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
		- HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
	- 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
	- 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.
4. Layered System(계층화)
- Client는 REST API Server만 호출한다.
- REST Server는 다중 계층으로 구성될 수 있다.
	- API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
	- 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다.
	- PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.
5. Code-On-Demand(optional)
- Server로부터 스크립트를 받아서 Client에서 실행한다.
- 반드시 충족할 필요는 없다.
6. Uniform Interface(인터페이스 일관성)
- URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
특정 언어나 기술에 종속되지 않는다.
#### 참고
- https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
