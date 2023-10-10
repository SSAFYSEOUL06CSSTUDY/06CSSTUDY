### 목차

1. [**OAuth란?**](#1-oauth란)

# 1. OAuth란?

> 인터넷 사용자들이 사이트에 직접 개인 정보를 제공 하지 않고, <br>
> 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 접근 위임을 위한 개방형 표준

- 개인정보 관리 책임과 로그인을 위한 부분을 'Third-Party Application(google, kakao, naver)'에 위임
- 부여받은 접근 권한을 통해 Third-Party Application이 가지고 있는 사용자의 정보 조회 또한 가능
- 현재는 개선버전인 OAuth 2.0을 주로 사용

# 2. OAuth Roles

> Authorization Server / Resource Server / Client / Resource Owner

## Authorization Server

    Authorization Server는 Resource Owner를 인증하고, Client에게 액세스 토큰을 발급해주는 서버

## Resource Server

    Resource Server는 구글, 페이스북, 트위터와 같이 리소스를 가지고 있는 서버
    즉, API Server Client가 토큰을 통해 요청할 자원을 가지고 있는 서버

## Client

> 명칭이 클라이언트이지만 사용자가 아닌 사용자가 OAuth를 통해 인증하려는 사이트를 의미<br>
> 액세스토큰을 발급받아 Resource Server에 API를 요청하는 애플리케이션

    ex) 사용자가 '네이버'를 통해 '오늘의 집'을 로그인 하려고 할 때, '오늘의 집' = 클라이언트

## Resource Owner

> 어플리케이션의 사용자, 액세스 토큰으로 접근하는 것의 Permission을 주는 주체

- OAuth2 프로토콜 흐름에서는 클라이언트를 인증(Authorize)하는 역할을 수행
- 인증이 완료되면 권한 획득 자격(Authorization Grant)을 Client에게 부여<br>
  ex) 카카오톡 OAuth 인증시 카카오톡으로 승인해주는 것 즉, 사용자 본인

# 3. OAuth Term

> grant / Access Token / Refresh Token

## grant

> Resource Owner(유저)가 Client에게 Permission(허락) 해준 것

## Access Token

> Authorization Server에서 grant발생 시 발급해주는 토큰

## Refresh Token

> 새로운 Access Token의 발급을 돕는 역할

    Access Token 은 만료기간을 짧게 설정하기 때문에, Refresh Token을 통해 주기적으로 새로운 Access Token을 갱시, 이를 통해 반복적인 재로그인 방지

# 4. OAuth Process

## 기본적이 흐름은 다음과 같다.

### 1. Client => resource owner 으로 authorization을 요청

### 2. Resource Owner 인가시, Client는 해당 grant(인가)를 Authorization Server 에 전달

### 3. 유효한 grant일 경우 Authorization Server는 액세스 토큰을 Client에게 전달

### 4. Client는 해당 access token을 resource server와의 API 통신에 사용
