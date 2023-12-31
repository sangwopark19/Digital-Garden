
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
- 객체의 생성과 연결은 `AppConfig`가 담당한다.
- **[[DIP (의존관계 역전 원칙)]]완성** `MemberServiceImpl`은 `MemberRepository`인 추상에만 의존하면 된다. 이제 구체 클래스를 몰라도 된다.
- **관심사의 분리** 객체를 생성하고 연결하는 역할과 실행하는 역할이 명확히 분리되었다.
- ![[Pasted image 20231130123838.png]]
- `AppConfig`객체는 `MemoryMemberRepository`객체를 생성하고 그 참조값을 `MemberServiceImpl`을 생성하면서 생성자로 전달한다.
- 클라이언트인 `MemberServiceImpl` 입장에서 보면 의존관계를 마치 외부에서 주입해주는 것 같다고 해서 **DI(Dependency Injection) 우리말로 의존관계 주입 또는 의존성 주입**이라 한다.

## 정리
---
- AppConfig를 통해서 관심사를 확실하게 분리했다.
- 배역, 배우를 생각해보자
- AppConfig는 공연 기획자다.
- AppConfig는 구체 클래스를 선택한다. 배역에 맞는 담당 배우를 선택한다. 애플리케이션이 어떻게 동작해야 할지 전체 구성을 책임진다.
- 이제 각 배우들은 담당 기능을 실행하는 책임만 지면 된다.
- `OrderServiceImpl`은 기능을 실행하는 책임만 지면 된다.
## 관련자료
---
- [[스프링 핵심 원리 - 기본편]]
	
