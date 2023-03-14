# XSS

## 세션 하이재킹 공격

세션 하이재킹은 두 시스템 간의 연결이 활성화된 상태 즉, 로그인된 상태를 가로채는 것을 말하는 것이다.

- 공격자가 인증 작업 등이 완료된 정상적으로 통신하고 있는 다른 사용자의 세션을 가로채서 별도의 인증 작업을 거치지 않고 가로챈 세션으로 통신을 계속하는 행위

<br>

## XSS란?

- Cross Site Scripting의 약자
- 가장 널리 알려진 웹 보안 취약점 중 하나
- XSS는 게시판이나 웹 메일 등에 자바 스크립트와 같은 스크립트 코드를 삽입 해 개발자가 고려하지 않은 기능이 작동하게 하는 치명적일 수 있는 공격
- 클라이언트 즉, 사용자를 대상으로 한 공격

<br>

## XSS 종류

### 1. Stored XSS
- 공격자가 제공한 데이터가 **서버에 저장된 후** 지속적으로 서비스를 제공하는 정상 페이지에서 **다른 사용자에게 스크립트가 노출**되는 기법
- Stored XSS는 Reflected XSS와는 달리 **데이터베이스에 스크립트가 저장**이 되며, 웹 사이트의 게시판에 스크립트를 삽입하는 공격 방식이다.

<img src="https://user-images.githubusercontent.com/48792230/224488784-b6968cbf-be3d-4373-a897-8f62cee58cd3.png" width="600" />


- 공격자는 게시판에 스크립트를 삽입한 후 공격 대상자가 해당 게시글을 클릭하도록 유도한다.

<br>

### 2. Reflected XSS
- 웹 어플리케이션의 **지정된 파라미터를 사용**할 때 발생하는 취약점을 이용한 공격법
- “검색어”같은 쿼리 스트링을 URL에 담아 전송했을 때, **서버가 필터링 거치지 않고** 쿼리에 포함된 스크립트를 응답 페이지에 담아 전송함으로써 발생
- stored XSS와는 다르게 **데이터베이스에 스크립트가 저장되지 않고** 응답 페이지로 바로 클라이언트에 전달됨

<img src="https://user-images.githubusercontent.com/48792230/224488777-9a9b8c91-9be6-4c71-8595-f88f05eaebc5.png" width="600" />

- 공격자가 미리 XSS 공격에 취약한 웹 사이트를 탐색하고, XSS 공격을 위한 스크립트를 포함 한 URL을 사용자에 노출시킨다.
- 사용자가 해당 URL을 클릭 할 경우, 취약한 웹 사이트의 서버에 스크립트가 포함 된 URL을 통해 Request를 전송하고, 웹 서버에서는 해당 스크립트를 포함한 Response를 전송하게 된다.

<br>

### 3. DOM Based XSS
- DOM 환경에서 악성 URL을 통해 사용자의 브라우저를 공격하는 것
    
    <img src="https://user-images.githubusercontent.com/48792230/224488781-3167e788-d170-4108-8108-7dde881b07ab.png" width="600" />

    
- 피해자의 브라우저가 HTML 페이지를 구문 분석할 때마다 공격 스크립트가 DOM 생성의 일부로 실행되면서 공격한다. 페이지 자체는 변하지 않으나, 페이지에 포함되어 있는 브라우저 측 코드가 DOM 환경에서 악성코드로 실행된다.
- 위에서 설명한 Stored XSS 및 Reflected XSS 공격의 악성 페이로드가 **서버 측 애플리케이션 취약점으로인해**, 응답 페이지에 악성 스크립트가 포함되어 브라우저로 전달되면서 공격하는 것이 반면, DOM Based XSS는 **서버와 관계없이** 브라우저에서 발생하는 것이 차이점이다.
- DOM Based XSS 취약점이 있는 브라우저를 대상으로 조작된 URL을 이메일을 통해 사용자에게 전송하면, 피해자는 이 URL 링크를 클릭하는 순간 공격 피해를 입게 된다.

<br>

## CSRF랑 XSS는 어떤 차이가 있나요?

### CSRF(Cross Site Request Forgery) 공격

- 사이트 간 요청 위조
- 인터넷 사용자(희생자)가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 만드는 공격

<img src="https://user-images.githubusercontent.com/48792230/224488785-dc949b50-4fe4-4334-953d-9f80b4d08575.png" width="600" />

<img src="https://user-images.githubusercontent.com/48792230/224488779-48724c57-f5b7-4701-bb07-1576c97931a7.png" width="600" />


- 해커들이 어떤 사용자에게 피싱을 해서 링크를 누르게 하고, 링크를 누르면 사용자가 모르게 사이트의 특정 기능을 실행하게 함
- 주로 사용자 모르게 패스워드를 변경함

<br>

### XSS와 CSRF의 차이

- **공통점** : 사용자의 브라우저를 대상
- **차이점**
    - **CSRF**
        - 사용자의 **인증된 세션을 악용**하는 공격 방식
        - **웹 애플리케이션(서버)**이 인증된 사용자의 요청을 신뢰한다는 사실을 이용한 공격 방식
        - 서버 측에서 스크립트가 실행
        - 요청을 위조함으로써 사용자 몰래 송금과 제품 구입 등 특정 행위를 수행하는 것을 목적으로 함
    - **XSS**
        - **인증된 세션 없이도** 공격을 진행할 수 있음
        - **사용자(클라이언트)**가 특정 사이트를 신뢰한다는 사실을 이용한 공격 방식
        - 사용자 측에서 스크립트가 실행
        - 사용자 PC에서 스크립트를 실행해 사용자의 정보를 탈취하는 것을 목적으로 함

<br>

## XSS로 인한 피해

- **쿠키 정보 및 세션 ID 획득**
    - 공격자는 XSS에 취약한 페이지 및 게시판에 XSS공격을 수행함으로써 해당 페이지를 이용하는 사용자의 쿠키 정보나 세션 ID를 획득할 수 있다.
    - 이렇게 되면 XSS공격을 통해 페이지를 사용하는 사용자의 세션ID를 얻어서 공격자가 불법적으로 사용자인 척 하면서 활보할 수 있다.
- **시스템 관리자 권한 획득**
    - XSS취약점이 있는 웹서버에 다양한 악성 데이터를 포함시킨 후, 사용자의 브라우저가 악성 데이터를 실행하게 할 수 있다.
    - 만약 이렇게 해킹이 될 경우, 내부로 악성코드가 이동해 중요 정보가 탈취될 수 있다.
- **거짓 페이지 노출**
    - `<script>`뿐만이 아니라 `<img>`와 같은 그림을 표시하는 태그를 사용해 원래 페이지와 전혀 관련 없는 페이지를 표시할 수가 있다.

<br>

## XSS 공격 방지 기법

1. **XSS 취약점이 있는 innerHTML 사용을 자제**
    - 클라이언트에서 방지하는 기법
    - HTML5에서는 innerHTML을 통해 주입한 스크립트는 실행되지 않지만 이벤트 속성을 통해 스크립트를 주입할 수 있는 방법이 있기 때문에 innerHTML 사용을 자제하는 것을 권장
2. **쿠키에 HttpOnly 옵션을 활성화**
    - 활성화하지 않으면 스크립트를 통해 쿠키에 접근할 수 있어서 세션 하이재킹 취약점이 발생할 수 있음
    - HttpOnly 옵션 : 클라이언트 측에서 HTTP 통신외에는 Cookie에 접근이 불가능하도록 하는것. HttpOnly Attribute를 가진 쿠키는 클라이언트에서 JavaScript를 통해 접근이 금지됨
3. **XSS 특수문자를 치환 or Script 문자 필터링**
    - XSS 공격은 입력값에 대한 검증이 제대로 이루어지지 않아 발생하는 취약점이기 때문에, 입력값에 대해 서버는  `< > " '`와 같은 문자열들을 정규표현식으로 필터링하여 태그를 제거해야 함
    - Spring에서는 Lucy XSS Servlet Filter라는 오픈소스 라이브러리를 사용하여 특수 문자를 치환할 수 있음

<br>

---

💡 **참고 자료**

[XSS(Cross Site Scripting) 공격이란?](https://4rgos.tistory.com/1)

[크로스 사이트 스크립팅, XSS](https://www.youtube.com/watch?v=LfI6TAchgT4)

[CSRF 공격이란? 그리고 CSRF 방어 방법](https://itstory.tk/entry/CSRF-공격이란-그리고-CSRF-방어-방법)

[[화이트해커][웹모의해킹] 24강. 아무도 모르게 비밀번호 변경을? CSRF 공격 개념](https://www.youtube.com/watch?v=atNmPzdvPD4)

[[10분 테코톡] 🔧알트의 XSS](https://www.youtube.com/watch?v=bSGqBoZd8WM)

[XSS와 CSRF의 차이](https://nordvpn.com/ko/blog/csrf/#:~:text=%EA%B3%B5%EA%B2%A9%EA%B3%BC%20%EB%A7%88%EC%B0%AC%EA%B0%80%EC%A7%80%EC%9D%B4%EA%B8%B0%20%EB%95%8C%EB%AC%B8%EC%9E%85%EB%8B%88%EB%8B%A4.-,XSS%EC%99%80%20CSRF%EC%9D%98%20%EC%B0%A8%EC%9D%B4,-%ED%81%AC%EB%A1%9C%EC%8A%A4%20%EC%82%AC%EC%9D%B4%ED%8A%B8%20%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)

[DOM based XSS 공격](https://berr-my.tistory.com/entry/XSS-%EA%B3%B5%EA%B2%A9#:~:text=3)-,DOM%20based%20XSS%20%EA%B3%B5%EA%B2%A9,-%2D%20DOM%20%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%20%EC%95%85%EC%84%B1)