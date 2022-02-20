# `Content Security Policy`(`CSP`)

> `HTTP` 통신에서 `XSS` 공격의 위험성과 영향을 현저히 줄일 수 있는 방어 정책인 `CSP`에 관한 조사.

## 목차

- [`Content Security Policy`란?](#content-security-policy란)
- [`HTML`에서 `CSP` 설정](#html에서-csp-설정)
- [`CSP` `Level`](#csp-level)

---

## `Content Security Policy`란?

- `Web`의 보안 모델은 동일 출처 정책에 근간을 두고 있음.
  - 한 도메인에서 다른 도메인의 데이터에 액세스할 권한이 없는 것이 원칙.
  - 각 출처는 나머지 웹과 격리되어, 안전한 샌드박스를 개발자에게 제공.
- 교차 사이트 스크립팅(`XSS`) 공격은 사이트를 속여 악성 코드를 의도된 콘텐츠와 함께 전달하여 동일 출처 정책을 우회.
- 이는 브라우저가 페이지의 모든 코드를 해당 페이지의 합법적인 보안 출처의 일부로 신뢰하므로 심각한 문제 발생.

> 최신 브라우저에서 `XSS` 공격의 위험과 영향을 현저히 줄일 수 있는 방어책인 `Contect Security Policy`(콘텐츠 보안 정책, `CSP`)을 사용.

- `CSP`은 기본적으로 다음과 같은 기능을 수행.

  - 허용 목록을 사용하여 허용되는 것과 허용되지 않는 것을 클라이언트에 제시.
  - 어떤 지시문을 사용할 수 있는지 제시.
  - 지시문에서 사용하는 키워드를 제시.
  - 인라인 코드와 `eval()`은 유해한 것으로 간주.
  - 정책 위반 사항을 적용하기 전에 서버에 신고(알림).

#### 참고

- [`Google Developer`-`CSP`](https://developers.google.com/web/fundamentals/security/csp?hl=ko)
- [`Content Security Policy`](https://content-security-policy.com/script-src/)
- [`Runebook`-`CSP`: `script-src`](https://runebook.dev/ko/docs/http/headers/content-security-policy/script-src)

---

## `HTML`에서 `CSP` 설정

- `HTML`에서 `<head></head>` 내의 `<meta />` 태그를 통해 `CSP`를 설정할 수 있음.
- `http-equiv` 어트리뷰트를 `Content-Security-Policy`로 선언한 후 `content` 어트리뷰트에 `CSP` 소스 목록을 띄어쓰기로 구분하여 작성.
- 한 요소(`Ex) script-src, img-src 등`)의 내용 작성이 끝나면 세미 콜론(`;`)로 구분.
- 소스 목록에는 4가지 키워드를 추가적으로 사용할 수 있음.

  - `none`: 아무 것도 일치하지 않음.
  - `self`: 현재 출처와 일치하지만 하위 도메인은 일치하지 않음.
  - `unsafe-inline`: 인라인 `JavaScript`, `CSS`를 허용.
  - `unsafe-eval`: `eval` 함수 허용.

  > 키워드 사용에는 작은 따옴표(`'`)가 필요함.

  ```html
  <!DOCTYPE html>
  <html lang="kr">
    <head>
      <meta
        http-equiv="Content-Security-Policy"
        content="
          script-src
          'self'
          'unsafe-inline'
          https://*.domain.com;
        "
      />
    </head>
    <body></body>
  </html>
  ```

#### 참고

- [`MDN` 공식 문서](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)

---

## `CSP` `Level`

#### 참고

- [`W3 - CSP Level 3`](https://www.w3.org/TR/CSP3/)
