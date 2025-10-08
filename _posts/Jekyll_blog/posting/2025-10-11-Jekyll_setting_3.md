---
title: "Jekyll 블로그 초기 세팅 가이드 - 3 (카테고리 설정, 사이드바 구성, Toc 옵션)"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 📘 이전 포스팅 내용

[Jekyll 블로그 초기 세팅 가이드 - 2](https://latentlabanonymous.github.io/posting/Jekyll_setting_2/)에서는 간단한 GitHub Blog 기본 세팅 방법을 다뤘습니다.  
이전 포스팅에서 진행한 내용은 다음과 같습니다:

- Blog 미리보기  
- Profile 설정  
- Posting 방법  

이번 포스팅에서는 아래의 내용을 다룹니다.

- 카테고리 설정  
- 사이드바 구성  
- Toc 옵션  

---

## 🗂 카테고리 설정 & 사이드바 구성

보통 블로그에서는 카테고리와 사이드바가 함께 연결되어 작동하므로, 두 설정을 함께 진행합니다.  
`GitHub Blog`의 카테고리 설정은 생각보다 간단합니다.  

### Step 1. `_config.yml` 수정

`{root}/_config.yml`에서 defaults 코드를 아래와 같이 변경합니다.

```
# Defaults
defaults:
  # _posts
  - scope:
      path: "_pages_guide"
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      comments: # true
      share: true
      related: true
      sidebar:                 
        nav: "sidebar-category"

  # _pages 
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true
      sidebar:                 
        nav: "sidebar-category"
```

---

### Step 2. `_data/navigation.yml` 수정

`{root}/_data/navigation.yml` 파일을 아래와 같이 커스터마이징합니다.

```
sidebar-category:
  - title: 💻 Jekyll_Blog
    children:
      - title: "📝 Posting"
        url: "/posting"
        category: "posting"
      - title: "📄 Markdown"
        url: "/markdown"
        category: "markdown"
  - title: 💵 Money
    children:
      - title: "📗 Excel"
        url: "/excel"
        category: "excel"
      - title: "📈 Investment"
        url: "/investment"
        category: "investment"
```

---

### Step 3. 카테고리 페이지 생성

`{root}/_pages/category-{새로만들 카테고리}.md` 파일을 만듭니다.  
예: `{root}/_pages/category-posting.md`

```
---
title: "posting"
layout: archive
permalink: /posting
author_profile: true
sidebar:
    nav: "sidebar-category"
---

`{`% assign posts = site.categories.posting %`}`
`{`% for post in posts %`}`
  `{`% include archive-single.html type=page.entries_layout %`}`
`{`% endfor %`}`
```

> ⚠️ 주의: 위 코드에서 `{`,`}` 로 바꾸세요.

---

### Step 4. 포스팅 시 카테고리 설정

포스팅 파일의 상단에 다음과 같이 카테고리를 지정합니다.

```
title: "Jekyll 블로그 초기 세팅 가이드 - 3 (카테고리 설정, 사이드바 구성, Toc 옵션)"
layout: single
categories:
  - posting
```

이렇게 설정하면 카테고리 구성이 완료됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting_3/side_bar.jpg" alt="사이드 바">

위 그림처럼 왼쪽에 카테고리별로 구성된 **사이드바**가 표시됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting_3/side_bar_1.jpg" alt="posting 선택">

‘posting’을 클릭하면 해당 카테고리에 포함된 포스트 목록을 볼 수 있습니다.

### 추가 정보

side-bar 옆에 몇개의 posting이 있는지 시각화 하고싶으면 아래의 파일을 받고 해당 위치 파일을 교체하면 됩니다.  

```
{root}/_includes/nav_list
```

<a href="https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/_includes/nav_list" download="nav_list">📂 nav_list 다운로드</a>

---

## 🧭 TOC 구성 (Table of Contents)

TOC는 *Table of Contents*의 약자로, 한국어로는 **“목차”**를 의미합니다. 📑  
아래 그림처럼 오른쪽에 표시되어 각 섹션으로 빠르게 이동할 수 있게 도와줍니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting_3/toc.jpg" alt="table of contents">

toc를 추가하는 방법은 매우 간단합니다.

`{root}/_pages/category-posting.md` 파일에 아래 설정을 추가합니다.

```
title: "posting"
layout: archive
permalink: /posting
author_profile: true
sidebar:
    nav: "sidebar-category"
toc: true # 추가
toc_sticky: true # 추가
```

위 코드를 적용하면 간편하게 **TOC(목차)** 기능을 활성화할 수 있습니다.

## ✍️ 마무리 및 앞으로의 계획

이번 글에서는 다음 내용을 다뤘습니다.

- **카테고리 설정**  
- **사이드바 구성**
- **Toc 옵션**  

다음 포스팅에서는 **상단바 구성**,**이미지 넣는 법 & 이미지 설정**,**git push** 등을 자세히 다루겠습니다.  
또한 **Markdown 문법**에 대한 내용도 앞으로 다룰 예정입니다.

---

> 💬 **오늘의 명언**  
> *발타사르 그라시안 (Baltasar Gracián)*  
> “even he who has nothing else has that. All that really belongs to us is time”  
> “심지어 가진게 아무것도 없는 사람조차도 시간은 있다. 우리가 진정 소유하고 있는 것은 시간뿐이다.”
