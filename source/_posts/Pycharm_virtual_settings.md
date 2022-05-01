---
title: "Pycharm 가상환경 세팅"
author: "Hanjeongin"
date: 2022-04-14 18:00:00
tags: Settings
---

- New Project 클릭
    
    ![Untitled](/images/Pycharm_virtual_settings/Untitled.png)
    

## 프로젝트 세팅

![Untitled](/images/Pycharm_virtual_settings/Untitled%201.png)

- 경로 잡아주기
- Base interpreter에서 특정 버전을 맞춰서 세팅할 수 있음
- New environment using에서 원하는 가상환경을 쓸 수 있음

## 프로젝트 열린 후

- 경로 확인 : /venv/Scripts/python 이 나와야 함
    
    ![Untitled](/images/Pycharm_virtual_settings/Untitled%202.png)
    
- /venv/Scripts/python이 없는 경우 venv/Scripts/activate 명령어를 실행함
    
    ![Untitled](%E1%84%80%E1%85%A1%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%209d9ef27a14c1474a8ffa1156e7ac54b4/Untitled%203.png)
    
- 메인을 실행할 때 밑에 Git Bash 창에서 python main.py를 입력해서 실행
    
    ![Untitled](%E1%84%80%E1%85%A1%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%209d9ef27a14c1474a8ffa1156e7ac54b4/Untitled%204.png)
    
- 라이브러리 설치는 pip install 패키지명 입력하면 됨
    
    ![Untitled](%E1%84%80%E1%85%A1%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%209d9ef27a14c1474a8ffa1156e7ac54b4/Untitled%205.png)
    
- main함수에 print(”패키지명 version : “, 패키지명.__version__)를 입력하면 버전을 확인할 수 있음
    
    ![Untitled](%E1%84%80%E1%85%A1%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%209d9ef27a14c1474a8ffa1156e7ac54b4/Untitled%206.png)
    
    ![Untitled](%E1%84%80%E1%85%A1%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%20%E1%84%89%E1%85%A6%E1%84%90%E1%85%B5%E1%86%BC%209d9ef27a14c1474a8ffa1156e7ac54b4/Untitled%207.png)