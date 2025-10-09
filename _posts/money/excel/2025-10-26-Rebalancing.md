---
title: "리스크 신호 기반 ETF 리밸런싱 google sheet (자동화 편)"
layout: single
categories:
  - excel
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---
# Google Sheets + Apps Script 자동화 완성본

[“리스크 신호 기반 ETF 리밸런싱 실전 예시 (자동화 편)”](https://latentlabanonymous.github.io/investment/Management_6/)에 대한 google sheet 적용 글입니다.  

이 문서는 리스크 신호 기반 ETF 리밸런싱 자동화를 위한 **Google Sheets + Apps Script 전체 구성**입니다.  
API 자리표시자, 수식, 코드, 설정, 트리거 설정까지 모두 포함되어 있습니다.

---

## 1️⃣ Google Sheets 시트 구조

**시트 1: `Data` (원데이터 + 계산)**

| 열 | 항목 | 설명 |
|----|------|------|
| A | 월 | YYYY-MM 또는 날짜 |
| B | 금리(%) | 예: 5.25 |
| C | VIX | 변동성 지수 |
| D | PER | 밸류에이션 |
| E | 금리점수 | 수식 자동계산 |
| F | VIX점수 | 수식 자동계산 |
| G | PER점수 | 수식 자동계산 |
| H | 리스크점수 | 가중합 |
| I | SPY 비중 | 계산값 |
| J | TLT 비중 | 계산값 |
| K | SHV 비중 | 계산값 |
| L | 리밸런스 알림 | “Rebalance” 또는 “Hold” |

**시트 2: `Settings`**

| 셀 | 항목 | 예시값 |
|-----|------|--------|
| A1 | Rate_min | 0 |
| A2 | Rate_max | 6 |
| A3 | VIX_min | 10 |
| A4 | VIX_max | 40 |
| A5 | PER_min | 10 |
| A6 | PER_max | 30 |
| A8 | Rate_w | 0.3 |
| A9 | VIX_w | 0.4 |
| A10 | PER_w | 0.3 |
| A12 | Stock_high | 0.7 |
| A13 | Stock_low | 0.4 |
| A14 | Cash_base | 0.05 |
| A15 | Cash_scale | 0.10 |

---

## 2️⃣ Google Sheets 수식

다음 수식을 Data 시트의 E2~L2에 입력하고 아래로 드래그합니다.

```excel
E2 =MIN(1, MAX(0, (B2 - Settings!$A$1) / (Settings!$A$2 - Settings!$A$1)))
F2 =MIN(1, MAX(0, (C2 - Settings!$A$3) / (Settings!$A$4 - Settings!$A$3)))
G2 =MIN(1, MAX(0, (D2 - Settings!$A$5) / (Settings!$A$6 - Settings!$A$5)))
H2 =E2*Settings!$A$8 + F2*Settings!$A$9 + G2*Settings!$A$10
I2 =Settings!$A$12 - (Settings!$A$12 - Settings!$A$13) * H2
K2 =Settings!$A$14 + Settings!$A$15 * H2
J2 =1 - I2 - K2
L2 =IF(ABS(M2 - I2) > 0.02, "Rebalance", "Hold")
```

---

## 3️⃣ Apps Script 코드

Google Sheets 메뉴에서 **Extensions → Apps Script**를 열고, 아래 코드를 복사해 붙여넣으세요.

```javascript
/**
 * 리스크 기반 리밸런싱 자동화 - Apps Script 완성본
 * 1) 외부 API에서 금리, VIX, PER을 가져와 'Data' 시트에 추가
 * 2) 수식이 자동 계산됨
 * 3) 목표비중을 읽어와 이메일/슬랙으로 알림 발송
 */

const SETTINGS = {
  dataSheetName: "Data",
  settingsSheetName: "Settings",
  emailTo: "you@example.com",
  slackWebhookUrl: "",
  FRED_API_KEY: "YOUR_FRED_KEY",
  ALPHA_VANTAGE_KEY: "YOUR_ALPHA_KEY",
  FRED_10Y_SERIES: "DGS10"
};

function fetchAndUpdateAll() {
  try {
    const ss = SpreadsheetApp.getActiveSpreadsheet();
    const dataSheet = ss.getSheetByName(SETTINGS.dataSheetName);

    const rateVal = fetch10yFromFRED();
    const vixVal = fetchVIX();
    const perVal = fetchPER();

    const now = new Date();
    dataSheet.appendRow([Utilities.formatDate(now, Session.getScriptTimeZone(), "yyyy-MM"), rateVal, vixVal, perVal]);

    const lastRow = dataSheet.getLastRow();
    const targetStock = dataSheet.getRange("I" + lastRow).getValue();
    const targetBond  = dataSheet.getRange("J" + lastRow).getValue();
    const targetCash  = dataSheet.getRange("K" + lastRow).getValue();

    const subject = "📊 리밸런싱 알림: 목표 비중 업데이트";
    const body = Utilities.formatString(
      "Date: %s\nRate: %s\nVIX: %s\nPER: %s\nTarget - SPY: %s%%, TLT: %s%%, SHV: %s%%",
      Utilities.formatDate(now, Session.getScriptTimeZone(), "yyyy-MM-dd"),
      rateVal, vixVal, perVal,
      (targetStock*100).toFixed(2), (targetBond*100).toFixed(2), (targetCash*100).toFixed(2)
    );
    MailApp.sendEmail(SETTINGS.emailTo, subject, body);
  } catch (err) {
    MailApp.sendEmail(SETTINGS.emailTo, "리밸런싱 스크립트 오류 알림", err.message);
  }
}

function fetch10yFromFRED() {
  const url = "https://api.stlouisfed.org/fred/series/observations?series_id=" +
              SETTINGS.FRED_10Y_SERIES +
              "&api_key=" + SETTINGS.FRED_API_KEY +
              "&file_type=json&sort_order=desc&limit=1";
  const resp = UrlFetchApp.fetch(url);
  const json = JSON.parse(resp.getContentText());
  return parseFloat(json.observations[0].value);
}

function fetchVIX() {
  return ""; // 자리표시자
}

function fetchPER() {
  return ""; // 자리표시자
}

function createTimeDrivenTrigger() {
  ScriptApp.newTrigger("fetchAndUpdateAll").timeBased().everyDays(1).atHour(6).create();
}
```

---

## 4️⃣ 설정 순서

1. Sheets에서 `Data`, `Settings` 시트를 만든다.
2. Settings에 기준값/가중치 입력.
3. Data에 수식 입력.
4. Apps Script 코드 붙여넣기.
5. `SETTINGS` 블록 수정 (이메일, API 키 등).
6. `createTimeDrivenTrigger()` 실행 → 트리거 설치.
7. `fetchAndUpdateAll()` 또는 `testRun()`으로 테스트.

---

## 5️⃣ 보안 및 팁

- API 키는 Script Properties나 Secret Manager에 저장 권장.
- 오류 시 이메일 자동 알림.
- 무료 API 호출 한도 주의.
- 실제 주문 자동화는 비권장 (알림 목적용).

---

## ✅ 체크리스트

- [ ] 시트 구조 생성  
- [ ] Settings 입력  
- [ ] 수식 복사  
- [ ] Apps Script 붙여넣기  
- [ ] API 키/이메일 수정  
- [ ] 트리거 실행  
- [ ] 테스트 성공 확인  
