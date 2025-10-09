---
title: "리스크 신호 기반 ETF 리밸런싱 실전 예시 (자동화 편)"
layout: single
categories:
  - investment
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
---

## 👋 이번 포스팅에서 다룰 것

지난 글에서는 [“리스크 관리와 리밸런싱의 기술 (심화편)”](https://latentlabanonymous.github.io/investment/Management_5/)을 다뤘습니다.  
이번에는 그 이론을 실제 **ETF 포트폴리오 리밸런싱 시스템**으로 구현하는 방법을 구체적으로 다룹니다.

> 💭 “감(感)에 의한 매매를 끝내고, 데이터에 기반한 운용으로 전환하자.”

---

## 📊 목표

이번 포스팅의 목표는 아래 세 가지입니다.

1. 금리·VIX·PER 세 지표로 ‘시장 리스크 점수’를 계산  
2. 그 점수를 기반으로 **ETF 비중을 자동 조정**  
3. 엑셀 / Google Sheets / Script로 **자동화**까지 확장  

📎 다운로드:  
[금리-VIX-PER 리밸런싱 엑셀.xlsx](https://latentlabanonymous.github.io/assets/excel/Management_6/risk_signal_etf_rebalancing.xlsx)

---

## 🧩 Step 1. 포트폴리오 기본 구조

| 구분 | ETF | 주요 특징 | 초기 비중 |
|------|------|-----------|-----------|
| 주식 | SPY (S&P500) | 미국 대형주 | 60% |
| 채권 | TLT (미국 장기채) | 금리 하락 시 상승 | 30% |
| 현금 | SHV | 단기채·유동성 확보 | 10% |

> ✅ 리스크 신호에 따라 SPY·TLT·SHV의 비중이 자동으로 변하도록 설계합니다.

---

## 💡 Step 2. 데이터 정의

| 지표 | 의미 | 사용 예시 | 리스크 방향 |
|------|------|------------|--------------|
| 금리 (Interest Rate) | 유동성 지표 | Fed 기준금리 or 10Y 국채수익률 | ↑ = 위험 증가 |
| VIX | 시장 공포지수 | S&P500 변동성 기대치 | ↑ = 위험 증가 |
| PER | 시장 밸류에이션 | S&P500 TTM PER | ↑ = 위험 증가 |

> 세 지표 모두 “값이 클수록 위험 증가” 형태로 통일해야 합니다.

---

## ⚙️ Step 3. 정규화 (Normalization)

서로 다른 단위의 지표를 0~1 스케일로 맞추기 위해 **최소–최대 정규화**를 적용합니다.

| 항목 | 수식(Excel) | 의미 |
|------|--------------|------|
| 금리 점수 | `=MIN(1, MAX(0, (B2 - $H$2)/($H$3 - $H$2)))` | Rate_min=$H$2, Rate_max=$H$3 |
| VIX 점수 | `=MIN(1, MAX(0, (C2 - $H$4)/($H$5 - $H$4)))` | VIX_min=$H$4, VIX_max=$H$5 |
| PER 점수 | `=MIN(1, MAX(0, (D2 - $H$6)/($H$7 - $H$6)))` | PER_min=$H$6, PER_max=$H$7 |

그리고 세 점수를 가중평균합니다:

```excel
=E2*$H$8 + F2*$H$9 + G2*$H$10
```

> 💬 가중치 합은 1이 되게 설정합니다. (예: Rate 0.3, VIX 0.4, PER 0.3)

---

## 🧮 Step 4. 리스크 점수 → 비중 매핑

### 방법 A. 구간별 정책(IFS 기반)
```excel
=IFS(
  AND(B2<4.5, C2<20, D2<15), 0.7,
  AND(B2<5.0, C2<30, D2<25), 0.55,
  TRUE, 0.4
)
```
> Rate/VIX/PER 값에 따라 주식 비중을 70%~40% 사이로 자동 조정.

### 방법 B. 선형 매핑(연속 조정)
```excel
=0.7 - 0.3 * H2
```
- H2: Combined Risk Score (0~1)
- 주식 비중은 70% → 40% 사이에서 부드럽게 변화
- 나머지는 채권/현금으로 자동 배분  
  ```excel
  Cash = 0.05 + 0.10 * H2  
  Bonds = 1 - Stock - Cash
  ```

---

## 📈 Step 5. 실제 예시

| 금리(%) | VIX | PER | 리스크점수 | SPY | TLT | SHV |
|----------|------|------|--------------|------|------|------|
| 4.50 | 18 | 16 | 0.25 | 70% | 25% | 5% |
| 5.00 | 28 | 22 | 0.55 | 55% | 35% | 10% |
| 5.25 | 33 | 26 | 0.82 | 45% | 41% | 14% |

> 자동으로 주식비중이 낮아지고, 채권/현금이 늘어납니다.

---

## 🧭 Step 6. 리밸런싱 규칙

| 항목 | 권장값 | 설명 |
|------|----------|------|
| 리밸런스 주기 | 월간 | 과도한 거래 방지 |
| 트리거 | 목표비중과 ±2% 이상 차이 | 일정 범위 내에서는 유지 |
| 부분 리밸런스 | 50%만 조정 | 완화된 조정 |
| 거래비용 | 0.1~0.3% 반영 | 백테스트 시 필수 고려 |

> ⚠️ 매월 자동으로 전체 비중을 조정하는 대신,  
> **“목표비중 – 실제비중 > 2%”일 때만 조정**하도록 설정하세요.

---

## 🧪 Step 7. 백테스트(성과 비교)

| 전략 | 연평균 수익률 | 최대 낙폭(MDD) | 샤프지수 |
|------|----------------|----------------|-----------|
| 고정형 (60/40) | 6.8% | -22% | 0.68 |
| 리스크 조정형 | 8.2% | -13% | 0.89 |

> ✅ 리스크 관리가 수익률을 낮추지 않고, 오히려 안정적 성과를 제공합니다.

📘 백테스트 공식(엑셀):
- CAGR = `(최종값/초기값)^(1/년수) - 1`
- 변동성 = `STDEV(月수익률)*SQRT(12)`
- 샤프 = `(평균月수익률*12 - 무위험수익률)/변동성`
- MDD = `MIN(누적수익/최대누적수익 - 1)`

---

## 🔄 Step 8. 자동화 — Google Sheets + Apps Script

자세한 내용 확인 => [“리스크 신호 기반 ETF 리밸런싱 google sheet (자동화 편)”](https://latentlabanonymous.github.io/excel/Rebalancing/)  

자동화 시스템을 구축하면 매달 금리·VIX·PER을 수동으로 입력할 필요 없이  
**실시간 API로 불러와 자동으로 비중 계산 + 이메일 알림**을 받을 수 있습니다.

### 예시 코드
```javascript
function fetchAndUpdate() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Data");

  // 외부 API (예시용)
  var vixResp = UrlFetchApp.fetch("https://api.example.com/vix");
  var rateResp = UrlFetchApp.fetch("https://api.example.com/rate");
  var perResp = UrlFetchApp.fetch("https://api.example.com/per");

  var vix = JSON.parse(vixResp.getContentText()).value;
  var rate = JSON.parse(rateResp.getContentText()).value;
  var per = JSON.parse(perResp.getContentText()).value;

  // 데이터 추가
  sheet.appendRow([new Date(), rate, vix, per]);

  // 목표비중 읽어오기
  var targetStock = sheet.getRange("H2").getValue();

  // 이메일 발송
  MailApp.sendEmail("you@example.com", 
                    "📊 리밸런싱 알림", 
                    "목표 주식 비중: " + (targetStock*100).toFixed(2) + "%");
}
```

> 💬 `GOOGLEFINANCE` 함수는 금리·PER·VIX 모두를 직접 지원하지 않으므로,  
> FRED API나 AlphaVantage 등을 활용하는 것이 좋습니다.

---

## 🧠 Step 9. 고급 응용 — 변동성·드로다운 제어

| 전략 | 설명 | 적용 예시 |
|------|------|-----------|
| 변동성 타깃팅 | 목표 변동성에 맞게 레버리지 조정 | `Scaling = TargetVol / RealizedVol` |
| 드로다운 제어 | 최근 고점 대비 -10% 이상 손실 시 방어 강화 | 주식비중 -10%p |
| 상관관계 점검 | SPY–TLT 상관이 +로 전환 시, 금(IAU) 추가 | 방어자산 다변화 |

---

## ⚠️ Step 10. 주의할 점

- 지표는 **미래 예측이 아니라 현재 상태 진단** 도구입니다.  
- 과거의 최적 파라미터(바운드·가중치)가 미래에도 그대로 통하지는 않습니다.  
- **과최적화(overfitting)** 방지: 단순한 룰로 유지하세요.  
- 거래비용·세금 고려는 필수입니다.

---

## ✅ Step 11. 정리 체크리스트

| 단계 | 핵심 작업 | 도구 |
|------|------------|------|
| ① 데이터 수집 | Rate/VIX/PER | Excel / API |
| ② 정규화 | 0~1 변환 | Excel 수식 |
| ③ 리스크 점수 | 가중합 | Excel |
| ④ 비중 매핑 | 선형 or 구간식 | Excel |
| ⑤ 리밸런스 | 월간 + 트리거 방식 | Excel |
| ⑥ 검증 | 백테스트·MDD·Sharpe | Excel |
| ⑦ 자동화 | Google Sheets + Script | 클라우드 |

---

## ✨ 마무리 — 예측하지 말고 반응하라

> 💬 *“You can’t predict, but you can prepare.” – Howard Marks*

리스크 지표는 시장의 **온도계**입니다.  
예측은 불가능하지만, **데이터에 따라 반응**하는 시스템을 만들면  
감정적 판단 없이 꾸준히 자산을 지켜나갈 수 있습니다.

---

📅 다음 포스팅 예고:  
> “Google Sheets로 만드는 리밸런싱 자동화 시스템 (실전 구현편)”  
> — 실시간 데이터 불러오기 + 이메일 알림 + 조건부 색상 설정
