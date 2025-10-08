---
title: "GitHub Blog(Jekyll) Google에 노출시키기"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 🌍 GitHub Blog(Jekyll)을 Google에 노출시키기

GitHub 블로그(Jekyll)를 운영하다 보면,  
“왜 내 블로그는 Google 검색에 나오지 않을까?”라는 의문이 생깁니다.  

Google에 노출시키기 위해서는 **Google Search Console**을 통해  
블로그를 등록하고, **사이트맵(sitemap.xml)**과 **robots.txt** 파일을 설정해야 합니다.

이번 포스팅에서는 그 과정을 단계별로 정리해보았습니다.

---

## ⚙️ Step 1. Google Search Console 접속

[👉 Google Search Console 바로가기](https://search.google.com/search-console/about)

접속 후 **URL 접두어**(Prefix)를 선택하고,  
본인의 블로그 주소를 입력한 뒤 `계속` 버튼을 누릅니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-12-Google_search/google_search_console.jpg" alt="google search console">

---

## 🧾 Step 2. 소유권 확인 (Ownership Verification)

Google은 블로그가 본인 소유인지 확인하기 위해  
특정 파일을 블로그의 `root` 폴더에 업로드하도록 요청합니다.  

해당 파일을 다운로드 받아,  
아래와 같은 구조로 루트 경로에 업로드합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-12-Google_search/verify_google.jpg" alt="verified google">

폴더 구조 예시:

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-12-Google_search/folder.jpg" alt="folder tree">

---

## 🗺️ Step 3. Sitemap.xml & Robots.txt 등록

Google이 사이트를 효율적으로 탐색할 수 있도록  
`sitemap.xml`과 `robots.txt` 파일을 추가해야 합니다.

📂 [sitemap.xml 다운로드](https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/sitemap.xml)  
📂 [robots.txt 다운로드](https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/robots.txt)

이후 **Google Search Console → Sitemaps** 메뉴로 이동하여  
`sitemap.xml`을 제출합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-12-Google_search/sitemap.jpg" alt="sitemap">

보통 2~3일 내에 색인이 완료됩니다.  
만약 색인이 지연된다면, `URL 검사` 기능에서 직접 **색인 요청**을 진행할 수 있습니다.  
이 부분은 별도 포스팅에서 자세히 다루겠습니다.

---

## ✍️ 마무리 및 앞으로의 계획

이번 포스팅에서는 다음을 다루었습니다:

- GitHub Blog를 Google에 노출시키는 기본 절차  
- Google Search Console 등록 및 사이트맵 제출 방법  

다음 포스팅에서는 **Naver 검색 엔진 노출 설정 방법**을 다뤄볼 예정입니다.  
꾸준히 기록하며 블로그의 성장 과정을 함께 만들어가겠습니다.

---

> 💬 **오늘의 명언**  
> *오프라 윈프리 (Oprah Winfrey)*  
> “The biggest adventure you can take is to live the life of your dreams.”  
> “여러분이 할 수 있는 가장 큰 모험은 바로 여러분이 꿈꾸는 삶을 사는 것입니다.”