# Postgres 베스트 프랙티스를 AI 에이전트에 직접 가르치는 Agent Skills 공개

](https://supabase.com/blog/postgres-best-practices-for-ai-agents) (supabase.com)

26P by [neo](https://news.hada.io/user?id=neo) 7시간전 | ★ favorite | [댓글과 토론](https://news.hada.io/topic?id=26188)

- AI 코딩 에이전트가 **Postgres를 올바르게 다루지 못하는 문제**를 해결하기 위해, 데이터베이스 규칙을 명시적으로 제공하는 Agent Skills 공개
- Postgres는 **수십 년간 축적된 기능, 엣지 케이스, 성능 특성**이 있어 에이전트가 작동하지만 **전체 테이블 스캔을 유발하거나 보안 정책을 누락하는 코드**를 생성할 수 있음
- **총 8개 카테고리, 30개 규칙**으로 구성되며 쿼리 성능, 연결 관리, 보안 및 RLS, 스키마 설계 등 영향도 기준 우선순위별로 정리
- 사람이 읽는 문서가 아닌, **AI 에이전트가 직접 참조하는 규칙 집합** 형태로 제공
- MCP 서버와 결합 시, 실행 능력과 판단 기준을 동시에 갖춘 **실전형 데이터베이스 에이전트** 구성 가능

---

## 문제 인식: AI는 코드를 쓰지만 시스템을 이해하지 못함

- AI 코딩 에이전트는 문법적으로는 올바른 코드를 생성하지만, **Postgres의 내부 특성과 운영 리스크를 고려하지 못하는 한계** 존재
- 전체 테이블 스캔을 유발하는 쿼리, 쓰기 성능을 저하시킬 인덱스 제안, **Row Level Security 누락** 같은 문제가 반복적으로 발생
- 프로덕션 환경에서 중요한 **성능, 보안, 안정성 요소를 에이전트가 놓치는 사례** 다수 확인됨

## Postgres Agent Skills 개요

- AI 에이전트가 Postgres 코드를 작성할 때 참고하도록 만든 **규칙 기반 지식 저장소**
- 총 **30개 규칙**, 영향도 기준으로 정렬된 **8개 카테고리**로 구성
- 각 규칙은 제목, 우선순위, 중요성 설명, 올바른/잘못된 코드 예제를 포함

## 8개 규칙 카테고리 구성

- 각 규칙은 일관된 형식을 따르며, 제목, 영향도, 설명, 태그 포함
- **Query Performance (Critical)**: 전체 테이블 스캔 방지, 효율적인 쿼리 작성 규칙
- **Connection Management (Critical)**: 커넥션 풀링, 클라이언트 수명 관리, 리소스 제한
- **Security and RLS (Critical)**: Row Level Security 정책, 접근 제어 패턴
- **Schema Design (High)**: 테이블 구조, 데이터 타입, 정규화 판단
- **Concurrency and Locking (Medium-High)**: 트랜잭션 격리, 데드락 방지, 락 관리
- **Data Access Patterns (Medium)**: 페이지네이션, 벌크 작업, 접근 패턴 설계
- **Monitoring and Diagnostics (Low-Medium)**: 쿼리 분석, 성능 추적, 디버깅
- **Advanced Features (Low)**: CTE, 윈도우 함수, 확장 기능 등 Postgres 고유 기능

## Row Level Security 규칙의 예시

- **잘못된 방식**: 애플리케이션 레벨 필터링만 의존할 경우, **버그나 우회로 인해 전체 데이터 노출 위험** 존재
    - `select * from orders where user_id = $current_user_id;` 형태로 작성하면 바이패스 시 전체 주문 반환
- **올바른 방식**: 데이터베이스 수준에서 RLS 강제 적용
    - `alter table orders enable row level security;`로 RLS 활성화
    - `create policy`로 사용자가 자신의 데이터만 볼 수 있도록 정책 생성
    - `alter table orders force row level security;`로 테이블 소유자에게도 RLS 강제
- 인증된 역할을 위한 정책 예시: `create policy orders_user_policy on orders for all to authenticated using (user_id = auth.uid());`

## Agent Skills 포맷과 생태계

- Agent Skills는 에이전트에게 도메인 전문성을 제공하는 오픈 표준 형식의 **AI 에이전트 전용 문서 포맷**, 필요 시 에이전트가 직접 읽고 적용
    - Claude Code, Cursor, GitHub Copilot, VS Code, Gemini CLI 등과 호환
    - 에이전트가 필요할 때 발견하고 사용할 수 있는 지침과 예제가 담긴 폴더 형태
    - 훈련 데이터에서 올바른 패턴을 학습했기를 기대하는 대신 **명시적 규칙**을 제공
- Anthropic이 정의한 오픈 표준으로, 업계 전반에서 채택 중
    - Vercel은 10년간의 React 및 Next.js 최적화 지식을 40개 규칙으로 패키징한 **react-best-practices** 공개
    - Cloudflare는 Workers, Pages, D1, R2 등 **40개 이상 서비스**에 대한 Skills 공개

## Supabase가 이 규칙을 만든 이유

- Supabase는 수십만 개 프로젝트에서 Postgres를 운영하며 동일한 실수가 반복되는 것을 목격
    - Foreign 키에 인덱스 누락
    - 실수로 RLS를 우회하는 쿼리
    - 프로덕션에서 테이블을 잠그는 마이그레이션
    - 잘못 관리된 클라이언트로 인한 **연결 풀 고갈**
    - ORM 뒤에 숨겨진 전체 테이블 스캔
- 지원팀, 데이터베이스 어드바이저, 문서에 이미 존재하는 지식을 **에이전트 친화적 형태로 재구성**하여 패키징

## MCP 서버와의 관계

- **Supabase MCP 서버**는 AI 에이전트가 Supabase 프로젝트에 직접 연결하여 테이블 생성, 쿼리 실행, 스키마 관리, 설정 구성을 자연어로 수행 가능하게 함
- MCP 서버는 에이전트에게 데이터베이스 작업 **능력**을 부여하고, 모범 사례는 **올바르게** 수행하도록 교육
- 비유: MCP 서버는 **핸들**, 모범 사례는 **운전 교습**
- MCP 접근 권한만 있는 에이전트는 요청받은 모든 쿼리 실행 가능
- MCP 접근 권한과 규칙을 모두 갖춘 에이전트는 테이블을 잠그는 인덱스 생성 전 경고, 안전하지 않은 코드 배포 전 RLS 정책 제안, 성능 문제를 피하는 쿼리 구조화 수행
- MCP 서버는 연결과 실행 담당, 이 모범 사례는 **판단**을 담당
- Agent Skills는 실행 이전에 **위험 요소를 경고하고 더 나은 선택을 유도**
- 실행 능력과 판단 기준을 분리해, **안전하고 신뢰 가능한 자동화 환경** 구성

## 설치 방법

- 저장소 위치: github.com/supabase/agent-skills
- **Vercel의 skills npm 패키지**를 사용하여 대화형으로 설치 가능
    - `npx skills add supabase/agent-skills`
- **Claude Code** 사용 시 플러그인으로 설치 가능
    - `/plugin marketplace add supabase/agent-skills`
    - `/plugin install postgres-best-practices@supabase-agent-skills`