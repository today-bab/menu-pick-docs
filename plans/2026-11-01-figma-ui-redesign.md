# Figma 화면 리디자인 작업 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 설계 문서의 화면 8종과 그 화면을 이루는 컴포넌트를 Figma 파일에 만든다.

**Architecture:** 빈 파일에 토큰(변수·텍스트 스타일) → 컴포넌트 → 화면 순서로 쌓는다. 화면은 컴포넌트 인스턴스만으로 조립하고 색·간격·모서리는 변수에 묶는다. 코드가 아니므로 "테스트"는 각 작업 끝의 스크린샷 검수로 대신한다.

**Tech Stack:** Figma MCP (`use_figma` — Plugin API, `get_screenshot`, `get_metadata`)

**Spec:** [`design/ui-redesign.md`](../design/ui-redesign.md)

**대상 파일:** `todaybab 디자인` — fileKey `LQF0ULuZ1qdHVaVmC27u5J` (2026-11-01 기준 빈 `Page 1` 하나, 연결된 라이브러리 없음)

## Global Constraints

- 모바일 프레임 390 × 844, PC 프레임 1440 × 900
- 라이트 모드만
- 주황 `#FF6B00` 은 뽑기 버튼, 선택된 가게(글자), 선택 핀, 떠 있는 버튼의 숫자에만
- 버튼 모서리 14, 시트 모서리 20 (위 두 모서리만)
- 글꼴 Pretendard. Figma에서 쓸 수 없으면 `Noto Sans KR`로 대신하고 보고한다
- 메뉴 이름 28 Bold, 그 밖의 글자는 11–15
- 문구는 지금 코드 그대로: 버튼 `메뉴 뽑기` / `다시 뽑기` / `위치 찾는 중…` / `고르는 중…`, 라벨 `오늘의 메뉴`, 서비스 이름 `오늘 뭐 먹지`, 오류 문구는 `menu-pick-frontend/src/App.tsx`의 `locationNotices`, `errorNotices`
- 이전/다음 화살표는 어디에도 그리지 않는다 (기록 버튼으로 합침)
- Figma에는 git이 없으므로 **작업 하나 = 검수 지점 하나**다. 작업이 끝나면 스크린샷과 요약을 보고하고 사용자가 다음을 요청할 때까지 멈춘다

## Review Focus

- 긴 이름: 메뉴 이름이 10자 넘을 때(예: `차돌된장찌개정식`), 가게 이름이 20자 넘을 때 줄바꿈·말줄임이 깨지지 않는다 → 작업 5 검수에서 한 줄을 긴 이름으로 넣어 본다
- 가게가 1곳뿐일 때 시트가 빈 공간 없이 낮아진다 → 작업 5에 1곳짜리 변형 하나
- 하단 툴바·safe area: 뽑기 버튼 아래 최소 24 여백 → 모든 모바일 화면 검수에서 확인
- 제외가 많을 때 제외 시트 칩이 여러 줄로 넘쳐도 시트 안에서 스크롤 → 작업 6에 대분류 4개 이상
- 숫자 0일 때 떠 있는 버튼에 숫자가 숨겨진다 → 작업 2 컴포넌트 변형, 작업 4 첫 화면에서 사용

---

### 작업 1: 페이지와 토큰

**만드는 것:**
- 페이지 이름: `Page 1` → `🎨 Foundations`, 새 페이지 `🧩 Components`, `📱 Screens`
- 색 변수 컬렉션 `color`: `orange/500 #FF6B00`, `orange/50 #FFF3EA`, `gray/900 #191F28`, `gray/700 #4E5968`, `gray/500 #8B95A1`, `gray/200 #E5E8EB`, `gray/100 #F2F4F6`, `white #FFFFFF`, `map/bg #F4F7F2`, `map/road #FFFFFF`, `blue/500 #3B82F6`, `scrim #000000 40%`
- 숫자 변수 컬렉션 `size`: `space/4,8,12,16,20,24`, `radius/button 14`, `radius/sheet 20`, `radius/chip 999`
- 텍스트 스타일: `menu/title 28 Bold`, `body/strong 15 SemiBold`, `body 15 Regular`, `caption 12 Regular`, `label 11 Medium`, `button 16 Bold`
- `🎨 Foundations`에 색 견본과 글자 견본 프레임

- [ ] 사용 가능한 글꼴에서 Pretendard 확인 (`figma.listAvailableFontsAsync`), 없으면 Noto Sans KR로 결정
- [ ] 페이지·변수·텍스트 스타일 생성
- [ ] 견본 프레임 생성
- [ ] 검수: `🎨 Foundations` 스크린샷, 변수 목록 출력 — 위 값과 일치
- [ ] 보고 후 멈춤

### 작업 2: 작은 컴포넌트 (버튼·떠 있는 버튼·칩)

**Interfaces — Produces:** 아래 컴포넌트 세트 (이후 작업은 이 이름으로 인스턴스를 만든다)
- `PickButton` — `state=default|loading`, 텍스트 속성 `label`. 높이 52, 가로 채움, `orange/500`, `radius/button`, loading은 투명도 60%
- `MapFab` — `count=none|some`, 텍스트 속성 `label`(`기록`/`제외`), `value`. 흰 배경, 그림자, 숫자만 주황
- `Chip` — `kind=menu|cuisine`, `excluded=false|true`. 제외는 `gray/100` 배경, `gray/500` 글자, 취소선. cuisine은 SemiBold, 글자 `○○ 전체`

- [ ] `🧩 Components`에 세 세트 생성, 모든 색·모서리 변수 연결
- [ ] 검수: 각 세트 스크린샷 — 변형 모두 보이고 주황은 버튼 배경·숫자에만
- [ ] 보고 후 멈춤

### 작업 3: 목록·지도·시트 컴포넌트

**Interfaces — Consumes:** 작업 2의 `PickButton`
**Produces:**
- `RestaurantRow` — `selected=false|true`, 텍스트 `name`, `distance`, 오른쪽 `카카오맵` 작은 링크. 선택 시 이름만 주황 SemiBold. 높이 48, 위 구분선 `gray/100`
- `HistoryRow` — `current=false|true`, 텍스트 `menu`, `meta`(`가게 N곳`). current는 `orange/50` 배경 + 메뉴 주황
- `MapPin` — `selected=false|true` (선택: 주황 16 원 + 흰 테두리 3, 기본: `gray/500` 10 원)
- `MeDot` — 파란 점 + 연한 원
- `MapCanvas` — 390 × 844 지도 대용 배경 (격자 + 흰 도로 2–3줄). 실제 지도 이미지 아님
- `BottomSheet` — 손잡이(36 × 4, `gray/200`) + 내용 슬롯 + 하단 `PickButton` 슬롯. 위 모서리 `radius/sheet`, 하단 여백 24
- `OverlaySheet` — 제목 + 닫기(✕) + 내용 슬롯, 뒤에 `scrim`

- [ ] 생성 및 변수 연결
- [ ] 검수: 스크린샷 — 행 높이·구분선, 핀 두 가지 구분이 분명
- [ ] 보고 후 멈춤

### 작업 4: 화면 1·2 — 첫 화면과 로딩

**Consumes:** 작업 2, 3 전부
- `01 첫 화면`: `MapCanvas` + `MeDot`, 왼쪽 위 `오늘 뭐 먹지` 라벨, 오른쪽 위 `MapFab`(기록, count=none)·(제외, count=none). 접힌 시트: `근처 1km 안에서 골라 드려요` + `PickButton` `메뉴 뽑기`
- `02a 위치 찾는 중`: 01과 같고 버튼 `위치 찾는 중…`(loading)
- (`02b 다시 뽑는 중`은 03이 있어야 해서 작업 5에서 만든다)

- [ ] `📱 Screens`에 두 프레임 생성
- [ ] 검수: 스크린샷 — 결과 없이도 지도가 화면 전체
- [ ] 보고 후 멈춤

### 작업 5: 화면 3 — 결과 (2026-11-01 변경: 펼침 단계 없앰, 목록은 시트 안 스크롤)

- `03 결과`: 시트 높이 약 절반(≈420). 위 고정 `오늘의 메뉴`·`김치찌개`, 가운데 스크롤 영역(세로 스크롤, clip)에 `RestaurantRow` 8개(첫 줄 selected) — 넘친 줄이 잘려 보여 스크롤된다는 걸 알 수 있게, 아래 고정 `다시 뽑기`. 지도에 `MapPin` selected 1 + 기본 3, `MeDot`. `MapFab` 기록 3, 제외 2. 지도의 핀은 시트 위쪽(지도가 보이는 영역)에 둔다
- `03-긴이름`: 메뉴 `차돌된장찌개정식`, 가게 이름 20자 이상 한 줄 — 말줄임 확인용
- `03-1곳`: 가게 1곳, 시트가 그만큼 낮아짐
- `02b 다시 뽑는 중`: 03을 복제해 결과를 흐리게(투명도 40%) + 버튼 `고르는 중…`(loading)
- 스크롤 영역은 `BottomSheet` 슬롯 안에 고정 높이 프레임(clipsContent, `overflowDirection='VERTICAL'`)으로 넣는다

- [ ] 생성
- [ ] 검수: 스크린샷 — 메뉴·가게 여러 곳·버튼이 한 화면에 보이고 지도도 절반 남음, Review Focus 긴 이름·1곳 확인
- [ ] 보고 후 멈춤

### 작업 6: 화면 5·7 — 제외 시트, 기록 시트

- `05 제외 설정`: 03 위에 `OverlaySheet` `제외할 메뉴`. 대분류 4개 이상(한식·중식·일식·양식), 각 `Chip` cuisine + menu 칩 여러 개. 상태 섞기: 중식 전체 제외, 한식에서 국밥만 제외, 일식 통째 제외인데 초밥만 다시 넣음. 뽑기 버튼 가려짐
- `07 기록`: 03 위에 `OverlaySheet` `이번에 뽑은 메뉴`. `HistoryRow` 3개 최신순(맨 위 current), 메뉴 · `가게 N곳`

- [ ] 생성
- [ ] 검수: 스크린샷 — 칩 넘침 시 시트 안 스크롤, 제외 상태 3가지가 구분됨
- [ ] 보고 후 멈춤

### 작업 7: 화면 6 — 오류

- `06a 위치 권한 없음`: `MapCanvas`(기본 위치, `MeDot` 없음), 메뉴 자리에 `locationNotices.denied` 문구, 버튼 `메뉴 뽑기`
- `06b 근처에 가게 없음`: 메뉴 자리에 `errorNotices['no-restaurants']`
- `06c 다시 뽑다 실패`: 03 그대로 + 버튼 바로 위 얇은 안내 줄 `errorNotices.upstream` (`gray/700` 글자, `gray/100` 배경 — 빨강 쓰지 않음)

- [ ] 생성
- [ ] 검수: 스크린샷 — 위치 오류에서도 지도가 있음
- [ ] 보고 후 멈춤

### 작업 8: 화면 8 — PC

- `08 PC 결과`: 1440 × 900. `MapCanvas` 전체(늘림) + 왼쪽 고정 패널 폭 400, 패널 안 내용은 03의 시트 내용(손잡이 없음, 가게 목록 전부, 맨 아래 `다시 뽑기`). 지도 위 오른쪽 위 `MapFab` 두 개

- [ ] 생성
- [ ] 검수: 스크린샷
- [ ] `📱 Screens` 전체 스크린샷으로 화면 목록이 설계 문서 8종(+변형)과 맞는지 최종 확인
- [ ] 보고 후 멈춤
