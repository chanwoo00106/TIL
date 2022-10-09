# 스프링 컨테이너

스프링 컨테이너는 스프링에서 자바 객체들을 **관리하는 공간**을 말한다<br>
스프링에선 자바 객체를 **빈(Bean)**이라고 한다<br>
스프링 컨테이너에서는 빈의 생성부터 소멸까지 개발자 대신 관리해준다

### 컨테이너 종류

- `BeanFactory`<br>
  빈을 생성하고 의존관계를 설정하는 기능을 담당하는 가장 기본적인 IoC 컨테이너이자 클래스를 말한다

- `ApplicationContext`<br>
  `BeanFactory`의 확장 버전이라고 생각하면 좋다<br>
  특별한 이유가 없다면 `ApplicationContext`를 쓰는 게 좋다

### 스프링 빈

- 컴포넌트 스캔<br>
  `@Component`, `@Service`, `@Repository`, `@Controller`와 같은 어노테이션을 클래스 위에 넣어주면 스프링 빈에 자동적으로 등록이 된다

- Configuration<br>
  빈을 직접 등록하는 방법으로 `@Configuration`과 `@Bean`을 통해 만든다

```java
  @Configuration
  public class AppConfig {
    @Bean
    public MemberService memberService() {
      return new MemberService();
    }
  }
```
