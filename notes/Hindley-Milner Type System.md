# Hindley-Milner Type System
---
- [힌들리-밀너 타입 추론 Hindley-Milner Type Inference | Jang's Blog](https://www.palindrom615.dev/hindley-milner-type-inference)
	- **힌들러-밀너 타입 시스템 Hindley-Milner type system**은 이름에서 알 수 있듯이 [논리학자 힌들러가 제시하고](https://www.semanticscholar.org/paper/THE-PRINCIPAL-TYPE-SCHEME-OF-AN-OBJECT-IN-LOGIC/fc64117e5d5ed5947a0c85c55597e4116d6e55c6)[컴퓨터 과학자 밀너가 재발견한](https://www.sciencedirect.com/science/article/pii/0022000078900144)타입 이론으로, 70년대에 이미 학문적으로 완성된 내용이다.
	- [[C++]]의 템플릿은 컴파일할 때는 그냥 사용되는 타입에 따라 함수 여러 개로 컴파일되고(monomorphication) 그 전에는 타입 안정성 (type soundness/type safety)을 체크하지는 않는다.
	- [[java]] 제네릭은 그냥 타입 캐스팅을 대신해주는 [[신택스 슈거 (Syntax Sugar)]]일 뿐이다.