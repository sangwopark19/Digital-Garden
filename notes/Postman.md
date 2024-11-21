## 팁
---
- localhost:8080 과 같이 반복되는 url들을 변수화할 수 있다 
마우스 오른쪽 클릭으로 변수를 만든후 {{url_name}}으로 사용한다

- 더 좋은 방법은 script의 Pre-request 옵션이다
다음과 같이 컬렉션의 script에서 작성을 하면 서버에 요청을 보내기전에 자동으로 js 코드를 실행하여 주소를 자동으로 붙여준다
pm.request.url.protocol=http
pm.request.url.host=localhost
pm.request.url.port=8080
