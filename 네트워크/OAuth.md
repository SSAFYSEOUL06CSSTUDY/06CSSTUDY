### 목차

1. [**OAuth란?**](#1-oauth란)

# 1. OAuth란?
> 인터넷 사용자들이 사이트에 직접 개인 정보를 제공 하지 않고, <br>
다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 접근 위임을 위한 개방형 표준

- 개인정보 관리 책임과 로그인을 위한 부분을 'Third-Party Application(google, kakao, naver)'에 위임
- 부여받은 접근 권한을 통해 Third-Party Application이 가지고 있는 사용자의 정보 조회 또한 가능
- 현재는 개선버전인 OAuth 2.0을 주로 사용

# 2. OAuth의 주체
> Client / Resource Owner / Authorization & Resource Server


## Client
    명칭이 클라이언트이지만 사용자가 아닌 사용자가 OAuth를 통해 인증하려는 사이트를 의미
ex) 사용자가 '네이버'를 통해 '오늘의 집'을 로그인 하려고 할 때, '오늘의 집' = 클라이언트

## Resource Owner
    클라이언트 가 사용 할 리소스를 가지고 있는 주체, 
    
- 리소스는 클라이언트에서 활용할 수 있는 '구글 캘린더 정보', '페이스북 친구 목록' 등의 정보를 의미
- OAuth2 프로토콜 흐름에서는 클라이언트를 인증(Authorize)하는 역할을 수행
- 인증이 완료되면 권한 획득 자격(Authorization Grant)을 Client에게 부여

## Authorization & Resource Server
> Authorization Server는 Resource Owner를 인증하고, Client에게 액세스 토큰을 발급해주는 서버 

> Resource Server는 구글, 페이스북, 트위터와 같이 리소스를 가지고 있는 서버
