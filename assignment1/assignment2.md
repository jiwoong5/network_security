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

## 실습 2: ettercap 툴을 통한 ARP Spoofing 공격 수행
- 공격자 노드에서 기존에 설치했던 ettercap graphical 툴을 실행한다(내가 언제 설치했더라... ?)
- 클라이언트에서 Mac Adddress Table 을 확인하고, 서버로 ping을 전송한다
- 공격자 노드에서 wireshark를 통해 수짖ㅂ한 ping packet의 mac address 변화를 확인한다

## 실습3: ARP spoofing 방지기법을 적용한다
- 클라이언트 노드에서 네트워크 내의 Mac Address를 정적으로 설정하고, Mac Address Table을 관찰한다
- 공격자 노드에서 ARP spoofing 공격을 수행 후 , wireshark를 통해 클라이언트의 Mac Address를 확인한

## 실습 준비
### hypervisor 
#### virtual machine 구성
- virtualbox 이용 가상화를 통해 윈도우 기반 랩탑 위에 ubuntu 등 운영체제를 올려서 사용
- 실습 1을 위해 3개의 가상머신 ubuntu server, ubuntu client, ubuntu sniffer 를 사용
![virtualmachine](https://github.com/jiwoong5/network_security/blob/main/assignment1/src/virtualmachine.png)

