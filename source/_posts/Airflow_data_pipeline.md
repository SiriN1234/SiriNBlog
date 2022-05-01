---
title: "Airflow 데이터 파이프라인 구축 예제"
author: "Hanjeongin"
date: 2022-04-17 18:00:00
tags: Settings
---

## ****Dags 폴더 생성****

- 프로젝트 Root 하단에 Dags 폴더를 만든 후 dags 폴더를 확인한다

![Untitled](/images/Airflow_data_pipeline/Untitled.png)

## ****가상의 데이터 생성****

- 이번 테스트에서 사용할 라이브러리가 없다면 우선 설치한다

`pip3 install faker pandas`

![Untitled](/images/Airflow_data_pipeline/Untitled%201.png)

- data 디렉토리를 만든다
- faker 라이브러리를 활용하여 가상의 데이터를 생성한다. (파일 경로 : data/step01_writecsv.py)

`vi step01_writecsv.py`

```jsx
from faker import Faker
import csv
output=open('data.csv','w')
fake=Faker()
header=['name','age','street','city','state','zip','lng','lat']
mywriter=csv.writer(output)
mywriter.writerow(header)
for r in range(1000):
    mywriter.writerow([fake.name(),fake.random_int(min=18, max=80, step=1), fake.street_address(), fake.city(),fake.state(),fake.zipcode(),fake.longitude(),fake.latitude()])
output.close()
```

- 생성된 후, 파일을 확인하도록 한다

![Untitled](/images/Airflow_data_pipeline/Untitled%202.png)

## ****csv2json 파일 구축****

- 이번에는 CSV와 JSON 변환 파일을 구축하는 코드를 작성한다. (파일 경로 : dags/**[csv2json.py](http://csv2json.py/)**
)
- 주요 목적 함수 csvToJson()의 역할은 `data/data.csv`파일을 불러와서 `fromAirflow.json`파일로 변경하는 것이다
- DAG는 csvToJson 함수를 하나의 작업으로 등록하는 과정을 담는다
- 작업의 소유자, 시작일시, 실패 시 재시도 횟수, 재시도 지연시 시간을 지정한다
- `print_starting >> csvJson` 에서 `>>` 는 하류 설정 연산자라고 부른다. (동의어 비트 자리이동 연산자)

`vi csv2json.py`

```jsx
import datetime as dt
from datetime import timedelta

from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator

import pandas as pd

def csvToJson():
    df=pd.read_csv('data/data.csv')
    for i,r in df.iterrows():
        print(r['name'])
    df.to_json('fromAirflow.json',orient='records')

default_args = {
    'owner': 'sirin',
    'start_date': dt.datetime(2020, 3, 18),
    'retries': 1,
    'retry_delay': dt.timedelta(minutes=5),
}

with DAG('MyCSVDAG',
         default_args=default_args,
         schedule_interval=timedelta(minutes=5),      # '0 * * * *',
         ) as dag:

    print_starting = BashOperator(task_id='starting',
                               bash_command='echo "I am reading the CSV now....."')
    
    csvJson = PythonOperator(task_id='convertCSVtoJson',
                                 python_callable=csvToJson)

print_starting >> csvJson
```

## ****Airflow Webserver 및 Scheduler 동시 실행****

- 이제 웹서버와 스케쥴러를 동시에 실행한다 (터미널을 2개 열어야 함에 주의한다)

```jsx
airflow webserver -p 8080
airflow scheduler
```

![Untitled](/images/Airflow_data_pipeline/Untitled%203.png)

![Untitled](/images/Airflow_data_pipeline/Untitled%204.png)