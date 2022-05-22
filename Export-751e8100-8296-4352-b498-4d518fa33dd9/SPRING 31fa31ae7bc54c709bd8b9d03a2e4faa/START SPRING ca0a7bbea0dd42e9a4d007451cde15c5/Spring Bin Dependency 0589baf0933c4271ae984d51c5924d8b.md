# Spring Bin Dependency

**스프링 빈**

스프링을 실행시키면 스프링 컨테이너가 자동으로 생성되는데 이 spring IoC 컨테이너가 관리하는 자바 객체.  자바 class또는 메서드에 애노테이션을 설정해서 스프링이 관리할 수 있도록 설정해야함

**스프링 빈을 등록하는 방법**

- `컴포넌트 스캔과 자동 의존관계 설정`
    
    @Component가 있으면 스프링 빈으로 자동으로 등록되며, @Component에는 @Controller, @Service, @Repository 포함
    
    → 스프링이 생성되면 Component와 관련된 애노테이션이 있는 클래스의 객체들을 생성하며 스프링컨테이너에 등록을 해준다. Autowired는 각각의 스프링빈들을 연결해준다.
    
    ```java
    // main/java/hello/hellospring/controller/
    
    package hello.hellospring.controller;
    
    import hello.hellospring.service.MemberService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    
    @Controller
    public class MemberController {
        private final MemberService memberService;
    
        @Autowired
        public MemberController(MemberService memberService) {
            this.memberService = memberService;
    		// DI 생성자 주입
        }
    }
    ```
    
    1. @Controller
        
        해당 클래스 객체를 생성해서 스프링 컨테이너에 넣어둠(이 객체는 스프링 빈이 되어 관리됨)
        
    2. @Autowired
        
        Controller와 Service를 연결해주며, 생성자에 붙여놓으면 스프링 컨테이너에 있는 인스턴스 memberService를 가져와서 연결(스프링 컨테이너에는 단 하나의 인스턴스만 등록이 되어 공용으로 사용 가능)
        
        → MemberController가 생성이 될 때 생성자가 호출되면서 스프링 빈에 등록 되어있는 MemberService객체를 넣어줌
        
    
    ```java
    @Service
    public class MemberService {
    	@Autowired
    	public MemberService(MemberRepository memberRepository) { ... }
    }
    ```
    
    1. @Service
        
        스프링이 실행이 될 때 MemberService를 스프링 컨테이너에 등록
        
    2. @Autowired
        
        MemberService가 생성이 될 때 생성자가 호출되면서 스프링 빈에 등록되어있는 MemberRepository의 구현체인 MemoryMemberRepository 객체를 넣어줌
        
    
    ```java
    @Repository 
    public class MemoryMemberRepository implements MemberRepository{ ... }
    ```
    
    1. @Repository
        
        스프링이 실행이 될 때 MemberRepository를 스프링 컨테이너에 등록
        
    
    ![Untitled](Spring%20Bin%20Dependency%200589baf0933c4271ae984d51c5924d8b/Untitled.png)
    
    Controller을 통해서 외부 요청을 받음 → Service에서 비니지스 로직을 만듬 → Repository에서 데이터를 저장
    

<aside>
💡 HelloSpringApplication가 있는 패키지 하위의 있는 파일들만 컨포넌트 스캔 가능(디폴트)

</aside>

> 스프링은 주로 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱클톤으로 등록한다.즉, 유일한 인스턴스를 공유한다.
> 
- `자바 코드로 직접 스프링 빈 등록`
    
    향후 메모리 리포지토리를 변경할 예정이므로 자바 코드로 스프링 빈을 설정하는 것이 편리
    
    ```java
    // main/java/hello/hellospring/controller/
    
    package hello.hellospring.controller;
    
    import hello.hellospring.service.MemberService;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.stereotype.Controller;
    
    @Controller
    public class MemberController {
        private final MemberService memberService;
    
        @Autowired
        public MemberController(MemberService memberService) {
            this.memberService = memberService;
        }
    }
    ```
    
    Controller에는 @Controller 어노테이션을 무조건 추가해줘야함
    
    ```java
    // main/java/hello/hellospring/
    // Application과 같은 위치
    
    package hello.hellospring;
    
    import hello.hellospring.repository.MemberRepository;
    import hello.hellospring.repository.MemoryMemberRepository;
    import hello.hellospring.service.MemberService;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    
    @Configuration
    public class SpringConfig {
    
        @Bean
        public MemberService memberService(){
            return new MemberService(memberRepository());
        }
    
        @Bean
        public MemberRepository memberRepository(){
            return new MemoryMemberRepository();
        }
    }
    ```
    
    1. @Configuration
        
        해당 클래스의 @Bean을 통해 스프링 빈을 직접 설정할 수 있게 함
        
    2. @Bean
        
        해당 클래스의 객체를 스프링 컨테이너에 넣어줌
        
    
    <aside>
    💡 **DI**
    
    Dependency Injection의 줄임말로 객체들 간의 의존 관계를 관리한다. 방법은 총 3가지가 있으며, 이 중 생성자 주입을 주로 사용한다.
    
    </aside>