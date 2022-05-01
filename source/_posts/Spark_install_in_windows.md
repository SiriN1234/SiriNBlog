---
title: "Windows에 Spark 설치하기"
author: "Hanjeongin"
date: 2022-04-18 18:00:00
tags: Settings
---

## Java 설치

- [https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)
    
    ![Untitled](/images/Spark_install_in_windows/Untitled.png)
    
- 경로변경을 해주어야 함 (Program Files에 띄어쓰기가 있기 때문)
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%201.png)
    
- jre도 경로 변경
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%202.png)
    
- 설치 완료
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%203.png)
    
- 

## Spark 설치

- [https://spark.apache.org/downloads.html](https://spark.apache.org/downloads.html)
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%204.png)
    
- 필요한 버전 찾고 3번 클릭
- [https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz](https://www.apache.org/dyn/closer.lua/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz)
- 여기선 3.2.0버전을 쓴다
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%205.png)
    
- Extract to “spark~” 클릭
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%206.png)
    
- 폴더명을 spark로 바꾸고 c로 위치 변경
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%207.png)
    
- conf에 들어가서 log4j.properties.template 파일을 메모장으로 실행
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%208.png)
    
- 원본은 주석처리 해주고 INFO를 ERROR로 변경하고 저장
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%209.png)
    

## WinRAR 설치

- [https://www.rarlab.com/download.htm](https://www.rarlab.com/download.htm)
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2010.png)
    
- 그냥 설치하면 됨

## ****winutils 설치****

- **[https://github.com/cdarlint/winutils](https://github.com/cdarlint/winutils)**
- 본인에 맞는 버전 들어가서 winutils 설치
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2011.png)
    
- C드라이브에 winutils 폴더 생성 후 그 안에 bin 폴더 생성
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2012.png)
    
- bin에 winutils 옮김
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2013.png)
    
- C드라이브에 tmp 폴더 생성 후 그 안에 hive 폴더 생성
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2014.png)
    
- cmd를 관리자 권한으로 실행
- 다음 코드를 입력

```jsx
cd c:\winutils\bin
winutils.exe chmod 777 \tmp\hive
```

![Untitled](/images/Spark_install_in_windows/Untitled%2015.png)

## 환경변수 설정

- 시스템 환경 변수 편집 실행
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2016.png)
    
- 환경변수 클릭
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2017.png)
    
- 새로 만들기 클릭
    - 변수 이름 - SPARK_HOME
    - 변수 값 - C:\spark (spark 폴더 경로)
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2018.png)
    
- 새로 만들기 클릭
    - 변수 이름 - JAVA_HOME
    - 변수 값 - C:\jdk (jdk 폴더 경로)
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2019.png)
    
- 새로 만들기 클릭
    - 변수 이름 - HADOOP_HOME
    - 변수 값 - C:\winutils (winutils 폴더 경로)

![Untitled](/images/Spark_install_in_windows/Untitled%2020.png)

- Path 들어가서 아래 코드 입력
    - %SPARK_HOME%\bin
    - %JAVA_HOME%\bin
        
        ![Untitled](/images/Spark_install_in_windows/Untitled%2021.png)
        
- 새로 만들기 클릭
    - 변수 이름 - PYSPARK_PYTHON
    - 변수 값 - python
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2022.png)
    

## 스파크 테스트

- cmd에서 c\spark 경로에 가서 pyspark 입력
    
    ![Untitled](/images/Spark_install_in_windows/Untitled%2023.png)
    
- 다음 코드 입력

```jsx
rd = sc.textFile("README.md")
rd.count()
```

![Untitled](/images/Spark_install_in_windows/Untitled%2024.png)