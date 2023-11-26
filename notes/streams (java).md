## 사용하면
---
[[streams]]을 사용하면 프로그래머는 [[컬렉션 (java)]]의 각 값에 대해 실행되는 이벤트 시퀀스를 정의한다.
이벤트 체인에는 일부 값 덤프, 한 형식에서 다른 형식으로의 값 변환 또는 계산이 포함될 수 있다.
스트림은 원래 데이터 컬렉션의 값을 변경하지 않고 단지 처리할 뿐이다. 변환을 유지하려면 다른 데이터 컬렉션으로 컴파일 해야한다.

## 예시
---
```java
// 스캐너와 입력을 읽을 목록을 초기화합니다.
Scanner scanner = new Scanner(System.in);
List<String> inputs = new ArrayList<>();

// 입력 읽기
while (true) {
    String row = scanner.nextLine();
    if (row.equals("end")) {
        break;
    }

    inputs.add(row);
}

// 3으로 나눌 수 있는 값의 개수 세기
long numbersDivisibleByThree = inputs.stream()
    .mapToInt(s -> Integer.valueOf(s))
    .filter(number -> number % 3 == 0)
    .count();

// 평균 계산하기
double average = inputs.stream()
    .mapToInt(s -> Integer.valueOf(s))
    .average()
    .getAsDouble();

// 통계 인쇄
System.out.println("Divisible by three " + numbersDivisibleByThree);
System.out.println("Average number: " + average);
```

|Purpose and method|Assumptions|
|---|---|
|스트림 형성(formation): `stream()`|이 메서드는 Collection 인터페이스를 구현하는 컬렉션(예: ArrayList 개체)에서 호출된다. 그리고 생성된 스트림에서 연산이 수행된다.|
|스트림을 정수 스트림으로 변환(Converting a stream into an integer stream): `mapToInt(value -> another)`|스트림은 정수를 포함하는 스트림으로 변환된다. 문자열이 포함된 스트림은 예를 들어 Integer 클래스의 parseInt 메서드를 사용하여 변환할 수 있다. 정수를 포함하는 스트림으로 연산이 수행된다.|
|값 필터링(Filtering values):`filter(value -> filter condition)`|필터 조건을 충족하지 않는 요소는 문자열에서 제거된다. 화살표의 오른쪽에는 부울을 반환하는 문이 있습니다. 부울이 `true`이면 요소가 스트림에 허용됩니다. 부울이 false로 평가되면 값이 스트림에 허용되지 않습니다. 필터링된 값으로 작업이 수행됩니다.|
|평균 계산(Calculating the average): `average()`|`double` 유형의 값을 반환하는 `getAsDouble()` 메서드가 있는 OptionalDouble 유형 객체를 반환합니다. `average()` 메소드를 호출하면 정수가 포함 된 스트림에서 작동하며, `mapToInt` 메소드로 만들 수 있습니다.|
|스트림의 요소 수 계산(Counting the number of elements in a stream): `count()`|스트림의 요소 수를 `long` 형식 값으로 반환합니다.|

## 스트림 API
[[java]] 8의 새로운 주요 기능중 하나인 요소 [[시퀀스 (Sequence)]]를