기본 모드는 MODE_THREADLOCAL 이다.
스레드 로컬이란 클라이언트에서 서버에 5개의 요청이 들어왔다고 해보자
이 요청들을 각각 t1 ~ t5라 이름붙이고 각 요청당 한개의 스레드로 작업을한다
이때 매번 security content에서 사용자 정보를 불러오는게 아닌 스레드로컬에 저장되어서 쉽게 가져올 수 있다.

MODE_INHERITABLETHREADLOCAL 모드는
한 요청의 스레드에서 개발자가 @Async 어노테이션을 사용해 비동기 메서드를 실행하는 경우가 있다.  t1 스레드에서 비동기 메서드가 실행되면 그 비동기 메서드는 t2 스레드로 실행되기 때문에  THREADLOCAL에 있는 사용자 정보를 얻지 못한다 이때 비동기 스레드에서도 공유할 수 있게 만드는 모드가 MODE_INHERITABLETHREADLOCAL모드다.