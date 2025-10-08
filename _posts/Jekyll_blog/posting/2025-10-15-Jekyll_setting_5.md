---
title: "Jekyll 블로그 초기 세팅 가이드 - 5 (댓글 활성화: Utterances)"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 📘 이전 포스팅 요약

[Jekyll 블로그 초기 세팅 가이드 - 4](https://latentlabanonymous.github.io/posting/Jekyll_setting_4/)에서는  
GitHub Blog의 기본 세팅과 관련하여 아래 내용을 다뤘습니다.

- 상단바 구성  
- 이미지 삽입 및 설정 방법  
- git push 과정  

이번 포스팅에서는 블로그의 완성도를 높이기 위해  
**댓글 기능 활성화 (Utterances 설정)** 방법을 다뤄보겠습니다.

---

## 💬 Utterances란?

**Utterances**는 GitHub의 **Issue 시스템**을 기반으로 동작하는 댓글 플러그인입니다.  
GitHub 계정을 이용해 로그인할 수 있으며, 댓글 내용이 GitHub Issue로 관리되는 구조를 가지고 있습니다.

---

## ⚙️ Utterances 설치하기

1️⃣ [Utterances 공식 페이지](https://github.com/apps/utterances)에 접속한 후 **GitHub 로그인**을 합니다.  

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/utterance.jpg" alt="utterances 설치 화면">

2️⃣ “Install” 버튼을 클릭합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/utterance_1.jpg" alt="utterances 설치 단계">

3️⃣ `verify via email`을 눌러 이메일 인증을 완료하면 Utterances를 사용할 수 있게 됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/utterance_2.jpg" alt="utterances 인증">

---

## 🧩 레포지토리 설정

인증이 끝나면 아래와 같은 화면이 나오며,  
자신의 **GitHub 닉네임과 레포지토리 이름**을 입력합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/code.jpg" alt="utterances 코드 복사">

이 화면에서 제공되는 **HTML 코드**는 일반적인 HTML 블로그에 삽입할 때 사용되지만,  
Minimal Mistakes 테마에서는 `_config.yml`을 수정하는 방식으로 간편히 적용할 수 있습니다.

---

## 🧾 `_config.yml` 설정 방법

아래와 같이 `comments` 항목을 수정합니다.

```yaml
comments:
  provider: utterances
```

그다음, `_config.yml` 파일의 마지막 부분에 **기본값 설정(defaults)** 항목을 아래처럼 추가합니다.

```yaml
# Defaults
defaults:
  # _posts 설정
  - scope:
      path: "_pages_guide"
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: true
      share: true
      related: true
      sidebar:
        nav: "sidebar-category"

  # _pages 설정 (새로 추가)
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      comments: true
      author_profile: true
      sidebar:
        nav: "sidebar-category"
```

이렇게 설정하면 모든 포스팅과 페이지에서 댓글 기능이 자동으로 활성화됩니다.

---

## 🚀 Issue 활성화하기

Utterances는 댓글 데이터를 **GitHub Issue**로 관리하기 때문에,  
해당 블로그 레포지토리에서 Issue 기능이 꺼져 있다면 반드시 켜줘야 합니다.

1️⃣ GitHub 레포지토리 우측 상단의 **Settings** 메뉴로 이동  
2️⃣ “Features” 섹션에서 **Issues** 옵션을 활성화  

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/issue.jpg" alt="GitHub issue 활성화">

---

## ✍️ 마무리

이번 포스팅에서는 **Jekyll 블로그에 댓글 기능(Utterances)** 을 적용하는 방법을 정리했습니다.  
이제 방문자들과의 소통이 가능한 완성형 블로그로 한 단계 나아갈 수 있습니다.  

---

> 💬 **오늘의 명언**  
> *벤저민 프랭클린 (Benjamin Franklin)*  
> “Tell me and I forget. Teach me and I remember. Involve me and I learn.”  
> “말해주면 잊어버리고, 가르쳐주면 기억하지만, 참여하면 배운다.”
