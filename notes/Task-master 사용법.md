# Task-master 사용법

## Task Master 초기화

에디터의 AI 채팅창에 다음과 같이 입력하여 프로젝트를 초기화하세요:

> **English:** Initialize taskmaster-ai in my project
> 
> **Korean:** 내 프로젝트에서 taskmaster-ai를 초기화해 줘.

## PRD로부터 태스크 생성

Cursor의 AI 채팅에서 에이전트에게 PRD(제품 요구사항 문서)를 읽고 태스크를 생성하도록 지시하세요:

> **English:** Please use the task-master parse-prd command to generate tasks from my PRD. The PRD is located at .taskmaster/docs/prd-name.md
> 
> **Korean:** task-master의 parse-prd 명령어를 사용해서 내 PRD를 기반으로 태스크를 생성해 줘. PRD 위치는 .taskmaster/docs/prd-name.md야.

## 태스크 확장 (Expand Tasks)

태스크에 세부 사항을 추가하고 하위 태스크(subtasks)를 생성하는 데 사용됩니다. 아래 요청을 사용하여 모든 태스크를 확장하는 것을 권장합니다:

> **English:** Expand all tasks into subtasks.
> 
> **Korean:** 모든 태스크를 하위 태스크(subtasks)로 확장해 줘.

## 태스크 업데이트 (Update Tasks)

태스크 세부 정보를 수정해야 하는 경우, 다음 예시처럼 요청하여 업데이트할 수 있습니다:

> **English:** Update Task 2 to use Postgres instead of MongoDB and remove the sharding subtask
> 
> **Korean:** Task 2를 수정해서 MongoDB 대신 Postgres를 사용하게 하고, 샤딩(sharding) 하위 태스크는 삭제해 줘.

## 복잡도 분석

Task Master는 작업을 시작하기 전 유용한 복잡도 보고서(complexity report)를 제공합니다. 어떤 작업을 더 세분화해야 할지 식별하는 데 도움이 됩니다.

> **English:** Can you analyze the complexity of our tasks to help me understand which ones need to be broken down further?
> 
> **Korean:** 어떤 작업을 더 세분화해야 할지 파악할 수 있도록 태스크 복잡도를 분석해 줄래?

다음 명령어를 사용하여 친숙한 테이블 형식으로 보고서를 볼 수 있습니다:

> **English:** Can you show me the complexity report in a more readable format?
> 
> **Korean:** 복잡도 보고서를 좀 더 읽기 편한 형식으로 보여줄 수 있어?

---

## 태스크 실행하기

**작업할 태스크 선택: Next Task**  
Task Master의 "next" 명령어를 통해 다음에 작업할 태스크를 찾습니다:

> **English:** What's the next task I should work on? Please consider dependencies and priorities.
> 
> **Korean:** 다음에 진행해야 할 태스크는 뭐야? 의존성과 우선순위를 고려해서 알려줘.

**Task 토론**  
작업할 Task가 결정되면, 에이전트와 대화하여 실행 계획을 제대로 이해했는지 확인합니다. 필요시 관련 파일(@models, @api 등)을 태그하세요.

> **English:** Please review Task [Number] and confirm you understand how to execute before beginning.
> 
> **Korean:** Task [번호]를 검토하고, 시작하기 전에 실행 방법을 이해했는지 확인해 줘.

**Agent Task 실행**  
실행 계획에 동의하신다면, Agent에게 작업을 시작하도록 지시하십시오.

> **English:** You may begin. I believe in you.
> 
> **Korean:** 시작해도 좋아. 믿고 맡길게.

**검토 및 테스트**  
Agent가 작업을 완료하면, 작업 테스트 전략을 참조하여 작업이 올바르게 수행되었는지 확인하세요.

**Task 상태 업데이트**  
작업이 올바르게 완료되었다면 상태를 done으로 업데이트할 수 있습니다.

> **English:** Please mark Task 5 as done
> 
> **Korean:** Task 5를 완료(done) 상태로 표시해 줘.

---
