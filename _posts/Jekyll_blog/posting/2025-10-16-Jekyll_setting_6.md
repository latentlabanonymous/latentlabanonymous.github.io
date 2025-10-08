---
title: "Jekyll 블로그 초기 세팅 가이드 - 6 (검색 기능 활성화: Lunr)"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 📘 이전 포스팅 요약

[Jekyll 블로그 초기 세팅 가이드 - 5](https://latentlabanonymous.github.io/posting/Jekyll_setting_5/)에서는  
GitHub Blog의 완성도를 높이기 위해 **댓글 기능(Utterances)** 설정 방법을 다뤘습니다.

이번 포스팅에서는 방문자들이 원하는 글을 더 쉽게 찾을 수 있도록  
**검색 기능 (Lunr Search)** 을 활성화하는 방법을 알아보겠습니다.

---

## 🔍 Lunr 검색 기능이란?

**Lunr.js**는 Jekyll에 기본 내장되어 있는 **클라이언트 사이드 검색 엔진**입니다.  
별도의 서버 설정 없이도 정적 블로그 내에서 검색 기능을 구현할 수 있어,  
Minimal Mistakes 테마와도 자연스럽게 연동됩니다.

---

## ⚠️ 사전 확인: `_layouts/default.html` 수정

검색 기능을 활성화하기 전,  
이전 포스팅([Jekyll 블로그 초기 세팅 가이드 - 4](https://latentlabanonymous.github.io/posting/Jekyll_setting_4/))에서 추가한  
`_layouts/default.html` 파일의 마지막 부분을 꼭 확인해야 합니다.

아래 코드가 **`</body>` 태그 바로 위**에 정확히 위치해야 합니다.

```html
    </div>
    <script src="{{ site.url }}{{ site.baseurl }}/assets/js/jquery.js"></script>
    <script src="{{ site.url }}{{ site.baseurl }}/assets/js/jquery.magnific-popup.js"></script>
    <script src="{{ site.url }}{{ site.baseurl }}/assets/js/magnific-popup-setting.js"></script>
  </body>
</html>
```

✅ 이 코드가 누락되거나 위치가 틀리면, 검색창이 정상적으로 동작하지 않을 수 있습니다.  
(저도 이 부분에서 시간을 꽤 썼습니다… 🥲)

---

## ⚙️ `_config.yml` 설정

Minimal Mistakes 테마는 `Lunr` 검색 기능을 기본 지원하므로,  
아래와 같이 `_config.yml` 파일에서 관련 설정을 활성화하면 됩니다.

```yaml
search                   : true  # 검색 기능 활성화
search_full_content      : true  # 포스트 전체 내용 검색
search_provider          : lunr  # 검색 엔진: lunr (기본값)
```

이 설정만으로 블로그 내 검색 기능이 작동하기 시작합니다.

---

## 🧭 검색 페이지 만들기

검색 결과를 보여줄 전용 페이지를 생성해야 합니다.  
`_pages/search.md` 파일을 새로 만들고 아래 내용을 입력합니다.

```markdown
---
title: "Search"
layout: search
permalink: /search/
author_profile: true
sidebar:
  nav: "sidebar-category"
---
```

이제 `/search/` 경로에서 검색 페이지를 사용할 수 있습니다.

---

## 🧾 상단 네비게이션에 검색 메뉴 추가

상단바에서 바로 접근할 수 있도록 `_data/navigation.yml` 파일에  
아래 내용을 추가합니다.

```yaml
  - title: "Search"
    url: /search/
  - title: "About"
    url: /test/
  - title: "Category"
    url: /categories/
  - title: "Tags"
    url: /tags/
```

이제 블로그 상단 메뉴에 **Search** 항목이 표시됩니다.

---

## 🚀 마무리

이번 포스팅에서는 **Jekyll 블로그에 Lunr 검색 기능**을 활성화하는 과정을 살펴봤습니다.  
이제 방문자들이 원하는 포스팅을 쉽게 찾아볼 수 있는,  
한층 더 편리한 블로그로 완성되었습니다.

---

> 💬 **오늘의 명언**  
> *스티브 잡스 (Steve Jobs)*  
> “Innovation distinguishes between a leader and a follower.”  
> “혁신은 리더와 추종자를 구분 짓는다.”
