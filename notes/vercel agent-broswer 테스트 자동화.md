[

#   
Vercel agent-browser, --native 기능 도입

](https://agent-browser.dev/) (agent-browser.dev)

13P by [xguru](https://news.hada.io/user?id=xguru) 17시간전 | ★ favorite | [댓글 2개](https://news.hada.io/topic?id=27218)

- AI 에이전트를 위한 **헤드리스 브라우저 자동화 CLI**
- 기존 node.js + playwright + CDP 구조에서 Rust 바이너리가 CDP를 직접 호출하게 변경
- 독립실행형 데몬으로 런타임에 Node.js 프로세스가 없어서 메모리 사용량 감소 및 풋프린트도 작아짐
- **AI 친화적 워크플로우 명령들**
    - `snapshot` 명령으로 접근성 트리를 가져와서 **고유 ref(@e1, @e2)** 를 생성하고 이 기반으로 동작 (전통적인 셀렉터도 지원)
    - 페이지 탐색(`open`, `goto`), 클릭·입력(`click`, `fill`, `type`, `hover`, `check`, ..), 스크린샷·PDF 생성(`screenshot`, `pdf`)
    - 상태 조회(`get text`, `get attr`), 상태 체크(`is`), 대기(`wait`), 마우스·키보드 제어(`mouse`, `keyboard`)
    - 엘리먼트 검색(`find`), 브라우저 세팅(`set`), 탭/윈도우/프레임/다이얼로그(`tab`, `window`, `frame`, `dialog`)
    - 비교 (`diff`) : 스냅샷/스크린샷/URL 등으로 비교
    - 세션·스토리지·쿠키 관리(`cookie`, `storage`), 네트워크 요청 가로채기 및 모킹(`network route`) 지원
- `--session`으로 **격리된 브라우저 인스턴스** 실행
- `--profile` 또는 `--session-name`으로 **로그인·스토리지 상태 유지**
- `--annotate` 옵션으로 **요소 번호가 표시된 주석 스크린샷** 생성 지원
- macOS, Linux, Windows 전용 Rust 바이너리 제공, Node.js 폴백 지원
- Apache-2.0 라이선스