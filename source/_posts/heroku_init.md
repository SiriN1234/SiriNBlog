---
title: "Heroku 연동 순서"
author: "Hanjeongin"
date: 2022-06-06 17:00:00
tags: heroku
---


### 이 순서대로 했더니 제대로 연동 됨

- 가상환경 접속
    
    ![Untitled](./images/heroku_init/Untitled.png)
    

- heroku login

```jsx
heroku login
```

![Untitled](./images/heroku_init/Untitled%201.png)

- Heroku 프로젝트 생성

```jsx
heroku create [프로젝트 이름]
```

![Untitled](./images/heroku_init/Untitled%202.png)

- Github와 Heroku 연동

```jsx
git init
git branch -M main
git remote add origin [github 레포]
heroku git:remote -a [heroku 프로젝트]
```

![Untitled](./images/heroku_init/Untitled%203.png)

- 배포

```jsx
git add .
git commit -am "update"
git push origin main
git push heroku main
```

![Untitled](./images/heroku_init/Untitled%204.png)

- 웹페이지로 가서 확인 main.py에 입력한 내용이 나오는지 확인
    
    ![Untitled](./images/heroku_init/Untitled%205.png)
    
    ![Untitled](./images/heroku_init/Untitled%206.png)