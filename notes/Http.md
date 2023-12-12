
## Response Status
---
### 404 not found
입력한 리소스가 존재하지 않을 떄

### 500 Server exception
서버에서 예외가 발생

### 400 Validation error,  Bad Request (유효성 검사 에러)
사용자 입력값 검증 에러 (사용자가 잘못된 입력을 했을 떄)

### 401 unauthorized (인증 실패 시)
사용자가 요청에 올바른 정보를 제공하지 않을 때 (인증 관련)


### 200 Success
성공

### 201 Created
새 리소스 생성에 성공 했을 때

### 204No Content
리소스를 업데이트하기위해 PUT 요청을 보냈을 때 콘텐츠가 없음을 응답 상태로 보내야할 때