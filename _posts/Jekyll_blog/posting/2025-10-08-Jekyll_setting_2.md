---
title: "Jekyll 블로그 초기 세팅 가이드 - 2 (Blog 미리보기, Profile 설정, Posting 방법)"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 📘 이전 포스팅 내용

[Jekyll 블로그 초기 세팅 가이드](https://latentlabanonymous.github.io/posting/Jekyll_setting/)에서는 간단한 GitHub Blog 기본 세팅 방법을 다뤘습니다.  
이전 포스팅에서 진행한 내용은 다음과 같습니다:

- GitHub ID 생성  
- GitHub 저장소 Fork  
- GitHub URL 만들기  

이번 포스팅에서는 아래의 내용을 다룹니다.

- Blog 미리보기  
- Profile 설정  
- Posting 방법  

---

## 🔍 Blog 미리보기

[Jekyll 블로그 초기 세팅 가이드](https://latentlabanonymous.github.io/posting/Jekyll_setting/)를 잘 따라왔다면 이제 **블로그 미리보기 기능**이 필요할 것입니다.  
하지만 GitHub Pages는 기본적으로 미리보기 기능이 없기 때문에, 로컬 환경에서 미리보기를 위해 별도의 도구를 설치해야 합니다.

필요한 도구는 총 2가지입니다.

1. **Ruby**  
2. **Git**

---

### 💎 Ruby란?

Ruby는 Python이나 C언어처럼 하나의 프로그래밍 언어입니다.  
자세한 동작 원리를 몰라도 괜찮습니다.  
우리는 단지 Ruby 환경에서 **gem**(패키지 관리자)을 사용하기 위해 설치합니다.

### 🧰 Git이란?

Git은 여러 개발자가 동시에 협업할 수 있도록 만든 버전 관리 도구입니다.  
이 포스팅에서는 Git의 개념보다는 **설치 및 사용**에만 초점을 맞춥니다.

---

### ⚙️ 설치 방법 (Windows 기준)

- [Ruby 설치](https://rubyinstaller.org/)  
  → "Download" 버튼을 눌러 설치 파일을 받고, 실행 후 모두 OK로 진행하면 설치 완료됩니다.

- [Git 설치](https://git-scm.com/download/win)  
  → “Git for Windows/x64 Setup”을 선택하여 설치 후 실행하면 완료됩니다.

설치가 끝났다면 이제 **git clone** 명령을 통해 프로젝트를 로컬로 가져옵니다.

```bash
git clone {URL}
```

> 위 명령에서 `{URL}`은 GitHub 프로젝트의 HTTPS 주소입니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting_2/git_clone.jpg" alt="git clone">

---

### 🖥️ CMD 여는 법

미리보기할 폴더에서 **마우스 오른쪽 클릭 → 터미널에서 열기**를 선택하면 CMD가 실행됩니다.

---

### 💻 Bundler 설치 및 실행

```bash
gem install bundler
cd {project}
bundle
```

이 과정을 통해 필요한 패키지가 자동으로 설치됩니다.  
그 다음 아래 명령으로 로컬 미리보기를 실행합니다.

```bash
bundle exec jekyll serve
```

많은 에러 메시지가 출력되더라도 대부분 **경고(Warning)**이므로 무시해도 됩니다.  
마지막에 다음과 같은 문구가 보이면 성공입니다.

```
Server address: http://127.0.0.1:4000
Server running... press ctrl-c to stop.
```

이제 웹 브라우저에서 `http://127.0.0.1:4000` 에 접속하면 **로컬 미리보기**를 확인할 수 있습니다.

> 💡 Tips: `127.0.0.1:4000`은 자신의 PC에서만 접근 가능한 주소입니다. 다른 사용자들은 볼 수 없습니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting_2/initial_site.jpg" alt="미리보기 예시">

---

## 👤 Profile 설정

이제 미리보기 기능까지 확인했다면, 다음 단계는 **프로필 설정**입니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-08-Jekyll_setting_2/profile_setting.jpg" alt="프로필 설정">

왼쪽에 표시되는 이름, 설명, 이메일, 위치 등의 정보는 `_config.yml` 파일에서 수정할 수 있습니다.

> 💡 **편집 방법**
> - VSCode가 있다면 VSCode에서 `_config.yml`을 열어 수정  
> - 없다면 메모장으로 열어서 수정 가능

아래는 `_config.yml`의 주요 설정 예시입니다.

```yaml
# theme                  : "minimal-mistakes-jekyll"
# remote_theme           : "mmistakes/minimal-mistakes"
minimal_mistakes_skin    : "dark" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"

# Site Settings
locale                   : "en-US"
title                    : "LatentLab"
description              : "An LatentLab Anonymous Blog`s"
url                      : "https://latentlabanonymous.github.io/"
baseurl                  : 
repository               : "latentlabanonymous/latentlabanonymous.github.io"
```

> ⚠️ **주의:** `_config.yml`은 실시간 반영되지 않습니다.  
> 설정 변경 후에는 반드시 `bundle exec jekyll serve`를 다시 실행해야 변경사항이 반영됩니다.

---

## 📝 Posting 하는 법

포스팅하는 방법은 간단합니다.
1. `_config.yml`file을 수정합니다.
2. `_posts` 폴더를 생성합니다.  
3. `{YYYY}-{MM}-{DD}-Title.md` 형식으로 파일을 만듭니다.

`_config.yml` file 마지막에 아래를 추가합니다.

```
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true
```

예시 폴더 구조:

```
_posts
│
├─ Jekyll_blog
│  ├─ posting
│  │  ├─ 2025-10-08-Jekyll_setting_2.md
│  │  ├─ 2025-10-09-Example_Post.md
│  │  ...
```

각 포스트의 상단에는 아래와 같은 **Front Matter**를 추가해야 합니다.

```yaml
---
title: "LatentLab 블로그의 첫 시작 - 목표와 방향성"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---
```

> `categories`, `sidebar`, `toc`, `toc_sticky` 등은 추후 포스팅에서 자세히 설명하겠습니다.  
> 우선 `title`, `layout`만 설정해도 게시 가능합니다.

포스팅 후에는 [포스팅 결과](https://latentlabanonymous.github.io/posting/Posting/)처럼 실시간으로 확인할 수 있습니다.

---

## ✍️ 마무리 및 앞으로의 계획

이번 글에서는 다음 내용을 다뤘습니다.

- Jekyll 블로그 **미리보기 기능** 설정  
- **프로필 수정 방법**  
- **포스팅 작성 절차**

다음 포스팅에서는 **카테고리 설정**, **사이드바 구성**, **TOC 옵션** 등을 자세히 다루겠습니다.  
또한 **Markdown 문법**에 대한 내용도 앞으로 다룰 예정입니다.

---

> 💬 **오늘의 명언**  
> *토머스 제퍼슨 (Thomas Jefferson)*  
> “I’m a great believer in luck, and I find the harder I work, the more I have of it.”  
> “나는 열심히 하면 할수록 행운이 더 따른다는 것을 믿는다.”
