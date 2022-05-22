# Backend Example

- `비지니스 요구사항 정리`
    1. 데이터 : 회원ID, 이름
    2. 기능 :  회원등록, 조회
    3. 데이터 저장소가 선정되지 않음 → 데이터 베이스를 뭐로 할지 결정이 안 된 상황
    
    - **일반적인 웹 애플리케이션 계층 구조**
        
        ![Untitled](Backend%20Example%205351462d176940959ad75decfd7fcfb7/Untitled.png)
        
    1. 컨트롤러
        
        웹 MVC, API의 컨트롤러 역할
        
    2. 서비스 
        
        핵심 비지니스 로직을 구현한 클래스
        
        ex) 회원은 중복 가입이 안됨
        
    3. 리포지토리
        
        db에 접근하여 도메인 객체를 db에 저장하고 관리
        
    4. 도메인
        
        비지니스 도메인 객체
        
        ex) 회원, 주문 등 주로 db에 저장하고 관리됨
        
    
    - **클래스 의존관계**
        
        ![Untitled](Backend%20Example%205351462d176940959ad75decfd7fcfb7/Untitled%201.png)
        
    
    1. MemberService
        
        회원 비지니스 로직
        
    2. MemberRepository
        
        회원을 저장하는 것으로, 아직 데이터 저장소가 선정되지 않았기 때문에 인터페이스로 설계 (인터페이스로 구현클래스를 변경할 수 있도록 설계)
        
    3. MemoryMemberRepository
        
        인터페이스 구현체로 가벼운 메모리 기반의 데이터 저장소를 사용
        
    
    - **전체적인 구조**
        - Member **class**
            
            < main/java/hello/hellospring/domain/ >
            
            id, name, setId(), getName(), setName()
            
        - MemberRepository **interface**
            
            < main/java/hello/hellospring/repository/ >
            
            save(), findById(), findByName(), findAll()
            
        - MemoryMemberRepository **class**
            
            < main/java/hello/hellospring/repository/ >
            
            store, sequence, save(), findById(), findByName(), findeAll(), clearStore()
            
        - MemberService **class**
            
             memberRepository, join(), validateDuplicateMember(), findMembers(), findOne()
            
- `회원 도메인과 리포지토리 만들기`
    1. 회원 도메인
        
        ```java
        // main/java/hello/hellospring/domain/
        
        package hello.hellospring.domain;
        
        public class Member {
        
            private Long id;
            private String name;
        
            public Long getId() {
                return id;
            }
        
            public void setId(Long id) {
                this.id = id;
            }
        
            public String getName() {
                return name;
            }
        
            public void setName(String name) {
                this.name = name;
            }
        }
        ```
        
    2. 리포지토리 인터페이스
        
        ```java
        // main/java/hello/hellospring/repository/
        
        package hello.hellospring.repository;
        
        import hello.hellospring.domain.Member;
        import java.util.List;
        import java.util.Optional;
        
        public interface MemberRepository {
            Member save(Member member);
            Optional<Member> findById(Long id);
            Optional <Member> finByName(String name);
            List<Member> findAll();
        }
        ```
        
        - Optional
            
            null을 처리하기 위해 Optional로 감싸서 반환함(Optional객체가 반환됨)
            
    3. 리포지토리 클래스
        
        ```java
        // main/java/hello/hellospring/repository/
        
        package hello.hellospring.repository;
        
        import hello.hellospring.domain.Member;
        import java.util.*;
        
        public class MemoryMemberRepository implements MemberRepository{
        
            private static Map<Long, Member> store = new HashMap<>();
            private static long sequence = 0L;
        
            @Override
            public Member save(Member member) {
                member.setId(++sequence);
                store.put(member.getId(),member);
                return member;
            }
        
            @Override
            public Optional<Member> findById(Long id) {
                return Optional.ofNullable(store.get(id));
            }
        
            @Override
            public Optional<Member> finByName(String name) {
                return store.values().stream()
                        .filter(member -> member.getName().equals(name))
                        .findAny();
            }
        
            @Override
            public List<Member> findAll() {
                return new ArrayList<>(store.values());
            }
        
        		public void clearStore(){
                store.clear();
            }
        }
        ```
        
        - private static Map<Long, Member> store = new HashMap<>()
            
            Map : 인페이스로, key를 통해 value를 얻을 수 있음
            
            (key : Long형, value : Member클래스형)
            
        - member.setId(++sequence)
            
            store에 넣기 전에 아이디값을 셋팅
            
        - return Optional.ofNullable(store.get(id))
            
             null이 반환 될 가능성이 있다면 Optional로 감싸고 ofNullable해주면 null이어도 감쌀 수 있게 해줌, 감싸서 반환을 해주면 클라이언트 쪽에서 관리 가능
            
        - return store.values().stream()
            
            valueStream을 생성
            
        - .filter(member -> member.getName().equals(name))
            
            인자로 받은 name과 같은것을 필터링
            
        - findAny()
            
            하나라도 찾으면 반환, findAny()는 Optional로 자동으로 반환됨
            
        - public List<Member> findAll()
            
            store의 values인 회원 Memeber들을 list로 반환
            
        - public void clearStore()
            
            store의 모든 내용 삭제 (테스트 파일에서 사용)
            
- `회원 리포지토리 테스트 케이스 작성`
    
    개발한 기능을 실행해서 테스트를 할 때 Junit 프레임워크로 테스트를 실행하는 것
    
    ```java
    // test/java/hello/hellospring/repository/
    
    package hello.hellospring.repository;
    
    import hello.hellospring.domain.Member;
    import org.assertj.core.api.Assertions;
    import org.junit.jupiter.api.AfterEach;
    import org.junit.jupiter.api.Test;
    
    import java.util.List;
    
    import static org.assertj.core.api.Assertions.assertThat;
    
    class MemoryMemberRepositoryTest {
        MemoryMemberRepository repository = new MemoryMemberRepository();
    
        @AfterEach
        public void afterEach(){
            repository.clearStore();
        }
    
        @Test
        public void save() {
            Member member = new Member();
            member.setName("spring");
    
            repository.save(member);
            Member result = repository.findById(member.getId()).get();
    
            assertThat(member).isEqualTo(result);
        }
    
        @Test
        public void findByName(){
            Member member1 = new Member();
            member1.setName("spring1");
            repository.save(member1);
    
            Member member2 = new Member();
            member2.setName("spring2");
            repository.save(member2);
    
            Member result = repository.finByName("spring1").get();
    
            assertThat(result).isEqualTo(member1);
        }
    
        @Test
        public void findAll(){
            Member member1 = new Member();
            member1.setName("spring1");
            repository.save(member1);
    
            Member member2 = new Member();
            member2.setName("spring2");
            repository.save(member2);
            List<Member> result = repository.findAll();
    
            assertThat(result.size()).isEqualTo(2);
        }
    }
    ```
    
    - hello.hellospring.repository
        
        보통 똑같은 패키지에 테스트 파일 생성
        
    - import static
        
        메소드명으로 바로 접근할 수 있음
        
    - class MemoryMemberRepositoryTest
        
        테스트를 할 파일 이름 + Test
        
    - @Test
        
        하위의 메소드 실행, 메인메서드 실행하는 것과 비슷
        
    - Assertions
        
        < org.junit.jupiter.api >
        
        Assertions.assertEquals(member, result)
        
        → 틀리면 오류
        
    - Assertions
        
        < org.assertj.core.api >
        
        Assertions.assertThat(member).isEqualTo(result)
        
        → 틀리면 오류
        
    - get()
        
        Optional에서 값을 꺼낼 때 사용
        
    - @AfterEach
        
        순서가 없이 테스트 되기 때문에 테스트를 메서드 별로 진행해야함. @AfterEach를 사용하면 하나의 테스트가 끝날 때마다 하위 메서드가 실행됨
        
    
    <aside>
    💡 **TDD**
    
    테스트 주도 개발. 테스트를 미리 만든 후 구현클래스를 만들어서 돌려보는 것. 
    
    </aside>
    
- `회원 서비스 개발`
    
    회원 리포지토리와 도메인을 이용해서 실제 비지니스 로직 작성(비지니스적으로 네이밍 작성)
    
    ```java
    // test/java/hello/hellospring/service/
    
    package hello.hellospring.service;
    
    import hello.hellospring.domain.Member;
    import hello.hellospring.repository.MemberRepository;
    import hello.hellospring.repository.MemoryMemberRepository;
    import java.util.List;
    import java.util.Optional;
    
    public class MemberService {
    	  private final MemberRepository memberRepository;
    
        public MemberService(MemberRepository memberRepository) {
            this.memberRepository = memberRepository;
        }
    
        // 회원가입
        public Long join(Member member){
            validateDuplicateMember(member); //중복 회원 검증
            memberRepository.save(member);
            return member.getId();
        }
    
        private void validateDuplicateMember(Member member) {
            memberRepository.finByName(member.getName())
                    .ifPresent(m -> {
                        throw new IllegalStateException("이미 존재하는 회원입니다.");
                    });
        }
    
        // 전체 회원 조회
        public List<Member> findMembers(){
            return memberRepository.findAll();
        }
        public Optional<Member> findOne(Long memberId){
            return memberRepository.findById(memberId);
        }
    }
    ```
    
    - ifPresent()
        
        Optional의 메소드로, Optional값이 존재한다면 괄호 실행.
        
        ( findByName()는 Optional반환)
        
    - throws new 오류클래스명()
        
        오류클래스 발생
        
    
    <aside>
    💡 드래기 후 메서드 생성 단축기  ctrl + t
    
    </aside>
    
- `회원 서비스 테스트`
    
    ```java
    package hello.hellospring.service;
    
    import hello.hellospring.domain.Member;
    import hello.hellospring.repository.MemoryMemberRepository;
    import org.assertj.core.api.Assertions;
    import org.junit.jupiter.api.AfterEach;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import java.util.Optional;
    
    import static org.assertj.core.api.Assertions.assertThat;
    import static org.junit.jupiter.api.Assertions.*;
    
    class MemberServiceTest {
    
        MemoryMemberRepository memberRepository;
        MemberService memberService;
    
        @BeforeEach
        public void beforeEach(){
            memberRepository = new MemoryMemberRepository();
            memberService = new MemberService(memberRepository);  // di
        }
    
        @AfterEach
        public void afterEach() {
            memberRepository.clearStore();
        }
    
        @Test
        void 회원가입() {
            //given
            Member member = new Member();
            member.setName("hello");
    
            //when
            Long saveId = memberService.join(member);
    
            //then
            Member findMember = memberService.findOne(saveId).get();
            assertThat(member.getName()).isEqualTo(findMember.getName());
    
        }
    
        @Test
        public void 중복_회원_예외() {
            //given
            Member member1 = new Member();
            member1.setName("spring");
    
            Member member2 = new Member();
            member2.setName("spring");
    
            //when
            memberService.join(member1);
    
            IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));
    
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    
            /*try {
                memberService.join(member2);
                fail("예외가 발생해야합니다.");
            } catch (IllegalStateException e) {
                assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
            }*/
        }
    }
    ```
    
    > **test 파일 자동 생성** cmd  + shift + t
    > 
    
    <aside>
    💡 테스트 파일은 한글로도 많이 적음, 빌드 될 때 실제 코드에 포함되지 않음
    
    </aside>
    
    - //given //when //Then
        
        어떤 데이터로 무엇을 검증할 건지
        
    - public void 중복_회원_예외()
        
        join에서 예외가 잘 실행되는지 확인
        
    - fail
        
        예외가 던져지지 않았을 때 fail메소드를 호출해서 AssertionFailedError발생으로 테스트 실패 처리
        
    - assertThrows(IllegalStateException.class, () -> memberService.join(member2))
        
        ()→memberService.join(member2) 로직이 실행되면 IllegalStateException.class 예외가 발생해야 테스트 성공
        
    - e.getMessage()
        
        예외클래스의 메세지를 반환하여 메세지를 확인
        
    - public void beforeEach()
        
        테스트가 각각 실행되기 전 독립적으로 새로운 memberRepository와 memberService 생성
        
        → 테스트 파일과 실제 파일이 같은 memberRepository를 갖게 된다. 현재는 store가 static변수로 모든 객체에서 공유되어 상관이 없지만, 다른 memberRepository를 갖게되면 다른 store가 되어서 프로그램이 제대로 실행되지 않을 수가 있음.
        
    
    <aside>
    💡 Junit은 각각의 테스트 메서드 마다 테스트 자체를 다시 만든다. 즉, Test classe의 처음부터 다시 실행을하는 것이다. 하지만 store는 static변수이기 때문에 새로운 객체가 생성되어도 초기화 되지 않아 @AfterEach로 clearStore()을 호출하여 데이터를 직접 삭제해 줘야한다. (store가 static이 아닐결우 매 테스트마다 초기화되어 오류가 나지 않는 것이다.)
    
    - 예시
        
        ```java
        MemoryMemberRepository memberRepository = new MemoryMemberRepository();
        MemberService memberService = new MemberService(memberRepository);
        
        @AfterEach
        void AfterEach() {
            memberRepository.clearStore();
        }
        ```
        
    </aside>