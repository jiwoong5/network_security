# 실습 목표 및 유의사항

## 실습목표
- 강의에서 활용된 자료 기반 직접 sniffing 공격을 따라해 봄으로써 공격 원리를 학습함

## 유의사항
- 모든 동작 결과 화면은 full screen 을 캡쳐한다. 가상 환경 사용자 이름은 학번으로 대체 하라

## 제출물
- 동작 원리와 각 동작 화면 및 공격 결과를 포함하는 보고서를 제출한다. 또한 동작화면과 결과에는 분석이 반드시 함께 서술되어야 한다.

# 실습 1
- 강의자료 3p~10p를 참고하여 TCP Dump를 사용해 sniffing 공격을 수행하라

## 실습 준비
### 운영체제
- promiscuous mode 지원
- 강의자료에서는 client system을 ubuntu desktop 14로, telnet server system을 ubuntu server 16으로 사용
### tcp dump
- 기본적이고 강력한 리눅스 스니핑 도구. 네트워크 관리툴로 개발
- 설치: sudo apt-get install tcpdump
- sudo tcpdump -i eth0 -xX host 192.168.0.2 로 dump
### telnet
- server 와 원격으로 연결하는 쉘. 패킷 메시지가 암호화되지 않아 보안 취약
- 연결: telnet 192.168.0.2 (서버 주소)
- 분석: telnet, ftp 같은 초기 서비스들은 account, password 등의 정보를 plain text 형태로 전송

## 실습 과정
1. lan card 확인: ifconfig
2. promiscuous mode 활성화: ifconfig eth0 promisc >> ifconfig 로 활성화 확인
3. 

# 실습 2
- 강의자료 11p~15p를 참고하여 Dsniff를 사용하여 sniffing 공격을 수행하라 

## 개념
- 같은 lan 에 device1, device2 가 통신을 하는 경우
- case1: 같은 서브넷에 존재
- case2: 다른 서브넷에 존재
- case1에서 device1은 device2의 mac주소를 직접 arp table에 기입을 한 후 패킷 전송
- case2에서 device1은 서브넷을 연결해주는 라우터(게이트웨이)의 mac주소로 패킷을 전송하고, 라우터는 최종목적지로 패킷 전송
- 이 때, 같은 lan에 공격자가 존재한다면 client 가 server 또는 router(gateway) mac 주소 요청 시 자신의 mac주소를 전송할 수 있음.
- ARP 프로토콜에서 누구든지 mac주소 요청에 응답할 수 있고, 주소를 검증하는 절차가 없음으로 client는 해당 mac주소를 ARP table에 기입.

## 실습 준비

### 운영체제
- frag router 를 지원해야함.
- 오래된 레거시 툴이라 미리 준비되어있는 kali linux를 사용함
- kali linux를 사용하면 프로토콜 exploit을 활용할 수 있다
  
### frag router
- 마치 공격자가 기존 라우터처럼 동작하도록 도와주는 스니핑에 도움을 주는 툴

### dsniff
- 분석된 정보를 가독성 있게 제공하는 

### urlsnarf
- web session 을 sniffing 하는 툴
