## 사용하면
---
[[stream]]을 사용하면 프로그래머는 [[컬렉션 (java)]]의 각 값에 대해 실행되는 이벤트 시퀀스를 정의한다.
이벤트 체인에는 일부 값 덤프, 한 형식에서 다른 형식으로의 값 변환 또는 계산이 포함될 수 있다.
스트림은 원래 데이터 컬렉션의 값을 변경하지 않고 단지 처리할 뿐이다. 변환을 유지하려면 다른 데이터 컬렉션으로 컴파일 해야한다.
[[람다 (lambda)]]을 사용한다.
stream은 1회용이다. [최종연산](#최종연산) 후에는 stream이 닫힌다.
## 스트림 메서드
---
스트림 메서드는 크게 두 가지 범주로 나눌 수 있다.
1. 요소 처리를 위한 중간 연산
2. 요소 처리를 종료하는 [최종연산](#최종연산)
`filter`메소드와 `mapToInt`메소드 모두 중간 연산이다. 중간 연산은 **추가로 처리할 수 있는 값을 반환**한다.
	실제로는 무한한 수의 중간 연산을 `.`으로 구분해 순차적으로 연결할 수 있다.
반면에 `average`메소드는 최종연산 이다. 최종연산은 처리할 값을 반환하며, 이 값은 스트림 요소에서 형성된다.

> [!note]- 아래 그림은 스트림의 작동 방식을 보여준다. 
>  (1) 값이있는 list다. list에서 stream() 메서드를 호출하면
>  (2) `stream<List>`가 생성된다. 그런다음 값은 개별적으로 처리된다. 스트림 값은
>  (3) filter 메서드로 조건을 충족하지 못하는 값을 제거하는 방식으로 필터링 된다.
>  (4) map 메서드는 스트림의 값을 한 형식에서 다른 형식으로 매핑하는 데 사용된다.
>  (5) collect 메서드는 제공된 컬렉션 (예: list)에 스트림 값을 수집한다.
> ![[Pasted image 20231128144824.png]]

위의 의미지에 표시된 예제의 코드. 이 예제 스트림에서는 값이 추가되는 새 ArrayList가 만들어진다. 이 연산은 마지막 줄에서 실행된다.
```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(17);
values.add(6);
values.add(8);

System.out.println("Values: " + values.stream().count());
```

## 최종연산 
---
총 4개지의 최종연산을 살펴보자.
	리스트의 값 개수를 세는 `count` 메소드, 리스트의 값들을 반복하는 `forEach`메소드, 리스트 값을 데이터 구조로 수집하는 `collect`메소드, 그리고 리스트 항목을 결합하는 `reduce`이다.

### count
스트림의 값 개수를 `long` 형식 변수로 알려준다.
```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(17);
values.add(6);
values.add(8);

// values : 5
System.out.println("Values: " + values.stream().count());
```

### forEach
각 리스트 값에 대해 수행할 작업을 정의하고 스트림 처리를 종료한다. 아래 예시에서는 먼저 숫자 리스트를 생성한 다음, 그 중에서 2로 나누어 떨어지는 숫자만 출력한다.
```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(17);
values.add(6);
values.add(8);

// 2, 6, 8
values.stream()
    .filter(value -> value % 2 == 0)
    .forEach(value -> System.out.println(value));
```

### collect
스트림 값을 다른 컬렉션으로 수집할 수 있다. 아래 예제에서는 양수 값만 포함하는 새 리스트를 만든다. `collect` 메서드는 스트림 값이 수집되는 Collectors 개체에 매개변수로 제공된다 (예: `Collectors.toCollection(ArrayList::::new`) 호출하면 수집된 값을 보유하는 새 ArrayList 객체가 만들어진다.
```java
List<Integer> values = new ArrayList<>();
values.add(3);
values.add(2);
values.add(-17);
values.add(-6);
values.add(8);

ArrayList<Integer> positives = values.stream()
    .filter(value -> value > 0)
    .collect(Collectors.toCollection(ArrayList::new));

// 3, 2, 8
positives.stream()
    .forEach(value -> System.out.println(value));
```



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
|값 필터링(Filtering values):`filter(value -> filter condition)`|필터 조건을 충족하지 않는 요소는 문자열에서 제거된다. 화살표의 오른쪽에는 boolean을 반환하는 조건문이 있다. boolean `true`면 요소가 스트림에 허용된다. boolean이 false로 평가되면 값이 스트림에 허용되지 않는다. 필터링된 값으로 연산이 수행된다.|
|평균 계산(Calculating the average): `average()`|`double` 유형의 값을 반환하는 `getAsDouble()` 메서드가 있는 [[Optional (java)]]Double 유형 객체를 반환한다. `average()` 메소드를 호출하면 정수가 포함 된 스트림에서 작동하며, `mapToInt` 메소드로 만들 수 있다.|
|스트림의 요소 수 계산(Counting the number of elements in a stream): `count()`|스트림의 요소 수를 `long` 형식 값으로 반환한다.|

## 스트림 API
[[java]] 8의 새로운 주요 기능중 하나인 요소 [[시퀀스 (Sequence)]]를