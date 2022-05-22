# Basic Spring Web

서버 프로그래밍의 두가지 방식 : MVC(html), API(data)

- `정적 컨텐츠`
    
    resources:static/ 의 .html파일(화면)을 웹브라우저에 단순히 전달해주는 컨텐츠
    
    - 스프링 부트는 정적 컨텐츠를 기본으로 제공
    - 정적파일에는 어떠한 프로그래밍도 할 수 없음
    - 확인 url : localhost:8080/hello-static.html
    
    <aside>
    💡 url : 인터넷에서 어느 사이트에 접속하기 위해 입력해야하는 주소
    
    </aside>
    
    - 동작과정설명
        
        ![Untitled](Basic%20Spring%20Web%2071d98020bec84223b7db82dc962f8651/Untitled.png)
        
        1. 웹브라우저에 [localhost:8080/hello-static.html](http://localhost:8080/hello-static.html) 
        2. 내장 톰켓 서버가 요청을 받고 스프링으로 넘겨줌
        3. 스프링은 우선적으로 controller에 hello-static.html이 있는지 찾음
        4. 없다면 resources:static/에서 찾고 있다면 화면을 반환해줌
- `MVC와 템플릿 엔진`
    
    확인 : localhost:8080/hello-mvc?name=spring
    
    - 템플릿 엔진
        
        서버에서 html을 동적으로 바꿔서 내리는 것
        
    - MVC
        
        html을 동적으로 바꾸기 위한 Model View Controller
        
    
    <aside>
    💡 과거에는 jsp로 View에서 Controller도 했었지만 지금은 mvc방식으로 진행함
    
    </aside>
    
    1) Model, Controller
    
    비지니스 로직과 관련되어있거나 내부적인거를 처리하는데에 집중
    
    ```java
    // main/java/hello/hellospring/controller/
    
    package hello.hellospring.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    
    @Controller
    public class HelloController{
    
        @GetMapping("hello")
        public String hello(Model model){
            model.addAttribute("data","hello!!");
            return "hello";
        }
    
    		@GetMapping("hello-mvc")
        public String helloMvc(@RequestParam("name") String name, Model model){
            model.addAttribute("name",name);
            return "hello-template";
        }
    }
    ```
    
    1. @GetMapping("hello-mvc")
        
        hello-mvc url을 받으면 하위 메소드 helloMvc 실행
        
    2. @RequestParam("name") String name
        
        웹에서 인자를 입력받아 name에 전달, “name”은 url의 name으로 model 의 key와 무관
        
        → http get 방식 : /hello-mvc?name=spring
        
    
    <aside>
    💡 파라미터 정보 보는 법 : cmd + p
    
    </aside>
    
    2) View
    
    화면을 그리는데에 집중 (.html)
    
    ```java
    // resource/template/hello-template.html
    
    <html xmlns:th="http://www.thymeleaf.org">
      <body>
      <p th:text="'hello ' + ${name}">hello! empty</p>
      </body>
    ```
    
    1. ${name}
        
        model의 key “name”에 해당하는 value “spring”이 전달
        
    2. hello! empty
        
        html파일을 서버 없이 열어서 틀을 볼 수 있고 템플릿 엔진으로서 동작을 하면 hello! empty부분이 "'hello ' + ${name}"로 치환. 서버없이 html을 볼때 사용
        
        < .html우클릭 copy path → Absolute path → 웹 브라우저 >
        
    - 동작과정설명
        
        ![Untitled](Basic%20Spring%20Web%2071d98020bec84223b7db82dc962f8651/Untitled%201.png)
        
        1. [localhost:8080/hello-mvc를](http://localhost:8080/hello-mvc를) 톰켓서버를 통해 스프링으로 던짐
        2. controller에 Mapping이 되어있는걸 찾아서 해당 매서드 호출
        3. model(name:spring)을 보내면서 hello-template.html 실행
        4. HTML 변환 : 정적파일은 변환을 안하는 반면에 동적 html은 변환을 해줌
        
        <aside>
        💡 viewResolver : 화면과 관련된 해결자로 view를 찾아주고 템플릿 엔진과 연결시켜줌
        
        </aside>
        
- `API`
    
    제이슨이라는 데이터 포멧으로 클라이언트한테 데이터를 직접 전달하는 것으로 view(html)없이 문자가 그대로 전달
    
    확인 : localhost:8080/hello-string?name=spring
    
    ```java
    package hello.hellospring.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.ResponseBody;
    
    @Controller
    public class HelloController{
    
        @GetMapping("hello")
        public String hello(Model model){
            model.addAttribute("data","hello!!");
            return "hello";
        }
    
        @GetMapping("hello-mvc")
        public String helloMvc(@RequestParam("name") String name, Model model){
            model.addAttribute("name",name);
            return "hello-template";
        }
    
        @GetMapping("hello-string")
        @ResponseBody
        public String helloString(@RequestParam("name") String name){
            return "hello " + name;
        }
    
        @GetMapping("hello-api")
        @ResponseBody
        public Hello helloApi(@RequestParam("name") String name){
            Hello hello = new Hello();
            hello.setName(name);
            return hello;
        }
    
        static class Hello {
            private String name;
    
            public String getName() {
                return name;
            }
    
            public void setName(String name) {
                this.name = name;
            }
        }
    
    }
    ```
    
    < helloString >
    
    1. @ResponseBody
        
        http의 Body에 data를 직접 반환하며 viewResolver사용안함
        
    2. return "hello " + name
        
        화면에 hello spring이 그대로 나타남
        
    
    < hello-api > Json방식
    
    1. static class Hello
        
         정적내부클래스로 외부클래스 생성과 무관하게 내부클래스 생성이 가능
        
    2. private String name
        
        “name”이 key가 됨
        
    3. setName
        
        key “name”의 value에 입력받은 인자 name 전달
        
    4. return hello
        
        Hello의 객체인 hello 반환, 객체를 반환하고 @ResponseBody를 하면 Json으로 반환하는게 default
        
    
    <aside>
    💡 Json : {key : value}로 이루어진 구조
    
    </aside>
    
    - 동작과정설명
        
        ![Untitled](Basic%20Spring%20Web%2071d98020bec84223b7db82dc962f8651/Untitled%202.png)
        
        1. [localhost:8080/hello-api가](http://localhost:8080/hello-api가) 톰켓서버로 전달되고 스프링에서 찾음
        2. 찾았더니 @ResponseBody가 되어있음 → http 응답에 그대로 데이터를 넣는 방식으로 진행
        3. HttpMessageConverter 동작
            - 기본 문자 처리 : StringHttpMessageConverter
                
                문자 그대로를 http응답에 바로 전달
                
            - 기본 객체 처리 : MappingJackson2HttpMessageConvertser(default)
                
                key:value 형태의 데이터를 만들어서 http 응답에 전달
                
                (Jackson : 객체를 Json으로 바꿔주는 라이브러리, 디폴트)
                
            - 기타 포멧 : HttpMessageConverter에 등록되어 있음