### 목차

1. [**ARP**](#1-ARP)
2. [**홉바이홉 통신**](#2-홈바이홈-통신)
3. [**IP 주소 체계**](#3-IP-주소-체계)
4. [**IP 주소를 이용한 위치 정보**](#4-IP-주소를-이용한-위치-정보)
5. [**QnA**](#5-qna)

# 1. ARP(Address Resolution Protocol)

> IP 주소로부터 MAC 주소를 구하는 IP와 MAC 주소의 다리 역할을 하는 프로토콜

<!-- ARP와 RARP 이미지 -->

<img width="600" alt="ARP와 RARP" src="https://goodgid.github.io/assets/img/server/arp_2.png">
<br><br>


- 컴퓨터 간의 통신 : IP 주소에서 ARP를 통해 MAC 주소를 찾아 MAC주소를 기반으로 통신
- ARP를 통해 가상 주소(IP)를 실제 주소(MAC)로 변환함.
- RARP를 통해 실제 주소(MAC)를 가상 주소(IP)로 변환함.
<br><br>


# 2. 홉바이홉(hop by hop) 통신

> IP 주소를 통해 통신하는 과정.

<!-- 홉바이홉 이미지 -->

<img width="600" alt="홉바이홉" src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc1weeP%2FbtqDahDuXh2%2FfcymOyyFb2FwiqZtVJnac1%2Fimg.png">
<br><br>


- 통신망에서 각 패킷이 여러 개의 라우터를 건너가는 모습을 비유적으로 표현(hop)
- 통신 장치에 있는 '라우팅 테이블'의 IP를 통해 시작 주소부터 시작하여<br>
  다음 IP로 계속해서 이동하는 '라우팅'과정을 거쳐 최종 목적지까지 도달하는 통신

## 2-1. 라우팅 테이블(routing table)

- 송신지에서 수신지까지 도달하기 위해 사용됨.
- 게이트웨이와 모든 목적지에 도달하기 위해 거쳐야 할 다음 라우터 정보를 가지고 있음.

## 2-2. 게이트웨이(gateway)

  <!-- 게이트웨이 이미지 -->
  <img width="600" alt="게이트웨이" src="https://velog.velcdn.com/images/givepro91/post/b02c8779-b3bb-468b-b8e5-f2dd9b452b20/image.png">

- 서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는<br>
  관문 역할을 하는 컴퓨터나 소프트웨어를 두루 일컫는 용어.
- 라우팅 테이블을 통해 게이트웨이 확인 가능함.
<br><br>
# 3. IP 주소 체계

> IPv4와 IPv6로 나뉘며 추세는 IPv6, 현재 가장 많이 쓰이는 주소 체계는 IPv4(설명기준)

<!-- IPv4와 IPv6 이미지 -->
<img width="600" alt="IPv4와 IPv6" src="https://files.itworld.co.kr/archive/image/2016/10/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202017-08-01%20%EC%98%A4%ED%9B%84%205_39_26.jpg">

## 3-1. 클래스 기반 할당 방식

  <!-- 클래스 기반 할당 방식 이미지 -->
  <img width="600" alt="클래스 기반 할당 방식" src="https://velog.velcdn.com/images%2Fkimyeji203%2Fpost%2Fd3cd1994-e67d-4f14-ae15-8c877f14aa09%2Fimage.png">

- IP 주소 체계는 처음에 다섯 개의 클래스로 구분하는 클래스 기반 할당 방식을 사용함.
- 사용하는 주소보다 버리는 주소가 많은 단점 해소를 위해 DHCP와 IPv6, NAT가 나옴.

## 3-2. DHCP(Dynamic Host Configuration Protocol)

- IP 주소 및 기타 통신 매개변수를 자동으로 할당하기 위한 네트워크 관리 프로토콜.
- 이 기술을 통해 인터넷에 접속할 때마다 자동으로 IP 주소를 할당할 수 있음.

## 3-3. NAT(Network Address Translation)

  <!-- NAT 이미지 -->
  <img width="600" alt="NAT" src="https://velog.velcdn.com/images/younghwan/post/8fd9f86b-72a5-42cf-9cc7-9cee09de6aec/image.jpg">

- IPv4 주소 체계만으론 많은 주소들을 모두 감당하지 못하는 단점이 있음.<br>
  이를 해결하기 위해 공인 IP와 서설 IP로 나눠서 많은 주소를 처리함.
- 공유기와 NAT : 공유기에 NAT 기능이 탑재되어 있기 때문에<br>
  인터넷 회선 하나를 개통 후 공유기를 통해 여러 대의 호스트가 하나의 공인 IP로 인터넷 접속 가능
- NAT를 이용한 보안 : 내부 네트워크 주소와 외부 주소를 다르게 유지할 수 있어 내부 네트워크에 대한 보안이 가능해짐.
- NAT의 단점 : 실제 접속하는 호스트 숫자에 따라서 접속 속도가 느려질 수 있음.
<br><br>
# 4. IP 주소를 이용한 위치 정보

> IP 주소는 인터넷에서 사용하는 네트워크 주소이기에 동/구까지 위치 추적이 가능.
<br><br>
# 5. QnA

> 무엇을 통해 MAC 주소를 가상 주소인 IP로 변환할까요?<br>

> 클래스 기반 할당 방식의 어떠한 단점 때문에 DHCP, IPv6, NAT가 나오게 되었을까요?
