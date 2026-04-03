---
title: "VoltAgent/awesome-design-md: Collection of DESIGN.md files that capture design systems from popular websites. Drop one into your project and let coding agents build matching UI."
source: "https://github.com/VoltAgent/awesome-design-md"
author:
published:
created: 2026-04-03
description: "Collection of DESIGN.md files that capture design systems from popular websites. Drop one into your project and let coding agents build matching UI. - VoltAgent/awesome-design-md"
tags:
  - "clippings"
---
[![claude-skills](https://private-user-images.githubusercontent.com/18739364/572052038-d012a0d2-cec3-4630-ba5e-acc339dbe6cf.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzUxNzc5NjEsIm5iZiI6MTc3NTE3NzY2MSwicGF0aCI6Ii8xODczOTM2NC81NzIwNTIwMzgtZDAxMmEwZDItY2VjMy00NjMwLWJhNWUtYWNjMzM5ZGJlNmNmLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA0MDMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNDAzVDAwNTQyMVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWFlZjU4ZmMxYWE2ZWI2YTJiZGVhZDIyMjY2MjgxN2VkNzA5MmU3NjZkMDhmYWVlMDZkZmVlNjJmOWE1N2ZjZTYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.JBejUoag22gIQR4-_uLc8YabfNEDxelc2W9YRWBxJp0)](https://github.com/VoltAgent/voltagent)  
  

**개발자 중심 웹사이트에서 영감을 받은 DESIGN.md 파일의 엄선된 컬렉션입니다.**  
  

## Awesome Design MD

DESIGN.md를 프로젝트에 복사하고 AI 에이전트에게 "이렇게 보이는 페이지를 만들어줘"라고 말하면 실제로 일치하는 픽셀 퍼펙트 UI를 얻을 수 있습니다.

## DESIGN.md란 무엇인가요?

[DESIGN.md](https://stitch.withgoogle.com/docs/design-md/overview/) 는 Google Stitch에서 도입한 새로운 개념입니다. AI 에이전트가 읽고 일관된 UI를 생성하기 위한 평문 디자인 시스템 문서입니다.

단순한 마크다운 파일일 뿐입니다. Figma 내보내기도 없고, JSON 스키마도 없고, 특별한 도구도 필요 없습니다. 프로젝트 루트에 드롭하면 모든 AI 코딩 에이전트나 Google Stitch가 즉시 UI가 어떻게 보여야 하는지 이해합니다. 마크다운은 LLMs이 가장 잘 읽는 형식이므로 파싱하거나 구성할 것이 없습니다.

| 파일 | 누가 읽는가 | 정의하는 내용 |
| --- | --- | --- |
| `AGENTS.md` | 코딩 에이전트 | 프로젝트를 빌드하는 방법 |
| `DESIGN.md` | 디자인 에이전트 | 프로젝트가 어떻게 보이고 느껴져야 하는지 |

**이 저장소는 실제 웹사이트에서 추출한 바로 사용 가능한 DESIGN.md 파일을 제공합니다**

## 각 DESIGN.md에 포함된 내용

모든 파일은 확장된 섹션이 포함된 [Stitch DESIGN.md 형식](https://stitch.withgoogle.com/docs/design-md/format/) 을 따릅니다:

| # | 섹션 | 캡처되는 내용 |
| --- | --- | --- |
| 1 | 시각적 테마 및 분위기 | 무드, 밀도, 디자인 철학 |
| 2 | 색상 팔레트 & 역할 | 의미론적 이름 + 16진수 + 기능적 역할 |
| 3 | 타이포그래피 규칙 | 글꼴 패밀리, 전체 계층 구조 표 |
| 4 | 컴포넌트 스타일링 | 버튼, 카드, 입력, 상태가 있는 네비게이션 |
| 5 | 레이아웃 원칙 | 간격 스케일, 그리드, 공백 철학 |
| 6 | 깊이 및 높이 | 그림자 시스템, 표면 계층 구조 |
| 7 | 권장사항 및 금지사항 | 디자인 가이드레일 및 안티패턴 |
| 8 | 반응형 동작 | 중단점, 터치 대상, 축소 전략 |
| 9 | 에이전트 프롬프트 가이드 | 빠른 색상 참조, 바로 사용 가능한 프롬프트 |

각 사이트에는 다음이 포함됩니다:

| 파일 | 목적 |
| --- | --- |
| `DESIGN.md` | 디자인 시스템 (에이전트가 읽는 내용) |
| `preview.html` | 색상 견본, 타입 스케일, 버튼, 카드를 보여주는 시각적 카탈로그 |
| `preview-dark.html` | 어두운 배경이 적용된 동일한 카탈로그 |

### 사용 방법

1. 사이트의 `DESIGN.md` 를 프로젝트 루트에 복사하세요
2. AI 에이전트에게 이를 사용하도록 지시하세요.

## 컬렉션

### AI & 머신러닝

- [**Claude**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/claude/) - Anthropic의 AI 어시스턴트. 따뜻한 테라코타 악센트, 깔끔한 에디토리얼 레이아웃
- [**Cohere**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/cohere/) - 엔터프라이즈 AI 플랫폼. 생생한 그래디언트, 데이터 중심의 대시보드 미학
- [**ElevenLabs**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/elevenlabs/) - AI 음성 플랫폼. 어두운 시네마틱 UI, 오디오 파형 미학
- [**Minimax**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/minimax/) - AI 모델 제공자. 네온 악센트가 있는 대담한 어두운 인터페이스
- [**Mistral AI**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/mistral.ai/) - 오픈 웨이트 LLM 제공자. 프랑스식 엔지니어링 미니멀리즘, 보라색 톤
- [**Ollama**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/ollama/) - LLMs를 로컬에서 실행. 터미널 우선, 단색 단순성
- [**OpenCode AI**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/opencode.ai/) - AI 코딩 플랫폼. 개발자 중심의 다크 테마
- [**Replicate**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/replicate/) - API를 통한 ML 모델 실행. 깔끔한 화이트 캔버스, 코드 중심
- [**RunwayML**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/runwayml/) - AI 비디오 생성. 영화 같은 다크 UI, 미디어 풍부한 레이아웃
- [**Together AI**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/together.ai/) - 오픈소스 AI 인프라. 기술적이고 블루프린트 스타일의 디자인
- [**VoltAgent**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/voltagent/) - AI 에이전트 프레임워크. 검은색 캔버스, 에메랄드 악센트, 터미널 네이티브
- [**xAI**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/x.ai/) - 엘론 머스크의 AI 랩. 스타크 모노크롬, 미래주의적 미니멀리즘

### 개발자 도구 및 플랫폼

- [**Cursor**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/cursor/) - AI 우선 코드 에디터. 세련된 다크 인터페이스, 그래디언트 악센트
- [**Expo**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/expo/) - React Native 플랫폼. 다크 테마, 타이트한 자간, 코드 중심
- [**Linear**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/linear.app/) - 엔지니어를 위한 프로젝트 관리. 극도로 미니멀, 정확함, 보라색 액센트
- [**Lovable**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/lovable/) - AI 풀스택 빌더. 재미있는 그래디언트, 친화적인 개발자 미학
- [**Mintlify**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/mintlify/) - 문서화 플랫폼. 깔끔함, 초록색 액센트, 읽기 최적화
- [**PostHog**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/posthog/) - 제품 분석. 장난스러운 고슴도치 브랜딩, 개발자 친화적인 다크 UI
- [**Raycast**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/raycast/) - 생산성 런처. 세련된 다크 크롬, 생생한 그래디언트 악센트
- [**Resend**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/resend/) - 개발자용 이메일 API. 미니멀 다크 테마, 모노스페이스 악센트
- [**Sentry**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/sentry/) - 에러 모니터링. 다크 대시보드, 데이터 밀집형, 핑크-퍼플 악센트
- [**Supabase**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/supabase/) - 오픈소스 Firebase 대안. 진한 에메랄드 테마, 코드 우선
- [**Superhuman**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/superhuman/) - 빠른 이메일 클라이언트. 프리미엄 다크 UI, 키보드 우선, 보라색 글로우
- [**Vercel**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/vercel/) - 프론트엔드 배포 플랫폼. 검은색과 흰색 정밀함, Geist 폰트
- [**Warp**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/warp/) - 현대적 터미널. 다크 IDE 같은 인터페이스, 블록 기반 명령 UI
- [**Zapier**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/zapier/) - 자동화 플랫폼. 따뜻한 주황색, 친근한 일러스트레이션 중심

### 인프라 & 클라우드

- [**ClickHouse**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/clickhouse/) - 빠른 분석 데이터베이스. 노란색 강조, 기술 문서 스타일
- [**Composio**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/composio/) - 도구 통합 플랫폼. 현대적인 다크 테마에 다채로운 통합 아이콘
- [**HashiCorp**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/hashicorp/) - 인프라 자동화. 엔터프라이즈급 깔끔함, 검은색과 흰색
- [**MongoDB**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/mongodb/) - 문서 데이터베이스. 녹색 잎 브랜딩, 개발자 문서 중심
- [**Sanity**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/sanity/) - 헤드리스 CMS. 빨간색 악센트, 콘텐츠 우선 편집 레이아웃
- [**Stripe**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/stripe/) - 결제 인프라. 시그니처 보라색 그래디언트, weight-300 우아함

### 디자인 & 생산성

- [**Airtable**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/airtable/) - 스프레드시트-데이터베이스 하이브리드. 화려하고 친근한 구조화된 데이터 미학
- [**Cal.com**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/cal/) - 오픈소스 스케줄링. 깔끔한 중립적 UI, 개발자 지향적 단순성
- [**Clay**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/clay/) - 크리에이티브 에이전시. 유기적 형태, 부드러운 그래디언트, 아트 디렉팅 레이아웃
- [**Figma**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/figma/) - 협업 디자인 도구. 생생한 다중 색상, 장난스럽지만 전문적
- [**Framer**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/framer/) - 웹사이트 빌더. 대담한 검은색과 파란색, 모션 우선, 디자인 중심
- [**Intercom**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/intercom/) - 고객 메시징. 친근한 파란색 팔레트, 대화형 UI 패턴
- [**Miro**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/miro/) - 시각적 협업. 밝은 노란색 악센트, 무한 캔버스 미학
- [**Notion**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/notion/) - 올인원 워크스페이스. 따뜻한 미니멀리즘, 세리프 헤딩, 부드러운 표면
- [**Pinterest**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/pinterest/) - 시각적 발견 플랫폼. 빨간색 악센트, 석조 그리드, 이미지 우선
- [**Webflow**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/webflow/) - 시각적 웹 빌더. 파란색 악센트, 세련된 마케팅 사이트 미학

### 핀테크 & 암호화폐

- [**Coinbase**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/coinbase/) - 암호화폐 거래소. 깔끔한 파란색 아이덴티티, 신뢰 중심, 기관 느낌
- [**Kraken**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/kraken/) - 암호화폐 거래 플랫폼. 보라색 강조 다크 UI, 데이터 밀집형 대시보드
- [**Revolut**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/revolut/) - 디지털 뱅킹. 세련된 다크 인터페이스, 그래디언트 카드, 핀테크 정밀성
- [**Wise**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/wise/) - 국제 송금. 밝은 녹색 강조, 친근하고 명확함

### 엔터프라이즈 & 소비자

- [**Airbnb**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/airbnb/) - 여행 마켓플레이스. 따뜻한 산호색 강조, 사진 중심, 둥근 UI
- [**Apple**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/apple/) - 소비자 전자제품. 프리미엄 여백, SF Pro, 영화적 이미지
- [**BMW**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/bmw/) - 럭셔리 자동차. 어두운 프리미엄 표면, 정밀한 독일식 엔지니어링 미학
- [**IBM**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/ibm/) - 엔터프라이즈 기술. Carbon 디자인 시스템, 구조화된 파란색 팔레트
- [**NVIDIA**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/nvidia/) - GPU 컴퓨팅. 녹색-검은색 에너지, 기술적 파워 미학
- [**SpaceX**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/spacex/) - 우주 기술. 스타크한 검은색과 흰색, 풀블리드 이미지, 미래지향적
- [**Spotify**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/spotify/) - 음악 스트리밍. 어두운 배경의 생생한 녹색, 대담한 타입, 앨범 아트 중심
- [**Uber**](https://github.com/VoltAgent/awesome-design-md/tree/main/design-md/uber/) - 모빌리티 플랫폼. 대담한 검은색과 흰색, 타이트한 타입, 도시적 에너지

## 기여하기

기여를 환영합니다! 지침은 [CONTRIBUTING.md](https://github.com/VoltAgent/awesome-design-md/blob/main/CONTRIBUTING.md) 를 참조하세요.

- [**사이트 요청**](https://github.com/VoltAgent/awesome-design-md/issues): URL과 함께 이슈를 열어주세요
- **기존 파일 개선**: 잘못된 색상, 누락된 토큰, 약한 설명 수정
- **문제 보고**: 뭔가 이상해 보이면 알려주세요

## 라이선스

MIT License - [LICENSE 참조](https://github.com/VoltAgent/awesome-design-md/blob/main/LICENSE)

이 저장소는 공개 웹사이트에서 추출한 디자인 시스템 문서의 엄선된 모음입니다. 모든 DESIGN.md 파일은 보증 없이 "있는 그대로" 제공됩니다. 추출된 디자인 토큰은 공개적으로 표시되는 CSS 값을 나타냅니다. 우리는 어떤 사이트의 시각적 정체성에 대한 소유권을 주장하지 않습니다. 이 문서들은 AI 에이전트가 일관된 UI를 생성하도록 돕기 위해 존재합니다.