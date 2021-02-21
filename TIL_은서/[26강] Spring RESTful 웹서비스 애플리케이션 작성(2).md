# Ajax 개요

#### Ajax(Asynchronous Javascript and XML)란?

* 웹 사용자들에게 좀 더 수준 높은 인터페이스를 제공할 수 있도록 도움을 주는 **기술의 묶음**

* **Ajax 자체는 특정한 기술이 아님**

  HTML, CSS, Javascript, XML, XMLHttpRequest 객체를 비롯한 여러 기술들을 조합해서 사용하는 웹의 새로운 접근법

* **비동기적(Asynchronous) : 서버로부터 데이터가 로드 되는 동안에도 계속해서 페이지를 사용할 수 있다는 뜻**
* 서버가 데이터를 전달해주면 Ajax 이벤트가 발생하고, 서버로부터 받은 데이터를 읽어 페이지의 일부를 수정하게 되는 것임



#### Ajax 활용 예시

1. 라이브 검색(자동완성: 검색어를 입력하는 동시에 검색 결과 나타나도록)
2. 사용자 정보 표시(회원가입시 아이디 입력과 동시에 중복아이디일 경우 경고문, 전체 페이지의 새로고침 없이 일부만 바뀌는 화면기능 구현시 사용)



#### XMLHttpRequest(XHR) 객체 사용법 (jquery 없이 javascript로만!)

* XHR객체는 자바스크립트 객체

1. XMLHttpRequest 객체 생성 : Request를 보낼 준비

   `var xhr = new XMLHttpRequest();`

2. Callback 함수 만들기 : 서버에서 Response가 왔을 때 실행되는 함수

   ```javascript
   xhr.onreadystatechange = function() {
       // state값이 4 = 응답이 다 처리되서 온 상태
   	if(xhr.readyState == 4) {
           var myDiv = document.getElementById('myDiv');
           myDiv.innerHTML = xhr.responseText;
       }
   }
   ```

3. Request를 Open한다 : HTTP method와 호출할 Server의 url 정보 전달

   `xhr.open("GET", "user.do");`

4. Request를 Send한다 : 요청을 서버로 보냄

   `xhr.send();`



##### ➡ 일반 Javascript로 Ajax 코드를 작성하게 되면 코딩량도 많아지고,

##### 	브라우저별로 구현을 다르게 해주어야 하는 단점이 있다(크로스 브라우징 불가)

##### 🚨 jQuery를 이용하면 더 적은 코드와 함께 효울적으로 구현할 수 있다

 



# jQuery 개요

* JavaScript를 좀 더 쉽게 사용하도록 만들어진 가벼운 **라이브러리**



#### jQuery 라이브러리 준비

방법 1. CDN(Content Delivery Network) 호스트 사용 ➡ 인터넷 연결 필요

방법 2. jQuery 공식 홈페이지에서 직접 파일을 다운로드하여 로컬에 저장후 사용



#### jQuery가 제공하는 기능

1. HTML 엘리먼트 selector
2. HTML 엘리먼트의 attribute 값 읽기, 쓰기
3. HTML 엘리먼트 동적 조작
4. Loop
5. CSS 조작
6. Event 처리
7. Ajax 처리



#### jQuery 시작하기

1. jQuery를 사용하는 모든 웹 페이지는 $(document).ready()로 시작해야 한다

   ````javascript
   <script>
      $(document).ready(function() {
      	//페이지 로드 시 해야 할 일
   });
   </script>
   ````

   또는 축약시켜서

   ```javascript
   <script>
      $(function() {
      	//페이지 로드 시 해야 할 일
   });
   </script>
   ```

   



# jQuery가 제공하는 기능

#### Selector

* `$("div");` - 태그명 직접입력하여 선택
* `$(".class");` - 클래스명으로 선택
* `$("#id");` - id명으로 선택
* `$("*").css("border", "1px");` - CSS 적용
* `$("div, span, p.myClass");` - 다중 셀렉터
* `$("div.gt(2)");` - div 요소 중 2보다 큰(이후) 번째의 요소 선택
* `$(" :checkbox");` - 모든 checkbox 요소 선택



#### Attribute 값 읽기, 쓰기

* attribute 값 설정

  `$("#myImage").attr("src", "myimg.jpg");`

* attribute 값이 여러개인 경우

  `$("#myImage").attr({src: "myimg.jpg", alt: "my image"});`



#### Manipulation

* append() 함수

  : 선택된 element의 요소 맨 끝에 인자로 넘어온 내용을 추가

  `$(".inner").append("<p>Hello</p>");` - inner 클래스 요소 맨 끝에 \<p> 태그 요소를 추가

* appendTo() 함수

  : 선택된 element를 **target**에 해당하는 element content 끝에 추가

  `$("<p>Hello</p>").appendTo(".inner");` - \<p>태그를 inner 클래스 요소 맨 끝에 추가한다

* html(), html(htmlString) 함수

  : 선택된 Element의 html을 리턴, 설정

  `$("div.demo-container").html();`

  `$("div").html("<h1>제목</h1>");`



#### Loop

* each(function(index, element)) 함수

  ```html
  <ul>
  	<li>foo</li>
  	<li>bar</li>
  </ul>
  ```

  ```javascript
  $("li").each(function(index, element) {
     alert(index + ": " + $(element).text()); 
  });
  ```

  * element ➡ li



#### CSS 조작

* css(propertyName), css(propertyName, value), css(object) 함수

  : 선택된 element의 propertyName에 해당되는 CSS 정보 리턴, 설정

* addClass(className) 함수

  : 선택된 element에 해당되는 클래스를 추가

  `$("p").addClass("myClass");`



#### Event 처리

* on() 함수

  : 선택된 element에 이벤트 핸들러를 묶어준다

  `$("p").on("click", function() { alert($(this).text()); });`

* 이벤트 종류

  * mouseenter : hover
  * mouseleave



#### Ajax 처리

* ajax() 함수

  ````javaScript
  $('#btnSelect').on('click', function() {
      $.ajax({
          url: 'users',
          type: 'GET',
          contentType: 'application/json; charset=utf-8',
          dataType: 'json',
          error:function(xhr,status,msg){
              alert("상태값: " + status + "Http 에러 메시지: " + msg);
          },
          success:userSelectResult
      });	
  });
  ````

  

