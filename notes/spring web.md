# spring web
---

## 기본동작구조
---
여기 있는 모든 동작들은 `logging.level.org.springframework`를 debug로 설정하면 확인해 볼 수 있다.


### 1. 요청은 어떻게 처리되나
- spring mvc에서 사용자의 모든 요청(url)은 가장먼저 **디스패처 서블릿**으로 간다. 이걸 **Front Controller Pattern**이라 한다.
	- 디스패처 서블릿은 url과 상관없이 요청이 가장 먼저 도달하는 곳이다. 디스패처 서블릿이 **루트 URL[ / ]**에 매핑되기 때문
- 디스패처 서블릿이 요청을 받으면 url에 알맞은 **컨트롤러로 매핑**한다.
- 디스패처 서블릿은 Auto Configuration `(DispatcherServletAutoConfiguration`에 의해 설정된다.
	- 클래스 경로에 있는 클래스를 기반으로 Spring Boot는 자동으로 우리가 웹 애플리케이션 혹은 REST API를 빌드한다는 사실을 탐지하고 **알아서 디스패처 서블릿을 설정**한다.

### 2. 어떻게 도메인 객체(Java Bean)가 JSON으로 변환이 되나
- `ResponseBody` + `JacksonHttpMessageConverters`
	- `@RestController` 내부에 `@ResponseBody`와 `@Controller`라는 주석이 있다. 여기서 `@ResponseBody`의 의미는 **객체(Bean)을 있는 그대로 반환하는 의미이다**
	- 스프링에선 그대로 반환되면 **메시지 변환**이 일어나는데 Spring Boot **Auto Configuration**이 설정한 기본 변환은 `JacksonHttpmessageConverters`를 사용한다

### 3. 오류 매핑은 어디에서 설정할까
- 유효하지 않은 url을 입력했을때 기본으로 404에러페이지를 출력해준다
- 이 역시 **Auto Configuration** (ErrorMvcAutoConfiguration)의 결과다

### 4. 어떻게 이 모든 jar를 사용할 수 있나 (Spring, Spring MVC, Jackson, Tomcat)
- **Starter Projects** 덕분이다
- Spring Web 의존성은 Spring boot Starter Web이라는 것이다.
- 그리고 그 안에 여러가지 기능들을 포함한 의존성을 가지고 있는 것이다.