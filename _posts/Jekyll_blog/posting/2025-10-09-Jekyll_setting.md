---
title: "Jekyll 블로그 초기 세팅 가이드 - 1 (GitHub ID 생성, GitHub 저장소 Fork,GitHub URL 만들기)"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 👋 Jekyll이란?

Jekyll은 티스토리나 벨로그와 달리, 내가 원하는 포맷과 테마(theme)를 마음껏 바꿔가며 꾸밀 수 있는 개발자용 블로그 플랫폼입니다.  
한국어 자료가 많지 않아서 처음 시작하기 막막할 수 있지만, HTML과 CSS를 조금만 알아도 쉽게 수정할 수 있다는 장점이 있죠.

### 🎨 Jekyll 테마

[Jekyll 테마](https://github.com/topics/jekyll-theme)는 GitHub에서 다양하게 찾아볼 수 있어요.  
대부분 개인 웹사이트용이지만, 블로그용으로도 쓸 수 있는 예쁜 테마가 많습니다.  
내 취향과 목적에 딱 맞는 테마를 골라 쓰면 됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting/jeklly_thema.jpg" alt="Jekyll 테마 예시">

저는 인기 많고 참고하기 좋은 `jekyll-minimal-mistakes` 테마를 선택했어요.

---

## 🚀 Jekyll 블로그 초기 세팅

GitHub 블로그를 처음 시작하면, GitHub 계정 만들기부터 테마 설정까지 뭐부터 해야 할지 막막할 수 있어요.  
GitHub는 원래 개발자들이 코드 공유를 위해 만든 곳이지만, 이제는 블로그 등 다양한 용도로도 쓰이고 있습니다.  
티스토리나 네이버 같은 서비스는 편리한 도구가 많은 반면, GitHub 블로그는 모든 걸 내가 직접 세팅해야 한다는 점이 있지만, 자유도가 엄청나게 높다는 게 큰 장점입니다.

아래에 Jekyll 블로그 초기 세팅 과정을 단계별로 쉽게 정리했으니 천천히 따라 해보세요.

### 1. 테마 선택하고 포크(fork)하기

먼저, Google이나 Naver 이메일로 GitHub 계정을 만들고, 원하는 Jekyll 테마 저장소를 내 계정으로 포크합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting/jeklly_thema_fork.jpg" alt="테마 포크">

### 2. 포크한 저장소 설정하기

포크한 `minimal-mistakes` 저장소로 가서 `Settings` 메뉴를 클릭합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting/github_repo_create.jpg" alt="GitHub 설정">

### 3. 저장소 이름 바꾸기

`Repository name`을 `내닉네임.github.io` 형식으로 바꾸고, `Rename` 버튼을 눌러 저장소 이름을 변경합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting/github_repo_create_1.jpg" alt="저장소 이름 변경">

### 4. GitHub Pages 활성화 확인

`Settings` > `Pages` 메뉴로 가면 “Your site is live at https://{내사이트주소}”라는 메시지가 보여요.  
링크를 클릭해서 내 블로그가 잘 열리는지 꼭 확인하세요.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting/github_repo_create_2.jpg" alt="GitHub Pages 활성화">

### 5. 초기 세팅 완료!

사이트가 정상적으로 잘 나온다면 초기 세팅은 성공!  
사이트 반영까지 5~10분 정도 기다려야 할 수도 있으니 조금만 참으세요.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting/github_repo_create_3.jpg" alt="초기 세팅 완료">

---

## ✍️ 마무리 및 앞으로의 계획

이번 글에서는 Jekyll 블로그를 처음 세팅하는 과정을 간단히 소개했습니다.  
앞으로 더 자세한 세팅 팁과 활용 방법, 그리고 저만의 커스터마이징 노하우도 공유할 예정이니 기대해 주세요!

> 천천히, 하지만 꾸준히