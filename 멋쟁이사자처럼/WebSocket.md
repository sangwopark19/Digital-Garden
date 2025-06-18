
WebSocket도 연결을 하기 위해서는 최초에 한번은 http 요청을 보낸다.

websocket은 실시간 양방향 통신

websocket을 사용한 플랫폼 배민, 토스의 주식거래등이 사용함

spring에서 websocket을 사용하는 대표적인 두가지
1. spring websocket
	1. 연결만 제공하고 핸들러를 직접 구현해야 함
2. spring websocket + stomp(simple text oriented messiging protocol) -> 텍스트 기반의 메시지 전송 프로토콜
	1. 채팅방(채널) 구분, 구독/발행, 메시지 라우팅을 stomp에서 자동 처리
	2. websocket 위에서 동작하는 메시징 프로토콜
	3. 메시지를 어떻게 주고받을지의 규칙이 있음

순수 websocket 메시지
그냥 문자열만 주고받음 서버/클라이언트가 어떤의미인지, 누구한테 보내는지 따로 해석해야함 (정해진 규약 없음)


sstomp 메시지 구조 
명확한 프레임 구조를 가짐
명령 헤더 본문으로 구성

command 메시지의 종류
header 키 값 쌍
body 실제 매시지 내용
