# 가상환경구성
## 실습목표: 수업시간에 학습한 여러 네트워크 공격 기법 실습을 위해, 가상환경을 구성한다
## 유의사항: 모든 동작 결과 화면은 full screen을 캡쳐하며, 가상환경 사용자 이름은 학번으로 대체하시오
## 제출물: VMware 설치 및 결과 화면을 포함하는 보고서를 제출한다

## 실습 1: 가상환경 구성을 위한 VMware 및 iso 파일 설치
- 가상환경 구성을 위한 vmware 설치
- linux 기반 가상 컴퓨터를 위한 iso 설치

## 1번 실습 과정 
### 가상화툴 설치
- vmware 인증코드가 안오는 문제 발생
![virtualboxsite](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/virtualboxsite.png)
- 비슷한 가상화 툴인 virtualbox 사용

### ubuntu 운영체제 설치
- 강의자료에는 각각 16, 14버전을 사용. 과제에는 18.04.6 이라고 되어있는 것으로 보아 최신버전을 사용해도 무방할꺼라 판단
![ubuntusite](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/ubuntusite.png)
- 최신버전인 ubuntu 24.04.3 LTS 사용

### kali linux 운영체제 설치
![kalilinuxsite](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/kalilinuxsite.png)
- 다운로드 후 사용

## 실습 2: 실습을 위한 네트워크 요소 구성
- ubuntu 운영체제로 server, client, attacker node 구성
![nodes](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/nodes.png)
- 과제에는 모두 ubuntu로 된 3개의 node를 명시하였으나 kali linux를 다운받았으니 총 4개의 쉘스크립트 구성

## 실습 3: 실습에 사용될 라이브러리 설치
- 각 노드마다 apt update를 수행하고, 다음과 같은 라이브러리를 설치한다
- 설치된 라이브러리 확인 명령어 (ex. dpkg -get-selections| grep -E "라이브러리1|라이브러리2"
- server: wireshark, net-tools, apache2
![server](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/server.png)
- dpkg --get-selections | grep -E "wireshark|net-tools|apache2"
- client: net-tools, gcc, vim
![client](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/client.png)
- dpkg -l | grep -E "net-tools|gcc|vim"
- attacker: wireshark, net-tools, gcc, vim, ettercap
![attacker](https://github.com/jiwoong5/network_security/blob/main/assignment0/src/attacker.png)
- dpkg --get-selections | grep -E "wireshark|net-tools|gcc|vim|ettercap"
