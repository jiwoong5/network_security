## 실습목표
- ARP spoofing 공격을 통해 희생자 노드의 패킷을 sniffing하고 패킷을 변조시켜 전파한다. 또한, 패킷 분석 툴을 이용해 동작 과정을 관찰하고 ARP spoofing 방지 기법에 대해 학습한다.

## 유의사항
- 모든 동작 결과 화면은 full screen 을 캡쳐한다. 가상 환경 사용자 이름은 학번으로 대체 하라

## 제출물
- 동작 원리와 각 동작 화면 및 공격 결과를 포함하는 보고서를 제출한다.

## 실습 1: 공격 발생 전 서버에 ICMP 패킷 전송
- 공격 수행 전, 모든 노드의 network를 "NAT"에서 "Host-only"로 변경하여 고립시킨다
- 클라이언트에서 ping을 통해 서버와 통신한다
- 서버 노드에서 wireshark를 통해 클라이언트 ping packet의 mac address를 확인한다

## 1번 실습준비
### hypervisor
#### virtual machine 구성
- client, server 용 ubuntu 가상머신 2대

#### virtual machine network
- NAT + host-only 구성
- 과제 지시사항에서는 host-only의 고립된 환경
- 하지만 virtualbox 특성 상 상태가 저장된 vm의 네트워크 설정을 변경할 수 없음
- 초기부터 host-only 상태라면 wireshark 등 툴을 활용할 수 없음 

### tshark
- 통신중인 패킷 흐름을 살펴보는 툴인 wire shark cli 버전
- 해당 실습에서는 packet mac 주소를 분석하기 위해 사용

## 1번 실습과정
### tshark 
- 설치 by sudo apt install tshark -y
![macfromping](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/macfromping.png)
- 데이터 추출 by sudo tshark -i enp0s8 -f "icmp" -T fields -e eth.src -e eth.dst -e ip.src -e ip.dst
- 순서대로 client mac, server mac, client ip, server ip
- client mac(08:00:27:00:99:64), server mac(08:00:27:bb:f3:13) 확인가능

## 실습 2: ettercap 툴을 통한 ARP Spoofing 공격 수행
- 공격자 노드에서 기존에 설치했던 ettercap graphical 툴을 실행한다
- 클라이언트에서 Mac Adddress Table 을 확인하고, 서버로 ping을 전송한다
- 공격자 노드에서 wireshark를 통해 수집한 ping packet의 mac address 변화를 확인한다

## 2번 실습준비
### hypervisor
#### virtual machine 구성
- client, server 용 ubuntu 가상머신 2대 구성 동일
- 추가로 spoofer 용 ubuntu 가상머신 1대

#### virtual machine network 구성 동일

### ettercat
- lan 에서 중간자공격을 위한 오픈소스 툴
- 이번 실습에서는 ARP poisoning 용도로 사용

## 2번 실습과정
### ettercat 
- 설치 by sudo apt install ettercap-text-only ettercap-graphical -y
![hostlist](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/hostlist.png)
- host list에 client(xx.105), server(xx.108) 이 잡힘
![targetlist](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/targetlist.png)
- server, client 각각을 target 1, target 2에 추가
![arppoisoning](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/arppoisoning.png)
- ARP 공격 수행
![servermac2](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/servermac2.png)
- server side에서 client의 mac 주소 변경 확인

## 실습3: ARP spoofing 방지기법을 적용한다
- 클라이언트 노드에서 네트워크 내의 Mac Address를 정적으로 설정하고, Mac Address Table을 관찰한다
- 공격자 노드에서 ARP spoofing 공격을 수행 후 , wireshark를 통해 클라이언트의 Mac Address를 확인한

## 3번 실습 준비 2번과 동일

## 3번 실습 과정
![terminatearp](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/terminatearp.png)
- ARP 공격 종료
![servermac3](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/servermac3.png)
- server side에서 client의 mac 주소 원래대로 돌아옴 확인
- server에서 sudo ip neigh replace 192.168.56.105 lladdr 08:00:27:00:99:64 dev enp0s8 nud permanent
- client에서 sudo ip neigh replace 192.168.56.105 lladdr 08:00:27:bb:f3:13 dev enp0s8 nud permanent
- 로 mac 주소 고정
![poisoningafterpermanent](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/poisoningafterpermanent.png)
- 다시 ARP 공격 수행
![servermac4](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/servermac4.png)
- 공격은 잘 수행되었지만, mac 주소는 바뀌지 않아 spoofing 공격으로부터 안전함 확인
