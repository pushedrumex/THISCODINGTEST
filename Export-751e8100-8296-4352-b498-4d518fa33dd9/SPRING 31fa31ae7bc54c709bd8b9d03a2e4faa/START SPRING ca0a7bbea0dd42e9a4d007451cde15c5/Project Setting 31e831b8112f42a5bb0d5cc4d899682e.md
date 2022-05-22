# Project Setting

- `프로젝트 생성`
    - **start.spring.io**
        
        스프링 부트를 기반으로 스프링 프로젝트를 만들어주는 사이트
        
        - project
            
            필요한 라이브러리를 가져오고 빌드하는 라이프 사이클을 관리해주는 tool, 요즘은 다 Gradle사용
            
            1) Maven
            
            2) Gradle 
            
        - language
            
            Java, Kotlin, Groovy
            
        - Spring Boot 버전 선택
            
            ()가 없는 가장 최신 버전 선택
            
        - Project Metadata
            
            1) Group : 기업 도메인명
            
            2) Artifact : 빌드돼서 나오는 결과물( = 프로젝트명)
            
        - Dependencies
            
            스프링 프로젝트를 진행할건데, 어떤 라이브러리를 가져올건지
            
            1) spring web 
            
            2) Thymeleaf : HTML을 만들어주는 템필릿 엔진 중 하나
            
            HTML : 웹페이지를 만들기 위한 언어
            
        - Generate
    - **IntelliJ**
        
        확인 : http://localhost:8080
        
        아무것도 설정하지 않았다면 에러메세지
        
        open → build.gradle → open as project
        
        main → jave → 소스파일 → Run
        
        Run : SpringBoot Aplication이 실행되며, tomcat웹서버를 내장하고 있음
        
        - .idea :  인텔리제이가 사용하는 설정파일
        - gradle-Wrapper : gradle를 쓰는 폴더
        - src
            
            1) main
            
            java 
            
            실제 패키지와 소스파일
            
            resources
            
            실제 자바소스파일을 제외한 모든 파일(설정파일, HTML 등)
            
            2) test 
            
            test코드들과 관련된 소스들
            
        - build.gradle
            
            1) SourceCompatibility : 자바 버전
            
            2) dependencies
            
            implementation : Thymleaf, web
            
            testImplementation : test라이브러리
            
            3) respositories-mavenvecentral
            
            공개된 사이트로, 라이브러리를 다운받는 곳
            
        - .gitignore
            
            소스코드 관리, 필요한 것들만 업로드 될 수 있도록
            
- `라이브러리`
    
    Gradle이나 Maven tool은 내가 가져온 라이브러리와 외존관계가 있는 것들을 같이 다운로드
    
    → 내가 불러온건 Thymleaf, web인데 External libraries를 보면 다른 것들이 많음
    
    - Dependencies
        
         인텔리제이 왼쪽 밑에 네모네모 → 오른쪽 상단에 Gradle → Dependencies에서 의존관계 라이브러리 확인
        
        - **스프링 부트 라이브러리**
            
            스프링 관련된 것들이 다 셋팅되어서 돌아감
            
            1) spring-boot-stater-web
            
            spring-boot-starter-tomcat : 톰캣(웹서버)
            
            spring-webmvc : 스프링 웹 mvc
            
            2) spring-boot-starter-thymleaf : 타임리프 템플릿 엔진(View)
            
            3) spring-boot-starter(공통) : 스프링 부트 + 스프링 코어 + 로깅
            
            spring-boot
            
            spring-core
            
            spring-boot-starter-logging
            
            slf4j, logback
            
            > 실무에서는 로깅을 써야함, System.out.println X (기록을 남겨 에러발생시 대비)
            > 
        - **테스트 라이브러리**
            
            1) spring-boot-starter-test
            
            junit : 테스트 프레임워크
            
            mockito : 목 라이브러리
            
            assertj : 테스트 코드를 좀 더 편하게 작성하도록 도와주는 라이브러리
            
            spring-test : 스프링 통합 테스트 지원
            
- `view 환경설정`
    
    확인 : http://localhost:8080/hello
    
    - Welcome Page
        
        도메인만 들어왔을 때, 즉 홈화면. static/index.html을 올려두면 스프링 부트가 Welcome page기능을 제공, 정적페이지
        
        ```java
        // resources/static/index.html 
        
        <!DOCTYPE HTML>
          <html>
          <head>
              <title>Hello</title>
              <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
          </head>
          <body>
          Hello
          <a href="/hello">hello</a>
          </body>
          </html>
        ```
        
    - thymleaf 템플릿 엔진
        
        thymleaf를 활용하여 정적페이지가 아닌 프로그래밍을하여 페이지를 구성
        
        ```java
        // main/java/hello/hellospring/controller/
        
        package hello.hellospring.controller;
        
        import org.springframework.stereotype.Controller;
        import org.springframework.ui.Model;
        import org.springframework.web.bind.annotation.GetMapping;
        @Controller
        public class HelloController{
        
            @GetMapping("hello")
            public String hello(Model model){
                model.addAttribute("data","hello!!");
                return "hello";
            }
        }
        ```
        
        - @Controller
            
            스프링은 Controller 애노테이션 필수
            
        - model.addAttribute("data","hello!!")
            
            MVC의 Model 객체로 key : value = “data” : “hello!!” 쌍이 추가
            
        - return “hello”
            
            resources:templates/에서 hello라는 이름의 html을 찾아 Model객체(model)를 넘겨주며서 화면 실행 
            
        
        <aside>
        💡 MVC : Model View Controller, 소프트웨어 설계와 관련된 디자인 패턴
        
        </aside>
        
        ```java
        // resource/templates/hello.html
        
        <!DOCTYPE HTML>
        <html xmlns:th="http://www.thymeleaf.org">
        <head>
            <title>Hello</title>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        </head>
        <body>
        <p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
        </body>
        </html>
        ```
        
        - <html xmlns:th="http://www.thymeleaf.org">
            
            타임리프 템플릿 엔진 선언, th로 타임리프 문법 사용 가능
            
        - ${data}
            
            model.addAttribute의 model key ${data}에 hello!!가 들어감
            
    - 동작 과정 설명
        
        ![Untitled](Project%20Setting%2031e831b8112f42a5bb0d5cc4d899682e/Untitled.png)
        
        1. 웹 브라우저에서 [localhost:8080/hello](http://localhost:8080/hello) 톰킷웹서버(스프링 부트에 내장)들어감
        2. 톰켓 서버에서 /hello와 스프링 컨테이너의 @GetMapping(”hello”) 즉, url에 매칭이 됨
        3. 스프링이 Model객체인 model를 생성해서 hello메서드의 인자로 넘기고 hello메서드가 실행
        4. addAttribute메서드를 통해 key : value = “data” : “hello!!”가 추가됨
        5. hello메서드가 return “hello”를 하면 resources:templates/의 hello.html을 찾고 model을 넘겨주며 화면을 실행시킴
        
        <aside>
        💡 Controller에서 문자열을 반환하면 viewResolver가 해당 html을 찾아 화면을 처리                 스프링프트 템플릿엔진 기본 viewName 매핑
        
        → resources:templates/ + {viewName} + .html
        
        </aside>
        
- `빌드 후 실행`
    
    실제 실행할 수 있는 파일(.jar) 생성
    
    > 인텔리제이는 종료후 실행해야 오류가 발생하지 않음, 동시 실행 불가능
    > 
    1. 터미널에서 프로젝트 저장했던 디렉토리로 이동
    2. ./gradlew build 
        
        (실행 안된다면 clean build : build파일 지운 후 생성)
        
    3. cd build/libs
    4. java -jar hello-spring-0.0.1-SNAPSHOT.jar
    5. 웹브라우저로 동작 확인
    6. 종료 : ^c