# 1. 스프링 핵심 원리 - 기본편
스프링 웹 애플리케이션 개발 전반에 대한 학습

### 전체 목차
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

- 새로운 구조와 할인 정책 적용(SOLID 원칙 활용)
    - `public DiscountPolicy discountPolicy() {
        return new RateDiscountPolicy();
        //return new FixDiscountPolicy(); }`
- IoC, DI, 그리고 컨테이너
- *스프링으로 전환하기*
---------------
4. 스프링 컨테이너와 스프링 빈
- 스프링 컨테이너 생성
- 컨테이너에 등록된 모든 빈 조회
- 스프링 빈 조회 (기본, 동일한 타입이 둘 이상, 상속 관계)
- BeanFactory와 ApplicationContext
- 다양한 설정 형식 지원 - 자바 코드, XML
- 스프링 빈 설정 메타 정보 - BeanDefinition
---------------
5. 싱글톤 컨테이너
- 웹 애플리케이션과 싱글톤
- 싱글톤 패턴, 컨테이너
- 싱글톤 방식의 주의점
- @Configuration과 싱글톤, 바이트코드 조작의 마법
---------------
6. 컴포넌트 스캔
- 컴포넌트 스캔과 의존관계 자동 주입 시작하기
- 탐색 위치와 기본 스캔 대상
- 필터
- 중복 등록과 충돌
---------------
7. 의존관계 자동 주입
- 다양한 의존관계 주입 방법
- 옵션 처리
- 생성자 주입
- 롬복과 최신 트랜드
- @Autowired 필드 명, @Qualifier, @Primary
- 애노테이션 직접 만들기
- 조회한 빈이 모두 필요할 떄, List, Map
- 자동, 수동의 올바른 실무 운영 기준
---------------
8. 빈 생명주기 콜백
- 빈 생명주기 콜백 시작
- 인터페이스 InitializingBean, DisposableBean
- 빈 등록 초기화, 소멸 메서드
- 애노테이션 @PostConstruct, @PreDestroy
---------------
9. 빈 스코프
- 빈 스코프란?
- 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 문제점 -> Provider로 문제 해결
- 웹 스코프
- requeust 스코프 예제 만들기
- 스코프와 Provider
- 스코프와 프록시
