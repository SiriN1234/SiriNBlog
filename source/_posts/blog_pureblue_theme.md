---
title: "PureBlue 테마 적용"
author: "Hanjeongin"
date: 2022-03-20 22:10:11
tags: 블로그 관리
---

- gitbash here에서 `git clone [https://github.com/Chorer/hexo-theme-PureBlue.git](https://github.com/Chorer/hexo-theme-PureBlue.git)` 를 입력하고 블로그 themes 폴더에 넣어주기
    
    ![Untitled](/images/blog_pureblue_theme/Untitled.png)
    
- _config.yml에서 ‘theme: hexo-theme-PureBlue’ 로 바꿔주기
    
    ![Untitled](/images/blog_pureblue_theme/Untitled%201.png)
    
- `npm i -S hexo-prism-plugin` 입력해서 hexo-prism-plugin 설치하기
- _config.yml에 코드 붙여 넣기
    
    ```
    prism_plugin:
      mode: 'preprocess'    # realtime/preprocess
      theme: 'default'line_number: false    # default false
      custom_css: 'path/to/your/custom.css'     # optional
    ```
    
    ![Untitled](/images/blog_pureblue_theme/Untitled%202.png)
    
- _config.yml에서 highlight: enable: 부분을 false로 바꿔주기
    
    ![Untitled](/images/blog_pureblue_theme/Untitled%203.png)
    
- hexo clean 입력
- `npm i --save hexo-wordcount` 입력
- hexo server를 입력하면 테마가 적용되는 것을 확인할 수 있다