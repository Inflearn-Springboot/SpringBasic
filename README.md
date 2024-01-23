# 1. 스프링 핵심 원리 - 기본편
스프링 웹 애플리케이션 개발 전반에 대한 학습

### 전체 목차
1. 객체 지향 설계와 스프링
2. 스프링 핵심 원리 이해1 - 예제 만들기
3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용
4. 스프링 컨테이너와 스프링 빈
5. 싱글톤 컨테이너
6. 컴포넌트 스캔
7. 의존관계 자동 주입
8. 빈 생명주기 콜백
9. 빈 스코프
---------------
1. 객체 지향 설계와 스프링
- 스프링이란? : 좋은 객체 지향 애플리케이션을 개발할 수 있게 도와주는 프레임워크
- 좋은 객체 지향 프로그래밍(SOLID)
  - SRP 단일 책임 원칙(single responsibility principle) <- 한 클래스는 하나의 책임을 가져야 한다.
  - OCP 개방-폐쇄 원칙 (Open/closed principle) <- 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
  - LSP 리스코프 치환 원칙 (Liskov substitution principle) <- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
  - ISP 인터페이스 분리 원칙 (Interface segregation principle) <- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
  - DIP 의존관계 역전 원칙 (Dependency inversion principle) <- 구현 클래스에 의존하지 말고, 인터페이스에 의존한다.
#### 다형성 만으로는 OCP, DIP를 지킬 수 없다...?
- 객체 지향 설계와 스프링
  - 스프링은 DI 기술로 다형성 + OCP, DIP 가능하게 지원
  - DI(Dependency Injection): 의존관계, 의존성 주입
---------------
2. 스프링 핵심 원리 이해1 - 예제 만들기
- 비즈니스 요구사항과 설계 : 회원가입 및 조회, 회원등급 : (일반, VIP) , 회원데이터 : 자체 DB or 외부 시스템(미확정), 상품 주문(회원만), 할인 정책 적용
- 회원 도메인 설계 및 개발
<img width="453" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/15109b20-3ab8-497b-b637-b8cd9cd54614">
<img width="451" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/6e78e715-4a1e-4b41-a19d-95c21be6ff38">

- 회원 도메인 실행과 테스트 : **의존관계가 인터페이스 뿐만 아니라 구현까지 모두 의존하는 문제점이 있음**
   - 문제 코드 : `public class MemberServiceImpl implements MemberService { private final MemberRepository memberRepository = new MemoryMemberRepository();`
   - 테스트 코드 : <img width="400" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/9a34e8dc-e3b4-4283-b15e-bbfec9abd7be">
     
- 주문과 할인 도메인 설계 및 개발
  
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/65c87388-8c7a-4379-8f75-bfad3c32b837">
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/4f1a1495-2fcf-4cea-b246-aac7b781ec30">
  <img width="400" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/ef67e94a-2565-47f6-9221-02cbc681554b">
  <img width="400" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/2d1906e6-4155-4651-a31d-9658b71af871">


- 주문과 할인 도메인 실행과 테스트
  
  <img width="400" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/232955db-7ffb-4d82-8311-d502337b9d99">

---------------
3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용
- 새로운 할인 정책 개발
  
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/4d0cb4fd-3534-4298-8a9e-57acd9b6b211">
  <img width="460" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/6224d6b0-e128-46d5-9c9b-e02b19e65d66">

- 새로운 할인 정책 적용과 문제점
  - 문제코드 (**OCP, DIP 위반**): <img width="650" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/aa0dad10-73ef-465b-b940-ada5e57d7554">

- 관심사의 분리(**해결방안** : 생성자 주입) , AppConfig 리팩토링
    - 클라이언트인 `memberServiceImpl` 입장에서 보면 의존관계를 마치 외부에서 주입해주는 것 같다고 해서 DI(Dependency Injection) 우리말로 의존관계 주입 또는 의존성 주입이라 한다.
  
    <img width="495" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/459eaf61-2be7-465a-9535-cc99ae920a06">
    <img width="470" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/3fedb165-deef-42c7-8000-97c2b4f88219">
    
    - Test 코드(BeforeEach 애노테이션 적용 <- **테스트 전 AppConfig 실행**)  <img width="300" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/09cb0f6a-8522-4837-8b0c-639c6b4d8ad1">

- AppConfig 리팩토링
  - 변경 전 : <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/3fedb165-deef-42c7-8000-97c2b4f88219">
  - 변경 후 : <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/740d9ac2-ebcf-4db0-a9da-eb4691ea01db">

- 새로운 구조와 할인 정책 적용(SOLID 원칙 활용) <- 여기서 3가지 SRP, DIP, OCP 적용
    - `public DiscountPolicy discountPolicy() {
        return new RateDiscountPolicy();
        //return new FixDiscountPolicy(); }`
- IoC, DI, 그리고 컨테이너
  - IoC(제어의역전): 프로그램에 대한 제어 흐름에 대한 권한은 모두 AppConfig가 가지고 있다. 심지어 `OrderServiceImpl` 도 AppConfig가 생성한다. 그리고 AppConfig는 `OrderServiceImpl` 이 아닌 OrderService 인터페이스의 다른 구현 객체를 생성하고 실행할 수 도 있다. 그런 사실도 모른체 `OrderServiceImpl` 은 묵묵히 자신의 로직 을 실행할 뿐이다. **이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전(IoC)이라 한다.**
  - (**참고**) **프레임워크 vs 라이브러리**
프레임워크가 내가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크가 맞다. (JUnit)
반면에 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 프레임워크가 아니라 라이브러리다.
  - DI : 의존관계는 **정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계** 둘을 분리해서 생각해야 한다. 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결 되는 것을 **의존관계 주입**이라 한다.
  - 컨테이너 : AppConfig 처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 IoC 컨테이너 또는 DI 컨테이너라 한다. 또는 어샘블러, 오브젝트 팩토리 등으로 불리기도 한다.
- *스프링으로 전환하기*

  <img width="470" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/011bc4af-df55-477f-8564-8a207efc5691">
  <img width="470" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/92967c6f-4c0b-433e-92a3-c5002c7d5bf7">
  <img width="440" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/88b02ace-9352-4c1d-b579-61911167dd6e">

**코드가 약간 더 복잡해진 것 같은데, 스프링 컨테이너를 사용하면 어떤 장점이 있을까?**

---------------
4. 스프링 컨테이너와 스프링 빈
- 스프링 컨테이너 생성
  
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/bc667041-5b68-41e3-af61-61c57dc46a67">
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/b51fef3c-f2d3-4825-81e1-e9199318c53b">
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/085cc26b-3f01-4efb-93de-42bb2d3c295a">
  <img width="450" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/61d447f6-c0d0-471a-b8f2-80476ef82258">

  **주의: 빈 이름은 항상 다른 이름을 부여해야 한다. 같은 이름을 부여하면, 다른 빈이 무시되거나, 기존 빈을 덮어버리거나 설정에 따라 오류가 발생한다.**


- 컨테이너에 등록된 모든 빈 조회  **TEST - beanfind package 참고**

      public class ApplicationContextInfoTest {

      AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
  
      @Test
      @DisplayName("모든 빈 출력하기")
      void findAllBean() {
          String[] beanDefinitionNames = ac.getBeanDefinitionNames();
          for (String beanDefinitionName : beanDefinitionNames) {
              Object bean = ac.getBean(beanDefinitionName);
              System.out.println("name = " + beanDefinitionName + "object = " + bean);
          }
      }
  
      @Test
      @DisplayName("애플리케이션 빈 출력하기")
      void findApplicationBean() {
          String[] beanDefinitionNames = ac.getBeanDefinitionNames();
          for (String beanDefinitionName : beanDefinitionNames) {
              BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
  
              //ROLE_APPLICATION : 직접 등록한 애플리케이션 빈
              //ROLE_INFRASTRUCTURE: 스프링이 내부에서 사용하는 빈
              if(beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION){
                  Object bean = ac.getBean(beanDefinitionName);
                  System.out.println("name = " + beanDefinitionName + "object = " + bean);
              }
          }
      }
  
- 스프링 빈 조회 (기본, 동일한 타입이 둘 이상, 상속 관계) **TEST - beanfind package 참고**
- BeanFactory와 ApplicationContext

  <img width="931" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/56080344-ce39-4ac8-8b4a-55559d62fb05">

  - MessageSource : 국제화 기능 (한국 -> 한국어, 영어권 -> 영어)
  - EnvironmentCapable : 환경변수 (로컬, 개발, 운영등을 구분해서 처리)
  - ApplicationEventPublisher : 애플리케이션 이벤트 (이벤트를 발행하고 구독하는 모델을 편리하게 지원)
  - ResourceRoader : 편리한 리소스 조회 (파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회)
    
- 다양한 설정 형식 지원 - 자바 코드, XML
  
    <img width="800" height="500" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/8813efd6-c366-4398-b75a-4402910249df">

    <img width="400" height="200" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/c80e500f-3fc5-4418-96e4-acda41621879">
    <- resources - appConfig.xml 
- 스프링 빈 설정 메타 정보 - BeanDefinition **(역할과 구현 개념적으로 나눈 것)**
  - `BeanDefinition` 을 빈 설정 메타정보라 한다.
    
  <img width="710" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/12e208d7-6650-493d-b3de-f03cabcdeca9">

---------------
5. 싱글톤 컨테이너
- 웹 애플리케이션과 싱글톤
    - 우리가 만들었던 스프링 없는 순수한 DI 컨테이너인 AppConfig는 요청을 할 때 마다 객체를 새로 생성한다.
    - 고객 트래픽이 초당 100이 나오면 초당 100개 객체가 생성되고 소멸된다! 메모리 낭비가 심하다.
    - 해결방안은 해당 객체가 딱 1개만 생성되고, 공유하도록 설계하면 된다. **싱글톤 패턴 필요이유**
- 싱글톤 패턴, 컨테이너
  
      public class SingletonService {

        private static final SingletonService instance = new SingletonService();
    
        public static SingletonService getInstance() {
            return instance;
        }
    
        private SingletonService() {
    
        }
    
        public void logic() {
            System.out.println("싱글톤 객체 로직 호출");
        }
      }
 
  - 이 객체 인스턴스가 필요하면 오직 `getInstance()` 메서드를 통해서만 조회할 수 있다. 이 메서드를 호출하면 항상 같은 인스턴스를 반환한다.

**이러한 문제점을 스프링은 어떻게 보완하였을까?**
  - **스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.**

  <img width="646" alt="image" src="https://github.com/Inflearn-Springboot/SpringBasic/assets/96871403/6bd82bd8-4288-4c63-9a3b-54fad3836b30">

- 싱글톤 방식의 주의점

      @Test
      void statefulServiceSingleton() {
          ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
          StatefulService statefulService1 = ac.getBean(StatefulService.class);
          StatefulService statefulService2 = ac.getBean(StatefulService.class);
  
          //ThreadA: A사용자 10000원 주문
          int userAPrice = statefulService1.order("userA", 10000);
          //ThreadB: B사용자 20000원 주문
          int userBPrice = statefulService2.order("userB", 20000);
  
          //ThreadA: 사용자 A 주문 금액 조회
          System.out.println("price = " + userAPrice);
  
          // Assertions.assertThat(statefulService1.getPrice()).isEqualTo(20000);
        }
  
  -`StatefulService` 의 `price` 필드는 공유되는 필드인데, 특정 클라이언트가 값을 변경한다
- @Configuration과 싱글톤, 바이트코드 조작의 마법
  - @Configuration 사용할 때 : 
    결과 ->  (한번씩 호출) call AppConfig.memberService, call AppConfig.memberRepository, call AppConfig.orderService
  - @Configuration 사용 안할 때 :
    결과 -> call AppConfig.memberService, call AppConfig.memberRepository, call AppConfig.orderService, call AppConfig.memberRepository, call AppConfig.memberRepository
  - @Bean만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
  - 크게 고민할 것이 없다. 스프링 설정 정보는 항상 `@Configuration` 을 사용하자.

---------------
6. 컴포넌트 스캔
- 컴포넌트 스캔과 의존관계 자동 주입 시작하기
  
      @Configuration
      @ComponentScan(
              excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
      )
      public class AutoAppConfig {
      
      }
  
  - `@ComponentScan` 은 `@Component` 가 붙은 모든 클래스를 스프링 빈으로 등록한다.(getBean(MemberRepository.class)과 동일하다고 이해)

  
        @Component
        public class MemberServiceImpl implements MemberService {
             private final MemberRepository memberRepository;
             @Autowired
             public MemberServiceImpl(MemberRepository memberRepository) {
                 this.memberRepository = memberRepository;
           }
        }
    
- 컴포넌트 기본 스캔 대상
    - `@Controller` : 스프링 MVC 컨트롤러로 인식
    - `@Repository` : 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환해준다.
    - `@Configuration` : 앞서 보았듯이 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가 처리를 한다.
    - `@Service` : 사실 `@Service` 는 특별한 처리를 하지 않는다. 대신 개발자들이 핵심 비즈니스 로직이 여기에 있겠구나 라고 비즈니스 계층을 인식하는데 도움이 된다.
      
- 필터

      @Configuration
      @ComponentScan(
              includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),
              excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
  
      )
  - `includeFilters` 에 `MyIncludeComponent` 애노테이션을 추가해서 BeanA가 스프링 빈에 등록된다.
  - `excludeFilters` 에 `MyExcludeComponent` 애노테이션을 추가해서 BeanB는 스프링 빈에 등록되지 않는다.
    
**옵션을 변경하면서 사용하기 보다 는 스프링의 기본 설정에 최대한 맞추어 사용하는 것을 권장하고, 선호하는 편이다.**
 
- 중복 등록과 충돌
    - **수동 빈 등록, 자동 빈 등록 오류시 스프링 부트 에러**
`Consider renaming one of the beans or enabling overriding by setting
spring.main.allow-bean-definition-overriding=true`
    
---------------
7. 의존관계 자동 주입
- 다양한 의존관계 주입 방법
  - 생성자 주입 <- **불변, 필수** 의존관계에 사용
  - 수정자 주입(setter 주입) <- **선택, 변경** 가능성이 있는 의존관계에 사용
  - 필드 주입, 일반 메서드 주입 (일반적으로 사용 x)
- 옵션 처리
  - `@Autowired(required=false)` : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
  - `org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 null이 입력된다.
  - `Optional<>` : 자동 주입할 대상이 없으면 `Optional.empty` 가 입력된다.
- **생성자 주입**
  - 생성자 주입 방식을 선택하는 이유는 여러가지가 있지만, 프레임워크에 의존하지 않고, 순수한 자바 언어의 특징 을 잘 살리는 방법이기도 하다.
  - 기본으로 생성자 주입을 사용하고, 필수 값이 아닌 경우에는 수정자 주입 방식을 옵션으로 부여하면 된다. 생성자 주입과 수정자 주입을 동시에 사용할 수 있다.
- 롬복과 최신 트랜드

      @Component
      @RequiredArgsConstructor //<- Lombok 제공
      public class OrderServiceImpl implements OrderService {
         private final MemberRepository memberRepository;
         private final DiscountPolicy discountPolicy;
      }
 
- @Autowired 필드 명, @Qualifier, @Primary
  - @Autowired private DiscountPolicy discountPolicy
  - @Qualifier("mainDiscountPolicy")
  - @Autowired 시에 여러 빈이 매칭되면 `@Primary` 가 우선권을 가진다.
- 애노테이션 직접 만들기

      @Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
      @Retention(RetentionPolicy.RUNTIME)
      @Inherited
      @Documented
      @Qualifier("mainDiscountPolicy")
      public @interface MainDiscountPolicy {
      }

  - **@MainDiscountPolicy로 사용**

- 조회한 빈이 모두 필요할 떄, List, Map
    - `Map<String, DiscountPolicy>` : map의 키에 스프링 빈의 이름을 넣어주고, 그 값으로 `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담아준다.
    - `List<DiscountPolicy>` : `DiscountPolicy` 타입으로 조회한 모든 스프링 빈을 담아준다.
- 자동, 수동의 올바른 실무 운영 기준
    - **편리한 자동 기능을 기본으로 사용하자**
    - 다형성을 적극 활용하는 비즈니스 로직은 수동 등록을 고민해보자
---------------
8. 빈 생명주기 콜백
- 빈 생명주기 콜백 시작
  - **스프링 빈의 이벤트 라이프사이클**
  - **스프링 컨테이너 생성** -> **스프링 빈 생성** -> **의존관계 주입** -> **초기화 콜백** -> **사용** -> **소멸전 콜백** -> **스프링 종료**
  - **초기화 콜백**: 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
  - **소멸전 콜백**: 빈이 소멸되기 직전에 호출
- 인터페이스 InitializingBean, DisposableBean
  - `InitializingBean` 은 `afterPropertiesSet()` 메서드로 초기화를 지원한다.
  - `DisposableBean` 은 `destroy()` 메서드로 소멸을 지원한다.
  - 참고: 인터페이스를 사용하는 초기화, 종료 방법은 스프링 초창기에 나온 방법들이고, 지금은 다음의 더 나은 방법 들이 있어서 거의 사용하지 않는다.
- 빈 등록 초기화, 소멸 메서드
  - `@Bean(initMethod = "init", destroyMethod = "close")` 처럼 초기화, 소멸 메서드를 지정할 수 있다.
- **애노테이션 @PostConstruct, @PreDestroy**
  - 최신 스프링에서 가장 권장하는 방법이다.
  - 애노테이션 하나만 붙이면 되므로 매우 편리하다.
  - 패키지를 잘 보면 `javax.annotation.PostConstruct` 이다. 스프링에 종속적인 기술이 아니라 JSR-250 라는 자바 표준이다. 따라서 스프링이 아닌 다른 컨테이너에서도 동작한다.
  - 컴포넌트 스캔과 잘 어울린다.
  - 유일한 단점은 외부 라이브러리에는 적용하지 못한다는 것이다. 외부 라이브러리를 초기화, 종료 해야 하면 @Bean의 기능을 사용하자.
---------------
9. 빈 스코프
- 빈 스코프란?
- 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 문제점 -> Provider로 문제 해결
- 웹 스코프
- requeust 스코프 예제 만들기
- 스코프와 Provider
- 스코프와 프록시
