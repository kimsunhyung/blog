---
title: "0727 WSL2 설치"
date: '2022-07-27'
---

# WSL2 설치 (WINDOW SUBSYSTEM FOR linux  설치)


![png](/images/0727/Untitled.png)

- power shell 명령창을 키고

`dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`

`dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`

`[https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)`

- 위에 두개의 명령어 작성 후 밑에 사이트로 들어가서 다운로드

![png](/images/0727/Untitled1.png)

- 마이크로소프트 스토어에서 ubuntu 20.04.4 다운로드
- 개발은 lts에서 진행하는 것이 더 수월

![png](/images/0727/Untitled2.png)

- 이런 창이 뜰 경우 가상 환경이 잘 세팅이 되어 있지 않다는 뜻
- powershell 명령 창을 띄워서 가상 환경 설정 명령어 작성이 필요

`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`

- 작성해도  오류가 뜨면
- 검색창에 윈도우 기능 켜기를 킴 (첫번째로 진행해야 함)
- 아이디 작성 후 비밀번호 작성 시 비밀번호가 뜨지 않으니 주의해야 함.

![png](/images/0727/Untitled3.png)

- window 하이퍼바이저 플랫폼을 체크해야함 재부팅 후

![png](/images/0727/Untitled4.png)

- powershell에서

`wsl --set-default-version 2` 명령어를 작성

- wsl -l -v 해서 version 2가 나오지 않고 1이 반복해서 나올 경우 ubuntu 제거 후 재 설치

- 요약

1단계 : 윈도우즈 기능/켜기 - 하이퍼 바이저 플랫폼 클릭, 가상머신 플랫폼, Hyper-V 클릭, 컴퓨터마다 조금씩 다름.2단계 : PowerShell 관리자 권한 실행 후 아래 명령어 입력

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

- 다음 명령어 입력

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

3단계 : 재부팅4단계 : 다운로드 및 실행하여 업데이트

```
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```

5단계 : Microsoft Store 접속 및 Ubuntu 검색6단계 : Ubuntu 실행 및 ID/PW 생성7단계 : wsl --set-default-version 2 (편집됨)