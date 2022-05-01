---
title: "JupyterLab에서 작업한 내용 github 블로그로 업로드하기"
author: "jeongin"
date: 2022-03-19 09:00:00
tags: 블로그 관리
---

- 왼쪽 상단에 File - Save and Export Notebook As... - Markdown 클릭
    
    ![Untitled](/images/JupyterLab_upload_to_blog/Untitled.png)
    
- 압축 풀고 블로그 source 폴더에 들어가서 _posts 폴더엔 md 파일을, images 파일엔 이미지가 들어있는 폴더를 넣어줌
- PyCharm md파일에 들어가서 타이틀과 날짜를 설정하고 이미지 경로를 잡아줌
    
    ![Untitled](/images/JupyterLab_upload_to_blog/Untitled%201.png)
    
    ![Untitled](/images/JupyterLab_upload_to_blog/Untitled%202.png)
    
- hexo server로 확인 후 이상 없으면 hexo generate —deploy로 업로드