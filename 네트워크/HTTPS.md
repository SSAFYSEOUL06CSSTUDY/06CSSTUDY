### 목차

1. [**HTTP란 무엇인가?**](#1-HTTP란-무엇인가?)
2. [**HTTP와 HTTPS의 차이점**](#2-HTTP와-HTTPS의-차이점)
3. [**SSL / TLS**](#3-ssl--tls)
 
# 1. HTTP란 무엇인가?
> HTTP(하이퍼 텍스트 전송 프로토콜, Hypertext Transfer Protocol)란 웹 브라우저와 웹 사이트 간에 데이터를 전송하는 데 사용되는 기본 프로토콜

    HTTP는 서버와 브라우저를 오가는 정보가 암호화되지 않는다는 문제점이 존재

보안상 취약하기 때문에 2014년 부터 권장되지 않는 방식
HTTP 프로토콜로 전송되는 데이터는 '스니핑'할 수 있는 데이터 패킷으로 구성되므로,    
    
    공용 Wi-Fi와 같이 안전하지 않은 매체를 통한 통신에서 데이터 탈취에 취약

크롬 등의 브라우저에서는 HTTP프로토콜을 사용 할 시 접근을 차단
<!-- 크롬 차단 이미지 자리 -->
<img width="400" alt="사용시브라우저경고" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/64faae71-0d08-4667-9f09-87064f2538b0">

# 2. HTTP의 문재점을 해결한 HTTPS
> HTTPS(Hyper-Text Transfer Protocol over Secure socket Layer)는 HTTP+Secure의 약자로서 서버와 브라우저 사이에 오가는 정보를 SSL / TSL 을 사용해 데이터를 암호화 함으로서 HTTP의 문제점을 해결

### 암호화 전 메세지
>"완전히 읽을 수 있는 텍스트 문자열입니다"

### 암호화 후 메세지
>"ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+9XCPk63Y+z0="

<!--https암호화 이미지 자리 -->
<img width="400" alt="왜https를사용하는가" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/60675bfe-35cd-49cb-83e1-ec493b224c0f">

    HTTP와 HTTPS의 가장 큰 차이점 '보안'
    HTTP의 일반 텍스트(text)에 SSL / TLS을 씌워 통해 데이터를 암호화하는 기법

>HTTPS의 암호화에서 필요한 개념
1. 공통키 방식<br>
    
        암호-복호화에 사용하는 키가 동일
        대칭키, Session Key, Secret Key, Shared Key 라고도 함

        장점 : 암호화방식에 속도가 빠르다. 대용량 Data 암호화에 적합하다.
        단점 : 키를 교환해야 하는 문제, 탈취 관리 걱정, 이용자가 증가할 수록 키관리가 어려워짐
        
        기밀성을 제공하나, 무결성/인증/부인방지 를 보장하지 않음
    

2. 비대칭키(공개키) 방식<br>
    
        암호-복호화에 사용하는 키가 다르며
        공개키 암호화 방식이라고도 부름

        장점 : 키분배 필요 X, 기밀성/인증/부인방지 기능 제공
               => 키 전송과정 중 빼앗겨도(해킹 당해도) 해커 가 해독을 할 수 없으니 공통 키 방식보다 보다 상대적으로 안전
        단점 : 공통키 방식보다 상대적으로 속도가 느림

> HTTPS 프로토콜에서는 위의 2가지 방식을 혼용해서 사용 

# 3. SSL 인증서
>  SSL은 위에서 설명한 HTTPS에서 사용하는 암호화 프로토콜 방식<br>
SSL은 SSL 인증서가 가 있는 웹 사이트에서만 실행 가능<br>
인증서를 발급하는 기관은 **CA**(certificate authority)


>SSL 인증서의 주요기능

1. 해당 서버가 신회 할 수 있는 서버 임을 보장
2. SSL 통신에 사용할 공개키를 클라이언트에게 제공 

>SSL 인증서가 서비스의 신뢰도를 보장하는 방법

1. 브라우저가 서버에 접속할 때 서버는 브라우저에 인증서를 제공
2. 브라우저는 이 인증서를 발급한 CA가 브라우저 내장 CA의 리스트에 있는지를 확인
3. 확인 결과 해당 인증서를 발급한 CA가 브라우저에 내장된 CA리스트에 포함되어 있다면 해당 CA의 공개키를 이용해 인증서를 복호화
4. 해당 절차를 통해 이 인증서가 CA의 비공개키에 의해서 암호화 된 것을 보장
5. CA에 의해서 발급된 인증서라는 것은 해당 사이트가 신뢰 할 수 있는 사이트라는 것을 의미

>SSL 인증서를 적용하는 방법

