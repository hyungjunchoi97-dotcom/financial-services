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

3. Build a structured Excel workbook with the following sheets:
   - **Sheet 1: Summary** — 핵심 재무지표 한눈에 보기 (매출, 영업이익, 순이익, EBITDA, 총자산, 부채비율)
   - **Sheet 2: P&L (손익계산서)** — 매출, 매출원가, 매출총이익, 판관비, 영업이익, EBITDA, 세전이익, 순이익 / YoY or QoQ % 변화율 포함
   - **Sheet 3: BS (재무상태표)** — 유동자산, 비유동자산(유형자산/무형자산 세분화), 총자산, 유동부채, 비유동부채, 자본 / YoY or QoQ % 변화율 포함
   - **Sheet 4: CF (현금흐름표)** — 영업활동/투자활동/재무활동 현금흐름, FCF(영업CF - CAPEX) / YoY or QoQ % 변화율 포함
   - **Sheet 5: Detail — PP&E / Intangibles** — 유형자산 취득원가/감가상각누계/장부가액, 무형자산 동일, 당기 감가상각비, 당기 무형자산상각비
   - **Sheet 6: Key Ratios** — 수익성(ROE/ROA/영업이익률/순이익률), 안정성(부채비율/유동비율/이자보상배율), 성장성(매출증가율/영업이익증가율), 밸류에이션 기초(EPS/BPS)

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
