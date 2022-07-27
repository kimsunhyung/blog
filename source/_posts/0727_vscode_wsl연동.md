---
title: "0727 vs code, wsl연동"
date: '2022-07-27'
---

# VS 코드 WSL연동

- 검색창에 환경변수 클릭 후
- 하단에 환경 변수 클릭

![png](/images/0727_vscode_wsl연동/Untitled.png)

- PATH 클릭 후 MICROSOFT VSCODE 가 있으면 연동 완료

---

- 빅데이터 플랫폼 구축
    - CONFIGGURATION - 각 설치 프로그램끼리 환경 변수로 유기적으로 연결시킴
    - 레고블록
    - 셀레니움, 경로 맞지 않을경우 되지 않음

---

- remote wsl 검색 후 설치
- VISUAL CODE 터미널에서 UBUNTU  뜨면 성공
- ubuntu 실행 후

![png](/images/0727_vscode_wsl연동/Untitled1.png)

- 상위 폴더로 올라가야 함.
- 리눅스 기본 명령어를 보고 공부해야 함-

![png](/images/0727_vscode_wsl연동/Untitled2.png)

- 두 개의 파일이 있다는 뜻 이어서 한 개만 선택하여 진행 한 후
- vs code 실행
- 가상 환경 설치를 위하여 파이썬 버전 확인 및

![png](/images/0727_vscode_wsl연동/Untitled3.png)

- 설치 명령어 작성
- 실행되지 않을 시

sudo apt-get update 작성 후

sudo apt install python3-pip 작성 후 

y/n이 뜨면 y 작성 후 진행

- 설치 후 pip3에 가상 환경 설정

![png](/images/0727_vscode_wsl연동/Untitled4.png)

![png](/images/0727_vscode_wsl연동/Untitled5.png)

- 가상 환경 접속 시 linux는 scripts가 아니라 bin으로 접속해야 함