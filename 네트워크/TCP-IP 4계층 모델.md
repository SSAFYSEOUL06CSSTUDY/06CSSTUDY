### 목차

1. [**계층 모델**](#1-계층-모델)
2. [**계층 간 데이터 송수신 과정**](#2-계층-간-데이터-송수신-과정)
3. [**애플리케이션 계층**](#3-애플리케이션-계층tcp-4)
4. [**전송 계층**](#4-전송-계층tcp-3)
5. [**인터넷 계층**](#5-인터넷-계층tcp-2)
6. [**링크 계층(네트워크 인터페이스 계층)**](#6-링크-계층네트워크-인터페이스-계층)
7. [**캡슐화 과정**](#7-캡슐화-과정)
8. [**PDU**](#8-pdu)
9. [**QnA**](#9-qna)

# 1. 계층 모델

> 컴퓨터들 간에 인터넷을 통한 정보교환을 위한 규약의 집합

&emsp; 컴퓨터들간의 대화 약속<br>
&emsp; 정식명칭은 인터넷 프로토콜 스위트<br>
&emsp; 이를 설명하기 위한 모델이 TCP / IP 4계층 또는 OSI 7계층 모델

## 1-1. TCP / IP 4계층

- 애플리케이션 계층 (4)
- 전송 계층 (3)
- 인터넷 계층 (2)
- 링크 계층 (1)

## 1-2. OSI 7계층

- 애플리케이션 계층 (7)
- 프레젠테이션 계층 (6)
- 세션 계층 (5)
- 전송 계층 (4)
- 네트워크 계층 (3)
- 데이터 링크 계층 (2)
- 물리 계층 (1)

## 1-3. 4계층과 7계층의 대응

<!-- 계층 비교 이미지 -->

<img width="600" alt="계층간 데이터 송수신 이미지" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/79710b96-53f1-4c7b-b827-d060d7e35d0c">
<br><br>

# 2. 계층 간 데이터 송수신 과정

### **각각의 계층에 대해 이해하기 전에**<br>

실제 컴퓨터 간의 대화에서 계층간 데이터 이동 과정을 이해해야 합니다.<br>
브라우저를 통해 클라이언트에서 서버로 데이터를 요청한다면 아래 그림과 같은 데이터 이동이 발생 합니다.

<!-- 계층간 데이터 송수신 이미지 -->
<img width="600" alt="계층간 데이터 송수신 이미지" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/048afdba-cbd7-4e35-8dd3-75eaf1caf793">

애플리케이션 => 전송 => 인터넷 => 링크 => 링크 => 인터넷 => 전송 => 애플리케이션<br>
위의 데이터 이동 과정을 통해 클라이언트와 서버간의 데이터 송수신이 이루어집니다.
<br><br>

# 3. 애플리케이션 계층(TCP-4)

> 실질적인 서비스가 이루어지는 계층

- 사용자가 응용 프로그램을 사용하는 계층
- FTP, HTTP, SSH, DNS 등의 프로그램 간의 데이터 통신을 위한 규범을 정의
- OSI 5,6,7계층에 대응
  <br><br>

# 4. 전송 계층(TCP-3)

> 애플리케이션-인터넷 계층 사이의 중계 역할, 전반적인 제어를 담당

- TCP,UDP 프로토콜
- 데이터 통신의 신뢰성을 확보하는 계층(TCP)

## 4-1. TCP

> 가상회선 패킷 교환 방식(Virtual Circuit Packet Switching)

- 패킷들이 가상 회선을 통해 이동
- 장점 : 패킷이 '순서대로' 도착
- 단점 : 1. 초기에 설정한 가상 회선에 따라 경로가 고정되어 있어,네트워크 상황이 급변하면 대응이 어려움<br>
  &emsp; &emsp; 2. 3way-handshake와 4way-handshake를 사용하기에 시간이 오래걸린다.<br>
  &emsp; &emsp; 3. 데이터 패킷의 손실이 1개라도 발생하면 재전송해야 한다.

- 신뢰성이 중요한 ATM등에 사용
  <!-- 가상회선 패킷 교환 방식 이미지 -->
  <img width="600" alt="가상 회선 패킷 교환 방식" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/2e30b96b-d3d0-45fc-98bf-8b06cbef2597">

## 4-2. TCP 연결 성립 과정 & 연결 해제 과정

> 연결 성립 과정 - 3way handshake

- 1단계-SYN : 클라이언트가 서버로 클라이언트의 ISN 주소 전송<br>
  &#8251; ISN : 초기 네트워크 연결을 할 때 할당된 고유 번호
- 2단계-SYN+ACK : 서버는 클라이언트의 SYN을 수신, 클라이언트에게 승인번호 전송(클라ISN+1)
- 3단계-ACK : 클라이언트도 서버로 승인번호 전송(서버ISN+1)
  <!-- 3way handshake 이미지 -->
  <img width="600" alt="가상 회선 패킷 교환 방식" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/b76ce2f5-4dfd-456f-967f-cedd95e22d59">

> 연결 해제 과정 - 4way handshake

- 1단계 : 클라이언트 연결 종료를 위해 세그먼트 전송(FIN-finsih로 설정된), FIN_WAIT_1로 대기
- 2단계 : 서버는 클라이언트에게 승인(응답) 세그먼트 전송(ACK-Acknowledgement), FIN_WAIT_2로 대기
- 3단계 : 서버는 일정시간이 지난 후 종료 세크먼트 전송(FIN)
- 4단계 : 클라이언트는 다시 서버로 ACK를 보내고 TIME_WAIT 상태가 되고 일정 시간 후에 연결을 종료
  <!-- 4way handshake 이미지 -->
  <img width="600" alt="가상 회선 패킷 교환 방식" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/1926c7c9-955e-4b10-b52c-f6cd0fbc70bd">

## 4-3. UDP

> 데이터그램 패킷 교환 방식(Datagram Packet Switching)

- 패킷들이 각각 독립적으로 최적의 경로를 선택해 이동
- IP주소를 활용한 인터넷에 주로 활용
- 장점 : 간단하고, 패킷들이 독립적으로 이동하기에 네트워크 상황이 급변하면 대응이 가능
- 단점 : 각각의 패킷이 독립적으로 도착하기에, 패킷들의 순서가 바뀌거나 손실이 발생 할 수 있어 신뢰성이 부족

<!-- 데이터그램 패킷 교환 방식 이미지 -->
<img width="600" alt="데이터그램 패킷 교환 방식" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/5847cb34-1ecb-43b8-9ab9-463a29544c75">

<br><br>

# 5. 인터넷 계층(TCP-2)

> 네트워크 패킷을 어떻게 전송할지 결정하는 계층

- OSI 7계층의 네트워크 계층과 대응
- 대표적으로 IP,ICMP 등의 프로토콜이 있음
- 주소를 지정해 패킷을 발송, 데이터의 온전한 도착을 보장하지는 않는 비연결형적 특징
- 단말 구분을 위한 IP주소를 할당해 주는 논리주소 기능
- IP주소를 기반으로 네트워크를 구분하는 라우팅 기능
- 수 많은 네트워크 경로 중 최적의 경로를 찾는 경로 설정 기능
  <br><br>

# 6. 링크 계층(네트워크 인터페이스 계층)

> 물리적인 TCP / IP 패킷의 송수신 과정을 담당하는 계층

- OSI7계층의 0과1을로 이루어진 데이터를 처리하는 **물리 계층**과 에러 확인 및 접근 제어를 하는**데이터 링크계층**으로 구성
- 데이터 링크 계층은 MAC 주소를 통해 통신
- 물리 계층은 전기적 신호를 통해 통신

<br><br>

# 7. 캡슐화 과정

> 데이터를 송수신하는 과정에서 캡슐호와 역캡슐화 과정이 발생

- 데이터를 보낼 때는 캡슐화, 받을 때는 역캡슐화

애플리케이션 계층에서 처음 생성된 데이터 패킷은 링크 계층을 지날 때까지
데이터를 전송하는데 필요한 정보인 헤더를 데이터 앞에 붙여 나가게 된다.
이 과정을 "캡슐화 과정"이라 하고<br>
<img width="600" alt="스크린샷 2023-08-14 오후 3 18 39" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/a466e7ff-a86e-4d05-9415-eb90a89389f2">

데이터를 받는 입장에서는 링크 계층에서 받은 데이터 패킷에서 헤더를 제거해나가며 최종적으로 최초로 보낸 데이터를 얻는 것을 "역캡슐화 과정"이라고 한다.<br>
<img width="600" alt="스크린샷 2023-08-14 오후 3 19 09" src="https://github.com/SSAFYSEOUL06CSSTUDY/06CSSTUDY/assets/108852263/858f4185-85a5-47e6-a573-511b738ec010">

# 8. PDU( Protocol Data Unit )

> 각 계층에서 처리되는 데이터의 단위
> 계층에서 계층으로 데이터가 전달 될 때의 데이터 한 덩어리의 단이위

- 헤더와 페이로드로 구성
  <br><br>
- 애플리케이션 계층: 메시지
- 전송 계층: 세그먼트 (TCP) 또는 데이터그램 (UDP)
- 인터넷 계층: 패킷
- 링크 계층: 프레임

# 9. QnA

> TCP 연결 성립 과정 과 연결 해제 과정의 가장 큰 차이점은 TIME WAIT이라고 할 수 있습니다.<br>
> TIME WAIT은 왜 필요할까요?

> 왜 계층 모델이 필요한가요?

> TCP와 UDP의 차이를 자유롭게 설명해주세요. 간단하게도 좋습니다.

> TCP의 연결과 연결해제 과정을 간략하게 묘사 할 수 있나요?

> 계층 간의 데이터 전달과정을 통해 캡슐화에 대해 설명해 주세요.
