# 프로젝트: DIT\_Web ver3

## 개요

* HTML 기반 웹 작업 폴더 (DIT-archive, Travel 페이지 등)
* 작업자: 정원 (디자이너, vibe coding 방식)



## 작업 방식

* 파일 수정 전 항상 계획을 먼저 제시하고 확인받은 후 진행
* 변경할 부분만 명확히 특정해서 수정 (전체 재작성 지양)
* git 미사용 환경 — 버전 비교가 필요하면 파일명에 버전 표기 (예: -v3.2) 유지



## 파일 구조

* \*.html: 단일 파일형 웹 페이지 (인라인 CSS/JS 포함 가능성 있음 — 확인 필요)



## 디자인 시스템

[메모] (26.07.01 아이콘 시스템 추가)
### 아이콘 시스템
아이콘은 반드시 Heroicons solid(@heroicons/react/24/solid)만 사용한다.
이모지, 다른 아이콘 라이브러리, outline 버전 혼용 금지.

---

[메모] (26.07.01 design.md 추가)
> Apple 디자인 분석에서 차용한 "규율 3원칙" + 기존 확정된 mint/coral 팔레트 조합.
> 목적: 산만하고 오락가락하는 디자인을 아래 규칙으로 강제 통일.

**그림자는 사진에만.** 카드/버튼/텍스트에 그림자 금지. 그림자는 여행지 사진(제품 사진 역할)에만 적용한다.

카드 구분은 그림자가 아니라 배경색 전환 또는 헤어라인 보더로 한다.

**라운드는 4단계 고정.** `sm / md / lg / pill` 외의 임의 radius 값 금지. 중간값 만들지 않는다.

---

### Colors

```yaml
colors:
  primary: "#00C896"          # mint — 유일한 인터랙티브 컬러
  primary-focus: "#00B085"    # primary보다 한 톤 진한 값, focus/active 전용
  accent: "#FF6B5E"           # coral — 강조/하이라이트 전용 (인터랙션 금지)
  ink: "#1D1D1F"              # 기본 텍스트
  ink-muted: "#6B6B70"        # 보조 텍스트, 캡션
  body-on-dark: "#FFFFFF"
  canvas: "#FFFFFF"
  canvas-soft: "#F7F7F8"      # 카드/섹션 배경 전환용
  surface-dark: "#1A1A1C"     # 다크 섹션 (여행지 사진 배경 등)
  hairline: "#E5E5E7"         # 카드 보더, 구분선
  success: "#00C896"          # primary 재사용 (별도 semantic 컬러 만들지 않음)
  error: "#FF3B30"
```

**Do**
- `primary`는 CTA 버튼, 선택된 탭, 링크, 활성 아이콘에만
- `accent`(coral)는 감정 태그, 통계 하이라이트, 뱃지에만 — 절대 버튼 배경으로 쓰지 않음
- 카드 구분이 필요하면 `canvas` → `canvas-soft` 전환 또는 `hairline` 1px 보더

**Don't**
- primary와 accent를 같은 컴포넌트에 동시에 강조색으로 쓰지 않기
- 그림자로 카드 깊이감 만들지 않기 (color-step 또는 hairline으로만)

---

### Typography

```yaml
typography:
  font-family: "Pretendard, -apple-system, BlinkMacSystemFont, sans-serif"
  font-family-mono: "SF Mono, Menlo, monospace"  # 날짜/좌표/통계 숫자 전용

  # ── Font Size 6단계 고정 (var(--fs-*) 사용) ──
  # 이 외의 임의 rem 값 사용 금지. 이모지/아바타/별점은 예외.
  fs-xs: 0.65rem   # 10.4px — 레이블, 뱃지, 태그, 상태 표시
  fs-sm: 0.75rem   # 12px   — 메타, 날짜, 캡션, 보조 텍스트
  fs-md: 0.85rem   # 13.6px — 본문, 입력 필드, 버튼
  fs-lg: 1rem      # 16px   — 카드 타이틀, 중요 본문, chart-title
  fs-xl: 1.3rem    # 20.8px — 섹션 헤드라인, wiz-headline
  fs-2xl: 1.9rem   # 30.4px — hero 숫자 (sh-val)

  # 예외 (토큰 미적용):
  # 1.4rem — 국기 이모지 (.rc-flag)
  # 1.5rem — 아바타 이모지 (.pv-avatar)
  # 1.6rem — 이모지/year-label 등 장식 요소
  # 1.7rem — 별점 버튼 (.star-btn) — 터치 타깃
  # 2rem   — 커버/공유카드 이모지 장식

  # chart-title: var(--fs-md), weight 600
  # chart-count: var(--fs-md), weight 400, color ink-muted (타이틀 옆 갯수 표기)
```

**Do**
- 헤드라인 700/600 두 단계만 사용. 500 사용 금지 (Apple 원칙 차용 — 굵기 사다리 단순화)

---

### 모션 시스템
> Apple HIG 기반 physics-driven motion. 인터페이스가 물리적으로 느껴지게.

**철학**
- 모든 인터랙티브 요소는 spring 사용. cubic-bezier는 비인터랙티브/ambient 전용.
- 모션은 공간 관계를 전달: push=깊이 이동, sheet=아래서 올라옴, alert=제자리 등장.
- 제스처 속도에 모션 연동. 빠른 스와이프 → 빠른 이탈. 느린 드래그 → 느린 안착.
- `prefers-reduced-motion` 감지 시 → transform 제거, opacity-only 250ms ease-out으로 대체.

**Duration**
| Token   | Value | 용도 |
|---------|-------|------|
| instant | 0ms   | 직접 조작, VoiceOver |
| fast    | 150ms | 툴팁, 뱃지, 소형 아이콘 |
| default | 300ms | 네비게이션, 모달, 시트 |
| slow    | 400ms | 전체화면 전환 |
| slower  | 500ms | Hero 이미지, 온보딩 |

**Easing (비인터랙티브 전용)**
| Token        | cubic-bezier           | 용도 |
|--------------|------------------------|------|
| ease-out     | (0.33, 1, 0.68, 1)     | 진입 — 알림, 오버레이 |
| ease-in      | (0.32, 0, 0.67, 0)     | 이탈 — 닫히는 UI |
| ease-in-out  | (0.65, 0, 0.35, 1)     | 리포지션, 크로스페이드 |
| deceleration | (0.0, 0.0, 0.2, 1.0)   | 화면 밖에서 진입 |
| linear       | (0, 0, 1, 1)           | 프로그레스바, 스피너 |

**Spring Configs**
| Name    | stiffness | damping | 용도 |
|---------|-----------|---------|------|
| Default | 300       | 30      | 모달, 네비게이션 — bounce 없음 |
| Snappy  | 500       | 40      | 인터랙티브 dismiss, 스와이프백 |
| Gentle  | 170       | 26      | 대형 요소 리포지션 |
| Tight   | 700       | 60      | 버튼 피드백, 즉각 반응 |

CSS 근사값:
- Default: `cubic-bezier(0.34, 1.05, 0.64, 1)`
- Snappy:  `cubic-bezier(0.34, 1.2, 0.64, 1)`
- Gentle:  `cubic-bezier(0.25, 1.0, 0.5, 1)`
- Tight:   `cubic-bezier(0.4, 1.0, 0.6, 1)`

**Enter / Exit 패턴**
- Modal/Sheet enter: `translateY(100%→0)`, Default spring, 300ms
- Modal/Sheet exit:  `translateY(0→100%)`, Snappy spring, 300ms
- Fade+Slide enter:  `opacity 0→1` + `scale 0.94→1`, ease-out, 300ms
- Fade+Slide exit:   `opacity 1→0` + `scale 1→0.94`, ease-in, 150ms
- Scale Pop enter:   `opacity 0→1` + `scale 0.8→1`, Snappy spring, 200ms
- Scale Pop exit:    `opacity 1→0` + `scale 1→0.8`, ease-in, 150ms
- Hero/Magic Move:   소스→목적지 프레임 직접 이동, Gentle spring, 400ms

**Stagger**
| 대상 | 간격 | 최대 |
|------|------|------|
| 리스트 아이템 | 25ms | 8개 |
| 카드 그리드   | 30ms | 6개 |
| 앱 아이콘     | 20ms | wave 방향 |
| 탭바/사이드바 | 없음 — 유닛으로 등장 | — |

**인터랙션 상태**
- Press: `scale 0.95` 즉각(0ms), 릴리즈 시 Tight spring 복귀
- Hover: 150ms ease-out highlight, scale 없음
- Loading shimmer: 1500ms ease-in-out loop, 패스 사이 400ms gap

---

### Radius

```yaml
rounded:
  none: 0px
  sm: 8px      # 인풋
  md: 12px     # 카드
  lg: 20px     # 모달, 바텀시트
  pill: 9999px # 태그, 캡슐 버튼
```

### Spacing

```
4px  → space-1 · stack-tight · gap-photo · gap-tag
8px  → space-2 · stack-md · gap-item
12px → space-3 · pad-input(horizontal) · pad-btn(horizontal) · gap-card
16px → space-4 · pad-btn(vertical) · pad-card(vertical) · stack-lg
20px → space-5 · pad-section(horizontal) · stack-section
24px → space-6 · large section spacing
```

**Padding (Component Level)**

| Token | Value | 용도 |
|-------|-------|------|
| pad-tag | 3px 8px | tag, badge, status pill |
| pad-chip-sm | 4px 8px | 소형 filter chip |
| pad-chip-md | 6px 12px | mood chip, 선택형 chip |
| pad-input | 8px 12px | input, textarea |
| pad-btn | 12px 16px | 일반 button |
| pad-card | 14px 16px | card 내부 |
| pad-section | 16px 20px | view section, modal body |

---

### 버튼 시스템

| 클래스 | 용도 | 배경 | 보더 | 라운드 | 패딩 | 폰트 크기 | 폰트 굵기 | 색상 |
|--------|------|------|------|--------|------|-----------|-----------|------|
| `.wiz-next` | 폼 주요 액션 (저장, 다음) | `primary` | none | `pill` | `12px 16px` | `fs-md` | 700 | #fff |
| `.wiz-prev` | 폼 보조 액션 (뒤로, 취소) | transparent | 1.5px `hairline` | `pill` | `12px 16px` | `fs-md` | 600 | `ink-muted` |
| `.wiz-delete` | 삭제 (위험 액션) | transparent | 1.5px `error` | `pill` | `12px 16px` | `fs-md` | 600 | `error` |
| `.btn-primary-land` | 랜딩/프로필 CTA | `primary` | none | `pill` | `.82rem 2rem` | `fs-md` | 700 | #fff |
| `.btn-ghost-land` | 랜딩/프로필 보조 | transparent | 1.5px `border` | `pill` | `.78rem 2rem` | `fs-md` | 600 | `sub` |
| `.ftb-close` | 폼 topbar X (닫기) | none | none | — | 4px | — | — | `ink-muted` |
| `.bn-add .bni` | FAB (+) | `primary` | none | 50% | — | — | — | #fff |

**레이아웃 규칙**
- 폼 하단 버튼은 항상 `.wiz-sticky-footer` 안에 넣어 `position:sticky; bottom:0; background:#F7F7F8` 유지
- `.wiz-footer`: `display:flex; gap:.65rem; padding-top:.25rem; margin-top:.25rem` — prev + next 나란히 배치
- 삭제 버튼은 `.wiz-footer` 위에 단독 full-width로 배치
- `.wiz-next:disabled`: `opacity:.35`
