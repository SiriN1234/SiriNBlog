---
title: "Windows 10에 WSL2 설치하기"
author: "Hanjeongin"
date: 2022-04-15 18:00:00
tags: Settings
---

### 참고 : [https://www.lainyzine.com/ko/article/how-to-install-wsl2-and-use-linux-on-windows-10/](https://www.lainyzine.com/ko/article/how-to-install-wsl2-and-use-linux-on-windows-10/)

- Microsoft Store에서 Windows Terminal 설치
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled.png)
    
- Windows Terminal을 관리자 권한으로 실행 후 `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart` 입력
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%201.png)
    
- 그 이후 `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart` 입력
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%202.png)
    
- 재부팅 후 WSL2 Linux 커널 업데이트 패키지 설치
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%203.png)
    
- 다시 윈도우 터미널을 열고 `wsl --set-default-version 2` 를 입력해 기본적으로 사용할 WSL 버전을 2로 변경
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%204.png)
    
- Microsoft Store에서 Ubuntu 설치
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%205.png)
    
- 앱을 실행하면 터미널이 열리면서 설치가 됨
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%206.png)
    
- username과 password 입력
    - username을 대문자와 소문자로 섞어서 썼더니 에러가 발생함
    - 소문자로만 작성하니 에러가 발생하지 않음
        
        ![Untitled](/images/WSL2_install_in_windows/Untitled%207.png)
        
- username과 password를 설정하면 다음과 같이 표시된다
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%208.png)
    
- 터미널을 열어서 `wsl -l -v` 를 입력해 리눅스 확인
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%209.png)
    
    - 버전이 2인지 확인해야 함
- 터미널에 `wsl cat /etc/lsb-release` 를 입력해 현재 입력된 우분투 버전을 확인함
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%2010.png)
    
- `wsl bash` 를 입력해서 bash shell을 사용할 수 있음
    
    ![Untitled](/images/WSL2_install_in_windows/Untitled%2011.png)