---
title: "Jekyll 블로그 초기 세팅 가이드 - 5 (댓글 활성화 utterances)"
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
GitHub Blog의 기본 세팅을 중심으로 다음 내용을 다뤘습니다.

- 상단바 구성  
- 이미지 넣는 법 & 이미지 설정  
- git push 방법  

이번 포스팅에서는 블로그의 구조를 좀 더 완성시키기 위해  
댓글 활성화를 다뤄보겠습니다.


---

## 댓글 활성화 하기

[utterances](https://github.com/apps/utterances)를 들어가고 github를 login하게 되면 다음과 같은 화면이 나오게 된다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/utterance.jpg" alt="category image">

여기서 설치를 눌러준다

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/utterance_1.jpg" alt="category image">

위와 같은 그림이 나오게 된다. 여기서 한번더 설치를 눌러주고 `verify via email`을 눌러준다.  
그다음 이메일 인증을 해주면 utterances를 사용할 수 있게 된다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/utterance_2.jpg" alt="category image">

위 그림에 자신의 닉네임과 레포토지를 작성해주면 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/Jeklly_blog/posting/2025-10-15-Jekyll_setting_5/code.jpg" alt="category image">

위 그림과 같이 code를 copy할 수 있는 란이 보인다.  
(사실 여기는 해당 minimal mistake에서는 쓸모가 없고 알아만 두면 좋다.)

그 다음으로 `_config.yml`을 수정해야되는데 line 36~37에
```
comments:
  provider               : utterances 
```
으로 바꿔주면 된다. 그리고 마지막 란에
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
      comments: true
      share: true
      related: true
      sidebar:                 
        nav: "sidebar-category"

  # _pages                      : 해당 하단 영역이 새로 추가되었습니다.
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

이렇게 바꿔주면 된다.