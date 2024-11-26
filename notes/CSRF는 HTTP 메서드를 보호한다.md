
다음은 securityFilterChain의 일부이다.
`/register` 경로를 `permitAll()`로 지정했지만
403 Forbidden이뜬다. 이유가 뭘까?

바로 security의 기본기능인 csrf 보호로 인해 GET 메서드를 제외한 수정, 삭제, 업데이트 메서드들은 모두 막혀있기 때문이다.

```java
 .authorizeHttpRequests(requests -> requests  
    .requestMatchers("/my-account", "/my-balance", "/my-loans", "/my-cards")  
    .authenticated()  
    .requestMatchers("/notices", "/contact", "/error", "/register").permitAll())
```

이를 해결하기 위해선 
```java
.csrf(AbstractHttpConfigurer::disable)
```
필터체인에 다음 람다식만 추가해주면 된다.