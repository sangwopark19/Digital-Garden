## 정의
---
람다 표현식은 "소유자"가 없는 익명 메소드, 즉 클래스나 인터페이스의 일부가 아닌 익명 메소드에 대해 [[java]]에서 제공하는 축약형이다.
이 함수에는 매개 변수 정의와 함수 본문이 포함되어 있다. 동일한 함수를 여러 가지 방법으로 작성할 수 있다.

## 예시
---
```java
// 원본
*stream*.filter(value -> value > 5).*furtherAction*

// 원본과 동일하다
*stream*.filter((Integer value) -> {
    if (value > 5) {
        return true;
    }

    return false;
}).*furtherAction*
```
