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
### hypervisor 
#### virtual machine 구성
- virtualbox 이용 가상화를 통해 윈도우 기반 랩탑 위에 ubuntu 등 운영체제를 올려서 사용
- 실습 1을 위해 3개의 가상머신 ubuntu server, ubuntu client, ubuntu sniffer 를 사용
![virtualmachine]()

#### virtualbox
- oracle 에서 만든 무료 가상화 툴
- 윈도우 호스트에서 거북이 문제 발생 시 다음 방법으로 해결
![turtle_problem]()
[turtle solution](https://learn.microsoft.com/en-us/troubleshoot/windows-client/application-management/virtualization-apps-not-work-with-hyper-v)
![after_turtle]()
- 문제해결 시 위와같이 V 아이콘이 뜨며 멈춤현상이 줄어드는 모습을 볼 수 있음

#### virtual machine 하드웨어 설정
- 램: 2GB , CPU: 2개, HDD: 20GB 사용

#### virtual machine 네트워크 설정
- NAT: 10.0.2.15 고정. server-2222/22, client-2223/22, sniffer-2224/22 (호스트-게스트 통신에 사용)
- 호스트 전용 어뎁터: server-192.158.56.102, client-192.168.56.105, sniffer-192.158.56.107 (게스트-게스트간 통신에 사용)

### ssh
- virtual box 무료버전의 한계로 인해 GUI 에서 작업 시 작업 멈춤현상 발생
- openssh + putty를 이용해서 window 환경에서 cli를 통해 작업

### 운영체제
- promiscuous mode 지원
- 강의자료에서는 client system을 ubuntu desktop 14로, telnet server system을 ubuntu server 16으로 사용
- 실험에서는 모두 ubuntu 22.04 lts 버전 사용

### tcp dump
- 기본적이고 강력한 리눅스 스니핑 도구. 네트워크 관리툴로 개발
- 설치: sudo apt-get install tcpdump
- 강의자료에서는 sudo tcpdump -i eth0 -xX host 192.168.0.2 로 dump
- 실험에서는 eth0이 아닌 enp0s8을 호스트간 통신에 사용함으로 이를 통해 대체
- ip주소는 server주소 사용. client 주소로 해도 동일할 것으로 추정

### telnet
- server 와 원격으로 연결하는 쉘. 패킷 메시지가 암호화되지 않아 보안 취약
- 연결: telnet 192.168.0.2 (서버 주소)
- 분석: telnet, ftp 같은 초기 서비스들은 account, password 등의 정보를 plain text 형태로 전송

## 실습 과정
### virtual machine setting 공통
- 각 virtual machine 에서 sudo apt update 및 sudo apt install openssh-server 동작
- sudo apt update 등이 제대로 동작하지 않는 경우 nat 네트워크 수동설정 by sudo ip link set dev enp0s3 up << 요걸로 enp0s3 활성화, sudo ip addr add 10.0.2.15/24 dev enp0s3, sudo ip route add default via 10.0.2.2, echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf, echo "nameserver 8.8.4.4" | sudo tee -a /etc/resolv.conf
- 과제 요구사항에 맞게 사용자 이름 설정 by sudo adduser ubuntuclient201924511, sudo usermod -aG sudo ubuntuclient201924511

### virtual machine setting sniffer
- sniffer의 경우 다른 ip로 가는 패킷을 sniffing하기 위해 promiscuous mode 를 켜야함
![promicious1]()
- 먼저 virtualbox-네트워크-어뎁터2(호스트전용)-promiscuous 부분에서 모두허용으로 설정
![promicious2]()
- vm 내부에서 promiscuous on by sudo ip link set enp0s8 promisc on

### virtual machine setting server
![telnetsetting]()
- telnet 설정: server 23번 포트 사용
- telnet /usr/sbin/in.telnetd 관련 문제: 설정파일이 없다고 뜨는 경우 sbin 에서 ls 후 비슷한 이름을 찾아 대체해볼 것. (ex. /usr/sbin/telnetd)

## 실습 결과
### 서버 로그인 
![accountdump]()

### account dump
- 실습에 사용된 계정은 ubuntuserver 계정인 ubuntuserver201924511
![accountdump1]()
![accountdump2]()
![accountdump3]()
- 위의 결과를 분석해보면 ack166~186 까지 계정명이 plain text로 출력된 것을 볼 수 있음

### password dump
- 실습에 사용된 계정 계정인 ubuntuserver201924511 의 password 는 so 로 시작함
![passworddump]()
- ack 240부터 password가 plain text로 출력된 것을 볼 수 있음
