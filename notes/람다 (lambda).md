## 정의
---
람다 표현식은 주로 [[java]]의 [[stream (java)]]에서 사용되며 **"소유자"가 없는 익명 메소드, 즉 클래스나 인터페이스의 일부가 아닌 익명 메소드** 에 대해 [[java]]에서 제공하는 축약형이다.
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

정적 메서드가 프로그램에 정의되도록 명시적으로 작성할 수 있으며, 이는 매개 변수로 스트림에 전달 된 함수 내에서 사용된다.
```java
public class Screeners {
    public static boolean greaterThanFive(int value) {
        return value > 5;
    }
}
```

```java
// 원본
*stream*.filter(value -> value > 5).*furtherAction*

// 원본과 동일하다
*stream*.filter(value -> Screeners.greaterThanFive(value)).*furtherAction*
```

이 함수는 매개 변수로 직접 전달될 수 도 있다. 아래에 있는 구문 `Screeneers::greaterThanFive`은 "`Screeners` 클래스에 있는 [[정적 메서드 (java)]] `greaterThanFive`를 사용해라" 라고 말한다.
```java
// 원본과 동일하다
*stream*.filter(Screeners::greaterThanFive).*furtherAction*
```
[[stream]]요소를 처리하는 함수는 함수 외부의 변수 값을 변경할 수 없다. 이것은 [[정적 메서드 (java)]]가 동작하는 방식과 관련이 있다. 스트림 함수를 사용하면 **함수 외부의 변수 값을 읽을 수 있으며, 해당 변수의 값이 프로그램에서 변경되지 않는다고 가정한다.**

아래 프로그램은 함수가 함수 외부의 변수를 사용하려고 시도하는 상황을 보여주며, 작동하지 않는다.
```java
// 스캐너와 값을 읽을 목록 초기화하기
Scanner scanner = new Scanner(System.in);
List<String> inputs = new ArrayList<>()

// 입력 받기
while (true) {
    String row = scanner.nextLine();
    if (row.equals("end")) {
        break;
    }

    inputs.add(row);
}

int numberOfMappedValues = 0;

// 3으로 나눌 수 있는 값의 수 결정하기
long numbersDivisibleByThree = inputs.stream()
    .mapToInt(s -> {
        // 익명 함수 외부에서 선언된 변수는 사용할 수 없으므로 작동하지 않는다.
        numberOfMappedValues++;
        return Integer.valueOf(s);
    }).filter(value -> value % 3 == 0)
    .count();
```