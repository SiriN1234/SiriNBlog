---
title: "Notion 페이지 github 블로그에 업로드 하기"
author: "jeongin"
date: 2022-03-19 09:00:00
tags: 블로그 관리
---

- notion 오른쪽 상단에 ... 표시에서 내보내기 클릭
    
    ![Untitled](/images/notion_upload_to_blog/Untitled.png)
    
    ![Untitled](/images/notion_upload_to_blog/Untitled%201.png)
    
- 압축 풀고 이름 수정
    
    ![Untitled](/images/notion_upload_to_blog/Untitled%202.png)
    
    ![Untitled](/images/notion_upload_to_blog/Untitled%203.png)
    
- source 폴더에 들어가서 _posts 폴더엔 md 파일을, images 파일엔 이미지가 들어있는 폴더를 넣어줌
    
    ![Untitled](/images/notion_upload_to_blog/Untitled%204.png)
    
- PyCharm md파일에 들어가서 타이틀과 날짜를 설정하고 이미지 경로를 잡아줌
    
    ![Untitled](/images/notion_upload_to_blog/Untitled%205.png)
    
    ![Untitled](/images/notion_upload_to_blog/Untitled%206.png)
    
- hexo server로 확인 후 이상 없으면 hexo generate —deploy로 업로드