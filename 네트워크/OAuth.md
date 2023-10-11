### 목차

1. [**OAuth란?**](#1-oauth란)
2. [**OAuth Roles**](#2-oauth-roles)
3. [**OAuth Term**](#3-oauth-term)
4. [**OAuth Process**](#4-oauth-process)
5. [**OAuth 인증의 종류**](#5-oauth-인증의-종류)

# 1. OAuth란?

<img width="500" alt="img" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/62a1458c-cedc-488b-abbd-4d45d97bba87">

> 인터넷 사용자들이 사이트에 직접 개인 정보를 제공 하지 않고, <br>
> 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 접근 위임을 위한 개방형 표준

- 개인정보 관리 책임과 로그인을 위한 부분을 'Third-Party Application(google, kakao, naver)'에 위임
- 부여받은 접근 권한을 통해 Third-Party Application이 가지고 있는 사용자의 정보 조회 또한 가능
- 현재는 개선버전인 OAuth 2.0을 주로 사용

> 간단히 말하면 유저 보안을 API를 통해서 위탁하는 것

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

## 기본적인 흐름은 다음과 같다.

### 1. Client => resource owner 으로 authorization을 요청

### 2. Resource Owner 인가시, Client는 해당 grant(인가)를 Authorization Server 에 전달

### 3. 유효한 grant일 경우 Authorization Server는 액세스 토큰을 Client에게 전달

### 4. Client는 해당 access token을 resource server와의 API 통신에 사용

<img width="550" alt="img" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/e5b4f5c5-7f9c-417e-ade7-7af870770f94">

# 5. OAuth 인증의 종류

> 각 방식을 자세하게 이해하면 좋지만, 실질적으로 어려우므로 특징을 이해하고 넘어가면 좋습니다.

## Implicit Grant

<img width="600" alt="img" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/76d76cab-ba5b-49d7-b01f-128bc165a15c">

> 가장 구현이 간단한 구조, Authorization code 발급이 없고, Refresh Token 발급이 없음

    - 주기적으로 Client가 AccessToken을 얻는 번거로운 과정 필요
    - Access Token이 URL을 통해 전달되어 보안이 취약

## Authorization Code Grant

<img width="600" alt="img" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/e23a12d9-29b1-4063-92bd-c140c3d02d87">

> 4가지 방식 중 가장 많이 사용되는 방식, Implicit Grant 방식의 단점을 보완한 방식

    - 더 이상 Access Token을 URL을 통해 전달 하지 않음, 백 채널을 이용해 안전하게 전달
    - 보안이 강화
    - refresh token을 이용해 불필요한 재인증을 방지

## Resource Owner Password Credentials Grant

<img width="600" alt="img" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/a1bebb6c-85ed-4fbc-bc24-6d5fa30bb128">

> 자사 앱(client)에 직접 로그인하는 방식, Authorization code없이 접속 가능

## Client Credential Grant

<img width="600" alt="img" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/2cbdcb85-bdb9-4c6e-aee0-26eb753d6314">

> Owner가 존재하지 않으며, Client가 직접 Authorization Server에 접근할 수 있음
