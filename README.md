# 미국주식 무료 공식 데이터 출처 (한국어 정리)

미국 주식·금리·공시 데이터를 **무료로, 공식 기관에서 직접** 받을 수 있는 곳을 모았습니다.
유료 단말이나 비공식 API(약관이 불분명한 것) 없이, 기관이 공개한 파일·API만 넣었습니다.

- 마지막 확인: **2026-09-26** — 아래 주소는 모두 이날 직접 호출해 응답(HTTP 200)을 확인했습니다. 예외는 상원 공시 검색 한 곳으로, 브라우저에서 이용 동의를 해야 열립니다.
- 틀린 곳이나 더 넣을 곳이 있으면 이슈로 알려 주세요.

## 금리·거시

| 무엇 | 어디서 | 형식 | 메모 |
|---|---|---|---|
| 미 국채 수익률 곡선(일별, 1개월~30년) | [미 재무부 Daily Treasury Par Yield Curve Rates](https://home.treasury.gov/resource-center/data-chart-center/interest-rates/TextView?type=daily_treasury_yield_curve) | CSV | 연도별 CSV: `.../daily-treasury-rates.csv/2026/all?type=daily_treasury_yield_curve&field_tdr_date_value=2026&page&_format=csv` |
| 미 국채 실질금리(TIPS) 곡선 | 같은 페이지, `type=daily_treasury_real_yield_curve` | CSV | 명목 − 실질 = 기대인플레이션(브레이크이븐) 계산에 씀 |
| 실효 연방기금금리(EFFR) | [뉴욕 연은 Markets API](https://markets.newyorkfed.org/api/rates/unsecured/effr/last/1.json) | JSON | 인증 없음 |
| FOMC 회의 일정 | [연준 FOMC Calendars](https://www.federalreserve.gov/monetarypolicy/fomccalendars.htm) | 웹 | 성명·의사록 날짜 포함 |
| GDP·PCE 발표 일정 | [BEA Release Schedule](https://www.bea.gov/news/schedule) | 웹 | 발표 시각은 미 동부 08:30 |
| 고용·CPI 발표 일정 | [BLS Release Calendar](https://www.bls.gov/schedule/) | 웹 | 자동 수집 요청은 차단된다(브라우저로 확인) |

## 거래·수급

| 무엇 | 어디서 | 형식 | 메모 |
|---|---|---|---|
| 일별 공매도 거래량(Reg SHO, 종목별) | FINRA Query API — `POST https://api.finra.org/data/group/otcMarket/name/regShoDaily` | JSON | 인증 없음. 본문에 `compareFilters`(종목·날짜)를 넣는다 |
| 같은 데이터의 일별 파일 | FINRA CDN — `https://cdn.finra.org/equity/regsho/daily/CNMSshvolYYYYMMDD.txt` | 텍스트(파이프 구분) | 날짜만 바꿔 받는다 |
| VIX 일별 역사 | [Cboe VIX_History.csv](https://cdn.cboe.com/api/global/us_indices/daily_prices/VIX_History.csv) | CSV | 1990년부터 |

FINRA 숫자를 볼 때 주의할 점: Reg SHO 파일의 «전체 거래량»은 FINRA에 보고된 **장외 체결분**만입니다. 흔히 말하는 «다크풀 비중»은 이 값을 **전체(통합) 거래량**으로 나눈 것이라, 분모를 따로 구해야 합니다. «공매도 거래량»도 공매도 **잔고**와는 다른 숫자입니다.

## 기업·공시

| 무엇 | 어디서 | 형식 | 메모 |
|---|---|---|---|
| 기업별 공시 목록(10-K·10-Q·8-K 등) | [SEC EDGAR submissions API](https://data.sec.gov/submissions/CIK0000723125.json) (예: 마이크론) | JSON | 요청 헤더 User-Agent 에 연락처를 넣어야 한다(SEC 규칙) |
| 티커 ↔ CIK 대응표 | [SEC company_tickers.json](https://www.sec.gov/files/company_tickers.json) | JSON | 위 API 에 넣을 CIK 를 여기서 찾는다 |
| 나스닥 상장 종목 목록 | [Nasdaq Trader Symbol Directory](https://www.nasdaqtrader.com/dynamic/SymDir/nasdaqlisted.txt) | 텍스트 | 매일 갱신 |

## 미 의회 의원 주식 거래 공시

| 무엇 | 어디서 | 메모 |
|---|---|---|
| 하원 거래 보고(PTR) | [House Clerk Financial Disclosure](https://disclosures-clerk.house.gov/FinancialDisclosure) | 연도별 검색·다운로드 |
| 상원 거래 보고 | [Senate eFD Search](https://efdsearch.senate.gov/search/) | 브라우저에서 이용 동의 후 검색(자동 요청은 403) |

## 이 데이터로 만든 것 (제작자 공개)

이 목록을 만든 사람이 위 출처로 매일 만드는 공개 데이터와 앱입니다.

- [options-market-structure-daily](https://github.com/myjr0629-hue/options-market-structure-daily) — 12개 종목 옵션 구조 일별 스냅샷(JSON·Markdown)
- [FINRA 공매도 거래량 데이터셋](https://myjr0629-hue.github.io/options-market-structure-daily/finra-short-volume.html) — 위 FINRA API 를 날짜별로 정리
- [미 의회 주식 거래 요약](https://myjr0629-hue.github.io/options-market-structure-daily/congress.html)
- SIGNUM HQ 앱 — 위 데이터(다크풀 비중·옵션 구조·금리·공시)를 종목별로 한 화면에 보여 줍니다. 무료, iPhone·Android: https://signumhq.com/app?from=github

---

라이선스: 이 목록 문서는 CC0(자유 이용). 각 데이터의 이용 조건은 해당 기관의 규정을 따릅니다.
