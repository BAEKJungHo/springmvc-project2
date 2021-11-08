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

