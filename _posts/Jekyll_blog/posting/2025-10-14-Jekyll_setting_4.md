---
title: "Jekyll 블로그 초기 세팅 가이드 - 4 (상단바 구성, 이미지 설정, git push)"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 📘 이전 포스팅 요약

[Jekyll 블로그 초기 세팅 가이드 - 3](https://latentlabanonymous.github.io/posting/Jekyll_setting_3/)에서는  
GitHub Blog의 기본 세팅을 중심으로 다음 내용을 다뤘습니다.

- 카테고리 설정  
- 사이드바 구성  
- TOC 옵션 적용  

이번 포스팅에서는 블로그의 구조를 좀 더 완성시키기 위해  
다음 세 가지를 다뤄보겠습니다.

- 상단바 구성  
- 이미지 넣는 법 & 이미지 설정  
- git push 방법  

---

## 🧭 상단바 구성하기

상단바(Navigation Bar)는 블로그의 주요 메뉴를 보여주는 영역입니다.  
설정 방법은 간단합니다.

`{root}/_data/navigation.yml` 파일을 아래와 같이 수정합니다.

```yaml
# main links
main:
  # - title: "Quick-Start Guide"
  #   url: https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
  - title: "About"
    url: /test/
  - title: "Category"
    url: /categories/
  - title: "Tags"
    url: /tags/
```

이렇게 수정하면 상단바에 **About**, **Category**, **Tags** 메뉴가 표시됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-14-Jekyll_setting_4/upper_bar.jpg" alt="upper bar result">

### 🗂 Category 페이지 확인

상단바의 `Category`를 클릭하면 다음과 같은 화면이 나타납니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-14-Jekyll_setting_4/category_page.jpg" alt="category page">

이 페이지는 `{root}/_pages/category-archive.md` 파일에서 수정할 수 있습니다.

```yaml
---
title: "Posts by Category"
layout: categories
permalink: /categories/
author_profile: true
---
```

위 설정 중 `permalink`가 `/categories/`이므로,  
이 주소가 “Category” 페이지로 연결됩니다.

📁 레이아웃(`{root}/_layouts/categories.html`) 파일을 확인해보면  
카테고리를 보여주는 템플릿이 정의되어 있습니다.

---

### 🧩 About 페이지 생성

`About` 메뉴를 눌렀을 때 오류가 발생한다면,  
`{root}/_pages/about-archive.md` 파일을 새로 만들어 아래와 같이 작성합니다.

```yaml
---
title: "About"
layout: single
permalink: /test/
author_profile: true
---
이곳은 LatentLab의 소개 페이지입니다.
```

이제 정상적으로 상단바의 “About” 페이지가 동작합니다.

---

## 🖼 이미지 넣는 방법 & 설정

GitHub Blog에서 이미지를 삽입하는 기본 코드는 다음과 같습니다.

```html
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-14-Jekyll_setting_4/category_page.jpg" alt="category image">
```

📁 `assets/images` 폴더 안에 포스트별 디렉토리를 만들어 관리하면 깔끔하게 정리할 수 있습니다.  
예:  
```
assets/images/Jekyll_blog/posting/2025-10-14-Jekyll_setting_4/
```

---

### 🔍 이미지 확대 기능 추가

기본 markdown 이미지는 확대 기능이 없습니다.  
이를 보완하려면 **Magnific Popup**을 적용하면 됩니다.

**Step 1.** 아래 파일을 다운로드합니다.  
📂 [magnific-popup-setting.js](https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/assets/js/magnific-popup-setting.js)  
📂 [magnific-popup.js](https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/assets/js/jquery.magnific-popup.js)  
📂 [jquery.js](https://github.com/latentlabanonymous/latentlabanonymous.github.io/blob/master/assets/js/jquery.js)

**Step 2.** 파일을 모두 `assets/js` 폴더에 넣습니다.  

**Step 3.** `_layouts/default.html`의 마지막 줄에 다음 스크립트를 추가합니다.

```html

    </div>
    <script src="{{ site.url }}{{ site.baseurl }}/assets/js/jquery.js"></script>
    <script src="{{ site.url }}{{ site.baseurl }}/assets/js/jquery.magnific-popup.js"></script>
    <script src="{{ site.url }}{{ site.baseurl }}/assets/js/magnific-popup-setting.js"></script>
    
  </body>
</html>
```

적용 후 이미지를 클릭하면 팝업 확대 기능이 작동합니다.

---

## 🚀 Git Push로 블로그 업데이트하기

이제 작성한 포스트를 GitHub에 반영해봅시다.  
`git push`는 블로그의 변경사항을 서버로 업로드하는 명령어입니다.

**Step 1. GitHub 이메일 등록**
```bash
git config --global user.email "your_email@example.com"
```

**Step 2. 변경 파일 추가**
```bash
git add .
```

**Step 3. 커밋 메시지 작성**
```bash
git commit -m "update: add new post"
```

**Step 4. 원격 저장소로 푸시**
```bash
git push
```

수 분 후, GitHub Pages에서 자동으로 반영됩니다.

---

## ✍️ 마무리 및 다음 계획

이번 포스팅에서는 다음 내용을 다뤘습니다.

- 상단바 메뉴 구성  
- 이미지 삽입 및 확대 기능 적용  
- Git push를 통한 블로그 업데이트  

다음 포스팅에서는 **블로그 댓글 활성화**, **자본 증식**, **AI 공부**등에 대해 이어가겠습니다.  
꾸준히 기록하며 **LatentLab의 블로그 빌드 업**을 이어가겠습니다.

---

> 💬 **오늘의 명언**  
> *앨런 케이 (Alan Kay)*  
> “The best way to predict the future is to invent it.”  
> “미래를 예측하는 가장 좋은 방법은 바로 그것을 발명하는 것이다.”
