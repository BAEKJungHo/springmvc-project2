# Inflearn 스프링 MVC 2편 - 백엔드 웹 개발 활용 기술

## 타임리프

- 타임리프 특징
  - 서버 사이드 HTML 렌더링(SSR)
  - 네츄럴 템플릿
  - 스프링 통합 지원

> 서버 사이드 HTML 렌더링 (SSR)
> 
> 타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용된다.
>
> 네츄럴 템플릿
>
> 타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.
타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할
수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다. 
JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어서 JSP 파일 자체를 그대로 웹 브라우저에서
열어보면 JSP 소스코드와 HTML이 뒤죽박죽 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수
없다. 오직 서버를 통해서 JSP가 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다.
> 반면에 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를
확인할 수 있다. 물론 이 경우 동적으로 결과가 렌더링 되지는 않는다. 하지만 HTML 마크업 결과가 어떻게
되는지 파일만 열어도 바로 확인할 수 있다.
이렇게 `순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿(natural templates)`이라 한다.
>
> 스프링 통합 지원
>
> 타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다.

#### 타임리프 사용 선언

`<html xmlns:th="http://www.thymeleaf.org">`

#### 기본 표현식

- 간단한 표현:
  - 변수 표현식: ${...}
  - 선택 변수 표현식: *{...}
  - 메시지 표현식: #{...}
  - 링크 URL 표현식: @{...}
  - 조각 표현식: ~{...}
- 리터럴
  - 텍스트: 'one text', 'Another one!',…
  - 숫자: 0, 34, 3.0, 12.3,…
  - 불린: true, false
  - 널: null
  - 리터럴 토큰: one, sometext, main,…
- 문자 연산:
  - 문자 합치기: +
  - 리터럴 대체: |The name is ${name}|
- 산술 연산:
  - Binary operators: +, -, *, /, %
  - Minus sign (unary operator): -
- 불린 연산:
  - Binary operators: and, or
  - Boolean negation (unary operator): !, not
- 비교와 동등:
  - 비교: >, <, >=, <= (gt, lt, ge, le)
  - 동등 연산: ==, != (eq, ne)
- 조건 연산:
  - If-then: (if) ? (then)
  - If-then-else: (if) ? (then) : (else)
  - Default: (value) ?: (defaultvalue)
- 특별한 토큰:
  - No-Operation: _

> https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#standard-expression-syntax

### 텍스트 - text, utext

- `<span th:text="${data}">이곳에 data 출력</span>`
- 컨텐츠 안에서 직접 출력 : `[[${data}]]`

#### Escape

HTML 문서는 `< , >` 같은 특수 문자를 기반으로 정의된다. 따라서 뷰 템플릿으로 HTML 화면을 생성할
때는 출력하는 데이터에 이러한 특수 문자가 있는 것을 주의해서 사용해야 한다.
앞에서 만든 예제의 데이터를 다음과 같이 변경해서 실행해보자.

#### HTML 엔티티

웹 브라우저는 `<` 를 HTML 태그의 시작으로 인식한다. 따라서 `<` 를 태그의 시작이 아니라 문자로 표현할 수
있는 방법이 필요한데, 이것을 `HTML 엔티티`라 한다. 그리고 이렇게 HTML에서 사용하는 특수 문자를
HTML 엔티티로 변경하는 것을 `이스케이프(escape)`라 한다. 그리고 타임리프가 제공하는 th:text ,
`[[...]]` 는 기본적으로 `이스케이스(escape)`를 제공한다.

- `< &lt;`
- `> &gt;`
- 기타 수 많은 HTML 엔티티가 있다.

#### Unescape

이스케이프 기능을 사용하지 않기 위해서 아래와 같이 하면 된다.

- `th:text > th:utext`
- `[[...]] > [(...)]`

### Spring EL

타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다.

변수 표현식 : `${...}`

그리고 이 변수 표현식에는 스프링 EL이라는 스프링이 제공하는 표현식을 사용할 수 있다.

- SpringEL 다양한 표현식 사용
  - Object
    - `user.username` : user의 username을 프로퍼티 접근 user.getUsername()
    - `user['username']` : 위와 같음 user.getUsername()
    - `user.getUsername()` : user의 getUsername() 을 직접 호출
  - List
    - `users[0].username` : List에서 첫 번째 회원을 찾고 username 프로퍼티 접근
    - `list.get(0).getUsername()`
    - `users[0]['username']` : 위와 같음
    - `users[0].getUsername()` : List에서 첫 번째 회원을 찾고 메서드 직접 호출
  - Map
    - `userMap['userA'].username` : Map에서 userA를 찾고, username 프로퍼티 접근
    - `map.get("userA").getUsername()`
    - `userMap['userA']['username']` : 위와 같음
    - `userMap['userA'].getUsername()` : Map에서 userA를 찾고 메서드 직접 호출

#### 지역 변수 선언

`th:with` 를 사용하면 지역 변수를 선언해서 사용할 수 있다. 지역 변수는 선언한 테그 안에서만 사용할 수
있다.

```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${users[0]}">
 <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```

### 기본 객체들

타임리프는 기본 객체들을 제공한다.

- ${#request}
- ${#response}
- ${#session}
- ${#servletContext}
- ${#locale}
 
그런데 #request 는 HttpServletRequest 객체가 그대로 제공되기 때문에 데이터를 조회하려면
request.getParameter("data") 처럼 불편하게 접근해야 한다.

이런 점을 해결하기 위해 편의 객체도 제공한다.

- HTTP 요청 파라미터 접근: param
  - Ex. ${param.paramData}
- HTTP 세션 접근: session
  - Ex. ${session.sessionData}
- 스프링 빈 접근: @
  - Ex. ${@helloBean.hello('Spring!')}

### 유틸리티 객체와 날짜

타임리프는 문자, 숫자, 날짜, URI등을 편리하게 다루는 다양한 유틸리티 객체들을 제공한다.

- 타임리프 유틸리티 객체들
  - #message : 메시지, 국제화 처리
  - #uris : URI 이스케이프 지원
  - #dates : java.util.Date 서식 지원
  - #calendars : java.util.Calendar 서식 지원
  - #temporals : 자바8 날짜 서식 지원
  - #numbers : 숫자 서식 지원
  - #strings : 문자 관련 편의 기능
  - #objects : 객체 관련 기능 제공
  - #bools : boolean 관련 기능 제공
  - #arrays : 배열 관련 기능 제공
  - #lists , #sets , #maps : 컬렉션 관련 기능 제공
  - #ids : 아이디 처리 관련 기능 제공, 뒤에서 설명

- 타임리프 유틸리티 객체
  - https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects
- 유틸리티 객체 예시
  - https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expression-utility-objects

> 이런 유틸리티 객체들은 대략 이런 것이 있다 알아두고, 필요할 때 찾아서 사용하면 된다.


#### 자바8 날짜

타임리프에서 자바8 날짜인 LocalDate , LocalDateTime , Instant 를 사용하려면 추가 라이브러리가
필요하다. 스프링 부트 타임리프를 사용하면 해당 라이브러리가 자동으로 추가되고 통합된다.

- 타임리프 자바8 날짜 지원 라이브러리
  - `thymeleaf-extras-java8time`
- 자바8 날짜용 유틸리티 객체
  - `#temporals`

```html
<span th:text="${#temporals.format(localDateTime, 'yyyy-MM-dd HH:mm:ss')}"></span>
```

### URL 링크

타임리프에서 URL을 생성할 때는 `@{...}` 문법을 사용하면 된다.

- 단순한 URL
  - @{/hello} /hello
- 쿼리 파라미터
  - @{/hello(param1=${param1}, param2=${param2})}
  - /hello?param1=data1&param2=data2
  - () 에 있는 부분은 쿼리 파라미터로 처리된다.
- 경로 변수
  - @{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}
  - /hello/data1/data2
  - URL 경로상에 변수가 있으면 () 부분은 경로 변수로 처리된다.
- 경로 변수 + 쿼리 파라미터
  - @{/hello/{param1}(param1=${param1}, param2=${param2})}
  - /hello/data1?param2=data2
  - 경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.

상대경로, 절대경로, 프로토콜 기준을 표현할 수 도 있다.

- /hello : 절대 경로
- hello : 상대 경로

> 참고: https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#link-urls

### 리터럴(Literals)

리터럴은 소스 코드상에 고정된 값을 말하는 용어이다.

예를 들어서 다음 코드에서 "Hello" 는 문자 리터럴, 10, 20 는 숫자 리터럴이다.

```
String a = "Hello"
int a = 10 * 20
```

타임리프는 다음과 같은 리터럴이 있다.

- 문자: 'hello'
- 숫자: 10
- 불린: true , false
- null: null

타임리프에서 문자 리터럴은 항상 `'' (작은 따옴표)`로 감싸야 한다.

`<span th:text="'hello'">`

그런데 문자를 항상 ' 로 감싸는 것은 너무 귀찮은 일이다. 공백 없이 쭉 이어진다면 하나의 의미있는
토큰으로 인지해서 다음과 같이 작은 따옴표를 생략할 수 있다. 

`룰: A-Z , a-z , 0-9 , [] , . , - , _`

`<span th:text="hello">`

- 오류
  - `<span th:text="hello world!"></span>`
  - 문자 리터럴은 원칙상 ' 로 감싸야 한다. 중간에 공백이 있어서 하나의 의미있는 토큰으로도 인식되지 않는다.
- 수정
  - `<span th:text="'hello world!'"></span>`
  - 이렇게 ' 로 감싸면 정상 동작한다.

#### 리터럴 대체(Literal substitutions)

`<span th:text="|hello ${data}|">`

마지막의 리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것 처럼 편리하다.

### 연산

타임리프 연산은 자바와 크게 다르지 않다. HTML안에서 사용하기 때문에 HTML 엔티티를 사용하는 부분만 주의하자.


- 비교연산: HTML 엔티티를 사용해야 하는 부분을 주의하자, 
  - `> (gt), < (lt), >= (ge), <= (le), ! (not), == (eq), != (neq, ne)`
- 조건식: 자바의 조건식과 유사하다.
- Elvis 연산자: 조건식의 편의 버전
- No-Operation: `_` 인 경우 마치 타임리프가 실행되지 않는 것 처럼 동작한다. 이것을 잘 사용하면 HTML 의 내용 그대로 활용할 수 있다. 마지막 예를 보면 데이터가 없습니다. 부분이 그대로 출력된다.

### 속성 값 설정

#### 타임리프 태그 속성(Attribute)

타임리프는 주로 HTML 태그에 `th:*` 속성을 지정하는 방식으로 동작한다. `th:*` 로 속성을 적용하면 기존
속성을 대체한다. 기존 속성이 없으면 새로 만든다.

- 속성 설정
 - `th:*` 속성을 지정하면 타임리프는 기존 속성을 `th:*` 로 지정한 속성으로 대체한다. 기존 속성이 없다면 새로 만든다.
  - `<input type="text" name="mock" th:name="userA" />`
    - `타임리프 렌더링 후 <input type="text" name="userA" />`
- 속성 추가
  - th:attrappend : 속성 값의 뒤에 값을 추가한다.
  - th:attrprepend : 속성 값의 앞에 값을 추가한다.
  - th:classappend : class 속성에 자연스럽게 추가한다.
- checked 처리
  - HTML에서는 `<input type="checkbox" name="active" checked="false" />` 이 경우에도 checked 속성이 있기 때문에 checked 처리가 되어버린다.
  - HTML에서 checked 속성은 checked 속성의 값과 상관없이 checked 라는 속성만 있어도 체크가 된다. 
  - 이런 부분이 true , false 값을 주로 사용하는 개발자 입장에서는 불편하다.
  - 타임리프의 th:checked 는 값이 false 인 경우 checked 속성 자체를 제거한다.
  - `<input type="checkbox" name="active" th:checked="false" />`
    - 타임리프 렌더링 후: `<input type="checkbox" name="active" />`

### 반복

타임리프에서 반복은 `th:each` 를 사용한다. 추가로 반복에서 사용할 수 있는 여러 상태 값을 지원한다.

- 반복 기능
  - `<tr th:each="user : ${users}">`
  - 반복시 오른쪽 컬렉션( ${users} )의 값을 하나씩 꺼내서 왼쪽 변수( user )에 담아서 태그를 반복 실행합니다.
  - th:each 는 List 뿐만 아니라 배열, java.util.Iterable , java.util.Enumeration 을 구현한 모든
객체를 반복에 사용할 수 있습니다. Map 도 사용할 수 있는데 이 경우 변수에 담기는 값은 Map.Entry
입니다.

- 반복 상태 유지
  - `<tr th:each="user, userStat : ${users}">`
  - 반복의 두번째 파라미터를 설정해서 반복의 상태를 확인 할 수 있습니다.
두번째 파라미터는 생략 가능한데, 생략하면 지정한 변수명( user ) + Stat 가 됩니다.
여기서는 user + Stat = userStat 이므로 생략 가능합니다.

- 반복 상태 유지 기능
  - index : 0부터 시작하는 값
  - count : 1부터 시작하는 값
  - size : 전체 사이즈
  - even, odd : 홀수, 짝수 여부(boolean)
  - first , last :처음, 마지막 여부(boolean)
  - current : 현재 객체

### 조건부 평가

타임리프의 조건식

`if , unless(if 의 반대)`

- if, unless
  - 타임리프는 해당 조건이 맞지 않으면 태그 자체를 렌더링하지 않는다.
  - 만약 다음 조건이 false 인 경우 `<span>...<span>` 부분 자체가 렌더링 되지 않고 사라진다.
  - `<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>`
- switch
  - `*` 은 만족하는 조건이 없을 때 사용하는 디폴트이다.

### 주석

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
 <meta charset="UTF-8">
 <title>Title</title>
</head>
<body>
<h1>예시</h1>
<span th:text="${data}">html data</span>
<h1>1. 표준 HTML 주석</h1>
<!--
<span th:text="${data}">html data</span>
-->
<h1>2. 타임리프 파서 주석</h1>
<!--/* [[${data}]] */-->
<!--/*-->
<span th:text="${data}">html data</span>
<!--*/-->
<h1>3. 타임리프 프로토타입 주석</h1>
<!--/*/
<span th:text="${data}">html data</span>
/*/-->
</body>
</html>
```

- 결과

```html
<h1>예시</h1>
<span>Spring!</span>
<h1>1. 표준 HTML 주석</h1>
<!--
<span th:text="${data}">html data</span>
-->
<h1>2. 타임리프 파서 주석</h1>
<h1>3. 타임리프 프로토타입 주석</h1>
<span>Spring!</span>
```

1. 표준 HTML 주석
자바스크립트의 표준 HTML 주석은 타임리프가 렌더링 하지 않고, 그대로 남겨둔다.

2. 타임리프 파서 주석
타임리프 파서 주석은 타임리프의 진짜 주석이다. 렌더링에서 주석 부분을 제거한다.

3. 타임리프 프로토타입 주석
타임리프 프로토타입은 약간 특이한데, HTML 주석에 약간의 구문을 더했다.
HTML 파일을 웹 브라우저에서 그대로 열어보면 HTML 주석이기 때문에 이 부분이 웹 브라우저가
렌더링하지 않는다.
타임리프 렌더링을 거치면 이 부분이 정상 렌더링 된다.
쉽게 이야기해서 HTML 파일을 그대로 열어보면 주석처리가 되지만, 타임리프를 렌더링 한 경우에만
보이는 기능이다.

### 블록

`<th:block>` 은 HTML 태그가 아닌 타임리프의 유일한 자체 태그다.

### 자바스크립트 인라인

타임리프는 자바스크립트에서 타임리프를 편리하게 사용할 수 있는 자바스크립트 인라인 기능을 제공한다.
자바스크립트 인라인 기능은 다음과 같이 적용하면 된다.

`<script th:inline="javascript">`

자바스크립트 인라인을 사용하지 않은 경우 어떤 문제들이 있는지 알아보고, 인라인을 사용하면 해당
문제들이 어떻게 해결되는지 확인해보자.

- 텍스트 렌더링
  - var username = `[[${user.username}]];`
    - 인라인 사용 전 var username = userA;
    - 인라인 사용 후 var username = "userA";
- 인라인 사용 전 렌더링 결과를 보면 userA 라는 변수 이름이 그대로 남아있다. 타임리프 입장에서는
정확하게 렌더링 한 것이지만 아마 개발자가 기대한 것은 다음과 같은 "userA"라는 문자일 것이다. 
결과적으로 userA가 변수명으로 사용되어서 자바스크립트 오류가 발생한다. 다음으로 나오는 숫자 age의
경우에는 " 가 필요 없기 때문에 정상 렌더링 된다.
- 인라인 사용 후 렌더링 결과를 보면 문자 타입인 경우 " 를 포함해준다. 추가로 자바스크립트에서 문제가 될
수 있는 문자가 포함되어 있으면 이스케이프 처리도 해준다. 예) " \"

- 자바스크립트 내추럴 템플릿
  - 타임리프는 HTML 파일을 직접 열어도 동작하는 내추럴 템플릿 기능을 제공한다. 자바스크립트 인라인
기능을 사용하면 주석을 활용해서 이 기능을 사용할 수 있다.
  - var username2 = `/*[[${user.username}]]*/ "test username";`
    - 인라인 사용 전 `var username2 = /*userA*/ "test username";`
    - 인라인 사용 후 `var username2 = "userA";`
- 인라인 사용 전 결과를 보면 정말 순수하게 그대로 해석을 해버렸다. 따라서 내추럴 템플릿 기능이 동작하지
않고, 심지어 렌더링 내용이 주석처리 되어 버린다.
- 인라인 사용 후 결과를 보면 주석 부분이 제거되고, 기대한 "userA"가 정확하게 적용된다.

- 객체
  - 타임리프의 자바스크립트 인라인 기능을 사용하면 객체를 JSON으로 자동으로 변환해준다.
  - `var user = [[${user}]];`
    - 인라인 사용 전 var user = BasicController.User(username=userA, age=10);
    - 인라인 사용 후 var user = {"username":"userA","age":10};
- 인라인 사용 전은 객체의 toString()이 호출된 값이다.
- 인라인 사용 후는 객체를 JSON으로 변환해준다.

### 자바스크립트 인라인 each

```html
<!-- 자바스크립트 인라인 each -->
<script th:inline="javascript">
 [# th:each="user, stat : ${users}"]
 var user[[${stat.count}]] = [[${user}]];
 [/]
</script>
```

- 결과

```html
<script>
var user1 = {"username":"userA","age":10};
var user2 = {"username":"userB","age":20};
var user3 = {"username":"userC","age":30};
</script>
```

### 템플릿 조각

웹 페이지를 개발할 때는 공통 영역이 많이 있다. 예를 들어서 상단 영역이나 하단 영역, 좌측 카테고리 등등
여러 페이지에서 함께 사용하는 영역들이 있다. 이런 부분을 코드를 복사해서 사용한다면 변경시 여러
페이지를 다 수정해야 하므로 상당히 비효율 적이다. 타임리프는 이런 문제를 해결하기 위해 템플릿 조각과
레이아웃 기능을 지원한다.

__주의! 템플릿 조각과 레이아웃 부분은 직접 실행해보아야 이해된다.__

- template/fragment/footer :: copy : template/fragment/footer.html 템플릿에 있는
th:fragment="copy" 라는 부분을 템플릿 조각으로 가져와서 사용한다는 의미이다.

- footer.html 의 copy 부분

```html
<footer th:fragment="copy">
 푸터 자리 입니다.
</footer>
```

- 부분 포함 insert

`<div th:insert="~{template/fragment/footer :: copy}"></div>`

```html
<h2>부분 포함 insert</h2>
<div>
<footer>
푸터 자리 입니다.
</footer>
</div>
```

th:insert 를 사용하면 현재 태그( div ) 내부에 추가한다.

- 부분 포함 replace

`<div th:replace="~{template/fragment/footer :: copy}"></div>`

- 부분 포함 단순 표현식

`<div th:replace="template/fragment/footer :: copy"></div>`

`~{...}` 를 사용하는 것이 원칙이지만 템플릿 조각을 사용하는 코드가 단순하면 이 부분을 생략할 수 있다.

- 파라미터 사용

다음과 같이 파라미터를 전달해서 동적으로 조각을 렌더링 할 수도 있다.

`<div th:replace="~{template/fragment/footer :: copyParam ('데이터1', '데이터2')}"></div>`

```html
<footer th:fragment="copyParam (param1, param2)">
 <p>파라미터 자리 입니다.</p>
 <p th:text="${param1}"></p>
 <p th:text="${param2}"></p>
</footer>
```

### 템플릿 레이아웃

템플릿 레이아웃
이전에는 일부 코드 조각을 가지고와서 사용했다면, 이번에는 개념을 더 확장해서 코드 조각을 레이아웃에
넘겨서 사용하는 방법에 대해서 알아보자.
예를 들어서 `<head>` 에 공통으로 사용하는 css , javascript 같은 정보들이 있는데, 이러한 공통
정보들을 한 곳에 모아두고, 공통으로 사용하지만, 각 페이지마다 필요한 정보를 더 추가해서 사용하고
싶다면 다음과 같이 사용하면 된다.

`common_header(~{::title},~{::link})` 이 부분이 핵심이다.

- ::title 은 현재 페이지의 title 태그들을 전달한다.
- ::link 는 현재 페이지의 link 태그들을 전달한다.

결과를 보자.

- 메인 타이틀이 전달한 부분으로 교체되었다.
- 공통 부분은 그대로 유지되고, 추가 부분에 전달한 <link> 들이 포함된 것을 확인할 수 있다.
- 이 방식은 사실 앞서 배운 코드 조각을 조금 더 적극적으로 사용하는 방식이다. 쉽게 이야기해서 레이아웃
개념을 두고, 그 레이아웃에 필요한 코드 조각을 전달해서 완성하는 것으로 이해하면 된다.

앞서 이야기한 개념을 `<head>` 정도에만 적용하는게 아니라 `<html>` 전체에 적용할 수도 있다.

layoutFile.html 을 보면 기본 레이아웃을 가지고 있는데, `<html>` 에 th:fragment 속성이 정의되어
있다. 이 레이아웃 파일을 기본으로 하고 여기에 필요한 내용을 전달해서 부분부분 변경하는 것으로
이해하면 된다.

layoutExtendMain.html 는 현재 페이지인데, `<html>` 자체를 th:replace 를 사용해서 변경하는 것을
확인할 수 있다. 결국 layoutFile.html 에 필요한 내용을 전달하면서 `<html>` 자체를
layoutFile.html 로 변경한다.

## 타임리프 스프링 통합

타임리프는 크게 2가지 메뉴얼을 제공한다.

- 기본 메뉴얼: https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html
- 스프링 통합 메뉴얼: https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html

타임리프는 스프링 없이도 동작하지만, 스프링과 통합을 위한 다양한 기능을 편리하게 제공한다. 그리고
이런 부분은 스프링으로 백엔드를 개발하는 개발자 입장에서 타임리프를 선택하는 하나의 이유가 된다.

### 스프링 통합으로 추가되는 기능들

- 스프링의 SpringEL 문법 통합
- `${@myBean.doSomething()}` 처럼 스프링 빈 호출 지원
- 편리한 폼 관리를 위한 추가 속성
  - th:object (기능 강화, 폼 커맨드 객체 선택)
  - th:field , th:errors , th:errorclass
- 폼 컴포넌트 기능
  - checkbox, radio button, List 등을 편리하게 사용할 수 있는 기능 지원
- 스프링의 메시지, 국제화 기능의 편리한 통합
- 스프링의 검증, 오류 처리 통합
- 스프링의 변환 서비스 통합(ConversionService)

### 설정 방법

타임리프 템플릿 엔진을 스프링 빈에 등록하고, 타임리프용 뷰 리졸버를 스프링 빈으로 등록하는 방법

- https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#the-springstandard-dialect
- https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#views-and-view-resolver

스프링 부트는 이런 부분을 모두 자동화 해준다. build.gradle 에 다음 한줄을 넣어주면 Gradle은
타임리프와 관련된 라이브러리를 다운로드 받고, 스프링 부트는 앞서 설명한 타임리프와 관련된 설정용
스프링 빈을 자동으로 등록해준다.

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf
```

타임리프 관련 설정을 변경하고 싶으면 다음을 참고해서 application.properties 에 추가하면 된다.
스프링 부트가 제공하는 타임리프 설정, thymeleaf 검색 필요

https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties-templating

### 입력 폼 처리

- th:object : 커맨드 객체를 지정한다.
- `*{...}` : 선택 변수 식이라고 한다. th:object 에서 선택한 객체에 접근한다.
- th:field
  - HTML 태그의 id , name , value 속성을 자동으로 처리해준다.

- 렌더링 전
  - `<input type="text" th:field="*{itemName}" />`
- 렌더링 후
  - `<input type="text" id="itemName" name="itemName" th:value="*{itemName}" />`

- `th:object="${item}"` : `<form>` 에서 사용할 객체를 지정한다. 선택 변수 식( `*{...}` )을 적용할 수 있다.
- `th:field="*{itemName}"`
  - `*{itemName}` 는 선택 변수 식을 사용했는데, `${item.itemName}` 과 같다. 앞서 th:object 로 item 을 선택했기 때문에 선택 변수 식을 적용할 수 있다.
  - th:field 는 id , name , value 속성을 모두 자동으로 만들어준다.
    - id : th:field 에서 지정한 변수 이름과 같다. id="itemName"
    - name : th:field 에서 지정한 변수 이름과 같다. name="itemName"
    - value : th:field 에서 지정한 변수의 값을 사용한다. value=""

- 렌더링 전

```html
<input type="text" id="itemName" th:field="*{itemName}" class="form-control" placeholder="이름을 입력하세요">
```

- 렌더링 후

```html
<input type="text" id="itemName" class="form-control" placeholder="이름을 입력하세요" name="itemName" value="">
```

### 체크 박스

HTML checkbox는 선택이 안되면 클라이언트에서 서버로 값 자체를 보내지 않는다. 수정의 경우에는
상황에 따라서 이 방식이 문제가 될 수 있다. 사용자가 의도적으로 체크되어 있던 값을 체크를 해제해도
저장시 아무 값도 넘어가지 않기 때문에, 서버 구현에 따라서 값이 오지 않은 것으로 판단해서 값을 변경하지
않을 수도 있다.

이런 문제를 해결하기 위해서 스프링 MVC는 약간의 트릭을 사용하는데, 히든 필드를 하나 만들어서,
`_open` 처럼 기존 체크 박스 이름 앞에 언더스코어(`_`)를 붙여서 전송하면 체크를 해제했다고 인식할 수
있다. 히든 필드는 항상 전송된다. 따라서 체크를 해제한 경우 여기에서 open 은 전송되지 않고, `_open` 만
전송되는데, 이 경우 스프링 MVC는 체크를 해제했다고 판단한다.

- 체크 해제를 인식하기 위한 히든 필드
  - `<input type="hidden" name="_open" value="on"/>`

- 체크 박스 체크
  - `open=on&_open=on`
  - 체크 박스를 체크하면 스프링 MVC가 open 에 값이 있는 것을 확인하고 사용한다. 이때 `_open` 은 무시한다.
- 체크 박스 미체크
  - `_open=on`
  - 체크 박스를 체크하지 않으면 스프링 MVC가 `_open` 만 있는 것을 확인하고, open 의 값이 체크되지 않았다고 인식한다.
  - 이 경우 서버에서 Boolean 타입을 찍어보면 결과가 null 이 아니라 false 인 것을 확인할 수 있다. log.info("item.open={}", item.getOpen());

```html
<div>판매 여부</div>
<div>
    <div class="form-check">
        <input type="checkbox" id="open" name="open" class="form-check-input">
        <input type="hidden" name="_open" value="on"/> <!-- 히든 필드 추가 -->
        <label for="open" class="form-check-label">판매 오픈</label>
    </div>
</div>
```

```
FormItemController : item.open=true //체크 박스를 선택하는 경우
FormItemController : item.open=null //체크 박스를 선택하지 않는 경우
```

하지만 매번 코드에 hidden 값을 넣으려면 불편하다. 타임리프는 이런 부분을 해결해 준다.

위 코드와 아래 코드는 동일하다.

```
<input type="checkbox" id="open" th:field="*{open}" class="form-check-input">
```

타임리프를 사용하면 체크 박스의 히든 필드와 관련된 부분도 함께 해결해준다. HTML 생성 결과를 보면
히든 필드 부분이 자동으로 생성되어 있다.

- 타임리프의 체크 확인

```html
<input type="checkbox" id="open" th:field="${item.open}" class="form-check-input" disabled>
```

th:field 를 사용하면 값이 true 인 경우에 체크를 자동으로 처리해 준다.

- 생성 결과

```html
<input type="checkbox" id="open" class="form-check-input" disabled name="open" value="true" checked="checked">
```

### 체크 박스 멀티

```java
@ModelAttribute("regions")
public Map<String, String> regions() {
 Map<String, String> regions = new LinkedHashMap<>();
 regions.put("SEOUL", "서울");
 regions.put("BUSAN", "부산");
 regions.put("JEJU", "제주");
 return regions;
}
```

- @ModelAttribute의 특별한 사용법
  - 등록 폼, 상세화면, 수정 폼에서 모두 서울, 부산, 제주라는 체크 박스를 반복해서 보여주어야 한다. 이렇게
하려면 각각의 컨트롤러에서 model.addAttribute(...) 을 사용해서 체크 박스를 구성하는 데이터를
반복해서 넣어주어야 한다.
@ModelAttribute 는 이렇게 컨트롤러에 있는 별도의 메서드에 적용할 수 있다.
이렇게하면 해당 컨트롤러를 요청할 때 regions 에서 반환한 값이 자동으로 모델( model )에 담기게 된다.
물론 이렇게 사용하지 않고, 각각의 컨트롤러 메서드에서 모델에 직접 데이터를 담아서 처리해도 된다.

```html
<div>
 <div>등록 지역</div>
 <div th:each="region : ${regions}" class="form-check form-check-inline">
 <input type="checkbox" th:field="*{regions}" th:value="${region.key}" class="form-check-input">
 <label th:for="${#ids.prev('regions')}" th:text="${region.value}" class="form-check-label">서울</label>
 </div>
</div>
```

- `th:for="${#ids.prev('regions')}"`
  - 멀티 체크박스는 같은 이름의 여러 체크박스를 만들 수 있다. 그런데 문제는 이렇게 반복해서 HTML 태그를
생성할 때, 생성된 HTML 태그 속성에서 name 은 같아도 되지만, id 는 모두 달라야 한다. 따라서
타임리프는 체크박스를 each 루프 안에서 반복해서 만들 때 임의로 1 , 2 , 3 숫자를 뒤에 붙여준다

- 서울, 부산 선택한 경우
  - `regions=SEOUL&_regions=on&regions=BUSAN&_regions=on&_regions=on`

- 선택 안한 경우
  - `_regions=on&_regions=on&_regions=on`

`_regions` 는 앞서 설명한 기능이다. 웹 브라우저에서 체크를 하나도 하지 않았을 때, 클라이언트가 서버에
아무런 데이터를 보내지 않는 것을 방지한다. 참고로 `_regions` 조차 보내지 않으면 결과는 null 이 된다.
`_regions` 가 체크박스 숫자만큼 생성될 필요는 없지만, 타임리프가 생성되는 옵션 수 만큼 생성해서 그런
것이니 무시하자

### 라디오 버튼

```java
@ModelAttribute("itemTypes")
public ItemType[] itemTypes() {
 return ItemType.values();
}
```

```html
<div>
 <div>상품 종류</div>
 <div th:each="type : ${itemTypes}" class="form-check form-check-inline">
 <input type="radio" th:field="*{itemType}" th:value="${type.name()}" class="form-check-input">
 <label th:for="${#ids.prev('itemType')}" th:text="${type.description}" class="form-check-label">
 BOOK
 </label>
 </div>
</div>
```

### 셀렉트 박스

```java
@ModelAttribute("deliveryCodes")
public List<DeliveryCode> deliveryCodes() {
 List<DeliveryCode> deliveryCodes = new ArrayList<>();
 deliveryCodes.add(new DeliveryCode("FAST", "빠른 배송"));
 deliveryCodes.add(new DeliveryCode("NORMAL", "일반 배송"));
 deliveryCodes.add(new DeliveryCode("SLOW", "느린 배송"));
 return deliveryCodes;
}
```
```html
<!-- SELECT -->
<div>
 <div>배송 방식</div>
 <select th:field="*{deliveryCode}" class="form-select">
 <option value="">==배송 방식 선택==</option>
 <option th:each="deliveryCode : ${deliveryCodes}" th:value="$ {deliveryCode.code}"th:text="${deliveryCode.displayName}">FAST</option>
 </select>
</div>
```

## 메시지, 국제화

악덕? 기획자가 화면에 보이는 문구가 마음에 들지 않는다고, `상품명`이라는 단어를 모두 `상품이름`으로 고쳐달라고 하면 어떻게 해야할까?
여러 화면에 보이는 상품명, 가격, 수량 등, label 에 있는 단어를 변경하려면 다음 화면들을 다 찾아가면서
모두 변경해야 한다. 지금처럼 화면 수가 적으면 문제가 되지 않지만 화면이 수십개 이상이라면 수십개의
파일을 모두 고쳐야 한다.

왜냐하면 해당 HTML 파일에 메시지가 하드코딩 되어 있기 때문이다.
이런 다양한 메시지를 한 곳에서 관리하도록 하는 기능을 `메시지 기능`이라 한다.

예를 들어서, messages.properties 라는 메시지 관리용 파일을 만들고

```
item=상품
item.id=상품 ID
...
```

- addForm.html
  - `<label for="itemName" th:text="#{item.itemName}"></label>`
- editForm.html
  - `<label for="itemName" th:text="#{item.itemName}"></label>`

이런식으로 관리한다.

- 국제화

메시지에서 한 발 더 나가보자.
메시지에서 설명한 메시지 파일(messages.properteis)을 각 나라별로 별도로 관리하면 서비스를
국제화 할 수 있다.
예를 들어서 다음과 같이 2개의 파일을 만들어서 분류한다.

- messages_en.properties

```
item=Item
item.id=Item ID
item.itemName=Item Name
item.price=price
item.quantity=quantity
```

- messages.ko.properties

```
item=상품
item.id=상품 ID
item.itemName=상품명
item.price=가격
item.quantity=수량
```

영어를 사용하는 사람이면 messages_en.propertis 를 사용하고,
한국어를 사용하는 사람이면 messages_ko.propertis 를 사용하게 개발하면 된다.
이렇게 하면 사이트를 국제화 할 수 있다.
한국에서 접근한 것인지 영어에서 접근한 것인지는 인식하는 방법은 `HTTP accept-language` 해더 값을
사용하거나 사용자가 직접 언어를 선택하도록 하고, 쿠키 등을 사용해서 처리하면 된다.

메시지와 국제화 기능을 직접 구현할 수도 있겠지만, 스프링은 기본적인 메시지와 국제화 기능을 모두
제공한다. 그리고 타임리프도 스프링이 제공하는 메시지와 국제화 기능을 편리하게 통합해서 제공한다.
지금부터 스프링이 제공하는 메시지와 국제화 기능을 알아보자.

### 스프링 메시지 소스 설정

스프링은 기본적인 메시지 관리 기능을 제공한다.
메시지 관리 기능을 사용하려면 스프링이 제공하는 `MessageSource` 를 스프링 빈으로 등록하면 되는데,
MessageSource 는 인터페이스이다. 따라서 구현체인 `ResourceBundleMessageSource` 를 스프링 빈으로
등록하면 된다.

- 직접 등록

```java
@Bean
public MessageSource messageSource() {
 ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
 messageSource.setBasenames("messages", "errors");
 messageSource.setDefaultEncoding("utf-8");
 return messageSource;
}
```

- `basenames` : 설정 파일의 이름을 지정한다.
  - messages 로 지정하면 messages.properties 파일을 읽어서 사용한다.
  - 추가로 국제화 기능을 적용하려면 messages_en.properties , messages_ko.properties 와 같이 파일명 마지막에 언어 정보를 주면된다. 만약 찾을 수 있는 국제화 파일이 없으면
messages.properties (언어정보가 없는 파일명)를 기본으로 사용한다.
  - 파일의 위치는 /resources/messages.properties 에 두면 된다.
  - 여러 파일을 한번에 지정할 수 있다. 여기서는 messages , errors 둘을 지정했다.
  - defaultEncoding : 인코딩 정보를 지정한다. utf-8 을 사용하면 된다.

- 스프링 부트
  - 스프링 부트를 사용하면 스프링 부트가 `MessageSource` 를 자동으로 스프링 빈으로 등록한다.
- 스프링 부트 메시지 소스 설정
  - application.properties
    - `spring.messages.basename=messages,config.i18n.messages`

- 스프링 부트 메시지 소스 기본 값
  - `spring.messages.basename=messages`
  - MessageSource 를 스프링 빈으로 등록하지 않고, 스프링 부트와 관련된 별도의 설정을 하지 않으면 messages 라는 이름으로 기본 등록된다. 따라서 messages_en.properties ,messages_ko.properties , messages.properties 파일만 등록하면 자동으로 인식된다.

```java
@SpringBootTest
public class MessageSourceTest {

    @Autowired
    MessageSource messageSource;

    @Test
    void helloMessage() throws Exception {
        String result = messageSource.getMessage("hello", null, null);
        assertThat(result).isEqualTo("안녕");
    }

    @Test
    void notFoundMessageCode() {
        assertThatThrownBy(() -> messageSource.getMessage("no_code", null, null))
                .isInstanceOf(NoSuchMessageException.class);
    }

    @Test
    void notFoundMessageCodeDefaultMessage() {
        String result = messageSource.getMessage("no_code", null, "기본 메시지", null);
        assertThat(result).isEqualTo("기본 메시지");
    }

    @Test
    void argumentMessage() throws Exception {
        String message = messageSource.getMessage("hello.name", new Object[]{"Spring"}, null);
        assertThat(message).isEqualTo("안녕 Spring");
    }

    @Test
    void defaultLang() throws Exception {
        assertThat(messageSource.getMessage("hello", null, null)).isEqualTo("안녕");
        assertThat(messageSource.getMessage("hello", null, Locale.KOREA)).isEqualTo("안녕");
    }

    @Test
    void enLang() {
        assertThat(messageSource.getMessage("hello", null, Locale.ENGLISH)).isEqualTo("hello");
    }

}
```

### 메시지 파일 만들기

메시지 파일을 만들어보자. 국제화 테스트를 위해서 messages_en 파일도 추가하자.

- messages.properties :기본 값으로 사용(한글)
- messages_en.properties : 영어 국제화 사용

> 주의! 파일명은 massage가 아니라 messages다! 마지막 s에 주의하자

### 국제화 파일 선택

- locale 정보를 기반으로 국제화 파일을 선택한다.
- Locale이 en_US 의 경우 messages_en_US messages_en messages 순서로 찾는다.
- Locale 에 맞추어 구체적인 것이 있으면 구체적인 것을 찾고, 없으면 디폴트를 찾는다고 이해하면 된다.

```java
@Test
void defaultLang() {
 assertThat(ms.getMessage("hello", null, null)).isEqualTo("안녕");
 assertThat(ms.getMessage("hello", null, Locale.KOREA)).isEqualTo("안녕");
}
```

- ms.getMessage("hello", null, null) : locale 정보가 없으므로 messages 를 사용
- ms.getMessage("hello", null, Locale.KOREA) : locale 정보가 있지만, message_ko 가 없으므로 messages 를 사용

```java
@Test
void enLang() {
 assertThat(ms.getMessage("hello", null,
Locale.ENGLISH)).isEqualTo("hello");
}
```

- ms.getMessage("hello", null, Locale.ENGLISH) : locale 정보가 Locale.ENGLISH 이므로 messages_en 을 찾아서 사용

### 타임리프 메시지 적용

타임리프의 메시지 표현식 `#{...}`을 사용하면 스프링의 메시지를 편리하게 조회할 수 있다.
예를 들어서, 방금 등록한 상품이라는 이름을 조회하려면 `#{label.item}` 이라고 하면 된다.

- 페이지 이름에 적용
  - `<h2>상품 등록 폼</h2>`
    - `<h2 th:text="#{page.addItem}">상품 등록</h2>`
- 레이블에 적용
  - `<label for="itemName">상품명</label>`
    - `<label for="itemName" th:text="#{label.item.itemName}">상품명</label>`
    - `<label for="price" th:text="#{label.item.price}">가격</label>`
    - `<label for="quantity" th:text="#{label.item.quantity}">수량</label>`
- 버튼에 적용
  - `<button type="submit">상품 등록</button>`
    - `<button type="submit" th:text="#{button.save}">저장</button>`
    - `<button type="button" th:text="#{button.cancel}">취소</button>`

### 웹으로 확인하기

웹 브라우저의 언어 설정 값을 변경하면서 국제화 적용을 확인해보자.

`크롬 브라우저 -> 설정 -> 언어`를 검색하고, 우선 순위를 변경하면 된다.

우선순위를 영어로 변경하고 테스트해보자.
웹 브라우저의 언어 설정 값을 변경하면 요청시 `Accept-Language` 의 값이 변경된다.

Accept-Language 는 클라이언트가 서버에 기대하는 언어 정보를 담아서 요청하는 HTTP 요청 헤더이다.
(더 자세한 내용은 모든 개발자를 위한 HTTP 웹 기본지식 강의를 참고하자.)

### LocaleResolver

스프링은 Locale 선택 방식을 변경할 수 있도록 LocaleResolver 라는 인터페이스를 제공하는데, 
스프링 부트는 기본으로 Accept-Language 를 활용하는 AcceptHeaderLocaleResolver 를 사용한다.

```java
public interface LocaleResolver {
Locale resolveLocale(HttpServletRequest request);
void setLocale(HttpServletRequest request, @Nullable HttpServletResponse 
response, @Nullable Locale locale);
}
```

## [검증](https://github.com/BAEKJungHo/springmvc-project2/tree/main/validation/src)

__컨트롤러의 중요한 역할 중 하나는 HTTP 요청이 정상인지를 검증하는 것이다.__ 그리고 정상 로직보다 이런
검증 로직을 잘 개발하는 것이 어쩌면 더 어려울 수 있다. 

> 간혹가다, SI 쪽에서 검증을 하지 않는 개발자들이 있는데 검증 없는 코드는 시체일 뿐이며 요구사항이 복잡할 수록 검증도 복잡한 경우가 많아서 검증이 차지하는 비율이 로직을 작성하는 것보다 훨씬 크기 때문에 빠르게 개발했다고 해서 소스를 봤는데 검증이 없으면 뚝빼기를 깨버려야한다.

- 참고: 클라이언트 검증, 서버 검증
  - 클라이언트 검증은 조작할 수 있으므로 보안에 취약하다.
  - 서버만으로 검증하면, 즉각적인 고객 사용성이 부족해진다.
  - 둘을 적절히 섞어서 사용하되, 최종적으로 서버 검증은 필수
  - API 방식을 사용하면 API 스펙을 잘 정의해서 검증 오류를 API 응답 결과에 잘 남겨주어야 함

### 타임리프 스프링 검증 오류 통합 기능

타임리프는 스프링의 BindingResult 를 활용해서 편리하게 검증 오류를 표현하는 기능을 제공한다.

- #fields : #fields 로 BindingResult 가 제공하는 검증 오류에 접근할 수 있다.
- th:errors : 해당 필드에 오류가 있는 경우에 태그를 출력한다. th:if 의 편의 버전이다.
- th:errorclass : th:field 에서 지정한 필드에 오류가 있으면 class 정보를 추가한다.
- 검증과 오류 메시지 공식 메뉴얼
  - https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#validation-and-error-messages

```html
<div>
    <label for="price" th:text="#{label.item.price}">가격</label>
    <input type="text" id="price" th:field="*{price}"
           th:errorclass="field-error" class="form-control" placeholder="가격을 입력하세요">
    <div class="field-error" th:errors="*{price}">
        가격 오류
    </div>
</div>
```

- 글로벌 오류 처리

```html
<div th:if="${#fields.hasGlobalErrors()}">
 <p class="field-error" th:each="err : ${#fields.globalErrors()}" th:text="${err}">전체 오류 메시지</p>
</div>
```

- 필드 오류 처리

```html
<input type="text" id="itemName" th:field="*{itemName}" th:errorclass="field-error" class="form-control" placeholder="이름을 입력하세요">
<div class="field-error" th:errors="*{itemName}">
 상품명 오류
</div>
```

### BindingResult

- 스프링이 제공하는 검증 오류를 보관하는 객체이다. 검증 오류가 발생하면 여기에 보관하면 된다.
- BindingResult 가 있으면 @ModelAttribute 에 데이터 바인딩 시 오류가 발생해도 컨트롤러가 호출 된다.
  - BindingResult 가 없으면 400 오류가 발생하면서 컨트롤러가 호출되지 않고, 오류 페이지로 이동한다.
  - BindingResult 가 있으면 오류 정보( FieldError )를 BindingResult 에 담아서 컨트롤러를 정상 호출한다.
- BindingResult 는 검증할 객체 바로 뒤에 와야 한다.

#### BindingResult 와 Errors 

- org.springframework.validation.Errors
- org.springframework.validation.BindingResult

BindingResult 는 인터페이스이고, Errors 인터페이스를 상속받고 있다.
실제 넘어오는 구현체는 `BeanPropertyBindingResult` 라는 것인데, 둘다 구현하고 있으므로
BindingResult 대신에 Errors 를 사용해도 된다. Errors 인터페이스는 단순한 오류 저장과 조회
기능을 제공한다. BindingResult 는 여기에 더해서 추가적인 기능들을 제공한다. addError() 도
BindingResult 가 제공하므로 여기서는 BindingResult 를 사용하자. 주로 관례상 BindingResult 를 많이 사용한다.

> BindingResult , FieldError , ObjectError 를 사용해서 오류 메시지를 처리하는 방법을 알아보았다. 그런데 오류가 발생하는 경우 고객이 입력한 내용이 모두 사라진다. 이 문제를 해결해보자.

### FieldError, ObjectError

- 목표
  - 사용자가 입력했던 값이 화면에 남도록 하자.

```java
// 검증 로직
if (!StringUtils.hasText(item.getItemName())) {
    bindingResult.addError(new FieldError("item", "itemName", item.getItemName(), false, null, null, "상품 이름은 필수 입니다."));
}
if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
    bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, null, null, "가격은 1,000 ~ 1,000,000 까지 허용합니다."));
}
if (item.getQuantity() == null || item.getQuantity() >= 9999) {
    bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, null ,null, "수량은 최대 9,999 까지 허용합니다."));
}

// 특정 필드가 아닌 복합 룰 검증
if (item.getPrice() != null && item.getQuantity() != null) {
    int resultPrice = item.getPrice() * item.getQuantity();
    if (resultPrice < 10000) {
        bindingResult.addError(new ObjectError("item",null ,null, "가격 * 수량의 합은 10,000원 이상이어야 합니다. 현재 값 = " + resultPrice));
    }
}
```

#### FieldError 생성자

FieldError 는 두 가지 생성자를 제공한다.

```java
public FieldError(String objectName, String field, String defaultMessage);
public FieldError(String objectName, String field, @Nullable Object 
rejectedValue, boolean bindingFailure, @Nullable String[] codes, @Nullable
Object[] arguments, @Nullable String defaultMessage)
```

- objectName : 오류가 발생한 객체 이름
- field : 오류 필드
- `rejectedValue` : 사용자가 입력한 값(거절된 값)
- bindingFailure : 타입 오류 같은 바인딩 실패인지, 검증 실패인지 구분 값
  - bindingFailure 는 타입 오류 같은 바인딩이 실패했는지 여부를 적어주면 된다. 여기서는 바인딩이 실패한 것은 아니기 때문에 false 를 사용한다.
- codes : 메시지 코드
- arguments : 메시지에서 사용하는 인자
- defaultMessage : 기본 오류 메시지

> ObjectError 도 유사하게 두 가지 생성자를 제공한다.
>
> FieldError 는 오류 발생시 사용자 입력 값을 저장하는 기능을 제공한다.

- __타임리프의 사용자 입력 값 유지__

`th:field="*{price}"`

__타임리프의 th:field 는 매우 똑똑하게 동작하는데, 정상 상황에는 모델 객체의 값을 사용하지만, 오류가 발생하면 FieldError 에서 보관한 값을 사용해서 값을 출력한다.__

- __스프링의 바인딩 오류 처리__

타입 오류로 바인딩에 실패하면 스프링은 FieldError 를 생성하면서 사용자가 입력한 값을 넣어둔다. 
그리고 해당 오류를 BindingResult 에 담아서 컨트롤러를 호출한다. 따라서 타입 오류 같은 바인딩 실패시에도 사용자의 오류 메시지를 정상 출력할 수 있다.

### 오류 메시지 관리

- `errors 메시지 파일 생성`
  - message.properties 를 사용해도 되지만, 오류 메시지를 구분하기 쉽게 `errors.properties` 라는 별도의 파일로 관리하자.
  - 먼저 스프링 부트가 해당 메시지 파일을 인식할 수 있게 다음 설정을 추가한다. 이렇게하면 messages.properties, errors.properties 두 파일을 모두 인식한다. 생략하면 messages.properties 를기본으로 인식한다.

- `application.properties`
  - spring.messages.basename=messages,errors

- `errors.properties`

```yml
required.item.itemName=상품 이름은 필수입니다.
range.item.price=가격은 {0} ~ {1} 까지 허용합니다.
max.item.quantity=수량은 최대 {0} 까지 허용합니다.
totalPriceMin=가격 * 수량의 합은 {0}원 이상이어야 합니다. 현재 값 = {1}
```

> 참고: errors_en.properties 파일을 생성하면 오류 메시지도 국제화 처리를 할 수 있다.

```java
// 검증 로직
if (!StringUtils.hasText(item.getItemName())) {
    bindingResult.addError(new FieldError("item", "itemName", item.getItemName(), false, new String[]{"required.item.itemName"}, null, null));
}
if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
    bindingResult.addError(new FieldError("item", "price", item.getPrice(), false, new String[]{"range.item.price"}, new Object[]{1000, 1000000}, null));
}
if (item.getQuantity() == null || item.getQuantity() >= 9999) {
    bindingResult.addError(new FieldError("item", "quantity", item.getQuantity(), false, new String[]{"max.item.quantity"} ,new Object[]{9999}, null));
}

// 특정 필드가 아닌 복합 룰 검증
if (item.getPrice() != null && item.getQuantity() != null) {
    int resultPrice = item.getPrice() * item.getQuantity();
    if (resultPrice < 10000) {
        bindingResult.addError(new ObjectError("item",new String[]{"totalPriceMin"} ,new Object[]{10000, resultPrice}, null));
    }
}

//검증에 실패하면 다시 입력 폼으로
if (bindingResult.hasErrors()) {
    log.info("errors={} ", bindingResult);
    return "validation/v2/addForm";
}
````

- `codes` : required.item.itemName 를 사용해서 메시지 코드를 지정한다. 메시지 코드는 하나가 아니라
배열로 여러 값을 전달할 수 있는데, 순서대로 매칭해서 처음 매칭되는 메시지가 사용된다.
- `arguments` : Object[]{1000, 1000000} 를 사용해서 코드의 {0} , {1} 로 치환할 값을 전달한다.

### 오류 메시지 깔끔하게 쓰기

BindingResult 가 제공하는 `rejectValue(), reject()` 를 사용하면 FieldError , ObjectError 를 직접 생성하지 않고, 깔끔하게 검증 오류를 다룰 수 있다.

```java
log.info("objectName={}", bindingResult.getObjectName());
log.info("target={}", bindingResult.getTarget());

if (!StringUtils.hasText(item.getItemName())) {
    bindingResult.rejectValue("itemName", "required");
}
if (item.getPrice() == null || item.getPrice() < 1000 || item.getPrice() > 1000000) {
    bindingResult.rejectValue("price", "range", new Object[]{1000, 10000000}, null);
}
if (item.getQuantity() == null || item.getQuantity() >= 9999) {
    bindingResult.rejectValue("quantity", "max", new Object[]{9999}, null);
}

// 특정 필드가 아닌 복합 룰 검증
if (item.getPrice() != null && item.getQuantity() != null) {
    int resultPrice = item.getPrice() * item.getQuantity();
    if (resultPrice < 10000) {
        bindingResult.reject("totalPriceMin", new Object[]{10000, resultPrice}, null);
    }
}

// 검증에 실패하면 다시 입력 폼으로
if (bindingResult.hasErrors()) {
    log.info("errors={} ", bindingResult);
    return "validation/v2/addForm";
}
```

- `rejectValue()`

```java
void rejectValue(@Nullable String field, String errorCode,
@Nullable Object[] errorArgs, @Nullable String defaultMessage);
```

- field : 오류 필드명
- errorCode : 오류 코드(이 오류 코드는 메시지에 등록된 코드가 아니다. 뒤에서 설명할 messageResolver 를 위한 오류 코드이다.)
- errorArgs : 오류 메시지에서 {0} 을 치환하기 위한 값
- defaultMessage : 오류 메시지를 찾을 수 없을 때 사용하는 기본 메시지

```java
bindingResult.rejectValue("price", "range", new Object[]{1000, 1000000}, null)
```

- `reject()`

```java
void reject(String errorCode, @Nullable Object[] errorArgs, @Nullable String defaultMessage);
```

#### 축약된 오류 코드

FieldError() 를 직접 다룰 때는 오류 코드를 range.item.price 와 같이 모두 입력했다. 그런데
rejectValue() 를 사용하고 부터는 오류 코드를 range 로 간단하게 입력했다. 그래도 오류 메시지를 잘
찾아서 출력한다. 무언가 규칙이 있는 것 처럼 보인다. 이 부분을 이해하려면 `MessageCodesResolver` 를
이해해야 한다. 왜 이런식으로 오류 코드를 구성하는지 바로 다음에 자세히 알아보자.


오류 코드를 만들 때 다음과 같이 자세히 만들 수도 있고,

- required.item.itemName : 상품 이름은 필수 입니다.
- range.item.price : 상품의 가격 범위 오류 입니다.

또는 다음과 같이 단순하게 만들 수도 있다.

- required : 필수 값 입니다.
- range : 범위 오류 입니다.

단순하게 만들면 범용성이 좋아서 여러곳에서 사용할 수 있지만, 메시지를 세밀하게 작성하기 어렵다. 
반대로 너무 자세하게 만들면 범용성이 떨어진다. 가장 좋은 방법은 범용성으로 사용하다가, 세밀하게
작성해야 하는 경우에는 세밀한 내용이 적용되도록 메시지에 단계를 두는 방법이다.

```yml
#Level1
required.item.itemName: 상품 이름은 필수 입니다.

#Level2
required: 필수 값 입니다.
```

물론 이렇게 객체명과 필드명을 조합한 메시지가 있는지 우선 확인하고, 없으면 좀 더 범용적인 메시지를
선택하도록 추가 개발을 해야겠지만, 범용성 있게 잘 개발해두면, 메시지의 추가 만으로 매우 편리하게 오류
메시지를 관리할 수 있을 것이다.

스프링은 `MessageCodesResolver` 라는 것으로 이러한 기능을 지원한다.

```java
public class MessageCodesResolverTest {

    MessageCodesResolver codesResolver = new DefaultMessageCodesResolver();

    @Test
    void messageCodesResolverObject() {
        String[] messageCodes = codesResolver.resolveMessageCodes("required", "item");
        assertThat(messageCodes).containsExactly("required.item", "required");
    }

    @Test
    void messageCodesResolverField() {
        String[] messageCodes = codesResolver.resolveMessageCodes("required", "item", "itemName", String.class);
        assertThat(messageCodes).containsExactly(
                "required.item.itemName",
                "required.itemName",
                "required.java.lang.String",
                "required"
        );
    }

}
```

- MessageCodesResolver
  - 검증 오류 코드로 메시지 코드들을 생성한다.
  - MessageCodesResolver 인터페이스이고 DefaultMessageCodesResolver 는 기본 구현체이다. 주로 다음과 함께 사용 ObjectError , FieldError

#### DefaultMessageCodesResolver의 기본 메시지 생성 규칙

- 객체 오류

```
객체 오류의 경우 다음 순서로 2가지 생성
1.: code + "." + object name
2.: code

예) 오류 코드: required, object name: item
1.: required.item
2.: required
```

- 필드 오류

```
필드 오류의 경우 다음 순서로 4가지 메시지 코드 생성
1.: code + "." + object name + "." + field
2.: code + "." + field
3.: code + "." + field type
4.: code

예) 오류 코드: typeMismatch, object name "user", field "age", field type: int
1. "typeMismatch.user.age"
2. "typeMismatch.age"
3. "typeMismatch.int"
4. "typeMismatch"
```

#### 동작 방식

rejectValue() , reject() 는 내부에서 MessageCodesResolver 를 사용한다. 여기에서 메시지 코드들을 생성한다.
FieldError , ObjectError 의 생성자를 보면, 오류 코드를 하나가 아니라 여러 오류 코드를 가질 수 있다.
MessageCodesResolver 를 통해서 생성된 순서대로 오류 코드를 보관한다.

이 부분을 BindingResult 의 로그를 통해서 확인해보자.

- `codes [range.item.price, range.price, range.java.lang.Integer, range]`

- FieldError rejectValue("itemName", "required") : 다음 4가지 오류 코드를 자동으로 생성
  - required.item.itemName
  - required.itemName
  - required.java.lang.String
  - required

- ObjectError reject("totalPriceMin") : 다음 2가지 오류 코드를 자동으로 생성
  - totalPriceMin.item
  - totalPriceMin

#### 오류 메시지 출력

타임리프 화면을 렌더링 할 때 th:errors 가 실행된다. 만약 이때 오류가 있다면 생성된 오류 메시지
코드를 순서대로 돌아가면서 메시지를 찾는다. 그리고 없으면 디폴트 메시지를 출력한다.

### 오류 코드 관리 전략

__핵심은 구체적인 것에서! 덜 구체적인 것으로!__

MessageCodesResolver 는 required.item.itemName 처럼 구체적인 것을 먼저 만들어주고,
required 처럼 덜 구체적인 것을 가장 나중에 만든다.
이렇게 하면 앞서 말한 것 처럼 메시지와 관련된 공통 전략을 편리하게 도입할 수 있다.

__왜 이렇게 복잡하게 사용하는가?__

모든 오류 코드에 대해서 메시지를 각각 다 정의하면 개발자 입장에서 관리하기 너무 힘들다.
크게 중요하지 않은 메시지는 범용성 있는 requried 같은 메시지로 끝내고, 정말 중요한 메시지는 꼭
필요할 때 구체적으로 적어서 사용하는 방식이 더 효과적이다.

- errors.properties

```yml
#required.item.itemName=상품 이름은 필수입니다.
#range.item.price=가격은 {0} ~ {1} 까지 허용합니다.
#max.item.quantity=수량은 최대 {0} 까지 허용합니다.
#totalPriceMin=가격 * 수량의 합은 {0}원 이상이어야 합니다. 현재 값 = {1}

#==ObjectError==
#Level1
totalPriceMin.item=상품의 가격 * 수량의 합은 {0}원 이상이어야 합니다. 현재 값 = {1}

#Level2 - 생략
totalPriceMin=전체 가격은 {0}원 이상이어야 합니다. 현재 값 = {1}

#==FieldError==
#Level1
required.item.itemName=상품 이름은 필수입니다.
range.item.price=가격은 {0} ~ {1} 까지 허용합니다.
max.item.quantity=수량은 최대 {0} 까지 허용합니다.

#Level2 - 생략

#Level3
required.java.lang.String = 필수 문자입니다.
required.java.lang.Integer = 필수 숫자입니다.
min.java.lang.String = {0} 이상의 문자를 입력해주세요.
min.java.lang.Integer = {0} 이상의 숫자를 입력해주세요.
range.java.lang.String = {0} ~ {1} 까지의 문자를 입력해주세요.
range.java.lang.Integer = {0} ~ {1} 까지의 숫자를 입력해주세요.
max.java.lang.String = {0} 까지의 숫자를 허용합니다.
max.java.lang.Integer = {0} 까지의 숫자를 허용합니다.

#Level4
required = 필수 값 입니다.
min= {0} 이상이어야 합니다.
range= {0} ~ {1} 범위를 허용합니다.
max= {0} 까지 허용합니다.
```

크게 `객체 오류` 와 `필드 오류` 를 나누었다. 그리고 범용성에 따라 레벨을 나누어두었다.

### ValidationUtils

- ValidationUtils 사용 전

```java
if (!StringUtils.hasText(item.getItemName())) {
  bindingResult.rejectValue("itemName", "required", "기본: 상품 이름은 필수입니다.");
}
```

- ValidationUtils 사용 후

다음과 같이 한줄로 가능, 제공하는 기능은 `Empty , 공백` 같은 단순한 기능만 제공

```java
ValidationUtils.rejectIfEmptyOrWhitespace(bindingResult, "itemName", "required");
```

#### 정리

1. rejectValue() 호출
2. MessageCodesResolver 를 사용해서 검증 오류 코드로 메시지 코드들을 생성
3. new FieldError() 를 생성하면서 메시지 코드들을 보관
4. th:erros 에서 메시지 코드들로 메시지를 순서대로 메시지에서 찾고, 노출

### typeMismatch 오류

스프링이 직접 만든 오류 메시지 처리

검증 오류 코드는 다음과 같이 2가지로 나눌 수 있다.

- 개발자가 직접 설정한 오류 코드 rejectValue() 를 직접 호출
- 스프링이 직접 검증 오류에 추가한 경우(주로 타입 정보가 맞지 않음)

price 필드에 문자 "A"를 입력해보자.

로그를 확인해보면 BindingResult 에 FieldError 가 담겨있고, 다음과 같은 메시지 코드들이 생성된
것을 확인할 수 있다.

`codes[typeMismatch.item.price,typeMismatch.price,typeMismatch.java.lang.Integer,typeMismatch]`

다음과 같이 4가지 메시지 코드가 입력되어 있다.

- typeMismatch.item.price
- typeMismatch.price
- typeMismatch.java.lang.Integer
- typeMismatch

그렇다. `스프링은 타입 오류가 발생하면 typeMismatch 라는 오류 코드를 사용한다.` 이 오류 코드가 MessageCodesResolver 를 통하면서 4가지 메시지 코드가 생성된 것이다.

실행해보자.

아직 errors.properties 에 메시지 코드가 없기 때문에 스프링이 생성한 기본 메시지가 출력된다.

```
Failed to convert property value of type java.lang.String to required type 
java.lang.Integer for property price; nested exception is 
java.lang.NumberFormatException: For input string: "A"
```

errors.properties 에 다음 내용을 추가

```yml
typeMismatch.java.lang.Integer=숫자를 입력해주세요.
typeMismatch=타입 오류입니다.
```

## [Validator 분리 1](https://github.com/BAEKJungHo/springmvc-project2/blob/main/validation/src/main/java/hello/itemservice/web/validation/ItemValidator.java)

복잡한 검증 로직을 별도로 분리하자.

컨트롤러에서 검증 로직이 차지하는 부분은 매우 크다. 이런 경우 별도의 클래스로 역할을 분리하는 것이
좋다. 그리고 이렇게 분리한 검증 로직을 재사용 할 수도 있다.

스프링은 검증을 체계적으로 제공하기 위해 다음 인터페이스를 제공한다.

```java
public interface Validator {
  boolean supports(Class<?> clazz);
  void validate(Object target, Errors errors);
}
```

- supports() {} : 해당 검증기를 지원하는 여부 확인(뒤에서 설명)
- validate(Object target, Errors errors) : 검증 대상 객체와 BindingResult

## Validator 분리 2

스프링이 Validator 인터페이스를 별도로 제공하는 이유는 체계적으로 검증 기능을 도입하기 위해서다. 
그런데 앞에서는 검증기를 직접 불러서 사용했고, 이렇게 사용해도 된다. 그런데 Validator 인터페이스를
사용해서 검증기를 만들면 스프링의 추가적인 도움을 받을 수 있다.

__WebDataBinder를 통해서 사용하기__

```java
@InitBinder
public void init(WebDataBinder dataBinder) {
 log.info("init binder {}", dataBinder);
 dataBinder.addValidators(itemValidator);
}
```

이렇게 WebDataBinder 에 검증기를 추가하면 해당 컨트롤러에서는 검증기를 자동으로 적용할 수 있다.
@InitBinder 해당 컨트롤러에만 영향을 준다. 글로벌 설정은 별도로 해야한다. (마지막에 설명)

```java
@PostMapping("/add")
public String addItemV6(@Validated @ModelAttribute Item item, BindingResult 
bindingResult, RedirectAttributes redirectAttributes) {
 if (bindingResult.hasErrors()) {
 log.info("errors={}", bindingResult);
 return "validation/v2/addForm";
 }
 //성공 로직
 Item savedItem = itemRepository.save(item);
 redirectAttributes.addAttribute("itemId", savedItem.getId());
 redirectAttributes.addAttribute("status", true);
 return "redirect:/validation/v2/items/{itemId}";
}
```

validator를 직접 호출하는 부분이 사라지고, 대신에 검증 대상 앞에 `@Validated` 가 붙었다.

#### 동작 방식

@Validated 는 검증기를 실행하라는 애노테이션이다.
이 애노테이션이 붙으면 앞서 WebDataBinder 에 등록한 검증기를 찾아서 실행한다. 그런데 여러 검증기를
등록한다면 그 중에 어떤 검증기가 실행되어야 할지 구분이 필요하다. 이때 supports() 가 사용된다. 
여기서는 supports(Item.class) 호출되고, 결과가 true 이므로 ItemValidator 의 validate() 가
호출된다.

```java
@Component
public class ItemValidator implements Validator {
 @Override
 public boolean supports(Class<?> clazz) {
  return Item.class.isAssignableFrom(clazz);
 }
 @Override
 public void validate(Object target, Errors errors) {...}
}
```

- 글로벌 설정 - 모든 컨트롤러에 다 적용

```java
@SpringBootApplication
public class ItemServiceApplication implements WebMvcConfigurer {
 public static void main(String[] args) {
  SpringApplication.run(ItemServiceApplication.class, args);
 }
 
 @Override
 public Validator getValidator() {
  return new ItemValidator();
 }
}
```

이렇게 글로벌 설정을 추가할 수 있다. 기존 컨트롤러의 @InitBinder 를 제거해도 글로벌 설정으로 정상
동작하는 것을 확인할 수 있다. 참고로 글로벌 설정을 직접 사용하는 경우는 드물다.

> 검증시 @Validated @Valid 둘다 사용가능하다.
> javax.validation.@Valid 를 사용하려면 build.gradle 의존관계 추가가 필요하다.
> implementation 'org.springframework.boot:spring-boot-starter-validation'
> @Validated 는 스프링 전용 검증 애노테이션이고, @Valid 는 자바 표준 검증 애노테이션이다.
> 자세한 내용은 다음 Bean Validation에서 설명하겠다.
