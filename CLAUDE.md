# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개요

`d6-api/` 폴더에 있는 한국어 단일 파일 HTML 화면 모음입니다. 브라우저에서 API 키가 필요 없는 무료 공개 API를 직접 호출합니다. 빌드 단계, 패키지 매니저, 테스트, git 저장소가 없습니다. `.html` 파일을 브라우저에서 바로 열면 실행됩니다. 각 파일이 CSS와 JS를 안에 모두 가지고 있고, 화면끼리 코드를 공유하지 않습니다.

| 화면 | 사용하는 API |
| --- | --- |
| `weather.html` | Open-Meteo 예보 (서울 기온 + 날씨 코드) |
| `city.html` | Open-Meteo 지오코딩 + 예보 + 대기질 (도시 검색) |
| `rate.html` | Frankfurter (선택한 통화 → 원화) |
| `morning.html` | 종합 화면: Open-Meteo 날씨·대기질, Frankfurter 환율, Nager.Date 한국 공휴일, Dog CEO |

## 모든 API 화면이 지키는 규칙

`morning.html`이 가장 완성도 높은 기준 예시입니다. 화면을 새로 만들거나 고칠 때 이 파일을 따르세요.

- **엔드포인트는 스크립트 맨 위 `const API = {...}` 객체 한 곳에 모읍니다.** 각 줄에 `// 🔌` 주석을 답니다. 모든 `fetch`에도 `// 🔌 API 호출` 주석을 붙입니다. 다른 곳에 URL을 직접 적지 않습니다.
- **화면 맨 아래에 "이 화면이 쓰는 API" 상자**를 둡니다. `API`에서 만든 엔드포인트 목록과 "새 탭에서 원본 JSON 보기" 링크를 보여 줍니다.
- **실패 처리**: 모든 `fetch`는 try/catch(또는 `Promise.allSettled`)로 감싸고, 사람이 읽을 수 있는 한국어 안내 문구를 보여 줍니다. 카드는 각각 따로 불러와서 하나가 실패해도 화면 전체가 비지 않게 합니다. 파싱하기 전에 `res.ok`를 확인합니다.
- **출처와 날짜**: 출처 문구를 표시하고(Open-Meteo는 "Weather data by Open-Meteo.com" 표기 필수), 데이터의 기준 날짜·시각을 보여 줍니다. "실시간"이라는 말은 쓰지 않습니다(Frankfurter는 하루 한 번 발표하는 값이고, 대기질은 모델 값입니다).
- **URL에 API 키를 넣지 않습니다** (`key=`, `apikey=`, `token=`). 키가 필요 없는 API만 씁니다.
- API에서 받은 문자열은 `innerHTML`에 넣기 전에 `esc()`로 이스케이프합니다.
- 스타일: `:root` CSS 변수와 `prefers-color-scheme: dark` 덮어쓰기, 가운데 정렬된 `main`(최대 너비 약 420px), 모바일 우선.
- 미세먼지 등급은 한국 환경부 기준 값을 씁니다(`morning.html`의 `PM_GRADES` 참고).

## 프로젝트 도구 (`.claude/`)

- `.claude/skills/api-check/SKILL.md` — `/api-check [파일.html]`로 화면이 위 규칙을 지켰는지 검사합니다. 검사 항목은 🔌 표시, API 상자, 실패 처리, 출처·날짜, API 키 노출입니다. 고치지 않고 표로 보고만 합니다. 규칙은 `CLAUDE.md`에서 읽으므로 위 규칙을 최신으로 유지하세요. 새로 배운 교훈은 `## 실수에서 배운 규칙` 섹션을 만들어 추가합니다.
- `.claude/rules/api.md`는 현재 비어 있습니다.
