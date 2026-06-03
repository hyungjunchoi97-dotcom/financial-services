---
name: dart-financials
description: DART에서 기업 재무데이터를 가져와 BS/PL/CF + 주요 재무항목을 연도별(YoY) 또는 분기별(QoQ) Excel로 출력
argument-hint: "[기업명] [시작연도-종료연도] [yoy|qoq]"
---

Load the `dart-financials` skill and execute the following workflow:

1. Parse the argument in the format: `기업명 시작연도-종료연도 yoy|qoq`
   - Example: `카카오 2015-2025 yoy` or `삼성전자 2020-2025 qoq`
   - If arguments are missing, ask the user for: 기업명, 기간(시작-종료), 주기(yoy/qoq)

2. Use the DART MCP to fetch the following data for the specified company and period:
   - 재무상태표 (Balance Sheet)
   - 손익계산서 (Income Statement / P&L)
   - 현금흐름표 (Cash Flow Statement)
   - 주석 데이터: 유형자산, 무형자산, 감가상각비, 상각비(EBITDA 조정용)

3. Build a structured Excel workbook with **8 sheets (재무제표 4 + 주석 4) by default**:
   - **Sheet ① Summary** — 핵심 재무지표 + YoY/QoQ% + 주요비율(부채비율/이익률 등)
   - **Sheet ② P&L (손익계산서)** — 매출~순이익, EBITDA / YoY or QoQ % 포함
   - **Sheet ③ BS (재무상태표)** — 유동/비유동(유형·무형자산 세분화), 부채, 자본 / 변화율 포함
   - **Sheet ④ CF (현금흐름표)** — 영업/투자/재무활동, CAPEX, FCF / 변화율 포함
   - **Sheet ⑤ 주석_자산** — 유형자산·무형자산 증감표(기초/취득/처분/상각/기말), 투자지분(자회사 지분율·장부가)
   - **Sheet ⑥ 주석_부채자본** — 차입금(이자율/만기/담보), 충당부채, 퇴직급여, 자본금, 결손금
   - **Sheet ⑦ 주석_주주_SO** — 주주현황, 스톡옵션 변동, 특수관계자, 우발채무
   - **Sheet ⑧ 주석_부가가치** — 급여·감가상각 등 비용 상세
   - 주석 데이터는 감사보고서에 있으면 **반드시 모두 시트로 만든다 (디폴트)**.

4. Excel formatting rules:
   - 헤더 행: 진한 배경 (네이비 #1F3864), 흰색 텍스트
   - 기간 열: 좌측 고정, 항목명 열 너비 자동조정
   - 숫자: 천단위 콤마, 단위는 억원(KRW) 또는 백만원 — 데이터 규모에 따라 자동 선택
   - 증가율(%) 셀: 양수 초록(#00B050), 음수 빨강(#FF0000)
   - YoY/QoQ 변화율 행은 회색 배경으로 구분
   - 각 시트 상단에 기업명, 기간, 단위 표시

5. Save the file as: `{기업명}_재무제표_{시작연도}-{종료연도}_{yoy|qoq}.xlsx`
   and place it in the user's current working directory or Desktop.

6. After completion, print a summary table in the chat showing:
   - 최근 5개 기간의 매출 / 영업이익 / 순이익 / EBITDA / 부채비율
