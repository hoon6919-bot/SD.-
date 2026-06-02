# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

단일 HTML 파일로 구성된 바닐라 계산기 웹 앱. 빌드 도구, 의존성, 서버 없이 브라우저에서 직접 열어 실행한다.

## Running the App

```powershell
start "C:\Users\SAMSUNG\Desktop\AI_AGENT\클로드코드\calculator.html"
```

또는 파일 탐색기에서 `calculator.html`을 더블클릭.

## Architecture

모든 코드가 `calculator.html` 한 파일에 집약되어 있으며, 세 영역으로 나뉜다:

- **CSS (`.calculator`, `.display`, `.buttons`)** — 다크 테마, 퍼플 linear-gradient 연산자 버튼. 버튼 타입별 클래스: `btn-num`, `btn-fn`, `btn-op`, `btn-eq`.
- **HTML** — 4×5 CSS Grid 레이아웃. `0` 버튼은 `grid-column: span 2`.
- **JS (인라인 `<script>`)** — 프레임워크 없는 순수 JS. 상태는 4개 변수로 관리:
  - `current` — 현재 입력값 (문자열)
  - `previous` — 연산자 누르기 전 피연산자
  - `operator` — 현재 선택된 연산자 (`'+'`, `'−'`, `'×'`, `'÷'`)
  - `shouldReset` — 다음 숫자 입력 시 `current`를 덮어쓸지 여부

핵심 흐름: `input()` → `setOperator()` → `calculate()`. 연산자를 연속으로 누르면 `calculate(chain=true)`가 호출되어 중간 결과로 체이닝된다.

## Communication Rules

- 결과값과 설명은 무조건 **한글**로 작성한다. (코드 내 주석, 커밋 메시지 등 모든 출력 포함)

## Design Constraints

- 외부 라이브러리·폰트·CDN 미사용 — 오프라인에서도 동작해야 함
- 디자인 언어: 다크 배경(`#0f0f0f`), 퍼플 계열(`#6c63ff`, `#a78bfa`) 액센트, linear-gradient 버튼
- 최대 입력 자릿수 12자리, 부동소수점 오차는 `toFixed(10)` + `parseFloat`으로 제거
