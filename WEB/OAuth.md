## OAuth 등장 배경

우리의 서비스가 사용자를 대신하여 구글의 캘린더에 일정을 추가하거나, 페이스북, 트위터에 글을 남기는 기능을 만들 수 있을 것 이다. 이때, 가장 쉽게 이 기능을 구현하는 방법은 사용자로부터 구글, 페이스북, 트위터의 ID, Password 를 직접 제공받아 우리의 서비스에 저장하고 활용하는 방법이다.

타사의 ID/PW를 직접 전달받는 위험한 방법

![스크린샷 2023-11-30 오전 10 37 51](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/90955152/75133082-7cd3-45f4-8a88-b9cef559f66a)

정보 유출의 위험성

이는 우리의 서비스와 구글, 페이스북, 트위터 등의 입장에서도 굉장한 부담이다. <br>
우리의 입장에서는 사용자의 아주 민감한 정보를 직접 저장하고 관리해야한다는 부담이 생길 것 이다. <br>
또한 구글, 페이스북, 트위터는 자신의 사용자 정보를 신뢰할 수 없는 제3자에게 맡긴다는 것이 매우 불만족스러울 것이다. <br>

이를 위해 등장한 것이 바로 OAuth 이다. 

<br>

## OAuth 란?

구글, 페이스북, 트위터와 같은 다양한 플랫폼의 특정한 사용자 데이터에 접근하기 위해 제3자 클라이언트(우리의 서비스)가 사용자의 접근 권한을 위임(Delegated Authorization)받을 수 있는 표준 프로토콜이다.

쉽게 말하자면, 우리의 서비스가 우리 서비스를 이용하는 유저의 타사 플랫폼 정보에 접근하기 위해서 권한을 타사 플랫폼으로부터 위임 받는 것 이다.

<br>

## OAuth 2.0 주체

> ### Resource Owner

리소스 소유자. 우리의 서비스를 이용하면서, 구글, 페이스북 등의 플랫폼에서 리소스를 소유하고 있는 사용자이다. <br>
리소스라 하면 '구글 캘린더 정보', '페이스북 친구 목록', '네이버 블로그 포스트 작성' 등이 해당될 것이다.

> ### Authorization & Resource Server

Authorization Server는 Resource Owner를 인증하고, Client에게 액세스 토큰을 발급해주는 서버이다. <br>
Resource Server는 구글, 페이스북, 트위터와 같이 리소스를 가지고 있는 서버를 말한다.

> ### Client

Resource Server의 자원을 이용하고자 하는 서비스. 보통 우리가 개발하려는 서비스가 될 것이다.

<br>

## 어플리케이션 등록

OAuth 2.0 서비스를 이용하기전에 선행되어야 하는 작업이 있다. <br>
Client를 Resource Server 에 등록해야하는 작업이다. 이때, Redirect URI를 등록해야한다. <br>
Redirect URI는 사용자가 OAuth 2.0 서비스에서 인증을 마치고 (예를 들어 구글 로그인 페이지에서 로그인을 마쳤을 때) 사용자를 리디렉션시킬 위치이다.

> ### Redirect URI

OAuth 2.0 서비스는 인증이 성공한 사용자를 사전에 등록된 Redirect URI로만 리디렉션 시킨다. <br>
승인되지 않은 URI로 리디렉션 될 경우, Authorization Code를 중간에 탈취당할 위험성이 있기 때문이다. 

> ### Client ID, Client Secret

등록과정을 마치면, Client ID와 Client Secret를 얻을 수 있다. <br>
발급된 Client ID와 Client Secret은 액세스 토큰을 획득하는데 사용된다. <br>
이때, Client ID는 공개되어도 상관없지만, Client Secret은 절대 유출되어서는 안된다. 심각한 보안사고로 이어질 수 있다.

<br>

## OAuth 2.0의 동작 메커니즘

![스크린샷 2023-11-30 오전 10 48 49](https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/90955152/6cd50a4e-1d1c-40a9-a240-c7af36ba466a)

### 1 ~ 2. 로그인 요청

Resource Owner가 우리 서비스의 '구글로 로그인하기' 등의 버튼을 클릭해 로그인을 요청한다. <br>
Client는 OAuth 프로세스를 시작하기 위해 사용자의 브라우저를 Authorization Server로 보내야한다.

클라이언트는 이때 Authorization Server가 제공하는 Authorization URL에 <br>
`response_type` , `client_id` , `redirect_uri` , `scope` 등의 매개변수를 쿼리 스트링으로 포함하여 보낸다.

예를 들어 어떤 OAuth 2.0 서비스의 Authorization URL이 `https://authorization-server.com/auth` 라면, <br>
결과적으로 Client는 아래와 같은 URL을 빌드할 것 이다.

```
https://authorization-server.com/auth?response_type=code
&client_id=29352735982374239857
&redirect_uri=https://example-app.com/callback
&scope=create+delete
```

- `client_id` : 애플리케이션을 생성했을 때 발급받은 Client ID<br>
- `redirect_uri` : 애플리케이션을 생성할 때 등록한 Redirect URI<br>
- `scope` : 클라이언트가 부여받은 리소스 접근 권한. 

<br>

### 3 ~ 4. 로그인 페이지 제공, ID/PW 제공

클라이언트가 빌드한 Authorization URL로 이동된 Resource Owner는 제공된 로그인 페이지에서 ID와 PW 등을 입력하여 인증할 것 이다.

<br>

### 5 ~ 6. Authorization Code 발급, Redirect URI로 리디렉트

인증이 성공되었다면, Authorization Server 는 제공된 Redirect URI로 사용자를 리디렉션시킬 것 이다. <br>
이때, Redirect URI에 Authorization Code를 포함하여 사용자를 리디렉션 시킨다. 구글의 경우 코드를 쿼리 스트링에 포함한다.

이때, Authorization Code란 Client가 Access Token을 획득하기 위해 사용하는 임시 코드이다. <br>
이 코드는 수명이 매우 짧다. (일반적으로 1~10분)

<br>

### 7 ~ 8. Authorization Code와 Access Token 교환

Client는 Authorization Server에 Authorization Code를 전달하고, Access Token을 응답받는다. <br>
Client는 발급받은 Resource Owner의 Access Token을 저장하고, <br>
이후 Resource Server에서 Resource Owner의 리소스에 접근하기 위해 Access Token을 사용한다.

Access Token은 유출되어서는 안된다. <br>
따라서 제 3자가 가로채지 못하도록 HTTPS 연결을 통해서만 사용될 수 있다.

<br>

### 9. 로그인 성공

위 과정을 성공적으로 마치면 Client는 Resource Owner에게 로그인이 성공하였음을 알린다.

<br>

### 10 ~ 13. Access Token으로 리소스 접근

이후 Resource Owner가 Resource Server의 리소스가 필요한 기능을 Client에 요청한다. <br>
Client는 위 과정에서 발급받고 저장해둔 Resource Owner의 Access Token을 사용하여 제한된 리소스에 접근하고, <br>
Resource Owner에게 자사의 서비스를 제공한다.

<br>

## 참고

- https://www.oauth.com/
- https://datatracker.ietf.org/doc/html/rfc6749
- https://ko.wikipedia.org/wiki/OAuth
