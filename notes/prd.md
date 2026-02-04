아래는 Cursor / Claude Code에서 그대로 `prd.md`로 써먹을 수 있는, “자동 개발 루프” 친화 한글 PRD 템플릿입니다.[github+3](https://github.com/nurettincoban/cursor-ai-prd-workflow)

---

# 제품개발계획서(PRD)

## 0. 메타 정보

- 프로젝트 이름:
    
- 버전: `v0.1.0`
    
- 작성일:
    
- 작성자:
    
- 관련 저장소 / 서비스 URL:
    
- 연동되는 자동화 루프: (예: Ralph, 자체 스크립트 등)[thetoolnerd+1](https://www.thetoolnerd.com/p/autonomous-ai-agent-loop-for-building)
    

---

## 1. 문제 정의 & 컨텍스트

## 1.1 배경

- 현재 비즈니스/운영 상황:
    
- 해결하려는 핵심 문제:
    
- 왜 지금 이 프로젝트가 필요한가:
    

## 1.2 타겟 유저 & 사용 시나리오

- 주요 유저 유형(페르소나):
    
- 대표 사용 시나리오(스토리 형태로 2–3개):
    

## 1.3 스코프 & 비스코프

- 이번 버전에서 **포함하는 것**:
    
- 이번 버전에서 **명시적으로 제외하는 것**:
    

---

## 2. 목표 & 성공 지표

## 2.1 비즈니스 / 제품 목표

- 목표 1:
    
- 목표 2:
    

## 2.2 성공 지표(KPI)

- 핵심 지표: (예: 하루 활성 사용자, 처리량, 오류율 등)
    
- 목표 수치 / 기준:
    

## 2.3 실패 조건 / 안티 목표

- 이 프로젝트가 실패했다고 판단하는 조건:
    
- 절대 만들지 말아야 할 형태(안티 패턴):
    

---

## 3. 기능 요구사항 (Features)

> 이 섹션은 나중에 `prd.json` / RFC / 태스크로 자동 분해되는 것을 전제로 합니다.[github+1](https://github.com/snarktank/ralph)

각 기능은 아래 포맷을 반복해서 작성합니다.

## F-001: (기능 이름)

- 설명:
    
- 유형: (핵심 / 보조 / 관리)
    
- 우선순위: (P0 / P1 / P2 …)
    
- 관련 목표: (2.x에 적은 목표 ID)
    
- 의존성: (다른 기능 ID 또는 외부 시스템)
    

**Acceptance Criteria**

-  AC1:
    
-  AC2:
    
-  AC3:
    

**UX / 플로우 메모 (선택)**

- 화면/페이지 이름:
    
- 주요 플로우 요약:
    

---

## 4. 비기능 요구사항(NFR)

## 4.1 성능

- 응답 시간 / 처리량 목표:
    
- 동시 사용자 / 작업 수 가정:
    

## 4.2 보안 / 인증 / 권한

- 인증 방식: (예: JWT, OAuth2 등)
    
- 권한 레벨 및 접근 제어 규칙:
    

## 4.3 안정성 / 가용성 / 장애 대응

- 장애 시 동작(Graceful degradation):
    
- 로그 / 모니터링 요구사항:
    

## 4.4 규제 / 컴플라이언스 / 데이터

- 개인정보 / 민감 데이터 취급 정책:
    
- 보존 기간, 삭제 정책:
    

---

## 5. 아키텍처 & 데이터 모델

## 5.1 시스템 개요

- 전체 시스템 요약: (한 단락 설명)
    
- 주요 컴포넌트: (예: 웹 프론트, API 서버, Worker, DB, 외부 API 등)
    

## 5.2 기술 스택

- 프론트엔드: (예: Next.js, Tailwind, etc.)
    
- 백엔드: (예: FastAPI, NestJS, etc.)
    
- 데이터베이스:
    
- 메시징/큐/캐시:
    

## 5.3 도메인 모델 요약

- 주요 엔티티
    
    - Entity 1: 필드, 간단 설명
        
    - Entity 2: …
        

## 5.4 외부 연동

- 연동 대상 서비스:
    
- 사용 목적:
    
- 주요 API 엔드포인트:
    

---

## 6. 리스크 & 엣지 케이스

## 6.1 리스크

- 기술적 리스크:
    
- 비즈니스/운영 리스크:
    

## 6.2 엣지 케이스 / 에러 시나리오

- 입력 값 엣지 케이스:
    
- 외부 시스템 장애 시 동작:
    
- 데이터 불일치 / 레이스 컨디션:
    

---

## 7. 구현 규칙 (AI Agents / 팀 공통 규칙)

> 이 섹션은 나중에 `rules.md`로 분리하거나, Cursor / Claude Code에 항상 같이 제공해서 에이전트의 행동을 제어하는 데 사용합니다.[github+2](https://github.com/nurettincoban/ai-prd-workflow)

## 7.1 코드 스타일 & 구조

- 언어 / 프레임워크 버전:
    
- 코드 스타일 규칙: (예: ESLint, Black, Prettier 등)
    
- 레이어 구조: (예: `app/`, `domain/`, `infra/`, `tests/` 등)
    

## 7.2 테스트 정책

- 테스트 최소 요구사항: (단위/통합/E2E)
    
- 필수 커버해야 하는 케이스:
    

## 7.3 작업 단위 & 브랜치 전략

- 브랜치 네이밍 규칙: (예: `feature/F-001-login`)
    
- 커밋 메시지 규칙: (예: Conventional Commits 등)
    

## 7.4 금지 사항 / 주의 사항

- 사용 금지 라이브러리 / 패턴:
    
- 보안상 절대 금지되는 행동:
    

---

## 8. 마일스톤 & 단계적 롤아웃

## 8.1 마일스톤

- M1: (예: 기본 CRUD 완성) – 포함 기능: F-001, F-002
    
- M2: (예: 권한/역할 추가) – 포함 기능: …
    

## 8.2 릴리즈 전략

- 베타 / 내부 사용 일정:
    
- 점진적 롤아웃 계획:
    

---

## 9. 부록

- 참고 문서 링크:
    
- 기존 시스템 스크린샷 / 예시:
    
- 용어 정리:
    

---

## Cursor / Claude Code에 넣을 때 추천 사용법 (짧게)

- PRD는 `prd.md`로 저장하고, 나중에 Ralph / 스크립트에서 `prd.json`으로 변환하거나 자동 추출하도록 둡니다.[thetoolnerd+1](https://www.thetoolnerd.com/p/autonomous-ai-agent-loop-for-building)
    
- 별도 파일로
    
    - `rules.md` (섹션 7 확장판)
        
    - `progress.md` (루프마다 학습/피드백 기록)[github+1](https://github.com/snarktank/ralph)
        
    - `rfc/feature-xxx.md` (각 기능의 세부 구현 스펙)[github+1](https://github.com/nurettincoban/cursor-ai-prd-workflow)  
        를 두고, 에이전트 호출 시 `prd.md + rules.md + 해당 RFC` 조합을 기본 컨텍스트로 주면 됩니다.[github+3](https://github.com/nurettincoban/ai-prd-workflow)
        

원하면, 위 템플릿에 맞춰서 “실제 샘플 PRD (예: Next.js 대시보드 앱)”를 채운 버전도 만들어 줄게요.

추가 질문하기

소스 확인