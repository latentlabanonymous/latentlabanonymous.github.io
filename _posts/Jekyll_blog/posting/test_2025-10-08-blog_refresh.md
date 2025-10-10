---
title: "GitHub Actions로 12시간마다 블로그 자동 새로고침하기"
layout: single
categories:
  - posting
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 📘 이번 포스팅에서 다룰 내용

지난 포스팅에서는 GitHub Blog의 기본 세팅을 완료하고,  
로컬 환경에서 Jekyll 미리보기까지 진행했습니다.  

이번 글에서는 **“GitHub Actions를 활용해 블로그를 자동으로 새로고침하는 방법”**을 다룹니다.  

즉, **포스팅을 하지 않아도**  
GitHub Actions가 **12시간마다 자동으로 실행**되어  
정적 페이지를 재빌드(= 새로고침)하도록 설정합니다. 🚀

---

## 🧩 왜 자동 새로고침이 필요할까?

Jekyll 기반 GitHub Blog는 정적 사이트이기 때문에,  
새로운 글을 올리거나 수정하지 않으면 사이트가 다시 빌드되지 않습니다.  

하지만 검색엔진(예: Google, Naver) 크롤링이나  
외부 데이터 갱신(예: API 기반 콘텐츠)을 위해  
**주기적으로 빌드를 다시 돌려주는 것**이 도움이 될 때가 있습니다.

이럴 때 **GitHub Actions**의 `schedule` 기능을 활용하면,  
매일 혹은 주기적으로 자동으로 페이지를 새로 빌드할 수 있습니다 ✅

---

## ⚙️ 1️⃣ 현재 `build.yml` 확인하기

먼저 레포지토리 안에서 다음 경로로 이동합니다.

```
.github/workflows/build.yml
```

기본적으로 아래와 같이 되어 있을 겁니다 👇

```yaml
name: build

on:
  push:
    branches:
      - master
  workflow_dispatch: {}
  repository_dispatch: {}

jobs:
  build:
    if: github.repository == 'mmistakes/minimal-mistakes'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'
```

이 상태에서는 **push를 해야만** 빌드가 실행됩니다.  
즉, commit을 안 하면 아무 일도 일어나지 않습니다.

---

## ⚙️ 2️⃣ 자동 실행 주기 설정 (`schedule` 추가)

GitHub Actions는 `cron` 문법으로  
**자동 실행 주기(schedule)**를 설정할 수 있습니다.

다음과 같이 `on:` 아래에 `schedule:`을 추가해 주세요 👇

```yaml
on:
  push:
    branches:
      - master
  schedule:
    - cron: '0 */12 * * *'   # ⏱ 12시간마다 자동 실행
  workflow_dispatch: {}       # 수동 실행 버튼도 유지
  repository_dispatch: {}
```

> 💡 참고  
> `'0 */12 * * *'` 은 UTC 기준으로 **매 12시간마다 실행**을 의미합니다.  
> 한국시간(KST, UTC+9)으로는 약 오전 9시, 오후 9시에 한 번씩 실행됩니다.

---

## 🧪 3️⃣ 10분 간격 테스트 (선택 사항)

설정이 잘 되는지 먼저 확인하고 싶다면  
12시간 대신 **10분 주기**로 잠깐 바꿔볼 수도 있습니다.

```yaml
schedule:
  - cron: '*/10 * * * *'   # 10분마다 테스트 실행
```

테스트 후 제대로 동작하면 다시 `'0 */12 * * *'`로 변경하세요 ✅

---

## 🔍 4️⃣ Actions 실행 확인

변경 후 **Commit → Push** 하면  
GitHub에서 자동으로 인식됩니다.

1. 레포지토리 상단 메뉴에서 **Actions** 클릭  
2. 왼쪽 메뉴에서 `build` 워크플로 선택  
3. 오른쪽 상단에서 아래와 같은 문구를 확인하세요 👇

```
Next runs at 2025-10-11 00:00 UTC
```

이 문구가 보이면 성공입니다 🎉  
지정한 주기에 맞춰 자동으로 블로그가 새로 빌드됩니다.

---

## ⚡ 5️⃣ 수동으로 실행하는 방법

`schedule` 외에도 `workflow_dispatch:` 옵션이 있으면  
원할 때마다 직접 Actions를 돌릴 수도 있습니다.

1. Actions 탭 → `build` 선택  
2. “Run workflow” 버튼 클릭  
3. ✅ 즉시 새 빌드 시작

---

## 🧱 최종 완성 예시 (`build.yml`)

```yaml
name: build

on:
  push:
    branches:
      - master
  schedule:
    - cron: '0 */12 * * *'   # 하루 2회 자동 실행 (12시간 간격)
  workflow_dispatch: {}
  repository_dispatch: {}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.2'

      - name: Install dependencies
        working-directory: docs/
        run: |
          bundle config set path vendor/bundle
          bundle install --jobs=4 --retry=3

      - name: Build and refresh site
        working-directory: docs/
        run: bundle exec jekyll build
```

---

## 🔔 마무리

이제 GitHub Actions가 자동으로  
12시간마다 당신의 블로그를 새로고침해줍니다.  

별도로 수정이나 commit을 하지 않아도  
사이트가 자동으로 최신 상태로 유지됩니다 🚀

---

> 💬 **오늘의 명언**  
> 아리스토텔레스 (Aristotle)  
> “We are what we repeatedly do. Excellence, then, is not an act, but a habit.”  
> “우리는 반복적으로 하는 그 행동의 결과물이다. 그러므로 탁월함은 행동이 아니라 습관이다.”