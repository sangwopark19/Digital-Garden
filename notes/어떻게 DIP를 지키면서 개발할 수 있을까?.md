
---
현재 다음과 같은 코드는 [[DIP (의존관계 역전 원칙)]] 원칙을 위반하고 있다. [[추상화]] (인터페이스)에만 의존하고 있는 것이 아니라, 구현체에도 의존하고 있기 때문이다.
```java
private DiscountPolicy discountPolicy = new RateDiscountPolicy;
```

따라서 [[OCP (개방-폐쇄 원칙)]] 도 위반하고 있다. 어떻게하면 DIP, OCP를 지키면서 개발할 수 있을까?

## 추상화만 의존
---
이 코드처럼 구현체를 지우고 추상화에만 의존하면 DIP 원칙을 지킬 수 있다.
하지만 이대로 코드를 실행하면 NullPointException이 터지게 된다.
```java
private DiscountPolicy discountPolicy;
```

이를 해결하려면 [[관심사의 분리]]를 적용해 **외부에서 구현체를 생성해서 주입을 해줘야 한다**

## 관심사의 분리
---
[[관심사의 분리]]

# AppConfig의 등장
---
### AppConfig
```java
public class AppConfig {  
  
    public MemberService memberService() {  
        return new MemberServiceImpl(new MemoryMemberRepository());  
    }  
  
    public OrderService orderService() {  
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());  
    }  
}
```
- 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, "**구현 객체를 생성**"하고, "**연결**"하는 책임을 가지는 별도의 설정 클래스를 만들자.
- AppConfig는 애플리케이션의 실제 동작에 필요한 **구현 객체를 생성**한다.
	- `MemberServiceImpl`
	- `MemoryMemberRepository`
	- `OrderServiceImpl`
	- `FixDiscountPolicy`
- Appconfig는 생성한 객체 인스턴스의 참조(레퍼런스)를 **생성자를 통해서 주입(연결** 해준다.
	- `MemberServiceImpl` -> `MemoryMemberRepository`
	- `OrderServiceImpl` > `MemoryMemberRepository`, `FixDiscountPolicy`

### MemberServiceImpl 생성자 주입 후
```java
public class MemberServiceImpl implements MemberService {  
  
    private final MemberRepository memberRepository;  
  
    public MemberServiceImpl(MemberRepository memberRepository) {  
        this.memberRepository = memberRepository;  
    }  
  
    @Override  
    public void join(Member member) {  
        memberRepository.save(member);  
    }  
  
    @Override  
    public Member findMember(Long memberId) {  
        return memberRepository.findById(memberId);  
    }  
}
```
- 설계 변경으로 `MemberServiceImpl`은 `MemoryMemberRepository`를 의존하지 않는다.
- 단지 `MemberRepository`인터페이스만 의존한다.
- `MemberServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)는 알 수 없다.
- `MemberServiceImpl`의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부(`AppConfig`)에서 결정된다.
- `MemberServiceImpl`은 이제부터 **의존관계에 대한 고민은 외부**에 맡기고 **실행에만 집중**하면 된다.
- ![[Pasted image 20231130123329.png]]

## 관련자료
---
- [[스프링 핵심 원리 - 기본편]]
	
