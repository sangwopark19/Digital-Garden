---
title: "alchaincyf/huashu-design: Huashu Design · HTML-native design skill for Claude Code · Claude Code 里 HTML 原生的设计 skill · 高保真原型 / 幻灯片 / 动画 + 20 设计哲学 + 5 维评审 + MP4 导出"
source: "https://github.com/alchaincyf/huashu-design"
author:
published:
created: 2026-04-24
description: "Huashu Design · HTML-native design skill for Claude Code · Claude Code 里 HTML 原生的设计 skill · 高保真原型 / 幻灯片 / 动画 + 20 设计哲学 + 5 维评审 + MP4 导出 · Agent-agnostic - alchaincyf/huashu-design"
tags:
  - "clippings"
---
## 화수 디자인

> *「타이핑. 엔터. 전달 가능한 디자인.」* *"Type. Hit enter. A finished design lands in your lap."*

**당신의 에이전트에 한 문장을 입력하고 전달 가능한 디자인을 받으세요.**

3~30분 안에 \*\*제품 출시 애니메이션\*\*, 클릭 가능한 앱 프로토타입, 편집 가능한 PPT 슬라이드, 인쇄 품질의 인포그래픽을 제공할 수 있습니다.

"AI가 잘 만든 수준"이 아니라 대형 기업의 디자인 팀이 만든 것처럼 보입니다. skill에 브랜드 자산(로고, 색상 팔레트, UI 스크린샷)을 제공하면 브랜드의 감성을 읽어냅니다. 아무것도 제공하지 않으면 내장된 20가지 디자인 언어로도 AI 저질 생성물을 피할 수 있습니다.

**이 README의 모든 애니메이션은 huashu-design이 직접 만든 것입니다.** Figma도 아니고, AE도 아니고, 단 한 줄의 프롬프트 + skill 실행일 뿐입니다. 다음 제품 출시 때 홍보 영상을 만들어야 한다면? 이제 당신도 할 수 있습니다.

```
npx skills add alchaincyf/huashu-design
```

에이전트 간 호환성 — Claude Code, Cursor, Codex, OpenClaw, Hermes 모두 설치 가능합니다.

[효과 보기](#demo-%E7%94%BB%E5%BB%8A) · [설치](#%E8%A3%85%E4%B8%8A%E5%B0%B1%E8%83%BD%E7%94%A8) · [할 수 있는 것](#%E8%83%BD%E5%81%9A%E4%BB%80%E4%B9%88) · [핵심 메커니즘](#%E6%A0%B8%E5%BF%83%E6%9C%BA%E5%88%B6) · [Claude Design과의 관계](#%E5%92%8C-claude-design-%E7%9A%84%E5%85%B3%E7%B3%BB)

---

[![huashu-design Hero · 打字 → 选方向 → 画廊展开 → 聚焦 → 品牌显形](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.gif)

<sub>▲ 25 秒 · Terminal → 4 方向 → Gallery ripple → 4 次 Focus → Brand reveal<br>👉 <a href="https://www.huasheng.ai/huashu-design-hero/">访问带音效的 HTML 互动版</a> · <a href="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.mp4">下载 MP4（含 BGM+SFX · 10MB）</a></sub>

---

## 설치하면 바로 사용 가능

```
npx skills add alchaincyf/huashu-design
```

그 다음 Claude Code에서 직접 말하면 됩니다:

```
「做一份 AI 心理学的演讲 PPT，推荐 3 个风格方向让我选」
「做个 AI 番茄钟 iOS 原型，4 个核心屏幕要真能点击」
「把这段逻辑做成 60 秒动画，导出 MP4 和 GIF」
「帮我对这个设计做一个 5 维度评审」
```

버튼 없음, 패널 없음, Figma 플러그인 없음.

---

## Star 트렌드

[![huashu-design Star History](https://camo.githubusercontent.com/606bb35e4902548268892c03101b3d8186b0b2d5a28df87820f5deb0ff9c7353/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f7376673f7265706f733d616c636861696e6379662f6875617368752d64657369676e26747970653d44617465)](https://star-history.com/#alchaincyf/huashu-design&Date)

---

## 무엇을 할 수 있는가

| 기능 | 결과물 | 일반적인 소요 시간 |
| --- | --- | --- |
| 인터랙티브 프로토타입 (App / Web) | 단일 파일 HTML · 실제 iPhone bezel · 클릭 가능 · Playwright 검증 | 10–15분 |
| 프레젠테이션 슬라이드 | HTML 덱(브라우저 프레젠테이션) + 편집 가능한 PPTX(텍스트 박스 유지) | 15–25분 |
| 타임라인 애니메이션 | MP4(25fps / 60fps 프레임 보간) + GIF(팔레트 최적화) + BGM | 8–12분 |
| 디자인 변형 | 3개 이상 나란히 비교 · Tweaks 실시간 파라미터 조정 · 다차원 탐색 | 10분 |
| 인포그래픽 / 시각화 | 인쇄급 타이포그래피 · PDF/PNG/SVG 내보내기 가능 | 10분 |
| 디자인 방향 컨설턴트 | 5가지 유파 × 20가지 설계 철학 · 3가지 방향 추천 · 병렬 Demo 생성 | 5분 |
| 5차원 전문가 평가 | 레이더 차트 + Keep/Fix/Quick Wins · 실행 가능한 수정 체크리스트 | 3분 |

---

## 데모 갤러리

### 디자인 방향 컨설턴트

모호한 요구사항일 때의 폴백: 5가지 유파 × 20가지 설계 철학에서 3가지 차별화된 방향을 선택하고, 3개의 데모를 병렬로 생성하여 선택할 수 있습니다.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w3-fallback-advisor.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w3-fallback-advisor.gif)

### iOS 앱 프로토타입

iPhone 15 Pro 정밀 기기 본체(Dynamic Island / 상태 표시줄 / Home Indicator) · 상태 기반 다중 화면 전환 · 실제 이미지는 Wikimedia/Met/Unsplash에서 가져오기 · Playwright 자동 클릭 테스트.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c1-ios-prototype.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c1-ios-prototype.gif)

### Motion Design 엔진

Stage + Sprite 타임 슬라이스 모델 · `useTime` / `useSprite` / `interpolate` / `Easing` 4가지 API로 모든 애니메이션 요구사항 충족 · 한 줄의 명령으로 MP4 / GIF / 60fps 프레임 보간 / BGM이 포함된 최종 영상 내보내기.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c3-motion-design.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c3-motion-design.gif)

### HTML 슬라이드 → 편집 가능한 PPTX

HTML 덱 브라우저 프레젠테이션 · `html2pptx.js` 가 DOM의 computedStyle을 읽어 요소별로 PowerPoint 객체로 변환 · 내보낸 것은 **실제 텍스트 박스** 이며, 이미지 배경이 아님.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c2-slides-pptx.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c2-slides-pptx.gif)

### 트윅스 · 실시간 변형 전환

색상 / 글꼴 / 정보 밀도 등 매개변수화 · 사이드 패널 전환 · 순수 프론트엔드 + `localStorage` 지속성 · 새로고침해도 손실 없음.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c4-tweaks.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c4-tweaks.gif)

### 인포그래픽 / 데이터 시각화

매거진 수준의 타이포그래피 · CSS Grid 정밀 컬럼 분할 · `text-wrap: pretty` 인쇄 세부사항 · 실제 데이터 기반 · PDF 벡터 / PNG 300dpi / SVG 내보내기 가능.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c5-infographic.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c5-infographic.gif)

### 5 차원 전문가 평가

철학적 일관성 · 시각적 계층 · 세부 실행 · 기능성 · 혁신성 각각 0–10점 · 레이더 차트 시각화 · Keep / Fix / Quick Wins 체크리스트 출력.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c6-expert-review.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c6-expert-review.gif)

### 주니어 디자이너 워크플로우

무작정 큰 작업을 하지 말고: 먼저 assumptions + placeholders + reasoning을 작성하고, 일찍 보여준 후 반복하세요. 이해를 잘못한 것을 늦게 수정하는 것보다 일찍 수정하는 것이 100배 저렴합니다.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w2-junior-designer.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w2-junior-designer.gif)

### 브랜드 자산 프로토콜 5단계 강제 프로세스

구체적인 브랜드가 관련될 때 강제 실행: 질문 → 검색 → 다운로드(3가지 폴백) → grep 색상값 → `brand-spec.md` 작성.

[![](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w1-brand-protocol.gif)](https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w1-brand-protocol.gif)

---

## 핵심 메커니즘

### 브랜드 자산 프로토콜

skill에서 가장 엄격한 규칙 섹션입니다. 특정 브랜드(Stripe, Linear, Anthropic, 자사 등)와 관련될 때 5단계를 강제로 실행합니다:

| 단계 | 작업 | 목적 |
| --- | --- | --- |
| 1 · 질문 | 사용자가 브랜드 가이드라인을 가지고 있습니까? | 기존 리소스 존중 |
| 2 · 공식 브랜드 페이지 검색 | `<brand>.com/brand` · `brand.<brand>.com` · `<brand>.com/press` | 권위 있는 색상값 추출 |
| 3 · 자산 다운로드 | SVG 파일 → 공식 웹사이트 HTML 전문 → 제품 스크린샷 색상 추출 | 세 가지 대체 방안, 이전 방안이 실패하면 즉시 다음 방안으로 진행 |
| 4 · grep으로 색상값 추출 | 자산에서 모든 `#xxxxxx` 를 추출하고, 빈도순으로 정렬한 후 검은색, 흰색, 회색 필터링 | **브랜드 색상을 절대 기억으로 추측하지 마세요** |
| 5 · Spec 고정화 | `brand-spec.md` + CSS 변수를 작성하고, 모든 HTML에서 `var(--brand-*)를 참조하세요` | 고정화하지 않으면 잊어버립니다 |

A/B 테스트(v1 vs v2, 각각 6개 에이전트 실행): **v2의 안정성 분산이 v1보다 5배 낮음**. 안정성의 안정성, 이것이 skill의 진정한 경쟁 우위입니다.

### 디자인 방향 컨설턴트(Fallback)

사용자 요구사항이 모호해서 착수할 수 없을 때 트리거됨:

- 일반적인 직관에 의존하지 않고 하드코딩하지 않으며, Fallback 모드로 진입
- 5가지 유파 × 20가지 설계 철학에서 3개의 **반드시 서로 다른 유파에서 선택된** 차별화된 방향을 추천
- 각 방향마다 대표작, 기질 키워드, 대표 설계자 배치
- 3개의 시각적 데모를 병렬로 생성하여 사용자가 선택하도록 제공
- 선택 후 메인 Junior Designer 프로세스로 진입

### 주니어 디자이너 워크플로우

모든 작업을 관통하는 기본 작업 모드:

- 작업 시작 전에 문제 체크리스트를 한 번에 사용자에게 제시하고, 일괄 답변을 받은 후 작업 시작
- HTML에서 먼저 assumptions + placeholders + reasoning comments 작성
- 사용자에게 조기에 보여주기 (회색 블록이라도 괜찮음)
- 실제 콘텐츠 채우기 → variations → Tweaks 이 세 단계를 각각 다시 한 번 보여주기
- 전달 전에 Playwright로 브라우저를 육안으로 한 번 확인하기

### AI 저질 생성물 방지 규칙

AI 스타일의 시각적 클리셰를 피하기 (보라색 그래디언트 / 이모지 아이콘 / 둥근 모서리 + 왼쪽 테두리 악센트 / SVG 얼굴 그리기 / 디스플레이용 Inter). `text-wrap: pretty` + CSS Grid + 신중하게 선택한 serif 디스플레이 폰트와 oklch 색상을 사용합니다.

---

## Claude Design과의 관계

솔직하게 인정합니다: 브랜드 자산 프로토콜의 철학은 Claude Design에서 유포된 프롬프트에서 배운 것입니다. 그 프롬프트는 **좋은 고충실도 디자인은 백지에서 시작하는 것이 아니라 이미 존재하는 디자인 컨텍스트에서 자라난다** 는 점을 반복해서 강조합니다. 이 원칙이 65점 작품과 90점 작품을 가르는 분수령입니다.

포지셔닝 차이:

|  | Claude Design | huashu-design |
| --- | --- | --- |
| 형태 | 웹 제품 (브라우저에서 사용) | skill (Claude Code에서 사용) |
| 할당량 | 구독 할당량 | API 소비 · 병렬 실행 에이전트는 할당량 제한을 받지 않음 |
| 산출물 | 캔버스 내 + Figma로 내보내기 가능 | HTML / MP4 / GIF / 편집 가능한 PPTX / PDF |
| 작동 방식 | GUI(클릭, 드래그, 수정) | 대화(말하기, Agent 완료 대기) |
| 복잡한 애니메이션 | 제한됨 | Stage + Sprite 타임라인 · 60fps 내보내기 |
| 에이전트 간 호환 | Claude.ai 전용 | 모든 skill과 호환되는 agent |

Claude Design은 **더 나은 그래픽 도구** 이고, huashu-design은 **그래픽 도구 레이어를 없애는 것** 입니다. 두 가지 경로, 다른 사용자층입니다.

---

## 제한사항

- **레이어 수준의 편집 가능한 PPTX에서 Figma로의 변환은 지원하지 않습니다**. HTML을 출력하며, 스크린샷, 화면 녹화, 마인드맵이 가능하지만 Keynote로 드래그하여 텍스트 위치를 변경할 수 없습니다.
- **Framer Motion 수준의 복잡한 애니메이션은 불가능합니다**. 3D, 물리 시뮬레이션, 파티클 시스템은 skill의 범위를 벗어납니다.
- **완전히 빈 브랜드는 처음부터 설계 품질이 60–65점으로 떨어집니다**. 공중에서 hi-fi를 그리는 것 자체가 이미 최후의 수단입니다.

이것은 100점의 제품이 아닌 80점의 skill입니다. 그래픽 인터페이스를 열기 싫어하는 사람들에게는 100점의 제품보다 80점의 skill이 더 사용하기 좋습니다.

---

## 저장소 구조

```
huashu-design/
├── SKILL.md                 # 主文档（给 agent 读）
├── README.md                # 本文件（给用户读）
├── assets/                  # Starter Components
│   ├── animations.jsx       # Stage + Sprite + Easing + interpolate
│   ├── ios_frame.jsx        # iPhone 15 Pro bezel
│   ├── android_frame.jsx
│   ├── macos_window.jsx
│   ├── browser_window.jsx
│   ├── deck_stage.js        # HTML 幻灯片引擎
│   ├── deck_index.html      # 多文件 deck 拼接器
│   ├── design_canvas.jsx    # 并排变体展示
│   ├── showcases/           # 24 个预制样例（8 场景 × 3 风格）
│   └── bgm-*.mp3            # 6 首场景化背景音乐
├── references/              # 按任务深入读的子文档
│   ├── animation-pitfalls.md
│   ├── design-styles.md     # 20 种设计哲学详细库
│   ├── slide-decks.md
│   ├── editable-pptx.md
│   ├── critique-guide.md
│   ├── video-export.md
│   └── ...
├── scripts/                 # 导出工具链
│   ├── render-video.js      # HTML → MP4
│   ├── convert-formats.sh   # MP4 → 60fps + GIF
│   ├── add-music.sh         # MP4 + BGM
│   ├── export_deck_pdf.mjs
│   ├── export_deck_pptx.mjs
│   ├── html2pptx.js
│   └── verify.py
└── demos/                   # 9 个能力演示 (c*/w*)，中英双版 GIF/MP4/HTML + hero v10
```

---

## 기원

Anthropic이 Claude Design을 출시한 날 나는 새벽 4시까지 가지고 놀았다. 며칠 후 자신이 다시는 그것을 열어본 적이 없다는 것을 발견했다. 그것이 좋지 않아서가 아니라—그것은 현재 이 분야에서 가장 성숙한 제품이다—나는 agent가 터미널에서 일을 도와주도록 하는 것이 어떤 그래픽 인터페이스도 열기보다 낫다고 생각했다.

그래서 agent에게 Claude Design 자체를 분해하도록 했다(커뮤니티에서 유통되는 시스템 프롬프트, 브랜드 자산 프로토콜, 컴포넌트 메커니즘 포함). 이를 구조화된 spec으로 증류한 후 skill로 작성하여 자신의 Claude Code에 설치했다.

Claude Design의 프롬프트를 명확하게 작성해준 Anthropic에 감사한다. 다른 제품의 영감을 바탕으로 한 이러한 2차 창작은 AI 시대의 오픈소스 문화의 새로운 형태이다.

---

## 라이선스 · 사용 허가

**개인 사용 무료, 자유** ——학습, 연구, 창작, 자신을 위한 것 만들기, 글쓰기, 부업, 웨이보 또는 공중호 게시, 자유롭게 사용하세요. 허락을 구할 필요가 없습니다.

**기업 상용 금지** ——모든 회사, 팀 또는 영리 목적의 조직이 본 skill을 제품에 통합하거나, 외부 서비스 제공, 고객에게 작업 전달 목적으로 사용하려면, **반드시 먼저 Huashu와 연락하여 허가를 받아야 합니다**. 다음을 포함하되 이에 국한되지 않습니다:

- skill을 회사 내부 도구 체인의 일부로 사용
- skill 산출물을 대외 납품물의 주요 창작 수단으로 활용
- skill을 기반으로 2차 개발하여 상용 제품으로 제작
- 고객 상용 프로젝트에서 사용

**상용 라이선스 문의** 는 아래 소셜 플랫폼을 참고하세요.

---

## Connect · 땅콩（땅숙）

땅콩은 AI Native Coder, 독립 개발자, AI 자미디어 블로거입니다. 대표작: 고양이 보조등(AppStore 유료 순위 Top 1), 《DeepSeek로 노는 한 권의 책》, 여와.skill(GitHub 12000+ star). 소셜미디어 전 플랫폼 30만+ 팔로워.

| 플랫폼 | 계정 | 링크 |
| --- | --- | --- |
| X / Twitter | @AlchainHust | [https://x.com/AlchainHust](https://x.com/AlchainHust) |
| 공개 계정 | 화숙 | 위챗에서 「화숙」 검색 |
| B 站 | 화숙 | [https://space.bilibili.com/14097567](https://space.bilibili.com/14097567) |
| YouTube | 화수 | [https://www.youtube.com/@Alchain](https://www.youtube.com/@Alchain) |
| 샤오홍슈 | 화수 | [https://www.xiaohongshu.com/user/profile/5abc6f17e8ac2b109179dfdf](https://www.xiaohongshu.com/user/profile/5abc6f17e8ac2b109179dfdf) |
| 공식 웹사이트 | huasheng.ai | [https://www.huasheng.ai/](https://www.huasheng.ai/) |
| 개발자 홈페이지 | bookai.top | [https://bookai.top](https://bookai.top/) |

상용 라이선스, 협력 상담, 소셜 미디어 기고 → 위의 모든 플랫폼에서 Huasheng에게 개인 메시지를 보내시면 됩니다.