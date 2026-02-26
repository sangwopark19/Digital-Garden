# AI 코드 생성 아키텍처 가이드

## AI Code Generation Architecture Guide

> **버전**: 1.0.0 **대상 AI 도구**: Claude Code **문서 목적**: AI 코드 생성 시 일관된 아키텍처 패턴을 보장하기 위한 규범적(Prescriptive) 가이드

---

## 목차

- [§1. 서론 및 목적](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#1-%EC%84%9C%EB%A1%A0-%EB%B0%8F-%EB%AA%A9%EC%A0%81)
- [§2. 핵심 철학](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#2-%ED%95%B5%EC%8B%AC-%EC%B2%A0%ED%95%99)
- [§3. 3-Layer 아키텍처 정의](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#3-3-layer-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EC%A0%95%EC%9D%98)
- [§4. 레이어별 구현 가이드라인](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#4-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%B3%84-%EA%B5%AC%ED%98%84-%EA%B0%80%EC%9D%B4%EB%93%9C%EB%9D%BC%EC%9D%B8)
- [§5. 패턴 선택 기준 (6단계 체크리스트)](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#5-%ED%8C%A8%ED%84%B4-%EC%84%A0%ED%83%9D-%EA%B8%B0%EC%A4%80-6%EB%8B%A8%EA%B3%84-%EC%B2%B4%ED%81%AC%EB%A6%AC%EC%8A%A4%ED%8A%B8)
- [§6. 금지 규칙 (Prohibition Rules)](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#6-%EA%B8%88%EC%A7%80-%EA%B7%9C%EC%B9%99-prohibition-rules)
- [§7. 권장 규칙 (Recommended Rules)](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#7-%EA%B6%8C%EC%9E%A5-%EA%B7%9C%EC%B9%99-recommended-rules)
- [§8. 참조 아키텍처 및 예제](https://claude.ai/chat/b0c5595c-4eaf-4b1c-8a83-164ebfae23e2#8-%EC%B0%B8%EC%A1%B0-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EB%B0%8F-%EC%98%88%EC%A0%9C)

---

## §1. 서론 및 목적

### 1.1 이 가이드가 존재하는 이유

AI 코드 생성 도구(Claude Code 등)는 프롬프트와 컨텍스트에 기반하여 코드를 생성한다. 그러나 **명시적인 아키텍처 규칙 없이** 코드를 생성하면 다음 문제가 발생한다:

1. **레이어 경계 붕괴**: 클라이언트에서 DB에 직접 접근하는 코드 생성
2. **보안 로직 누락**: RLS 정책 없이 테이블 접근 코드 생성
3. **패턴 불일치**: 같은 유형의 기능인데 서로 다른 패턴으로 구현
4. **비즈니스 로직 분산**: SQL, 서버, 클라이언트에 로직이 산재

이 가이드는 AI 코드 생성 시 **판단 기준**을 제공하여 위 문제를 예방한다.

### 1.2 AI 코드 생성 도구를 위한 핵심 전제

> **AI 도구는 이 가이드의 모든 규칙을 "하드 제약(Hard Constraint)"으로 취급해야 한다.** 사용자의 프롬프트가 이 가이드의 규칙과 충돌할 경우, AI 도구는 규칙을 우선하고 사용자에게 충돌 사실을 알려야 한다.

---

## §2. 핵심 철학

### 2.1 서버 중심 (Server-Centric)

**원칙**: 비즈니스 로직의 중심은 **서버 TypeScript**이다.

```
┌─────────────────────────────────────────────────────┐
│                     핵심 철학                         │
│                                                     │
│   "클라이언트는 표현(Presentation)만 담당하고,       │
│    서버가 판단(Decision)을 내리며,                    │
│    데이터베이스가 무결성(Integrity)을 보장한다."       │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**왜 서버 중심인가?**

|관점|클라이언트 중심의 문제|서버 중심의 이점|
|---|---|---|
|**보안**|클라이언트 코드는 사용자가 조작 가능|서버 로직은 사용자 접근 불가|
|**일관성**|웹/모바일 각각 로직 중복 구현|서버 한 곳에서 로직 관리|
|**원자성**|클라이언트 멀티 요청 = 부분 실패 위험|서버에서 트랜잭션으로 원자 처리|
|**시크릿**|클라이언트에 API 키 노출 불가|서버에서만 시크릿 사용|
|**테스트**|UI 테스트는 느리고 불안정|서버 로직은 단위 테스트 용이|

### 2.2 DB 무결성 활용 (Database Integrity)

**원칙**: 데이터 무결성은 **데이터베이스 레벨에서** 강제한다.

- `UNIQUE` 제약 → 중복 방지 (애플리케이션 레벨 체크에 의존하지 않음)
- `CHECK` 제약 → 값 범위 강제
- `FOREIGN KEY` → 참조 무결성 보장
- `NOT NULL` → 필수 필드 강제
- `RLS(Row Level Security)` → 행 단위 접근 제어

**안티패턴**: 애플리케이션 코드에서만 유효성 검사를 수행하고 DB 제약을 설정하지 않는 것

```typescript
// ❌ BAD: 애플리케이션에서만 유효성 검사
async function createUser(email: string) {
  const existing = await db.query('SELECT id FROM users WHERE email = $1', [email]);
  if (existing.length > 0) throw new Error('Email already exists');
  // Race condition 가능!
  await db.query('INSERT INTO users (email) VALUES ($1)', [email]);
}

// ✅ GOOD: DB 제약 + 애플리케이션 검증 병행
// DB: ALTER TABLE users ADD CONSTRAINT users_email_unique UNIQUE (email);
async function createUser(email: string) {
  try {
    await db.query('INSERT INTO users (email) VALUES ($1)', [email]);
  } catch (error) {
    if (error.code === '23505') throw new ConflictError('Email already exists');
    throw error;
  }
}
```

### 2.3 역할 분리 (Separation of Concerns)

**원칙**: 각 레이어는 자신의 역할만 수행하며, 다른 레이어의 역할을 침범하지 않는다.

|레이어|역할|하지 않는 것|
|---|---|---|
|**클라이언트**|UI 렌더링, 사용자 입력 처리, 낙관적 업데이트|비즈니스 판단, 데이터 가공, 시크릿 사용|
|**서버**|비즈니스 로직, 인증/인가, 외부 API 호출, 데이터 변환|UI 렌더링, DB 스키마 결정|
|**데이터**|저장, 무결성, 접근 제어(RLS), 감사(Audit)|비즈니스 판단, 외부 API 호출, 복잡한 계산|

### 2.4 원자성 우선 (Atomicity First)

**원칙**: 여러 데이터 변경이 함께 성공하거나 함께 실패해야 할 때, 반드시 **트랜잭션/RPC**를 사용한다.

```
상황: 주문 생성 시 재고 차감 + 결제 기록 + 알림 생성이 필요

❌ 클라이언트에서 3번의 API 호출:
   1. POST /api/inventory/deduct  ← 성공
   2. POST /api/payments/charge   ← 실패!
   3. POST /api/notifications     ← 실행 안 됨
   → 재고만 차감되고 결제는 안 된 비정상 상태

✅ 서버에서 1번의 RPC로 원자 처리:
   1. supabase.rpc('create_order', {...})
      → 내부에서 재고 차감 + 결제 기록 + 알림 생성을 트랜잭션으로 처리
      → 하나라도 실패하면 전부 롤백
```

---

## §3. 3-Layer 아키텍처 정의

### 3.0 전체 아키텍처 다이어그램

```
┌─────────────────────────────────────────────────────────────────┐
│                         LAYER 1: 클라이언트                       │
│                                                                 │
│  ┌──────────────────────┐    ┌──────────────────────┐          │
│  │     Expo (Mobile)     │    │    Next.js (Web)      │          │
│  │                      │    │                      │          │
│  │  • React Native UI    │    │  • React UI (CSR/SSR) │          │
│  │  • Zustand (로컬상태)  │    │  • Zustand (로컬상태)  │          │
│  │  • TanStack Query     │    │  • TanStack Query     │          │
│  │  • repositories/      │    │  • Server Components   │          │
│  │    (Supabase 캡슐화)   │    │  • Server Actions      │          │
│  └──────────┬───────────┘    └──────────┬───────────┘          │
│             │                          │                       │
│             ▼                          ▼                       │
│  ┌──────────────────────┐    ┌──────────────────────┐          │
│  │  Expo API Routes      │    │  Next.js Route        │          │
│  │  (서버 사이드 로직)     │    │  Handlers / Actions   │          │
│  └──────────┬───────────┘    └──────────┬───────────┘          │
└─────────────┼──────────────────────────┼───────────────────────┘
              │                          │
              ▼                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                       LAYER 2: 서버 (공유 비즈니스 로직)            │
│                                                                 │
│  ┌────────────────────────────────────────────────────────┐     │
│  │  packages/shared/                                      │     │
│  │  • validators/ (Zod 스키마)                             │     │
│  │  • types/ (TypeScript 타입)                             │     │
│  │  • utils/ (순수 함수)                                    │     │
│  └────────────────────────────────────────────────────────┘     │
│                                                                 │
│  ┌────────────────────────────────────────────────────────┐     │
│  │  Supabase Edge Functions (필요 시)                       │     │
│  │  • 외부 API 연동 (결제, 이메일 등)                        │     │
│  │  • 크론 작업                                             │     │
│  │  • Webhook 수신                                         │     │
│  └────────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       LAYER 3: 데이터 (Supabase)                 │
│                                                                 │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────┐ │
│  │   Tables    │  │    Views   │  │    RPC     │  │ Triggers │ │
│  │  + RLS      │  │ (읽기전용)  │  │ (쓰기/원자) │  │ (감사용)  │ │
│  │  + 제약조건   │  │+sec_invoker│  │            │  │          │ │
│  └────────────┘  └────────────┘  └────────────┘  └──────────┘ │
│                                                                 │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐               │
│  │  Storage    │  │  Realtime  │  │    Auth    │               │
│  │ (파일 저장)  │  │ (실시간)    │  │ (인증/JWT) │               │
│  └────────────┘  └────────────┘  └────────────┘               │
└─────────────────────────────────────────────────────────────────┘
```

### 3.1 Supabase 데이터 레이어 — RLS (Row Level Security)

> **역할**: 모든 테이블에 대한 **행 단위 접근 제어의 최후 방어선**

#### 적용 원칙

|규칙|설명|
|---|---|
|**전체 적용 필수**|예외 없이 모든 테이블에 RLS를 활성화한다|
|**최소 권한 원칙**|기본은 모든 접근 차단. 필요한 권한만 명시적으로 허용|
|**이중 방어**|RLS는 서버 레이어 인가 검증의 **보조 수단**이지, 유일한 수단이 아님|
|**성능 고려**|RLS 정책에 복잡한 서브쿼리 사용 자제. 인덱스 활용|

#### 표준 RLS 패턴

```sql
-- ① 테이블 생성 시 즉시 RLS 활성화
ALTER TABLE public.posts ENABLE ROW LEVEL SECURITY;

-- ② 기본 정책: 인증된 사용자의 자기 데이터 접근
CREATE POLICY "Users can view own posts"
  ON public.posts
  FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own posts"
  ON public.posts
  FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update own posts"
  ON public.posts
  FOR UPDATE
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can delete own posts"
  ON public.posts
  FOR DELETE
  USING (auth.uid() = user_id);

-- ③ 역할 기반 정책 (관리자)
CREATE POLICY "Admins can do anything"
  ON public.posts
  FOR ALL
  USING (
    EXISTS (
      SELECT 1 FROM public.user_roles
      WHERE user_id = auth.uid()
      AND role = 'admin'
    )
  );

-- ④ 공개 데이터 정책
CREATE POLICY "Anyone can view published posts"
  ON public.posts
  FOR SELECT
  USING (status = 'published');
```

#### RLS 누락 감지 쿼리

```sql
-- 이 쿼리로 RLS가 비활성화된 테이블을 찾아라
SELECT schemaname, tablename, rowsecurity
FROM pg_tables
WHERE schemaname = 'public'
  AND rowsecurity = false;
```

### 3.2 Supabase 데이터 레이어 — RPC (Remote Procedure Call / DB 함수)

> **역할**: 원자적 쓰기 작업 및 복합 트랜잭션 처리

#### 사용 시점 (MUST)

|상황|이유|
|---|---|
|여러 테이블에 동시 쓰기|원자성 보장 필요|
|읽기 후 쓰기 (Read-then-Write)|Race condition 방지|
|재고 차감 등 경쟁 상태 가능 로직|`SELECT ... FOR UPDATE` 필요|
|권한 검증 + 데이터 변경 조합|단일 트랜잭션으로 처리|

#### 사용하지 않는 경우 (MUST NOT)

|상황|대신 사용할 것|
|---|---|
|단순 CRUD (한 테이블)|서버에서 Supabase Client 직접 사용|
|복잡한 비즈니스 판단 로직|서버 TypeScript에서 처리 후 DB 호출|
|외부 API 호출이 포함된 로직|서버/Edge Function에서 처리|

#### 표준 RPC 패턴

```sql
-- 예: 주문 생성 RPC (원자적 처리)
CREATE OR REPLACE FUNCTION public.create_order(
  p_user_id UUID,
  p_items JSONB,
  p_total_amount NUMERIC
)
RETURNS UUID
LANGUAGE plpgsql
SECURITY DEFINER  -- 주의: 필요한 경우에만 사용
SET search_path = public
AS $$
DECLARE
  v_order_id UUID;
  v_item JSONB;
BEGIN
  -- 1. 주문 레코드 생성
  INSERT INTO orders (user_id, total_amount, status)
  VALUES (p_user_id, p_total_amount, 'pending')
  RETURNING id INTO v_order_id;

  -- 2. 주문 항목 추가 + 재고 차감 (원자적)
  FOR v_item IN SELECT * FROM jsonb_array_elements(p_items)
  LOOP
    INSERT INTO order_items (order_id, product_id, quantity, price)
    VALUES (
      v_order_id,
      (v_item->>'product_id')::UUID,
      (v_item->>'quantity')::INT,
      (v_item->>'price')::NUMERIC
    );

    -- 재고 차감 (동시성 안전)
    UPDATE products
    SET stock = stock - (v_item->>'quantity')::INT
    WHERE id = (v_item->>'product_id')::UUID
      AND stock >= (v_item->>'quantity')::INT;

    IF NOT FOUND THEN
      RAISE EXCEPTION 'Insufficient stock for product %',
        v_item->>'product_id';
    END IF;
  END LOOP;

  -- 3. 감사 로그 (트리거가 아닌 명시적 기록)
  INSERT INTO audit_log (action, entity_type, entity_id, user_id)
  VALUES ('CREATE', 'order', v_order_id, p_user_id);

  RETURN v_order_id;
END;
$$;
```

#### RPC 보안 주의사항

```sql
-- ✅ SECURITY INVOKER (기본값, RLS 적용됨) - 일반적으로 이것을 사용
CREATE FUNCTION public.get_user_stats(p_user_id UUID)
RETURNS JSON
LANGUAGE plpgsql
SECURITY INVOKER  -- 호출자의 권한으로 실행 → RLS 적용
AS $$ ... $$;

-- ⚠️ SECURITY DEFINER (RLS 우회) - 신뢰할 수 있는 서버에서만 호출
-- 반드시 입력 검증 + 권한 확인 로직을 함수 내부에 포함해야 함
CREATE FUNCTION public.admin_reset_password(p_user_id UUID)
RETURNS VOID
LANGUAGE plpgsql
SECURITY DEFINER
SET search_path = public  -- search_path 고정 필수
AS $$
BEGIN
  -- 반드시 호출자 권한 검증
  IF NOT EXISTS (
    SELECT 1 FROM user_roles
    WHERE user_id = auth.uid() AND role = 'admin'
  ) THEN
    RAISE EXCEPTION 'Unauthorized';
  END IF;
  -- ... 실제 로직
END;
$$;
```

### 3.3 Supabase 데이터 레이어 — View

> **역할**: 복합 읽기 쿼리의 캡슐화 및 클라이언트 데이터 노출 범위 제어

#### 사용 시점

|상황|이유|
|---|---|
|여러 테이블 조인이 필요한 읽기|클라이언트에 단순한 인터페이스 제공|
|민감한 컬럼 숨기기|원본 테이블의 일부만 노출|
|집계/통계 데이터 제공|반복적인 집계 쿼리 캡슐화|
|복잡한 필터/정렬 로직|클라이언트 쿼리 단순화|

#### `security_invoker` 설정 규칙

```sql
-- ✅ CORRECT: security_invoker = true → RLS가 적용됨
CREATE VIEW public.posts_with_author
WITH (security_invoker = true)
AS
SELECT
  p.id,
  p.title,
  p.content,
  p.created_at,
  u.display_name AS author_name,
  u.avatar_url AS author_avatar
FROM public.posts p
JOIN public.users u ON p.user_id = u.id;

-- ❌ WRONG: security_invoker 없음 → RLS 우회됨 (보안 구멍)
CREATE VIEW public.posts_with_author AS
SELECT p.*, u.* FROM posts p JOIN users u ON p.user_id = u.id;
```

#### View vs RPC 판단 기준

```
읽기 전용인가?
  ├── YES → View 사용
  └── NO → 쓰기가 포함되는가?
        ├── YES → RPC 사용
        └── 복합 읽기 + 단순 쓰기 → 읽기는 View, 쓰기는 별도 RPC
```

### 3.4 Supabase 데이터 레이어 — Trigger

> **역할**: 감사 로그, 타임스탬프 자동 갱신, 데이터 동기화 **전용**

#### 허용되는 트리거 사용 범위

|용도|예시|허용|
|---|---|---|
|**타임스탬프 자동 갱신**|`updated_at` 필드 자동 업데이트|✅|
|**감사 로그 기록**|변경 이력 자동 추적|✅|
|**데이터 동기화**|비정규화 필드 자동 동기화|✅ (주의)|
|**비즈니스 로직**|포인트 계산, 등급 변경 등|❌|
|**외부 API 호출**|이메일 발송, 웹훅 등|❌|
|**복잡한 조건 분기**|if-else 체인|❌|

#### 표준 트리거 패턴

```sql
-- ✅ 허용: updated_at 자동 갱신
CREATE OR REPLACE FUNCTION public.handle_updated_at()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$;

CREATE TRIGGER set_updated_at
  BEFORE UPDATE ON public.posts
  FOR EACH ROW
  EXECUTE FUNCTION public.handle_updated_at();

-- ✅ 허용: 감사 로그 자동 기록
CREATE OR REPLACE FUNCTION public.audit_log_trigger()
RETURNS TRIGGER
LANGUAGE plpgsql
SECURITY DEFINER
SET search_path = public
AS $$
BEGIN
  INSERT INTO audit_log (
    table_name, record_id, action, old_data, new_data, changed_by
  ) VALUES (
    TG_TABLE_NAME,
    COALESCE(NEW.id, OLD.id),
    TG_OP,
    CASE WHEN TG_OP = 'DELETE' THEN row_to_json(OLD) ELSE NULL END,
    CASE WHEN TG_OP != 'DELETE' THEN row_to_json(NEW) ELSE NULL END,
    auth.uid()
  );
  RETURN COALESCE(NEW, OLD);
END;
$$;

-- ❌ 금지: 비즈니스 로직을 트리거에 넣지 마라
-- 아래는 안티패턴의 예시
CREATE OR REPLACE FUNCTION public.calculate_user_level()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
  -- ❌ 이런 비즈니스 로직은 서버 TypeScript에 있어야 한다
  IF NEW.total_points >= 1000 THEN
    NEW.level = 'gold';
  ELSIF NEW.total_points >= 500 THEN
    NEW.level = 'silver';
  ELSE
    NEW.level = 'bronze';
  END IF;
  RETURN NEW;
END;
$$;
```

---

## §4. 레이어별 구현 가이드라인

### 4.1 Layer 1 — 클라이언트 (Expo / Next.js)

#### 4.1.1 Expo (모바일) 아키텍처

```
apps/mobile/src/
├── app/                    # Expo Router (파일 기반 라우팅)
│   ├── (tabs)/             # 탭 네비게이션
│   ├── (auth)/             # 인증 관련 화면
│   └── _layout.tsx         # 루트 레이아웃
├── components/             # UI 컴포넌트
│   ├── common/             # 공용 컴포넌트
│   └── features/           # 기능별 컴포넌트
├── hooks/                  # 커스텀 훅
│   ├── queries/            # TanStack Query 훅
│   └── mutations/          # TanStack Mutation 훅
├── repositories/           # Supabase 접근 캡슐화 ⭐
│   ├── postRepository.ts
│   ├── userRepository.ts
│   └── types.ts
├── stores/                 # Zustand 스토어 (로컬 UI 상태만)
├── utils/                  # 유틸리티 함수
└── constants/              # 상수 정의
```

**repositories 패턴 (핵심)**:

```typescript
// repositories/postRepository.ts
// 목적: Supabase 구현 세부사항을 캡슐화하여 교체 가능하게 만듦

import { supabase } from '@/lib/supabase';
import type { Post, CreatePostInput } from '@shared/types';

export const postRepository = {
  // 읽기: View를 통해 조회
  async getAll(filters?: PostFilters): Promise<Post[]> {
    const query = supabase
      .from('posts_with_author')  // View 사용
      .select('*');

    if (filters?.status) {
      query.eq('status', filters.status);
    }

    const { data, error } = await query;
    if (error) throw new RepositoryError('Failed to fetch posts', error);
    return data;
  },

  // 쓰기: RPC를 통해 원자적 처리
  async create(input: CreatePostInput): Promise<Post> {
    const { data, error } = await supabase
      .rpc('create_post', {
        p_title: input.title,
        p_content: input.content,
        p_tags: input.tags,
      });

    if (error) throw new RepositoryError('Failed to create post', error);
    return data;
  },

  // 단순 업데이트: 직접 테이블 접근 (RLS가 보호)
  async updateTitle(postId: string, title: string): Promise<void> {
    const { error } = await supabase
      .from('posts')
      .update({ title })
      .eq('id', postId);

    if (error) throw new RepositoryError('Failed to update post', error);
  },

  // 실시간 구독
  subscribeToChanges(callback: (post: Post) => void) {
    return supabase
      .channel('posts-changes')
      .on(
        'postgres_changes',
        { event: '*', schema: 'public', table: 'posts' },
        (payload) => callback(payload.new as Post)
      )
      .subscribe();
  },
};
```

**hooks → repositories 의존성 방향**:

```typescript
// hooks/queries/usePosts.ts
// hooks는 repositories에 의존. 직접 Supabase에 의존하지 않음

import { useQuery } from '@tanstack/react-query';
import { postRepository } from '@/repositories/postRepository';

export function usePosts(filters?: PostFilters) {
  return useQuery({
    queryKey: ['posts', filters],
    queryFn: () => postRepository.getAll(filters),
    staleTime: 1000 * 60 * 5, // 5분
  });
}
```

```
의존성 방향 (단방향):
  Component → Hook → Repository → Supabase Client
  ✅ 단방향: 각 레이어는 하위 레이어만 의존

  ❌ 순환 참조 금지:
     Repository ←→ Hook  (X)
     Component → Supabase Client 직접 (X)
```

#### 4.1.2 Next.js (웹) 아키텍처

```
apps/web/src/
├── app/                    # App Router
│   ├── (public)/           # 공개 페이지
│   ├── (auth)/             # 인증 필요 페이지
│   ├── api/                # Route Handlers (API)
│   └── layout.tsx
├── components/             # UI 컴포넌트
│   ├── server/             # Server Components
│   └── client/             # Client Components
├── actions/                # Server Actions ⭐
│   ├── postActions.ts
│   └── userActions.ts
├── lib/                    # 라이브러리 초기화
│   ├── supabase/
│   │   ├── server.ts       # 서버용 Supabase 클라이언트
│   │   └── client.ts       # 클라이언트용 Supabase 클라이언트
│   └── utils.ts
└── stores/                 # Zustand (클라이언트 상태만)
```

**Server Actions 패턴 (쓰기 작업의 표준)**:

```typescript
// actions/postActions.ts
'use server';

import { createServerClient } from '@/lib/supabase/server';
import { createPostSchema } from '@shared/validators';
import { revalidatePath } from 'next/cache';

export async function createPost(formData: FormData) {
  // 1. 서버에서 입력 검증
  const validated = createPostSchema.safeParse({
    title: formData.get('title'),
    content: formData.get('content'),
  });

  if (!validated.success) {
    return { error: validated.error.flatten() };
  }

  // 2. 서버에서 Supabase 호출
  const supabase = await createServerClient();
  const { data: { user } } = await supabase.auth.getUser();

  if (!user) {
    return { error: 'Unauthorized' };
  }

  // 3. RPC로 원자적 처리 (또는 단순 INSERT)
  const { data, error } = await supabase
    .rpc('create_post', {
      p_title: validated.data.title,
      p_content: validated.data.content,
      p_user_id: user.id,
    });

  if (error) {
    return { error: error.message };
  }

  // 4. 캐시 무효화
  revalidatePath('/posts');
  return { data };
}
```

**Server Components 패턴 (읽기 작업의 표준)**:

```typescript
// app/(auth)/posts/page.tsx
// Server Component: 서버에서 데이터를 가져와서 렌더링

import { createServerClient } from '@/lib/supabase/server';

export default async function PostsPage() {
  const supabase = await createServerClient();

  // View를 통해 조회 (서버 사이드)
  const { data: posts } = await supabase
    .from('posts_with_author')
    .select('*')
    .order('created_at', { ascending: false });

  return <PostList posts={posts ?? []} />;
}
```

### 4.2 Layer 2 — 서버 (공유 비즈니스 로직)

#### 4.2.1 공유 패키지 구조

```
packages/shared/
├── src/
│   ├── types/              # TypeScript 타입 정의
│   │   ├── database.ts     # DB 스키마 기반 타입 (Drizzle 추론 또는 수동)
│   │   ├── api.ts          # API 요청/응답 타입
│   │   └── domain.ts       # 도메인 모델 타입
│   ├── validators/         # Zod 스키마
│   │   ├── post.ts
│   │   ├── user.ts
│   │   └── common.ts       # 공통 검증 (pagination, sort 등)
│   ├── utils/              # 순수 함수 유틸리티
│   │   ├── formatting.ts   # 날짜, 통화, 텍스트 포맷
│   │   ├── calculation.ts  # 비즈니스 계산 로직
│   │   └── constants.ts    # 공유 상수
│   └── errors/             # 에러 타입 정의
│       └── index.ts
├── package.json
└── tsconfig.json
```

#### 4.2.2 타입 일관성 보장

```typescript
// packages/shared/src/types/database.ts
// Supabase 타입 생성기 또는 Drizzle 추론으로 생성

export interface Database {
  public: {
    Tables: {
      posts: {
        Row: {
          id: string;
          title: string;
          content: string;
          user_id: string;
          status: 'draft' | 'published' | 'archived';
          created_at: string;
          updated_at: string;
        };
        Insert: Omit<Database['public']['Tables']['posts']['Row'],
          'id' | 'created_at' | 'updated_at'>;
        Update: Partial<Database['public']['Tables']['posts']['Insert']>;
      };
      // ... 다른 테이블
    };
    Views: {
      posts_with_author: {
        Row: Database['public']['Tables']['posts']['Row'] & {
          author_name: string;
          author_avatar: string | null;
        };
      };
    };
    Functions: {
      create_post: {
        Args: { p_title: string; p_content: string; p_user_id: string };
        Returns: string; // post id
      };
    };
  };
}
```

#### 4.2.3 Zod 스키마와 타입 동기화

```typescript
// packages/shared/src/validators/post.ts
import { z } from 'zod';

// Zod 스키마 정의 (검증 + 타입 추론 소스)
export const createPostSchema = z.object({
  title: z.string()
    .min(1, '제목은 필수입니다')
    .max(200, '제목은 200자 이내여야 합니다'),
  content: z.string()
    .min(1, '내용은 필수입니다')
    .max(50000, '내용은 50,000자 이내여야 합니다'),
  tags: z.array(z.string()).max(10).optional(),
  status: z.enum(['draft', 'published']).default('draft'),
});

// 타입 추론 (별도 타입 정의 불필요)
export type CreatePostInput = z.infer<typeof createPostSchema>;

// 업데이트 스키마 (부분 수정 허용)
export const updatePostSchema = createPostSchema.partial();
export type UpdatePostInput = z.infer<typeof updatePostSchema>;

// 쿼리 파라미터 스키마
export const postQuerySchema = z.object({
  page: z.coerce.number().int().positive().default(1),
  limit: z.coerce.number().int().min(1).max(100).default(20),
  status: z.enum(['draft', 'published', 'archived']).optional(),
  search: z.string().optional(),
  sortBy: z.enum(['created_at', 'updated_at', 'title']).default('created_at'),
  sortOrder: z.enum(['asc', 'desc']).default('desc'),
});
export type PostQuery = z.infer<typeof postQuerySchema>;
```

#### 4.2.4 Edge Functions (외부 API 연동)

```typescript
// supabase/functions/send-notification/index.ts
// Edge Function: 외부 API 호출이 필요한 서버 로직

import { serve } from 'https://deno.land/std@0.177.0/http/server.ts';
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2';

serve(async (req) => {
  try {
    const { userId, message, type } = await req.json();

    // 입력 검증
    if (!userId || !message || !type) {
      return new Response(
        JSON.stringify({ error: 'Missing required fields' }),
        { status: 400 }
      );
    }

    // Supabase 클라이언트 (서비스 키 사용 — 서버 전용)
    const supabase = createClient(
      Deno.env.get('SUPABASE_URL')!,
      Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!,
    );

    // 사용자 알림 설정 조회
    const { data: preferences } = await supabase
      .from('notification_preferences')
      .select('*')
      .eq('user_id', userId)
      .single();

    // 외부 API 호출 (푸시 알림, 이메일 등)
    if (preferences?.push_enabled) {
      await fetch('https://fcm.googleapis.com/fcm/send', {
        method: 'POST',
        headers: {
          'Authorization': `key=${Deno.env.get('FCM_SERVER_KEY')}`,
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          to: preferences.fcm_token,
          notification: { title: type, body: message },
        }),
      });
    }

    return new Response(JSON.stringify({ success: true }), { status: 200 });
  } catch (error) {
    return new Response(
      JSON.stringify({ error: error.message }),
      { status: 500 }
    );
  }
});
```

### 4.3 Layer 3 — 데이터 (Supabase PostgreSQL)

#### 4.3.1 스키마 설계 원칙

```sql
-- ✅ 모든 테이블에 적용해야 하는 기본 패턴

-- 1. UUID 기본키
-- 2. 타임스탬프 (created_at, updated_at)
-- 3. 소프트 삭제 (선택적)
-- 4. RLS 즉시 활성화
-- 5. 인덱스 전략

CREATE TABLE public.posts (
  -- 기본키
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

  -- 데이터 필드
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'draft'
    CHECK (status IN ('draft', 'published', 'archived')),

  -- 외래키
  user_id UUID NOT NULL REFERENCES public.users(id) ON DELETE CASCADE,
  category_id UUID REFERENCES public.categories(id) ON DELETE SET NULL,

  -- 타임스탬프
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),

  -- 소프트 삭제 (선택적)
  deleted_at TIMESTAMPTZ DEFAULT NULL
);

-- RLS 즉시 활성화
ALTER TABLE public.posts ENABLE ROW LEVEL SECURITY;

-- 인덱스 전략
CREATE INDEX idx_posts_user_id ON public.posts(user_id);
CREATE INDEX idx_posts_status ON public.posts(status) WHERE deleted_at IS NULL;
CREATE INDEX idx_posts_created_at ON public.posts(created_at DESC);
CREATE INDEX idx_posts_search ON public.posts USING gin(
  to_tsvector('korean', title || ' ' || content)
);
```

#### 4.3.2 마이그레이션 명명 규칙

```
supabase/migrations/
├── 20240101000000_create_users_table.sql
├── 20240101000001_create_posts_table.sql
├── 20240101000002_create_posts_rls_policies.sql
├── 20240101000003_create_posts_views.sql
├── 20240101000004_create_posts_rpc_functions.sql
├── 20240101000005_create_posts_triggers.sql
└── 20240102000000_add_posts_search_index.sql
```

**규칙**: `{timestamp}_{action}_{target}.sql`

- action: `create`, `alter`, `drop`, `add`, `remove`
- 하나의 마이그레이션 파일은 하나의 목적만 수행

---

## §5. 패턴 선택 기준 (6단계 체크리스트)

> **AI 코드 생성 시 어떤 패턴을 사용할지 결정하는 의사결정 트리**
> 
> 모든 새 기능 구현 시 이 체크리스트를 순서대로 통과시켜라.

```
┌────────────────────────────────────────────────────────┐
│           §5. 패턴 선택 6단계 체크리스트                   │
│                                                        │
│  새 기능을 구현할 때, 아래 질문을 순서대로 확인한다.        │
│  가장 먼저 YES가 되는 단계의 패턴을 따른다.               │
└────────────────────────────────────────────────────────┘
```

### Step 1: 시크릿 키가 필요한가?

```
시크릿 키(API 키, 서비스 계정, 외부 API 토큰 등)가 필요한가?
  ├── YES → 반드시 서버 레이어에서 처리
  │         • Next.js: Route Handler 또는 Server Action
  │         • Expo: Expo API Route 또는 Edge Function
  │         • 절대로 클라이언트 코드에 시크릿 포함 금지
  │         → Step 2로 진행하여 추가 판단
  │
  └── NO  → Step 2로 진행
```

**예시**: 결제 API 호출, 이메일 발송, 외부 서비스 인증

### Step 2: 원자적 처리가 필요한가?

```
여러 데이터 변경이 함께 성공/실패해야 하는가?
  ├── YES → RPC(DB Function)로 트랜잭션 처리
  │         • 서버에서 supabase.rpc() 호출
  │         • 복잡한 비즈니스 판단은 서버 TypeScript에서 먼저 수행
  │         • RPC는 데이터 조작만 담당
  │         → 구현 완료
  │
  └── NO  → Step 3으로 진행
```

**예시**: 주문 생성(재고 차감 + 주문 기록), 포인트 전환(차감 + 적립)

### Step 3: 단순 조회인가?

```
데이터를 읽기만 하는가? (쓰기 없음)
  ├── YES → 조회가 복잡한가? (여러 테이블 조인, 집계 등)
  │         ├── YES → View 사용
  │         │         • security_invoker = true 설정
  │         │         • 클라이언트에서 View를 직접 조회
  │         │
  │         └── NO  → 단일 테이블 직접 조회
  │                   • supabase.from('table').select()
  │                   • RLS가 보호
  │         → 구현 완료
  │
  └── NO (쓰기 포함) → Step 4로 진행
```

**예시**: 게시글 목록(View), 사용자 프로필(직접 조회)

### Step 4: 실시간 업데이트가 필요한가?

```
데이터 변경을 실시간으로 클라이언트에 반영해야 하는가?
  ├── YES → Supabase Realtime 구독 사용
  │         • repository에서 채널 구독 구현
  │         • TanStack Query 캐시와 연동
  │         • 쓰기는 여전히 서버/RPC를 통해 수행
  │         → Step 5로 진행 (쓰기 방식 결정)
  │
  └── NO  → Step 5로 진행
```

**예시**: 채팅 메시지, 실시간 알림, 라이브 댓글

### Step 5: 파일 처리가 필요한가?

```
파일 업로드/다운로드가 포함되는가?
  ├── YES → Supabase Storage 사용
  │         • 업로드: 클라이언트 → Storage (직접 또는 서명된 URL)
  │         • 메타데이터: 서버에서 DB에 기록
  │         • 다운로드: Storage 공개 URL 또는 서명된 URL
  │         → 구현 완료
  │
  └── NO  → Step 6으로 진행
```

**예시**: 프로필 이미지, 첨부 파일, 미디어 콘텐츠

### Step 6: 순수 UI 상태인가?

```
서버와 무관한 클라이언트 전용 상태인가?
  ├── YES → Zustand 스토어 사용
  │         • 모달 열림/닫힘, 사이드바 토글
  │         • 입력 폼 임시 상태
  │         • UI 테마, 로컬 설정
  │         → 구현 완료
  │
  └── NO  → 위 단계를 다시 검토하거나, 복합 패턴 조합
```

### 체크리스트 요약 테이블

|Step|질문|YES 패턴|주요 기술|
|---|---|---|---|
|1|시크릿 필요?|서버 레이어 필수|Server Action, Route Handler, Edge Function|
|2|원자 처리 필요?|RPC (DB Function)|`supabase.rpc()`|
|3|단순 조회?|View 또는 직접 쿼리|View + `security_invoker`, `.select()`|
|4|실시간 필요?|Realtime 구독|`supabase.channel().on()`|
|5|파일 처리?|Storage|`supabase.storage`|
|6|UI 상태만?|Zustand|`create()` 스토어|

### 복합 패턴 예시

일부 기능은 여러 Step을 조합해야 한다:

```
예: "실시간 채팅 + 이미지 첨부"

Step 1: 시크릿 필요? → NO (Supabase 클라이언트 키 사용)
Step 2: 원자 처리? → NO (단일 메시지 삽입)
Step 3: 단순 조회? → NO (쓰기 포함)
Step 4: 실시간? → YES → Realtime 구독 ✅
Step 5: 파일? → YES → Storage 사용 ✅
Step 6: UI 상태? → YES → 입력 상태 Zustand ✅

→ 조합: Realtime 구독 + Storage 업로드 + Zustand 입력 상태
→ 쓰기: repository에서 직접 INSERT (RLS 보호)
→ 읽기: Realtime으로 자동 수신
```

---

## §6. 금지 규칙 (Prohibition Rules)

> **이 규칙들은 절대적이다. 어떤 상황에서도 예외를 허용하지 않는다.** AI 코드 생성 시 이 규칙에 위배되는 코드가 생성되면 즉시 수정해야 한다.

### 규칙 P1: 클라이언트에서 서버를 우회한 데이터 변조 금지

```
❌ 금지: 클라이언트 코드에서 직접 Supabase에 쓰기 작업을 수행하되,
         비즈니스 로직 검증 없이 수행하는 것

✅ 허용: RLS가 충분한 보호를 제공하는 단순 CRUD는 클라이언트에서 직접 가능
         (예: 사용자가 자신의 프로필 이름 변경)

✅ 허용: 서버 레이어(Server Action, Route Handler, Edge Function)를 통한 쓰기

판단 기준:
  - 단순 필드 업데이트 + RLS가 소유자 확인 → 클라이언트 직접 가능
  - 비즈니스 규칙 검증 필요 (예: 잔액 확인 후 차감) → 반드시 서버 경유
  - 여러 테이블 변경 → 반드시 서버/RPC 경유
```

```typescript
// ❌ P1 위반: 클라이언트에서 잔액 확인 없이 직접 차감
async function purchaseItem(userId: string, itemId: string) {
  const { data: user } = await supabase
    .from('users')
    .select('balance')
    .eq('id', userId)
    .single();

  // 클라이언트에서 잔액 체크 → 조작 가능!
  if (user.balance >= item.price) {
    await supabase
      .from('users')
      .update({ balance: user.balance - item.price })
      .eq('id', userId);
    // Race condition + 클라이언트 조작 가능
  }
}

// ✅ P1 준수: 서버에서 RPC로 원자적 처리
// Server Action
async function purchaseItem(userId: string, itemId: string) {
  const { data, error } = await supabase.rpc('purchase_item', {
    p_user_id: userId,
    p_item_id: itemId,
  });
  // RPC 내부에서 잔액 확인 + 차감 + 기록을 트랜잭션으로 처리
}
```

### 규칙 P2: 클라이언트 코드에 시크릿 키 포함 금지

```
❌ 금지: 다음 항목이 클라이언트 코드에 존재하는 것
  - Supabase Service Role Key
  - 외부 API Secret Key
  - 데이터베이스 비밀번호
  - JWT Secret
  - 암호화 키
  - 서비스 계정 자격 증명

✅ 허용: 클라이언트에 존재 가능한 키
  - Supabase Anon Key (공개 키, RLS로 보호됨)
  - 공개 API 키 (Google Maps Public Key 등)
```

```typescript
// ❌ P2 위반
const supabase = createClient(url, process.env.SUPABASE_SERVICE_ROLE_KEY!);

// ✅ P2 준수
// 클라이언트
const supabase = createClient(url, process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!);
// 서버에서만
const supabaseAdmin = createClient(url, process.env.SUPABASE_SERVICE_ROLE_KEY!);
```

**AI 도구 검증 방법**: 생성된 코드에서 `NEXT_PUBLIC_` 접두사가 없는 환경 변수가 클라이언트 컴포넌트에서 사용되면 P2 위반이다.

### 규칙 P3: 비즈니스 로직의 SQL 전용 구현 금지

```
❌ 금지: 비즈니스 판단/분기 로직이 SQL 함수에만 존재하고,
         서버 TypeScript에 대응하는 로직이 없는 것

✅ 허용: SQL 함수가 데이터 조작(쓰기/트랜잭션)만 담당하고,
         비즈니스 판단은 서버 TypeScript에서 수행하는 것

핵심: "SQL 함수는 HOW(어떻게 저장할까), 서버는 WHAT/WHY(무엇을/왜 저장할까)"
```

```sql
-- ❌ P3 위반: 비즈니스 로직이 SQL에만 존재
CREATE FUNCTION calculate_shipping_cost(p_order_id UUID)
RETURNS NUMERIC AS $$
DECLARE
  v_weight NUMERIC;
  v_distance NUMERIC;
  v_cost NUMERIC;
BEGIN
  -- 복잡한 비즈니스 로직 (무게별 요율, 거리별 할인, 프로모션 등)
  -- 이 로직은 서버 TypeScript에서 관리해야 함
  SELECT total_weight INTO v_weight FROM orders WHERE id = p_order_id;

  IF v_weight < 1 THEN v_cost := 3000;
  ELSIF v_weight < 5 THEN v_cost := 5000;
  ELSIF v_weight < 10 THEN v_cost := 8000;
  ELSE v_cost := v_weight * 1000;
  END IF;

  -- 프로모션 할인 (비즈니스 정책)
  IF EXISTS (SELECT 1 FROM promotions WHERE ...) THEN
    v_cost := v_cost * 0.8;
  END IF;

  RETURN v_cost;
END;
$$ LANGUAGE plpgsql;
```

```typescript
// ✅ P3 준수: 비즈니스 로직은 서버 TypeScript, SQL은 데이터 조작만
// packages/shared/src/utils/shipping.ts
export function calculateShippingCost(weight: number, promotions: Promotion[]): number {
  let cost: number;

  if (weight < 1) cost = 3000;
  else if (weight < 5) cost = 5000;
  else if (weight < 10) cost = 8000;
  else cost = weight * 1000;

  const activePromo = promotions.find(p => p.type === 'shipping_discount');
  if (activePromo) {
    cost *= (1 - activePromo.discountRate);
  }

  return Math.round(cost);
}

// Server Action에서 사용
async function createOrder(input: CreateOrderInput) {
  const cost = calculateShippingCost(input.totalWeight, input.promotions);
  await supabase.rpc('create_order', { ...input, shipping_cost: cost });
}
```

### 규칙 P4: 트리거에 비즈니스 로직 배치 금지

```
❌ 금지: 트리거에서 수행하는 것
  - 사용자 등급 계산/변경
  - 포인트 적립/차감
  - 알림 발송 로직
  - 가격 계산
  - 외부 API 호출
  - 조건부 비즈니스 분기

✅ 허용: 트리거에서 수행하는 것 (3가지만)
  - updated_at 타임스탬프 자동 갱신
  - 감사(audit) 로그 자동 기록
  - 비정규화 필드 동기화 (카운터 캐시 등)
```

**왜 금지하는가?**

1. **디버깅 불가**: 트리거는 암묵적으로 실행되어 오류 추적이 극히 어려움
2. **테스트 불가**: 트리거 내부 로직을 단위 테스트하기 어려움
3. **순서 의존성**: 여러 트리거 간 실행 순서가 보장되지 않음
4. **성능 저하**: 모든 INSERT/UPDATE에 부가 로직이 실행됨
5. **이중 관리**: 서버 로직과 트리거 로직이 분리되어 불일치 발생

### 규칙 P5: RLS 미설정 테이블 배포 금지

```
❌ 금지: RLS가 비활성화된 테이블이 프로덕션에 존재하는 것
         (임시 테이블, 마이그레이션 중 테이블 포함)

✅ 필수: 모든 테이블 생성 시 즉시 RLS 활성화 + 최소 1개 정책 설정

예외 없음. 데이터가 없는 빈 테이블도 RLS를 활성화해야 한다.
```

```sql
-- ✅ 테이블 생성 즉시 RLS 활성화 (마이그레이션 파일 내)
CREATE TABLE public.user_settings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES public.users(id),
  theme TEXT DEFAULT 'light',
  language TEXT DEFAULT 'ko'
);

-- 같은 마이그레이션 파일 내에서 즉시 RLS 설정
ALTER TABLE public.user_settings ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can manage own settings"
  ON public.user_settings
  FOR ALL
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);
```

---

## §7. 권장 규칙 (Recommended Rules)

> **강제는 아니지만, 준수하면 코드 품질과 유지보수성이 크게 향상된다.** AI 코드 생성 시 기본 패턴으로 적용하되, 합리적 이유가 있으면 예외를 허용한다.

### 규칙 R1: TypeScript Strict Mode 전체 적용

```jsonc
// tsconfig.json (모노레포 루트)
{
  "compilerOptions": {
    "strict": true,                    // 모든 strict 옵션 활성화
    "noUncheckedIndexedAccess": true,  // 배열/객체 인덱스 접근 시 undefined 체크 강제
    "noImplicitReturns": true,         // 모든 코드 경로에서 반환값 필수
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true,
    "exactOptionalPropertyTypes": true  // optional 프로퍼티에 undefined 명시적 할당 금지
  }
}
```

**왜 권장하는가?**

- AI 생성 코드의 타입 오류를 컴파일 타임에 잡을 수 있음
- `any` 타입 사용을 자연스럽게 억제
- 런타임 에러를 줄여 디버깅 시간 절감

### 규칙 R2: 모든 API 경계에 Zod 검증 적용

```
모든 데이터 입력 지점에 Zod 스키마 검증을 적용한다:
  - Server Action 입력
  - Route Handler 요청 body
  - Edge Function 요청
  - 클라이언트 폼 데이터 (추가 방어)
```

```typescript
// ✅ R2 준수: Server Action에서 Zod 검증
'use server';

import { createPostSchema } from '@shared/validators';

export async function createPost(rawInput: unknown) {
  // 1. 입력 검증 (Zod)
  const result = createPostSchema.safeParse(rawInput);
  if (!result.success) {
    return {
      success: false,
      error: result.error.flatten(),
    };
  }

  // 2. 검증된 데이터만 사용
  const { title, content, tags } = result.data;
  // ...
}
```

### 규칙 R3: Repositories 패턴으로 데이터 접근 캡슐화

```
Expo(모바일) 앱에서는 repositories 패턴을 통해 Supabase 접근을 캡슐화한다.
이를 통해:
  - Supabase를 다른 백엔드로 교체할 때 repositories만 수정
  - 테스트 시 repository를 모킹하여 단위 테스트 용이
  - 데이터 접근 로직이 한 곳에 집중
```

```typescript
// ✅ R3 준수: Repository 인터페이스 정의
// repositories/types.ts
export interface IPostRepository {
  getAll(filters?: PostFilters): Promise<Post[]>;
  getById(id: string): Promise<Post | null>;
  create(input: CreatePostInput): Promise<Post>;
  update(id: string, input: UpdatePostInput): Promise<Post>;
  delete(id: string): Promise<void>;
  subscribeToChanges(callback: (post: Post) => void): () => void;
}

// repositories/supabase/postRepository.ts
// 구현체는 Supabase에 의존하지만, 인터페이스는 범용적
export class SupabasePostRepository implements IPostRepository {
  constructor(private client: SupabaseClient) {}

  async getAll(filters?: PostFilters): Promise<Post[]> {
    // Supabase 전용 구현
  }
  // ...
}

// 나중에 교체 가능:
// repositories/firebase/postRepository.ts
// repositories/rest/postRepository.ts
```

### 규칙 R4: 일관된 에러 처리 패턴 적용

```typescript
// packages/shared/src/errors/index.ts

// 도메인 에러 정의
export class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number = 500,
    public details?: unknown,
  ) {
    super(message);
    this.name = 'AppError';
  }
}

export class NotFoundError extends AppError {
  constructor(entity: string, id: string) {
    super(`${entity} with id ${id} not found`, 'NOT_FOUND', 404);
  }
}

export class ValidationError extends AppError {
  constructor(details: unknown) {
    super('Validation failed', 'VALIDATION_ERROR', 400, details);
  }
}

export class UnauthorizedError extends AppError {
  constructor(message = 'Unauthorized') {
    super(message, 'UNAUTHORIZED', 401);
  }
}

export class ConflictError extends AppError {
  constructor(message: string) {
    super(message, 'CONFLICT', 409);
  }
}

// 에러 응답 표준 형식
export interface ErrorResponse {
  success: false;
  error: {
    code: string;
    message: string;
    details?: unknown;
  };
}

export interface SuccessResponse<T> {
  success: true;
  data: T;
}

export type ApiResponse<T> = SuccessResponse<T> | ErrorResponse;
```

```typescript
// ✅ R4 준수: 일관된 에러 처리 (Server Action)
'use server';

export async function createPost(input: unknown): Promise<ApiResponse<Post>> {
  try {
    // 검증
    const validated = createPostSchema.safeParse(input);
    if (!validated.success) {
      return {
        success: false,
        error: { code: 'VALIDATION_ERROR', message: 'Invalid input', details: validated.error.flatten() },
      };
    }

    // 인증 확인
    const supabase = await createServerClient();
    const { data: { user } } = await supabase.auth.getUser();
    if (!user) {
      return {
        success: false,
        error: { code: 'UNAUTHORIZED', message: 'Authentication required' },
      };
    }

    // 비즈니스 로직 수행
    const { data, error } = await supabase.rpc('create_post', { ... });
    if (error) throw error;

    return { success: true, data };
  } catch (error) {
    // 예상치 못한 에러 → 로깅 후 일반 메시지 반환
    console.error('createPost failed:', error);
    return {
      success: false,
      error: { code: 'INTERNAL_ERROR', message: 'An unexpected error occurred' },
    };
  }
}
```

### 규칙 R5: TanStack Query로 서버 상태 관리 + 낙관적 업데이트

```typescript
// ✅ R5 준수: 낙관적 업데이트 패턴
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { postRepository } from '@/repositories/postRepository';

export function useCreatePost() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: (input: CreatePostInput) => postRepository.create(input),

    // 낙관적 업데이트: 서버 응답 전에 UI 즉시 반영
    onMutate: async (newPost) => {
      await queryClient.cancelQueries({ queryKey: ['posts'] });
      const previousPosts = queryClient.getQueryData<Post[]>(['posts']);

      queryClient.setQueryData<Post[]>(['posts'], (old = []) => [
        { ...newPost, id: 'temp-id', created_at: new Date().toISOString() } as Post,
        ...old,
      ]);

      return { previousPosts };
    },

    // 에러 시 롤백
    onError: (_err, _newPost, context) => {
      queryClient.setQueryData(['posts'], context?.previousPosts);
    },

    // 성공/실패 무관하게 최종 동기화
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ['posts'] });
    },
  });
}
```

### 규칙 R6: 공유 코드의 Monorepo 패키지 분리

```
모든 플랫폼(Expo, Next.js)에서 공통으로 사용하는 코드는
packages/shared에 분리한다.
```

**공유 대상 목록**:

|분류|예시|패키지 위치|
|---|---|---|
|**타입 정의**|DB 스키마 타입, API 타입, 도메인 모델|`packages/shared/types/`|
|**검증 스키마**|Zod 스키마 (생성/수정/쿼리)|`packages/shared/validators/`|
|**유틸리티**|날짜 포맷, 금액 계산, 문자열 처리|`packages/shared/utils/`|
|**에러 정의**|AppError, ErrorResponse 타입|`packages/shared/errors/`|
|**상수**|상태 enum, 설정 값|`packages/shared/constants/`|

**공유하지 않는 것** (플랫폼별 구현):

|분류|이유|
|---|---|
|**네비게이션**|Expo Router vs Next.js App Router|
|**스토리지**|AsyncStorage vs localStorage|
|**푸시 알림**|expo-notifications vs Web Push API|
|**카메라/미디어**|expo-camera vs MediaDevices API|
|**인증 UI**|네이티브 vs 웹 OAuth 플로우|

```typescript
// packages/shared/package.json
{
  "name": "@shared/core",
  "version": "0.1.0",
  "main": "./src/index.ts",
  "types": "./src/index.ts",
  "dependencies": {
    "zod": "^3.23.0"
  },
  "devDependencies": {
    "typescript": "^5.5.0"
  }
}

// 각 앱에서 사용
// apps/mobile/package.json 또는 apps/web/package.json
{
  "dependencies": {
    "@shared/core": "workspace:*"
  }
}
```

---

## §8. 참조 아키텍처 및 예제

### 8.1 기능별 완성 예제: "게시글 CRUD"

이 예제는 하나의 기능이 모든 레이어를 어떻게 관통하는지 보여준다.

```
┌─────────────────────────────────────────────────────────┐
│ 기능: 게시글 CRUD                                        │
│                                                         │
│ [Layer 3: DB]                                           │
│   Tables: posts (RLS ✅)                                │
│   Views: posts_with_author (security_invoker ✅)         │
│   RPC: create_post (태그 연결 원자 처리)                  │
│   Trigger: updated_at 자동 갱신 ✅                       │
│                                                         │
│ [Layer 2: Server/Shared]                                │
│   validators/post.ts → Zod 스키마                       │
│   types/database.ts → 타입 정의                          │
│   Server Action: createPost(), updatePost()             │
│                                                         │
│ [Layer 1: Client]                                       │
│   Expo:                                                 │
│     repositories/postRepository.ts                      │
│     hooks/queries/usePosts.ts                           │
│     hooks/mutations/useCreatePost.ts                    │
│     components/PostList.tsx, PostForm.tsx                │
│   Next.js:                                              │
│     Server Component: PostsPage (읽기)                   │
│     Client Component: PostForm (쓰기, Server Action 호출)│
└─────────────────────────────────────────────────────────┘
```

### 8.2 데이터 흐름 시퀀스: "게시글 생성"

```
사용자 → [PostForm 컴포넌트]
           │
           ├─ (Expo) useCreatePost hook → postRepository.create()
           │                                    │
           │                                    ├─ Zod 검증 (클라이언트 사전 검증)
           │                                    └─ supabase.rpc('create_post', {...})
           │
           └─ (Next.js) Server Action: createPost()
                              │
                              ├─ Zod 검증 (서버 검증)
                              ├─ auth.getUser() (인증 확인)
                              └─ supabase.rpc('create_post', {...})
                                        │
                                        ▼
                              [RPC: create_post]
                                 │
                                 ├─ INSERT INTO posts
                                 ├─ INSERT INTO post_tags (다대다 관계)
                                 ├─ 제약조건 검증 (UNIQUE, FK, CHECK)
                                 └─ RETURN post_id
                                        │
                                        ▼
                              [Trigger: updated_at 갱신]
                              [Trigger: audit_log 기록]
                                        │
                                        ▼
                              [RLS 정책: 소유자 확인 통과]
                                        │
                                        ▼
                              응답 반환 → 클라이언트
                                        │
                                        ├─ (Expo) TanStack Query 캐시 무효화
                                        └─ (Next.js) revalidatePath('/posts')
```

### 8.3 새 기능 추가 시 수정 범위 예시

#### 시나리오 A: "게시글에 좋아요 기능 추가"

|레이어|수정 내용|파일|
|---|---|---|
|**DB**|`likes` 테이블 + RLS + 카운터 트리거|`migrations/`|
|**Shared**|`Like` 타입, `toggleLikeSchema`|`packages/shared/`|
|**Server**|`toggleLike` Server Action|`apps/web/actions/`|
|**Expo**|`likeRepository`, `useToggleLike` hook|`apps/mobile/`|
|**Next.js**|`LikeButton` Client Component|`apps/web/components/`|

#### 시나리오 B: "결제 시스템 연동"

|레이어|수정 내용|파일|
|---|---|---|
|**DB**|`orders`, `payments` 테이블 + RLS + RPC|`migrations/`|
|**Shared**|주문/결제 타입, 검증 스키마, 금액 계산 util|`packages/shared/`|
|**Server**|Edge Function (결제 API 연동, 시크릿 필요)|`supabase/functions/`|
|**Server**|`createOrder`, `processPayment` Server Action|`apps/web/actions/`|
|**Expo**|`orderRepository`, `useCreateOrder` hook|`apps/mobile/`|
|**Next.js**|결제 페이지 (Server Component + Client Component)|`apps/web/`|

### 8.4 인증/인가 표준 흐름

```
┌─────────────────────────────────────────────────────┐
│                  인증(Authentication) 흐름             │
│                                                     │
│  1. 로그인 요청 → Supabase Auth                      │
│  2. JWT 토큰 발급 → 클라이언트 저장                    │
│  3. 모든 요청에 JWT 포함 → Supabase 자동 처리          │
│                                                     │
│                  인가(Authorization) 흐름              │
│                                                     │
│  Layer 1 (클라이언트):                                │
│    • UI 수준 접근 제어 (버튼 숨기기 등) — 보안이 아닌 UX │
│                                                     │
│  Layer 2 (서버):                                     │
│    • 역할 기반 접근 제어 (RBAC)                        │
│    • 리소스 소유권 검증                                │
│    • 비즈니스 규칙 기반 권한 확인                       │
│                                                     │
│  Layer 3 (데이터):                                    │
│    • RLS 정책 (최후 방어선)                             │
│    • 행 단위 접근 제어                                 │
│    • 서버 레이어가 뚫려도 데이터는 보호                  │
│                                                     │
│  ⚠️ 핵심: 인가는 3중으로 적용 (UI + 서버 + DB)          │
│     하나의 레이어에만 의존하지 않는다                    │
└─────────────────────────────────────────────────────┘
```

### 8.5 환경 변수 관리 표준

```bash
# .env.local (각 앱에 존재, 절대 커밋하지 않음)

# ✅ 클라이언트에서 사용 가능 (NEXT_PUBLIC_ 접두사)
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGc...

# ❌ 서버에서만 사용 (접두사 없음)
SUPABASE_SERVICE_ROLE_KEY=eyJhbGc...
SUPABASE_DB_URL=postgresql://...
STRIPE_SECRET_KEY=sk_live_...
RESEND_API_KEY=re_...

# Expo의 경우
# ✅ 클라이언트: EXPO_PUBLIC_ 접두사
EXPO_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=eyJhbGc...
```

### 8.6 테스트 전략 표준

|레이어|테스트 유형|도구|대상|
|---|---|---|---|
|**Shared**|단위 테스트|Vitest|validators, utils, 계산 로직|
|**Server**|통합 테스트|Vitest + Supabase Local|Server Actions, RPC|
|**Client**|컴포넌트 테스트|Testing Library|주요 UI 컴포넌트|
|**E2E**|E2E 테스트|Playwright (웹), Detox (모바일)|핵심 사용자 시나리오|

```typescript
// ✅ 테스트 예시: Shared 유틸리티 테스트
// packages/shared/src/utils/__tests__/shipping.test.ts
import { describe, it, expect } from 'vitest';
import { calculateShippingCost } from '../shipping';

describe('calculateShippingCost', () => {
  it('1kg 미만은 3000원', () => {
    expect(calculateShippingCost(0.5, [])).toBe(3000);
  });

  it('프로모션 적용 시 할인', () => {
    const promos = [{ type: 'shipping_discount', discountRate: 0.2 }];
    expect(calculateShippingCost(0.5, promos)).toBe(2400);
  });
});
```

---

## 부록 A: AI 코드 생성 체크리스트

> AI 도구가 코드를 생성한 후, 아래 체크리스트로 자가 검증한다.

### 생성 전 확인

- [ ] §5의 6단계 체크리스트를 통과하여 올바른 패턴을 선택했는가?
- [ ] 해당 기능이 어떤 레이어에서 처리되어야 하는지 명확한가?

### 생성 후 검증

**금지 규칙 (§6)**:

- [ ] P1: 비즈니스 로직 검증 없이 클라이언트에서 직접 데이터 변조하지 않았는가?
- [ ] P2: 클라이언트 코드에 시크릿 키가 포함되지 않았는가?
- [ ] P3: 비즈니스 판단 로직이 SQL에만 존재하지 않는가?
- [ ] P4: 트리거에 비즈니스 로직을 넣지 않았는가?
- [ ] P5: 새 테이블에 RLS가 즉시 활성화되었는가?

**권장 규칙 (§7)**:

- [ ] R1: TypeScript strict mode에서 에러 없이 컴파일되는가?
- [ ] R2: API 입력에 Zod 검증이 적용되었는가?
- [ ] R3: (Expo) Repository 패턴으로 Supabase 접근이 캡슐화되었는가?
- [ ] R4: 에러 처리가 표준 패턴(AppError, ApiResponse)을 따르는가?
- [ ] R5: TanStack Query로 서버 상태가 관리되고 있는가?
- [ ] R6: 공유 가능한 코드가 packages/shared에 분리되었는가?

**아키텍처 일관성**:

- [ ] Layer 경계를 넘는 직접 접근이 없는가?
- [ ] 의존성 방향이 단방향인가? (Component → Hook → Repository → Client)
- [ ] View에 security_invoker가 설정되었는가?
- [ ] RPC에 적절한 SECURITY 설정이 적용되었는가?

---

## 부록 B: 용어 사전

|용어|정의|
|---|---|
|**RLS**|Row Level Security. PostgreSQL의 행 단위 접근 제어 기능|
|**RPC**|Remote Procedure Call. Supabase에서 DB 함수를 호출하는 방식|
|**View**|SQL View. 가상 테이블로, 복잡한 쿼리를 캡슐화|
|**Trigger**|DB 이벤트(INSERT/UPDATE/DELETE) 시 자동 실행되는 함수|
|**Server Action**|Next.js의 서버 사이드 함수. 클라이언트에서 직접 호출 가능|
|**Route Handler**|Next.js의 API 엔드포인트 (GET, POST, PUT, DELETE)|
|**Edge Function**|Supabase의 서버리스 함수 (Deno 런타임)|
|**Repository**|데이터 접근 로직을 캡슐화하는 패턴|
|**security_invoker**|View를 호출자의 권한으로 실행 (RLS 적용)|
|**SECURITY DEFINER**|DB 함수를 함수 소유자의 권한으로 실행 (RLS 우회)|
|**Anon Key**|Supabase 공개 키. 클라이언트에서 사용 가능. RLS로 보호됨|
|**Service Role Key**|Supabase 서버 전용 키. RLS를 우회함. 절대 클라이언트 노출 금지|
|**낙관적 업데이트**|서버 응답 전에 UI를 먼저 업데이트하고, 실패 시 롤백하는 패턴|

---

## 부록 C: 문서 간 참조 관계

```
이 아키텍처 가이드는 프로젝트의 모든 문서의 기준이 된다:

  ┌──────────────────────────────────────────┐
  │     AI 코드 생성 아키텍처 가이드 (이 문서)   │
  │           ▲ 기준 문서 (Normative)          │
  └──────────┬───────────┬───────────┬───────┘
             │           │           │
     ┌───────▼──┐  ┌─────▼────┐  ┌──▼──────────┐
     │   PRD    │  │Architecture│  │   UI/UX     │
     │ (요구사항) │  │  (설계)    │  │ (디자인 명세) │
     └───────┬──┘  └─────┬────┘  └──┬──────────┘
             │           │          │
             └─────┬─────┘          │
                   │                │
              ┌────▼────────────────▼──┐
              │       Stories          │
              │    (구현 명세)          │
              └────────────────────────┘
                         │
                         ▼
              ┌────────────────────────┐
              │   Claude Code 코드 생성  │
              └────────────────────────┘

  규칙:
  - 하위 문서가 이 가이드와 충돌하면, 이 가이드가 우선
  - Stories는 이 가이드의 패턴 선택 기준(§5)을 반드시 따라야 함
  - 모든 코드 생성은 금지 규칙(§6)을 위반해서는 안 됨
```

---

> **이 문서의 최종 목표**: Claude Code가 이 가이드를 읽고 생성하는 코드가 **항상 일관되고, 안전하며, 유지보수 가능한** 아키텍처를 따르도록 하는 것.