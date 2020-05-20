### SSO
- web에서 인증 방식의 하나인 SSO(Single Sign On)

#### SSO가 필요한 이유
- sso가 만약 없다면
```
우선 카카오톡, 페이스북, 넷플리스 3가지 서비스 예시
3개의 서비스는 각자의 방식으로 로그인을 요구
각각의 서비스에 로그인이 필요하고, 로그인을 처리하는 방식, 사용자 정보를 저장하는 방식 또한 다름

만약, 3개의 애플리케이션이 독립적으로 작도하기는 하지만 서로 무언된 무언가가 있다고 상상.
만약 같은 회사에서 제공하는 서비스들이란? gmail, 구글 드라이브, 구글 행아웃이라면
- 사용자 입장에서 <id-pw> 쌍을 기억
- 서비스를 만드는 입장에서 로그인 화면 3개, DB 3개, 로그인 프로세스 3개를 각자 구현 . 서비스끼리 데이트 싱크라도 구현하려고하면..복잡

``` 
![enter image description here](http://hanee24.github.io/images/without-sso-1.png)

#### SSO란 무엇인가
- 위에서 기술한 것과 같은 불편을 해소할 수 있는 인증방식이 SSO(Single Sign On)
- 여러 서비스를  ```로그인 한번 ``` 으로 이용하도록 하는 기술
- SP(Service Provider): 서비스. 애플리케이션 등을 SSO용어로 SP라고 일컫음
- IdP(Identity Provider) : SP들의 인증 관련 부분만 구현 해 놓은것 (하나의 웹 서비스)
![enter image description here](http://hanee24.github.io/images/sso.png)

#### SSO 는 어떻게 동작하는가 
- 구글 드라이브 시나리오 
1. 사용자가 SP(구글드라이브) 사용하려고함
2. 사용자의 세션 정보가 SP에 나아있지 않아서, SP는 사용자를 IdP(구글 통합 로그인 페이지)로 redirect시킴
3. 사용자의 세션 정보가 IdP에 남아있지 않아서, 사용자는 IdP에서 로그인(또는 인증)
4. 인증이 완료되면, IdP는 사용자를 SP로 다시 redirect 시키며, 동시에 사용자의 '인증 정보'를 SP에게 전달
5. 로그인 처리가 완료 되어 구글 드라이브 사용가능
- 그리고 직후에, gmail을 사용한다면,
6. 사용자가 SP(gmail)을 사용하려고함
7. 사용자의 세션정보가 SP에 남아있지 않아서, SP는 사용자를 IdP로 redirect
8. 사용자의 세션정보가 IdP에 남아 있음. 그러면 IdP는 사용자를 SP로 다시 redirect시키며, 동시에 상요자의 ```인증정보```를 SP에게 전달
9. 로그인 처리되어 서비스 사용 가능
#### SSO는 어떻게 구현될까?
- 제일 중요한 부분은 ```IdP에서 SP로 사용자 인증 정보 전달을 처리할 떄```의 보안
- 여러 표준, 프레임워크 등에 의해 구현 
- 대표적 SAML,OAuth, JWT 등등 

### References

-   https://www.youtube.com/watch?v=i8wFExDSZv0
-   https://en.wikipedia.org/wiki/Single_sign-on
-   https://auth0.com/blog/what-is-and-how-does-single-sign-on-work/
