everything-claude-code 플러그인이 이미 성공적으로 설치되어 있습니다! 이 플러그인은 다양한 개발 워크플로우를 지원하는 스킬들을
  제공합니다.

  주요 스킬들과 사용법

  1. 계획 및 설계

  /plan
  - 요구사항을 재확인하고, 위험을 평가하며, 단계별 구현 계획을 생성합니다
  - 코드를 수정하기 전에 사용자 확인을 기다립니다

  2. 테스트 주도 개발 (TDD)

  /tdd
  - 테스트 우선 작성 워크플로우를 강제합니다
  - 80% 이상의 테스트 커버리지를 보장합니다

  Go 전용:
  /go-test
  - Go의 table-driven 테스트 패턴을 사용합니다

  Spring Boot 전용:
  /springboot-tdd
  - JUnit 5, Mockito, Testcontainers를 사용합니다

  3. 코드 리뷰 및 빌드

  /go-review
  - Go 코드의 관용적 패턴, 동시성 안전성, 에러 처리를 검토합니다

  /go-build
  - Go 빌드 오류와 린터 경고를 점진적으로 수정합니다

  4. E2E 테스트

  /e2e
  - Playwright를 사용한 end-to-end 테스트를 생성하고 실행합니다

  5. 보안 검토

  /security-review
  - 인증, 사용자 입력 처리, API 엔드포인트의 보안을 검토합니다

  6. 패턴 가이드

  이 스킬들은 자동으로 적용되며, 특정 기술 스택 작업 시 best practice를 제공합니다:
  - golang-patterns - Go 관용구
  - springboot-patterns - Spring Boot 아키텍처
  - postgres-patterns - PostgreSQL 최적화
  - frontend-patterns - React/Next.js 패턴
  - backend-patterns - Node.js/Express 패턴

  7. 학습 및 진화

  /continuous-learning-v2
  - 세션에서 패턴을 자동 추출하여 학습합니다

  /instinct-status
  - 학습된 instinct들을 확인합니다

  /skill-create
  - 로컬 git 히스토리를 분석하여 SKILL.md 파일을 생성합니다

  사용 예시

  예를 들어, 새로운 기능을 추가할 때:

  1. 계획 수립: 먼저 /plan을 사용하여 구현 전략을 세웁니다
  2. TDD 실행: /tdd를 사용하여 테스트 우선 개발을 진행합니다
  3. 보안 검토: /security-review로 보안 취약점을 확인합니다
  4. E2E 테스트: /e2e로 end-to-end 테스트를 작성합니다

  또는 단순히 "TDD로 새로운 사용자 인증 기능 추가해줘"라고 요청하면, 제가 자동으로 적절한 스킬을 활용하여 작업을 진행합니다!

  어떤 기능을 개발하고 싶으신가요?