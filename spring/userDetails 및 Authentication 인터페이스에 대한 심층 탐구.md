![[Pasted image 20241123132517.png]]
## 정리
---
다음과 같은 스프링 시큐리티 내부 흐름이 있다.
클라이언트에서 사용자 이름과 비밀번호를 서버쪽에 전달하면 spring security filters에서 사용자이름과 비밀번호로 authentication 객체를 생성해 authentication manger로 전달을 한다 여기서 인증 여부를 판단하는데 판단을 위해 먼저 authentication providers로 전달을해 provider가 authentication 객체에서 username을 꺼내 userdetails manager / service 의 loaduserbyusername() 메서드를 실행한다. 여기서 유저에 대한 정보를가진 userdetails를 provider에게 반환한 후 provider는 다시 authentication manager에게 authentication 으로 변환해 반환해준다 
이 떄 개발자들이 많이 헷갈려 하는 이유가 왜 굳이 providers에서 authentication 객체와 userdetails 객체로 변환하는지에 대한 것이다.
이유는 책임분리와 관련이 있는데 유저에 대한 이름, 비밀번호, 각종 다른정보들이 필요한건 유저를 검증할때만 필요해 userdetails로 나눈거고 이미 검증한 후 에는 검증이 되었는지 안되었는지 여부만 필요하기 때문에 authentication 객체에서 검증이 true인지만 확인하면 되기 때문이다 따라서 서로 상황에따라 필요없는 메서드들을 들고가지 않게 하기 위해 책임분리를 한 것이다.
	마지막으로 authentication 객체는 추상메서드인 AbstractAuthenticationToken 로 구현되고 이 추상클래스를 구현하는 UsernamePasswordAuthenticationToken이 스프링에서 기본적으로 사용되는데 이 때 추상 클래스에 정의된 공통으로 사용되는 메서드인 eraseCredentials() 메서드가 있다 이유는 무엇일까?
	위에서 말한것과 같이 이미 사용자의 검증이 끝난 후엔 authentication 객체 내부에 사용자의 비밀번호화 같은 민감한 정보가 메모리에 있어도 필요없으니 검증후에 삭제하는 것이다.

## 수정본
---
### 스프링 시큐리티 내부 인증 흐름

1. **클라이언트 요청**  
    클라이언트가 **사용자 이름(username)**과 **비밀번호(password)**를 서버로 전송합니다.
    
2. **Authentication 객체 생성**  
    Spring Security의 필터(Spring Security Filters)는 전달받은 사용자 이름과 비밀번호를 사용해 **`Authentication` 객체**를 생성합니다.  
    이 객체는 인증 과정에서 사용자 정보를 담고 인증 결과를 반환받는 역할을 합니다.
    
3. **AuthenticationManager 호출**  
    생성된 `Authentication` 객체는 **`AuthenticationManager`**에게 전달됩니다.  
    `AuthenticationManager`는 이 객체를 이용해 인증 여부를 판단합니다.
    
4. **AuthenticationProvider로 위임**  
    `AuthenticationManager`는 내부적으로 하나 이상의 **`AuthenticationProvider`**를 사용합니다.  
    각 `AuthenticationProvider`는 `supports()` 메서드로 자신이 처리할 수 있는 타입의 `Authentication` 객체인지 확인한 후, 적절한 `Provider`가 요청을 처리합니다.
    
5. **사용자 정보 로드 (UserDetailsService/Manager)**  
    적절한 `AuthenticationProvider`는 `Authentication` 객체에서 **사용자 이름(username)**을 추출한 뒤,  
    **`UserDetailsService` 또는 `UserDetailsManager`**의 `loadUserByUsername()` 메서드를 호출하여 **`UserDetails`** 객체를 반환받습니다.
    
    - `UserDetails` 객체에는 사용자 이름, 비밀번호, 권한 정보 등 인증에 필요한 정보가 담겨 있습니다.
6. **검증 및 Authentication 반환**  
    `AuthenticationProvider`는 반환받은 `UserDetails` 객체를 사용해:
    
    - 입력받은 비밀번호와 `UserDetails`의 비밀번호를 비교합니다.
    - 계정 상태(잠금 여부, 비활성화 여부 등)를 확인합니다.
    
    인증이 성공하면 **`Authentication` 객체**를 **인증 완료 상태**로 변환하여 `AuthenticationManager`로 반환합니다.
    
7. **인증 결과 반환**  
    `AuthenticationManager`는 인증이 성공한 `Authentication` 객체를 반환하고, 인증이 완료된 사용자는 보안 컨텍스트(Security Context)에 저장됩니다.
    

---

### 헷갈리기 쉬운 부분: 왜 `Authentication`과 `UserDetails`를 분리할까?

많은 개발자들이 **`Authentication`과 `UserDetails` 객체를 왜 나누는지**에 대해 궁금해합니다. 그 이유는 **책임 분리**(Separation of Concerns) 원칙과 관련이 있습니다.

1. **`UserDetails`는 사용자 정보 제공에 집중**
    
    - 사용자 이름, 암호화된 비밀번호, 권한, 계정 상태(잠금, 비활성화, 만료 여부 등)와 같은 **인증에 필요한 모든 정보**를 제공합니다.
    - 이는 인증 과정에서만 필요합니다.  
        인증이 끝난 후에는 더 이상 사용되지 않습니다.
2. **`Authentication`은 인증 상태를 나타냄**
    
    - 인증이 성공했는지 여부(`isAuthenticated()`), 인증된 사용자의 권한, 사용자 이름 등 **인증 후 필요한 정보만 유지**합니다.
    - 인증 과정 이후에는 `Authentication` 객체만 보안 컨텍스트에 저장됩니다.  
        즉, 인증 후 불필요한 세부 정보(`UserDetails`)를 시스템 전반에서 참조하지 않도록 설계한 것입니다.
3. **책임 분리의 이유**
    
    - 인증 과정에서는 세부적인 사용자 정보(`UserDetails`)가 필요하지만, 인증 이후에는 불필요합니다.
    - 따라서 **인증 과정과 인증 결과를 구분**하여:
        - **인증 과정:** `UserDetails` 객체로 검증을 수행.
        - **인증 결과:** `Authentication` 객체만 사용해 시스템의 다른 부분에서 간결하게 처리.

이 설계는 **필요 없는 데이터를 참조하지 않도록** 하고, **유지보수와 테스트를 용이**하게 합니다.

---

### 최종 수정된 내용

Spring Security 인증 흐름은 다음과 같이 정리할 수 있습니다.

1. 클라이언트가 사용자 이름과 비밀번호를 서버에 전송합니다.
2. Spring Security 필터가 이를 사용해 `Authentication` 객체를 생성하고, `AuthenticationManager`로 전달합니다.
3. `AuthenticationManager`는 적절한 `AuthenticationProvider`에 인증 요청을 위임합니다.
4. `AuthenticationProvider`는 `UserDetailsService` 또는 `UserDetailsManager`를 호출하여 사용자 정보를 로드하고, 이를 바탕으로 인증을 수행합니다.
5. 인증이 성공하면 `AuthenticationProvider`는 인증 완료된 `Authentication` 객체를 반환합니다.
6. `AuthenticationManager`는 인증 결과를 반환하고, 인증된 `Authentication` 객체는 보안 컨텍스트(Security Context)에 저장됩니다.

책임 분리를 통해:

- 사용자 정보를 로드하는 **`UserDetails`**와 인증 상태를 나타내는 **`Authentication`**를 구분.
- 불필요한 데이터 의존성을 줄이고 인증 흐름을 명확하게 만듭니다.
