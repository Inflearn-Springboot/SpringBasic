# 0. 스프링 핵심 원리 - 기본편
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
- 비즈니스 요구사항과 설계
- 회원 도메인 설계 및 개발
- 회원 도메인 실행과 테스트
- 주문과 할인 도메인 설계 및 개발
- 주문과 할인 도메인 실행과 테스트
---------------
3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용
- 새로운 할인 정책 개발
- 새로운 할인 정책 적용과 문제점
- 관심사의 분리
- AppConfig 리팩토링
- 새로운 구조와 할인 정책 적용(SOLID 원칙 활용)
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
