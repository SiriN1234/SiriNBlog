---
title: "git push를 했는데도 contribution이 채워지지 않는 문제 해결하기"
author: "Hanjeongin"
date: 2022-04-24 18:00:00
tags: git
---

## 문제 인식

- 예전부터 깃허브 블로그를 꾸준히 올렸음에도 불구하고 contribution에 불이 들어오지 않음
- 시험이 끝나서 시간이 생겼기에 해결해보기로 함

## 해결 방법

- 참고글 : [https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-graphs-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-graphs-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile)
- 확인을 해보니 이메일이 문제였음
- Git Bash에서 다음을 입력해 이메일을 확인

```jsx
git config user.email
```

![Untitled](/images/git_contribution_error/Untitled.png)

- 이메일이 아니라 닉네임이 나오는 것을 확인
- 다음 코드를 입력해서 수정

```jsx
git config --global user.email your_email_address
```

- 변경 후 push를 하니 제대로 불이 들어옴을 확인할 수 있음