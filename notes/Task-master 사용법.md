# Task-master 사용법
## Task Master 초기화
에디터의 AI 채팅창에 다음과 같이 입력하세요:  
Initialize taskmaster-ai in my project
## Cursor의 AI 채팅에서 에이전트에게 PRD로부터 태스크를 생성하도록 지시하세요:
Please use the task-master parse-prd command to generate tasks from my PRD. The PRD is located at .taskmaster/docs/prd-name.md
## Expand Tasks  
태스크에 세부 사항을 추가하고 하위 태스크(subtasks)를 생성하는 데 사용됩니다. 아래의 MCP 요청을 사용하여 모든 태스크를 확장하는 것을 권장합니다:  
Expand all tasks into subtasks.
## Update Tasks
태스크 세부 정보를 수정해야 하는 경우, 다음 요청을 사용하여 태스크를 업데이트할 수 있습니다:
Update Task 2 to use Postgres instead of MongoDB and remove the sharding subtask
## 복잡도 분석
Task Master는 작업을 시작하기 전에 참고하면 유용한 복잡도 보고서(complexity report)를 제공할 수 있습니다. 아직 모든 작업을 확장하지 않았다면, 어떤 작업을 하위 작업(subtasks)으로 더 세분화할 수 있는지 식별하는 데 도움이 됩니다.
Can you analyze the complexity of our tasks to help me understand which ones need to be broken down further?
## 다음 명령어를 사용하여 친숙한 테이블 형식으로 보고서를 볼 수 있습니다:
Can you show me the complexity report in a more readable format?

--------------------------
## 태스트 실행하기
**작업할 태스크 선택: Next Task**
Task Master에는 다음에 작업할 태스크를 찾는 "next" 명령어가 있습니다. 다음 요청을 통해 액세스할 수 있습니다:
What's the next task I should work on? Please consider dependencies and priorities.

**Task 토론**
다음에 작업할 Task가 결정되면, 에이전트와 대화를 시작하여 에이전트가 실행 계획을 제대로 이해하고 있는지 확인할 수 있습니다.

에이전트가 계획을 생성할 때 참고해야 할 컨텍스트를 알 수 있도록 관련 파일과 폴더를 태그할 수 있습니다. 예시는 다음과 같습니다:
Please review Task  and confirm you understand how to execute before beginning. 
(여기서부터 안써도됨. 참고할거 있을때만 사용하면 좋음)Refer to @models @api and @schema
## Agent Task 실행
실행 계획에 동의하신다면, Agent에게 작업을 시작하도록 지시하십시오.
You may begin. I believe in you.

**검토 및 테스트**
Agent가 작업을 완료하면, 작업 테스트 전략(task testing strategy)을 참조하여 작업이 올바르게 수행되었는지 확인할 수 있습니다.

[**​**](https://docs.task-master.dev/getting-started/quick-start/execute-quick#update-task-status)**Task 상태 업데이트**
작업이 올바르게 완료되었다면 상태를 done으로 업데이트할 수 있습니다.
Please mark Task 5 as done