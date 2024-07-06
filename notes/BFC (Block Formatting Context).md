# 블록 포맷컨텍스트
---
CSS 레이아웃에서 중요한 개념으로, 요소들의 배치와 그들 간의 상호작용을 제어하는 독립된 컨테이너를 의미한다. BFC는 레이아웃 계산에서 **특정 규칙**을 따르는 요소의 영역을 형성하며, 이 규칙은 요소들이 서로 영향을 주지 않고 **독립적**으로 배치되도록 한다.

## BFC의 주요 특징과 생성 조건
---

1. BFC의 특징:
	- BFC 내의 박스는 BFC 컨테이너의 경계를 침범하지 않는다.
	- BFC는 내부 요소의 float를 처리한다. 즉, BFC 내부의 float 요소는 BFC 컨테이너에 의해 감싸여서 배치된다.
	- BFC는 외부의 요소들과 상호작용하지 않는다. 예를 들어, BFC 내의 요소는 외부 요소의 float 영향을 받지 않는다.
	- BFC는 margin collapsing을 방지한다. 즉, 두 개의 인접한 블록 요소의 상단과 하단 margin이 겹쳐지지 않는다.

1. BFC 생서 조건:
	- `float` 속성이 `none`이 아닌 경우.
	- `overflow` 속성이 `visible(기본값), clip`이 아닌 경우 ( `hidden, auto, scroll`)
	- `display` 속성이 `inline-block, table-cell, table-caption, flex, grid`인 경우.
	- `position`속성이 `absolute, fixed`인 경우


## 주의할 점
---
- `overflow`에 `visible, clip`이외의 값을 지정했을때 float요소가 내부에 있을 경우 각 스크롤 단계 후에 콘텐츠를 강제로 다시 래핑하여 스크롤 속도가 **느려진다** 즉, 성능 저하 이슈가 있다.