---
title: "github 블로그 만들기"
author: "Hanjeongin"
date: 2022-03-20 22:10:07
tags: 블로그 관리
---

# Node.js 설치

- LTS 설치
- [Hexo Blog 만들기 - Data Science | DSChloe](https://dschloe.github.io/settings/hexo_blog/)

![Untitled](/images/made_github_blog/Untitled.png)

- 실행 후 체크박스 체크하고 설치
- git bash here 실행해서 ‘node -v’ 실행 (버전 확인)
    
    ![Untitled](/images/made_github_blog/Untitled%201.png)
    

# hexo 설치

- git bash에 `npm install -g hexo-cli` 실행 (hexo 설치)
    
    ![Untitled](/images/made_github_blog/Untitled%202.png)
    
- git bash에 hexo init MyBlog 실행
    
    ![Untitled](/images/made_github_blog/Untitled%203.png)
    
- 바탕화면에 MyBlog 폴더가 생겼는지 확인
- MyBlog폴더를 오른쪽 클릭해서 Open Folder as PyCharm Community Edition Project로 실행
    
    ![Untitled](/images/made_github_blog/Untitled%204.png)
    
- Terminal 열고 Git Bash에서 hexo server 입력
    
    ![Untitled](/images/made_github_blog/Untitled%205.png)
    
    ![Untitled](/images/made_github_blog/Untitled%206.png)
    

# github 연동

- github에서 Your repositories 들어가서 New 클릭
    
    ![Untitled](/images/made_github_blog/Untitled%207.png)
    
    ![Untitled](/images/made_github_blog/Untitled%208.png)
    
- 바탕화면에 만든 폴더와 파일명이 일치하게 이름 입력하고 생성
    
    ![Untitled](/images/made_github_blog/Untitled%209.png)
    
- PyCharm으로 들어가서 `echo "# MyBlog" >> [README.md](http://README.md)` 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2010.png)
    
- README.md가 생성 됐는지 확인
    
    ![Untitled](/images/made_github_blog/Untitled%2011.png)
    
- `git init` 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2012.png)
    
- `git add [README.md](http://README.md)` 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2013.png)
    
- `git commit -m "first commit"` 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2014.png)
    
- $ git config --global user.email "UserName” 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2015.png)
    
- `git commit -m "first commit"` 다시 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2016.png)
    
- `git branch -M main` 입력 후 마스터에서 메인으로 바뀌었는지 확인
    
    ![Untitled](/images/made_github_blog/Untitled%2017.png)
    
- `git remote add origin [https://github.com/SiriN1234/MyBlog.git](https://github.com/SiriN1234/MyBlog.git)` 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2018.png)
    
- `git push -u origin main` 입력 후 Sign in 창이 뜨는지 확인
    
    ![Untitled](/images/made_github_blog/Untitled%2019.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2020.png)
    
- Sign in with your browser를 클릭
    
    ![Untitled](/images/made_github_blog/Untitled%2021.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2022.png)
    
- git add 파일명 # —> 해당 파일명을 올리겠다
- git add . # —> 모든 파일을 올리겠다
- git add -A —> 작업 디렉토리 변경파일을 모두 올리겠다
- git rm 파일명 # —> 해당 파일 삭제
- git commit # —> “UPDATE : 00function update” / 설명 첨부
- `git remote add origin [https://github.com/SiriN1234/'폴더명'.git](https://github.com/SiriN1234/'폴더명'.git)` : github와 해당 폴더를 연동함 (최초 1번만 하면 됨)
- git push # 최종단계 : 모든 파일을 깃허브 인터넷에 올려라

# 블로그 업로드

- hexo server 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2023.png)
    
- source에서 hello-world 들어가서 글 수정
    
    ![Untitled](/images/made_github_blog/Untitled%2024.png)
    
- 관련 패키지 설치
    
    ![Untitled](/images/made_github_blog/Untitled%2025.png)
    
- _config.yml에서 설정 변경-
    
    ![Untitled](/images/made_github_blog/Untitled%2026.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2027.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2028.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2029.png)
    
- github에서 `SiriN1234.github.io`레퍼 새로 생성
- PyCharm에서 hexo generate 입력 후 hexo deploy 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2030.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2031.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2032.png)
    
- PyCharm에 hexo new "My New Post” 를 입력하면 My-New-Post가 생김
    
    ![Untitled](/images/made_github_blog/Untitled%2033.png)
    
- hexo generate —deploy를 입력하면 생성 배포가 한번에 됨

# 테마 변경

- [Themes | Hexo](https://hexo.io/themes/)

PyCharm에서 icarus 설치

- `npm install -S hexo-theme-icarus`
- `hexo config theme icarus`

에러가 생겼을 때

- `npm install --save bulma-stylus@0.8.0 hexo-renderer-inferno@^0.1.3` 입력
    
    ![Untitled](/images/made_github_blog/Untitled%2034.png)
    
- hexo clean 후 hexo generate —deploy 입력

# 이미지 넣기

- [https://dschloe.github.io/settings/hexo_img/](https://dschloe.github.io/settings/hexo_img/)
- _post 폴더에 R에서 만든 md파일 옮기기
    
    ![Untitled](/images/made_github_blog/Untitled%2035.png)
    
- source 폴더에서 images 폴더를 만들고 blog_files 폴더를 옮겨줌
    
    ![Untitled](/images/made_github_blog/Untitled%2036.png)
    
    ![Untitled](/images/made_github_blog/Untitled%2037.png)
    
- PyCharm에서 경로를 잡아 줌
    
    ![Untitled](/images/made_github_blog/Untitled%2038.png)
    
- hexo server를 입력해서 이미지가 잘 올라갔는지 확인
    
    ![Untitled](/images/made_github_blog/Untitled%2039.png)