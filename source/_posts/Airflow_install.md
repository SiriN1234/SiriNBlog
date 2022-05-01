---
title: "Airflow 설치하기"
author: "Hanjeongin"
date: 2022-04-16 18:00:00
tags: Settings
---

## 사전 작업

- ubuntu에서 경로를 잡는다
    
    ![Untitled](/images/Airflow_install/Untitled.png)
    
- vi /etc/resolv.conf 실행 후 nameserver 8.8.8.8을 입력한다
    
    ```jsx
    vi /etc/resolv.conf
    
    nameserver 8.8.8.8
    
    :w !sudo tee % > /dev/null
    ```
    
    ![Untitled](/images/Airflow_install/Untitled%201.png)
    
    ![Untitled](/images/Airflow_install/Untitled%202.png)
    
- airflow를 설치하기 위해 pip를 설치한다 (이미 설치를 했었음)

`sudo apt install python3-pip`

![Untitled](/images/Airflow_install/Untitled%203.png)

- 가상환경을 세팅할 디렉토리를 만들고 virtualenv 라이브러리를 설치한다

`sudo pip3 install virtualenv`

![Untitled](/images/Airflow_install/Untitled%204.png)

- 이 세팅할 디렉토리에서 가상환경을 생성한다

`virtualenv venv`

![Untitled](/images/Airflow_install/Untitled%205.png)

- 가상환경에 접속한다

`source venv/bin/activate`

- .bashrc 파일을 수정한다

`vi ~/.bashrc`

- 파일을 연 후, 다음과 같은 코드를 추가한다 (= 뒤에는 경로)

`export AIRFLOW_HOME=/mnt/c/airflow-test`

![Untitled](/images/Airflow_install/Untitled%206.png)

- 파일을 닫을 때는 ESC → :wq 순서대로 작성한다.
- 수정된 코드를 업데이트 하기 위해서는 아래와 같이 반영한다.

`source ~/.bashrc`

- 실제로 코드가 반영되었는지 확인하기 위해서는 다음과 같이 확인해본다 (현재 경로와 똑같이 나와야 한다)

`echo $AIRFLOW_HOME`

![Untitled](/images/Airflow_install/Untitled%207.png)

## ****Apache Airflow 설치****

- 다시 가상환경에 접속 후 PostgreSQL, Slack, Celery 패키지를 동시에 설치하는 코드를 작성한다

`pip3 install 'apache-airflow[postgres, slack, celery]'`

![Untitled](/images/Airflow_install/Untitled%208.png)

- username을 추가한다

`airflow users create --username sirin --password airflow --firstname han --lastname airflow --role Admin --email your_email@some.com`

![Untitled](/images/Airflow_install/Untitled%209.png)

- 에어플로 실행 위해 DB 초기화를 해줘야 한다

`airflow db init`

- 실제로 잘 구현이 되었는지 확인하기 위해 webserver를 실행한다

`airflow webserver -p 8080`

- 인터넷창에서 `localhost:8080`을 입력한다

![Untitled](/images/Airflow_install/Untitled%2010.png)

![Untitled](/images/Airflow_install/Untitled%2011.png)

## Default 예제 제거하기

- `code .`을 입력하면 vscode가 열린다

![Untitled](/images/Airflow_install/Untitled%2012.png)

- `airflow.cfg`파일을 열고, `load_examples = True`로 되어 있는 것을 `load_examples = False`로 변경한다
- 그 후에, 다시 터미널로 돌아와서 `airflow db reset` 실행한다 (Proceed? y 선택)
- webserver를 다시 실행한다
- 예제가 전부 사라진 것을 확인할 수 있다

![Untitled](/images/Airflow_install/Untitled%2013.png)