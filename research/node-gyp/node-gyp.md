# `node-gyp`

> `Node.js`의 네이티브 애드온 빌드 도구 `node-gyp`에 관한 조사.

## 목차

- [`node-gyp`란?](#node-gyp란)
- [`addons`란?](#addons란)
- [구성 방법](#구성-방법)
- [빌드 방법](#빌드-방법)

---

## `node-gyp`란?

- `Node.js`용 기본 `addon` 모듈을 컴파일 하기 위해 `Node.js`로 작성된 크로스 플랫폼 커맨드 라인 도구.
- `Node.js`의 버전에 관계 없이 사용 가능한 모듈을 빌드함.
- 이 모듈을 사용하여 빌드하면 `Node.js`에서 네이티브 프로그래밍. `C`, `C++`을 컴파일하여 사용 가능 함.
- 사용 환경

  - `Windows`

    - [`Visual Studio Build Tools`](https://visualstudio.microsoft.com/ko/thank-you-downloading-visual-studio/?sku=BuildTools) 또는 [`Visual Studio Community`](https://visualstudio.microsoft.com/ko/thank-you-downloading-visual-studio/?sku=Community) 설치.
    - [`Microsoft Store`](https://docs.python.org/3/using/windows.html#the-microsoft-store-package) 패키지에서 최신 버전의 `Python` 설치.

  - `macOS`

    - `Python` 설치.
    - `XCode`를 설치.
    - `xcode-select --install`을 통해서 `XCode Command Line Tools` 또한 설치해야 함.

  - `Python` 종속성 구성

    - `node-gyp`와 호환되는 `Python` 버전 `v3.6`, `v3.7`, `v3.8`, `v3.9` 중 하나 설치.
    - 여러 버전이 설치된 경우 식별 방법
      1. `--python` 명령줄 옵션 사용.
         > `node-gyp <command> --python </path/to/executable/python>`
      2. `node-gyp`는 `npm`에 의해 호출되므로 `npm`에서 적절한 `Python` 버전 설정.
         > `npm config set python </path/to/executable/python>`
      3. `PYTHON` 환경 변수를 적절한 버전으로 변경.
      4. `NODE_GYP_FORCE_PYTHON` 환경 변수를 적절한 버전의 실행 파일 경로로 변경.

---

## `addons`란?

- `C++`로 작성된 동적으로 연결된 공유 객체.
- `Node.js`에서 `require()` 함수를 통해 `addon`을 일반 `Node.js module`로 로드할 수 있음.
- `JavaScript`와 `C/C++` 라이브러리 간의 인터페이스 제공.
- `addon`을 구현하기 위한 세 가지 옵션

  1. `Node-API`, `nan` 사용.
  2. [`V8`](https://v8docs.nodesource.com/) 엔진 사용.
     - `JavaScript` 구현을 제공하기 위해 `Node.js`가 사용하는 `C++` 라이브러리.
     - 객체 생성, 함수 호출 등을 위한 알고리즘 제공.
     - `v8.h` 헤더 파일 사용.
  3. `libuv` 사용.
     - `Node.js` 이벤트 루프, 워커 스레드 및 플랫폼의 모든 비동기 동작을 구현하는 `C` 라이브러리.
     - 크로스 플랫폼 추상화 라이브러리 역할.
     - 주요 운영체제에서 파일 시스템, 소켓, 타이머 및 시스템 이벤트와 상호 작용하는 것과 같은 시스템 작용에 대해 쉬운 액세스 제공.

- 다음의 `JavaScript` 소스 코드와 같은 기능을 하는 `C++ addon` 소스 코드.

- `JavaScript` 소스 코드

  ```js
  module.exports.hello = () => "world";
  ```

- `C++ addon` 소스 코드

  ```cpp
  #include <node.h>

  namespace demo {
    using v8::FunctionCallbackInfo;
    using v8::Isolate;
    using v8::Local;
    using v8::Object;
    using v8::String;
    using v8::Value;
    void Method(const FunctionCallbackInfo<Value>& args) {
      // args는 함수의 인수로 사용.
      Isolate *isolate = args.GetIsolate();
      args.GetReturnValue().Set(String::NewFromUtf8(
        isolate, "world").ToLocalChecked()
      );
    }
    void Initialize(Local<Object> exports) {
      NODE_SET_METHOD(exports, "hello", Method);
    }
    NODE_MODULE("module_name", Initialize)
  }
  ```

- `Node.js addon`은 초기화 함수를 내보내야 함.
- `NODE_MODULE`은 함수가 아니므로 세미콜론을 포함하지 않음.
- `module_name`은 바이너리의 파일 이름과 일치해야 함.

---

## 구성 방법

- `package.json`이 있는 루트 폴더에 `binding.gyp` 파일을 생성.
- `JSON` 형식의 파일로 작성.
- 작성 예시

  ```json
  {
    "targets": [
      {
        "target_name": "binding",
        "sources": ["src/binding.cc"]
      }
    ]
  }
  ```

- [`.gyp` 파일 사용 문서](https://gyp.gsrc.io/docs/UserDocumentation.md)
- [`.gyp` 파일 `input` 문서](https://gyp.gsrc.io/docs/InputFormatReference.md)

---

## 빌드 방법

- `npm`을 통해 `node-gyp`을 접근할 수 있도록 루트 폴더에서 `npm install node-gyp` 또는 `yarn add node-gyp`.
- 전역 명령으로 실행할 경우 `npm install -g node-gyp` 또는 `yarn add -global node-gyp`.
- 빌드할 루트 폴더에서 `node-gyp configure` 명령을 실행함.(전역 설치가 아닌 경우 `script`를 통해 실행)
  > 현재 플랫폼에 대한 프로젝트 빌드 파일을 생성.
- 그 후 `node-gyp build` 명령을 실행함.(전역 설치가 아닌 경우 `script`를 통해 실행)
  > 모듈을 빌드.

---

#### 참고

- [`node-gyp` 문서](https://github.com/nodejs/node-gyp/tree/2ef5fb86277c4d81baffc0b9f642a8d86be1bfa5/docs)
- [`Node.js addons` 문서](https://nodejs.org/api/addons.html)
- [`binding.gyp` 사용 예시](https://github.com/nodejs/node-gyp/blob/HEAD/docs/binding.gyp-files-in-the-wild.md)
