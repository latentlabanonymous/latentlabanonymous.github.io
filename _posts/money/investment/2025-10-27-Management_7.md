---
title: "FRED API 받아오기 & Google Sheet에 실시간 반영하기"
layout: single
categories:
  - investment
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 👋 이번 포스팅에서 다룰 내용

지난 글에서는 [“리스크 관리와 리밸런싱의 기술 (심화편)”](https://latentlabanonymous.github.io/investment/Management_5/)을 다뤘습니다.  
이번에는 **FRED API를 활용해 실시간 금융 데이터를 가져오고, Google Sheet에 자동 반영하는 방법**을 알아보겠습니다.

---

## API란?

**API(애플리케이션 프로그래밍 인터페이스)**는 서로 다른 소프트웨어가 데이터를 주고받을 수 있도록 도와주는 인터페이스입니다.  
쉽게 말해, **프로그램과 프로그램이 서로 “대화”할 수 있게 해주는 통신 도구**입니다.

예를 들어, 우리가 웹사이트나 앱에서 실시간 주가나 날씨 데이터를 확인할 수 있는 이유도 대부분 API 덕분입니다.

---

### 🔑 주요 개념

- **요청(Request)**: 원하는 정보를 서버에 요청하는 행위.  
  예) “서울의 날씨를 알려줘.”
- **응답(Response)**: 서버가 요청을 처리한 뒤 보내주는 결과.  
  예) “현재 서울은 맑고 기온은 20도입니다.”
- **API 키(API Key)**: 사용자 인증을 위한 고유 키.  
  대부분의 공개 API는 이 키를 요구합니다.

---

### 🌐 왜 API를 사용하는가?

- **시스템 간 연결성**: 예를 들어, 구글 지도 API를 통해 다른 앱에서도 지도를 표시할 수 있습니다.  
- **자동화 가능**: 데이터를 수동으로 복사할 필요 없이, 코드 한 줄로 자동으로 가져올 수 있습니다.  
- **확장성**: 기존 시스템에 새로운 기능을 쉽게 연동할 수 있습니다.

---

### 📊 API 사용 예시

- **금융 데이터 API**: 실시간 주가, 환율, 경제지표 등  
- **소셜 미디어 API**: 트위터, 페이스북 등의 게시물, 팔로워 수 등  
- **날씨 API**: 지역별 기온, 강수량, 기상특보 등  

---

### 🛠 주요 API 유형

| 종류 | 설명 |
|------|------|
| REST API | 가장 널리 쓰이는 방식. HTTP 기반으로 JSON 데이터 전송에 최적화 |
| SOAP API | XML 기반. 보안과 규칙이 엄격한 환경에 적합 |
| GraphQL API | 필요한 데이터만 정밀하게 요청 가능. 데이터 낭비가 적음 |

---

API는 다양한 시스템과 데이터를 효율적으로 연결해주는 **“다리”** 역할을 합니다.  
이번 포스팅에서 다룰 자동화 리밸런싱 역시, **실시간 금융 데이터를 API로 받아오는 과정**이 핵심입니다.

---

## 🧩 데이터 자동 수집 — Power Query로 FRED API 연결하기

이제 **FRED(Federal Reserve Economic Data)** 홈페이지에서 API Key를 발급받고, 실시간 데이터를 가져오는 방법을 알아보겠습니다.

---

### 1️⃣ FRED 홈페이지 접속 및 로그인

[FRED 홈페이지](https://fred.stlouisfed.org/)에 접속해 Google 계정으로 로그인합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/money/investment/2025-10-08-Management_7/fred_login.jpg" alt="FRED 홈페이지 로그인">

---

### 2️⃣ API Key 발급받기

로그인 후 **프로필 → My Account → API Keys** 메뉴로 이동합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/money/investment/2025-10-08-Management_7/account_api_key.jpg" alt="FRED API Key 메뉴">

“Request API Key” 버튼을 클릭합니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/money/investment/2025-10-08-Management_7/request_api_key.jpg" alt="Request API Key 버튼">

발급이 완료되면 아래처럼 고유의 API Key가 생성됩니다.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/money/investment/2025-10-08-Management_7/api_key.jpg" alt="FRED API Key 예시">

> ⚠️ 이 API Key는 앞으로 실시간 데이터를 가져올 때 필요하므로 반드시 저장해 두세요.

---

### 3️⃣ API Key 테스트

아래 URL에 본인의 키를 입력해 웹브라우저에서 정상적으로 JSON 데이터가 오는지 확인합니다.

```
https://api.stlouisfed.org/fred/series/observations?series_id=VIXCLS&api_key={YOUR_FRED_KEY}&file_type=json
```

정상이라면 아래처럼 데이터가 반환됩니다.

```json
{
  "realtime_start": "2025-10-08",
  "realtime_end": "2025-10-08",
  "observation_start": "1600-01-01",
  "observation_end": "9999-12-31",
  "units": "lin",
  "output_type": 1,
  "file_type": "json",
  "observations": [
    {
      "date": "1990-01-02",
      "value": "17.24"
    }
  ]
}
```

---

## 📈 Google Sheet에 실시간 반영하기

API Key를 발급받았으니 이제 **Google Sheet와 연결**해보겠습니다.

---

### 1️⃣ IMPORTJSON 함수 추가

Google Sheet를 열고  
**[확장 프로그램] → [Apps Script]**로 이동하여 아래 코드를 추가합니다.

```java
// --- IMPORTJSON 함수 정의 ---
function IMPORTJSON(url, query, headers) {
  try {
    var response = UrlFetchApp.fetch(url);
    var content = response.getContentText();
    var json = JSON.parse(content);
    var data = [];

    if (json.observations) {
      // FRED 형식일 때
      data.push(['date', 'value']);
      json.observations.forEach(function (obs) {
        data.push([obs.date, parseFloat(obs.value)]);
      });
    } else {
      // 일반 JSON용 fallback
      for (var key in json) {
        data.push([key, json[key]]);
      }
    }
    return data;
  } catch (e) {
    return [['Error', e]];
  }
}
```

> 💡 Google Sheet는 기본적으로 JSON 파일을 직접 불러오는 기능이 없기 때문에,  
> 위 함수를 추가해야 데이터를 변환할 수 있습니다.

---

### 2️⃣ 시트에 API 연동하기

이제 시트 셀에 아래 공식을 입력합니다.

- **VIX 지수**
```excel
=IMPORTJSON("https://api.stlouisfed.org/fred/series/observations?series_id=VIXCLS&api_key={YOUR_FRED_KEY}&file_type=json")
```

- **미국 10년물 금리**
```excel
=IMPORTJSON("https://api.stlouisfed.org/fred/series/observations?series_id=DGS10&api_key={YOUR_FRED_KEY}&file_type=json")
```

---

### 3️⃣ 최근 값만 가져오기

만약 최신 데이터 한 줄만 가져오고 싶다면 다음 함수를 사용합니다.  
(O 열: date, P 열: value 기준)

```excel
=INDEX(
  SORT(
    FILTER(
      {O2:O, P2:P},
      (P2:P <> "") * (P2:P <> ".") * (NOT(ISERROR(P2:P)))
    ),
    1, FALSE
  ),
  1, 2
)
```

---

### 4️⃣ 자동 새로고침 설정

자동으로 최신 데이터를 갱신하려면 아래 함수를 추가하세요.  
(Apps Script → 트리거 ⏱ 아이콘 → “autoUpdate”를 1시간마다 실행으로 설정)

```java
function autoUpdate() {
  SpreadsheetApp.getActiveSpreadsheet().getSheets().forEach(function(sheet) {
    sheet.getRange('O1').setValue('=IMPORTJSON("https://api.stlouisfed.org/fred/series/observations?series_id=VIXCLS&api_key={YOUR_FRED_KEY}&file_type=json")');
  });
}
```

> ✅ 이렇게 설정하면 매시간 자동으로 시트가 최신 데이터로 갱신됩니다.

---

## 🧾 마무리

이제 **FRED API → Google Sheet → 실시간 업데이트**의 완전 자동화 파이프라인이 완성되었습니다.  
이를 활용하면 매일 수동으로 데이터를 입력할 필요 없이, 리스크 신호나 경제지표를 실시간으로 추적할 수 있습니다.

---

> “An investment in knowledge pays the best interest.” – Benjamin Franklin  
> (지식에 대한 투자는 최고의 이자를 낳는다.)
