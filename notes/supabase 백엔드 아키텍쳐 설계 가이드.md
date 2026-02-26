# Supabase + Expo React Native / Next.js

## AI 코드 생성 아키텍처 가이드 (검증 반영본)

기준일: 2026-02-26  
대상: Claude Code 포함 모든 AI 코드 생성 워크플로우

---

## 1) 목적

이 문서는 “편의성 높은 패턴”보다 “유지보수성, 확장성, 보안성”을 우선하기 위한 팀 표준이다.  
다만 기술을 절대 금지하지 않고, 각 기술을 어디에 써야 하는지 명확히 분리한다.

---

## 2) 핵심 철학

1. 비즈니스 로직의 중심은 서버 레이어(TypeScript)다.
2. Supabase는 단순 저장소가 아니라, 데이터 무결성/권한/원자성 보장을 담당하는 계층이다.
3. 모든 핵심 로직은 코드 리뷰 가능한 위치에 있어야 한다.
4. 시크릿 키가 필요한 로직은 100% 서버에서만 실행한다.
5. AI가 제안한 패턴은 “기본 원칙 + 예외 조건” 체크 후 채택한다.

---

## 3) 기존 원칙 수정사항 (중요)

### 3.1 “DB 안에 로직 금지” → 수정

- 금지 대상: 도메인 로직 전체를 SQL에 숨기는 설계
- 허용/권장 대상:
    - RLS 정책
    - 제약조건(UNIQUE/CHECK/FK)
    - 트랜잭션 원자성이 필요한 DB 함수(RPC)
    - 최소한의 트리거(감사 로그, 타임스탬프, 동기화)

핵심: 앱 비즈니스 흐름은 서버 TypeScript에 두고, 데이터 무결성과 원자성은 DB에서 보강한다.

### 3.2 “Trigger/RPC/Webhook 대체” → 수정

- Trigger, RPC, Webhook은 “금지”가 아니라 “목적 기반 사용”이 맞다.
- 대체 관계가 아니라 역할 분리다.

### 3.3 “Views가 RPC 대체” → 수정

- Views: 읽기 모델 단순화/조회 최적화 용도
- RPC(Function): 쓰기 트랜잭션/원자 연산/권한 캡슐화 용도
- 서로 대체 불가. 함께 사용해야 한다.

### 3.4 View 보안 주의

- View는 설정에 따라 RLS를 우회할 수 있다.
- 보안이 필요한 View는 `security_invoker`를 적용해 호출자 권한으로 평가되게 한다.

---

## 4) 스택별 권장 레이어 구조

## [A] Expo React Native + Supabase

### Layer 1: 앱 번들(클라이언트)

- `screens/`: UI 렌더링
- `components/`: 재사용 UI
- `hooks/`: TanStack Query 서버 상태
- `stores/`: Zustand 클라이언트 상태
- `repositories/`: Supabase CRUD 접근 캡슐화
- `lib/supabase.ts`: 클라이언트 전용 싱글톤

원칙:

- 클라이언트에서 직접 가능한 작업은 최소 권한(RLS 전제)으로 수행
- 결제/AI 호출/시크릿 사용/외부 서비스 서명은 금지

### Layer 2: 서버 레이어 (Expo API Routes 또는 별도 서버)

- `app/api/payment+api.ts`: 결제/정산
- `app/api/ai/generate+api.ts`: AI API 호출
- `app/api/upload+api.ts`: 업로드 검증/전처리

주의:

- Expo API Routes는 유용하지만 런타임 제약이 있으므로, 복잡한 백엔드 요구 시 Next.js Route Handler 또는 별도 서버로 분리 가능해야 한다.

### Layer 3: 데이터 계층 (Supabase)

- PostgreSQL + PostgREST
- RLS Policies
- PostgreSQL Views (조회 모델)
- PostgreSQL Functions(RPC) (원자 쓰기/트랜잭션)
- Triggers (최소 사용)
- Auth / Realtime / Database Webhooks

---

## [B] Next.js + Supabase

### Layer 1: 클라이언트 컴포넌트

- `components/`: UI
- `hooks/`: TanStack Query
- `stores/`: Zustand

### Layer 2: 서버 레이어 (Next.js)

- `app/api/`: Route Handlers (외부 API, 시크릿, 민감 로직)
- `app/actions/`: Server Actions (폼 기반 mutation 중심)
- `lib/supabase-server.ts`: 서버 전용 클라이언트
- `lib/supabase-client.ts`: 클라이언트 전용 클라이언트

원칙:

- 조회(read)는 가능하면 Server Component/서버 데이터 패칭에 우선 배치
- 변경(write)은 Server Action 또는 Route Handler로 처리
- 인증/인가 검증은 Server Action에서도 API와 동일 강도로 적용

### Layer 3: 데이터 계층 (Supabase)

- A 구조와 동일

---

## 5) 패턴 선택 기준 (AI 필수 체크리스트)

AI가 코드 생성 시 아래 순서로 판단한다.

1. 시크릿 키가 필요한가?

- 예: 서버(Route Handler/API) 강제

2. 단일 SQL로 원자 처리해야 하는가?

- 예: RPC(Function) 우선

3. 단순 조회 조합인가?

- 예: View 우선

4. 이벤트를 외부 시스템으로 보내야 하는가?

- 예: Database Webhook 또는 서버 이벤트 파이프라인

5. 클라이언트 실시간 UI 동기화가 필요한가?

- 예: Realtime 구독

6. 장시간/고비용/외부 의존 작업인가?

- 예: 백그라운드 잡/큐/재시도 가능한 서버 경로 사용 (Edge 여부는 latency/지역성 기준으로 선택)

---

## 6) 금지 규칙 (유지)

1. 앱 번들에 서비스 롤 키/서드파티 시크릿 포함 금지
2. RLS 없이 클라이언트 직접 테이블 접근 금지
3. 비즈니스 핵심 로직을 SQL 함수에만 숨기는 설계 금지
4. 트리거 남용 금지 (흐름 추적 어려운 자동화 로직 누적 금지)
5. “AI가 추천했으니 채택” 방식 금지 (근거 문서/운영 요구로 검증 필수)

---

## 7) 권장 규칙 (추가)

1. Supabase 마이그레이션 기반으로 스키마 변경 이력 관리
2. RPC/Trigger/View는 목적, 입력/출력, 권한 모델을 문서화
3. 서버 레이어에서 observability(로그, 에러 코드, 트레이싱) 표준화
4. 읽기 성능 이슈는 View/인덱스/쿼리 튜닝으로 해결하고, 쓰기 원자성 이슈는 RPC로 해결
5. Next.js는 waterfall 제거/병렬 fetch/캐시 전략 우선 적용
6. Expo는 리스트 성능/이미지 최적화/네이티브 내비게이션 우선 적용

---

## 8) 최종 원칙 요약

- “서버 우선”은 맞다.
- “DB 로직 완전 배제”는 틀리다.
- 정답은 “서버 중심 + DB 무결성/원자성 활용 + 역할 분리”다.

이 원칙을 따르면 AI가 생성한 코드도 장기 운영 가능한 형태로 수렴한다.

---

## 참고 문서

- Supabase Database Functions: [https://supabase.com/docs/guides/database/functions](https://supabase.com/docs/guides/database/functions)
- Supabase Postgres Triggers: [https://supabase.com/docs/guides/database/postgres/triggers](https://supabase.com/docs/guides/database/postgres/triggers)
- Supabase Database Webhooks: [https://supabase.com/docs/guides/database/webhooks](https://supabase.com/docs/guides/database/webhooks)
- Supabase RLS: [https://supabase.com/docs/guides/database/postgres/row-level-security](https://supabase.com/docs/guides/database/postgres/row-level-security)
- Supabase REST API(2-tier/3-tier): [https://supabase.com/docs/guides/api](https://supabase.com/docs/guides/api)
- Supabase Edge Functions: [https://supabase.com/docs/guides/functions](https://supabase.com/docs/guides/functions)
- Supabase Edge Function Limits: [https://supabase.com/docs/guides/functions/limits](https://supabase.com/docs/guides/functions/limits)
- Supabase SSR Client(Next.js): [https://supabase.com/docs/guides/auth/server-side/creating-a-client?queryGroups=framework&framework=nextjs](https://supabase.com/docs/guides/auth/server-side/creating-a-client?queryGroups=framework&framework=nextjs)
- Next.js Updating Data (Server Functions/Actions): [https://nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)
- Next.js Route Handlers: [https://nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)
- Next.js Backend for Frontend: [https://nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)
- Expo API Routes: [https://docs.expo.dev/router/web/api-routes/](https://docs.expo.dev/router/web/api-routes/)
- Expo Server Middleware: [https://docs.expo.dev/router/web/middleware/](https://docs.expo.dev/router/web/middleware/)