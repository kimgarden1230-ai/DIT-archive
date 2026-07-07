# DIT Archive — Version History

---

## v3.5
**시작일**: 2026-07-07  
**파일**: `DIT-archive-v3.5.html`  
**상태**: 🟢 진행 중

> v3.4에서 이어지는 새 버전. 변경사항은 여기에 기록 예정.

---

## v3.4
**파일**: `DIT-archive-v3.4.html`  
**상태**: ✅ 완료

### 주요 기능
- 타임라인 날짜 기반 데이터 구조로 전환 (`tripDays[]`)
- 모달 미니맵 — Leaflet 경로 polyline + 사진 썸네일 마커
- 타임라인 카드 캐러셀 + 슬라이더 연동
  - CSS scroll-snap → transform 기반 custom 드래그로 교체
  - 카드·슬라이더 양방향 실시간 sync
  - momentum flick, rubber-band 경계 저항
- 도시 단위 위치 지원 (city-level location)
- 일자별 사진 업로드 (per-day photo)
- 사진 탭 대표 사진 선택 UX 개선 (별표 UI)

### 디자인 / UX 정리
- 디자인 시스템 위반 수정 (font-weight:500, border-radius:999px 등)
- 그림자 법칙 통일 — 카드·버튼 box-shadow 전면 제거
- Tag system 색상 위계 3단계 통일 (primary / accent / muted)
- Tag cloud hover 모션 (센터 기준 scale)
- 지도뷰 필터칩 '계획 중' 제거
- 통계 히어로 카드 2열 정리
- 모달 편집버튼 full-width
- modal height: 고정 88dvh → max-height 전환 (콘텐츠 높이 대응)
- Dead CSS/JS 코드 정리 (stats-bar, cmp-*, acc-* 등 ~90줄)

---

## v3.3 이전
**파일**: 별도 보관 없음 (git 히스토리로 확인 가능)

| 커밋 | 내용 |
|------|------|
| `2ac44df` | DIT Archive v3.4 초기 — 사진 뷰어, UI 정리, story 탭 제거 |
| `43b3626` | 사진 탭 업로드 버튼 추가 |
| `78692b6` | 국가 항목에 일자별 기록 추가 |
| `1b2a60a` | 일자별 사진 업로드 |
| `4de2857` | Polarsteps 스타일 수평 캐러셀 (초기 시도) |
| `054c594` | Wizard step 3 보조 필드 접기 (더 기록하기 토글) |
| `c24bfe4` | 도시 단위 위치 지원 추가 |
| `ca2d402` | 날짜 우선 타임라인 기록 방식 재설계 |
