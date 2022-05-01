---
title: "WSL2에서 Spark 설치하기"
author: "Hanjeongin"
date: 2022-04-18 18:00:00
tags: Settings
---

## 필수 파일 설치

- 자바 및 spark 파일 설치
- 이미 했을 경우 그냥 넘겨도 됨

```jsx
sudo apt-get install openjdk-8-jdk
sudo wget https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz
sudo tar -xvzf spark-3.2.0-bin-hadoop3.2.tgz
```

- vi ~/.bashrc 입력 후 다음 코드 입력

```jsx
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export SPARK_HOME=/mnt/c/hadoop/spark-3.2.0-bin-hadoop3.2
export PATH=$JAVA_HOME/bin:$PATH
export PATH=$SPARK_HOME/bin:$PATH
export PYSPARK_PYTHON=/usr/bin/python3
```

## ****.bashrc 파일 수정****

- source ~/.bashrc 입력해서 적용
    
    ![Untitled](/images/Spark_install_in_WSL2/Untitled.png)
    
- 경로(`/mnt/c/hadoop/spark-3.2.0-bin-hadoop3.2`)를 잡아주고 pyspark 실행
    
    ![Untitled](/images/Spark_install_in_WSL2/Untitled%201.png)
    
- 다음 코드를 입력해서 잘 설치됐는지 테스트

```jsx
rd = sc.textFile("README.md")
rd.count()
```

![Untitled](/images/Spark_install_in_WSL2/Untitled%202.png)

## UI로 확인하기

- 새 디렉토리 생성 (아무곳에 해도 상관 없음)
- 가상환경 설치
    
    ![Untitled](/images/Spark_install_in_WSL2/Untitled%203.png)
    
- pyspark 설치
    
    ![Untitled](/images/Spark_install_in_WSL2/Untitled%204.png)
    
- 디렉토리 밑에 data 디렉토리 만들고 README.md에 다음 내용 입력

<aside>
💡 This program just counts the number of lines containing ‘a’ and the number containing ‘b’ in a text file. Note that you’ll need to replace YOUR_SPARK_HOME with the location where Spark is installed. As with the Scala and Java examples, we use a SparkSession to create Datasets. For applications that use custom classes or third-party libraries, we can also add code dependencies to spark-submit through its --py-files argument by packaging them into a .zip file (see spark-submit --help for details). SimpleApp is simple enough that we do not need to specify any code dependencies.

</aside>

![Untitled](/images/Spark_install_in_WSL2/Untitled%205.png)

- 다시 상위 디렉토리로 이동해서 SimpleApp.py에 다음 코드 입력

```jsx
from pyspark.sql import SparkSession

logFile = "data/README.md"  # Should be some file on your system
spark = SparkSession.builder.appName("SimpleApp").getOrCreate()
logData = spark.read.text(logFile).cache()

numAs = logData.filter(logData.value.contains('a')).count()
numBs = logData.filter(logData.value.contains('b')).count()

print("Lines with a: %i, lines with b: %i" % (numAs, numBs))

input("Typing....")

spark.stop()
```

- `$SPARK_HOME/bin/spark-submit --master local[4] [SimpleApp.py](http://simpleapp.py/)` 입력
    
    ![Untitled](/images/Spark_install_in_WSL2/Untitled%206.png)
    
- 첫 줄에 보면 using 다음 주소가 있음
- 주소를 복사한 뒤 인터넷창에서 붙여넣고 뒤에 :4040을 붙임 (여기선 [172.31.145.15:4040](http://172.31.145.15:4040/jobs/))
- UI로 확인할 수 있음

![Untitled](/images/Spark_install_in_WSL2/Untitled%207.png)