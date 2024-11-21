 #단축키
## 단축키
---
- cmd + opt + v: 변수 표현식  예) memberService.findMember(1L); -> Member findMember = memberService.findMember(1L);
- ctrl + opt + n: 현재 디렉토리에서 새 파일 생성
- (fn) + F2: 오류가 난 곳으로 바로 이동
- cmd + e: 최근연 파일들 보기(히스토리)
- cmd + opt + m: 한번누르면 퀵메소드 추출, 두번연속누르면 자세한 메소드추출
- opt + f8: 표현식 평가 다음과 같은 코드의 결과값을 알 수 있음
```java
header.substring(6).getBytes(StandardCharsets.UTF_8);
```

## 단축어
---
- soutv: 근처에있는 변수 로그출력 예) Member findMember; -> System.out.println("findMember = " + findMember);
- soutm: 메소드명 로그 출력
- psvm: public static void main
- iter: 근처에 배열, 리스트가 있을때 for문 자동생성