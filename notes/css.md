## inline / block 깨달음
---
가운데 정렬을 하기위해 text-aline: center를 할때 가 있다
이 때 inline요소는 text로 취급되어 가운데 정렬이 되지만 block 요소는 적용되지 않는다.
이는 특히 원래 인라인 요소 였던 button같은 요소를 다음 줄로 넘기기 위해 block으로 바꿨을 때 많이 일어난다. 이 때 해결 방법은 무엇일까?
바로 `margin: (원하는 상하 마진 값) auto`를 적용하면 된다 또는 상하 마진값을 서로 다르게 주고 싶다면 `margin: (top) auto (bottom)`을 적용하면 된다.

이 해결방법의 원리는 무엇일까? 사실 간단하다.
inline과 block의 속성을 다시 생각해보면 되는데 inline은 width, hegiht 값이 적용되지 않고 margin, padding 값이 존중되지 않는다. 즉 block 요소의 값들이 전부 제대로 적용이 안되는 대신 글자 취급을 받아 text-aline의 적용을 받는 것이다.

반대로 block 요소 같은 경우엔 width, hegiht가 적용되고 maring, padding 을 존중받고 또한 block 요소에 특별한 값을 주지않으면 기본적으로 그 줄의 모든 가로 값을 자신이 먹기 때문에 text-aline의 위치값이 적용되지 않는것이다.