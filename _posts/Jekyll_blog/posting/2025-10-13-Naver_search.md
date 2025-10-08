---
title: "GitHub Blog(Jekyll) Naver에 노출시키기"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 🌍 GitHub Blog(Jekyll)을 Naver에 노출시키기

[이전 포스팅 - Google 검색 노출 설정](https://latentlabanonymous.github.io/posting/2025-10-12-Google_search/)에서는  
**Google Search Console**을 활용해 블로그를 검색엔진에 등록하는 방법을 다뤘습니다.  

이번 글에서는 **Naver Search Advisor(네이버 서치 어드바이저)**를 이용해  
GitHub 블로그(Jekyll)를 **네이버 검색 결과에 노출시키는 방법**을 단계별로 정리하겠습니다.

---

## ⚙️ Step 1. Naver Search Advisor 접속

먼저 [👉 Naver 서치 어드바이저](https://searchadvisor.naver.com/)에 접속합니다.  
상단 메뉴에서 **웹마스터 도구**를 선택합니다.

---

## 🧾 Step 2. 사이트 등록 및 소유권 확인

자신의 GitHub 블로그 URL을 등록한 뒤,  
**HTML 확인 파일**을 다운로드하여 블로그의 `root` 폴더에 업로드합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-13-Naver_search/webmaster.jpg" alt="webmaster setting">

파일 업로드가 완료되면,  
등록한 URL을 선택해 정상적으로 소유권이 확인되는지 확인합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-13-Naver_search/webmaster_1.jpg" alt="URL verification">

---

## 🗺️ Step 3. Sitemap.xml 제출

소유권 인증이 완료되면,  
왼쪽 메뉴에서 **요청 → 사이트맵 제출**로 이동합니다.

`sitemap.xml` 파일의 URL을 입력한 뒤 **확인**을 누르면 됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-13-Naver_search/naver_sitemap.jpg" alt="naver sitemap submission">

---

## 📰 Step 4. RSS(feed.xml) 등록

사이트맵과 함께 RSS 파일을 등록하면,  
새로운 포스트가 발행될 때 자동으로 네이버에 갱신됩니다.

왼쪽 메뉴에서 **요청 → RSS 제출**을 선택한 후  
아래 파일의 URL을 입력해 등록합니다.

📂 [feed.xml 다운로드](https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/feed.xml)

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-13-Naver_search/naver_feed.jpg" alt="naver feed submission">

---

## ✅ 모든 설정 완료

이제 모든 등록이 완료되었습니다.  
보통 1~3일 내에 색인이 반영되며,  
검색 결과에 블로그 글이 노출되는 것을 확인할 수 있습니다.

---

## ✍️ 마무리 및 앞으로의 계획

이번 포스팅에서는 다음 내용을 다뤘습니다.

- GitHub Blog를 Naver에 노출시키는 기본 절차  
- Sitemap 및 RSS 등록 방법  

다음 포스팅에서는  
**상단바 구성**, **이미지 넣는 법 & 이미지 설정**, **git push**에 대해 다룰 예정입니다.  
꾸준히 기록하며 **LatentLab의 성장 과정**을 함께 만들어가겠습니다.

---

> 💬 **오늘의 명언**  
> *프레드리히 빌헬름 니체 (Nietzsche, Friedrich Wilhelm)*  
> “What is done out of love always takes place beyond good and evil.”  
> “사랑에 의해 행해지는 것은 언제나 선악을 초월한다.”