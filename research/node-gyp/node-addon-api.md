# `node-addon-api`

> `node-gyp`의 `C++` 래퍼 클래스인 `node-addon-api` 모듈과 `napi.h` 헤더 파일에 관한 조사.

## 목차

- [`node-addon-api`란?](#node-addon-api란)
- [설치 및 구성](#설치-및-구성)
- [`Data Types`](#data-types)

---

## `node-addon-api`란?

- `C` 기반 `Node-API`의 사용을 단순화하는 헤더 전용 `C++` 래퍼 클래스.
- 낮은 오버헤드로 `C++` 개체 모델 및 예외 처리 의미 체계를 제공.
- `addon`을 구현하기 위한 세 가지 옵션 중 하나.(`nan`, `Node-API`, 내부 `V8`, `libuv`, `Node.js` 라이브러리 직접 사용)

---

## 설치 및 구성

1. `node-addon-api`를 `package.json`의 의존성 모듈로 설치

   ```shell
   npm install node-addon-api
   ```

2. `binding.gyp` 파일에서 패키지 참조

   ```json
   {
     "include_dirs": ["<!@(node -p \"require('node-addon-api').include\")"],
     "include_dirs": ["<!(node -p \"require('node-addon-api').include_dir\"))"]
   }
   ```

3. `Node-API`에서 `C++` 예외 활성화 여부 결정하여 `binding.gyp`에 작성

   - 활성화

   ```json
   {
     "cflags!": ["-fno-exceptions"],
     "cflags_cc!": ["-fno-exceptions"],
     "conditions": [
       [
         "OS=='win'",
         {
           "defines": ["_HAS_EXCEPTIONS=1"],
           "msvs_settings": {
             "VCCLCompilerTool": {
               "ExceptionHandling": 1
             }
           }
         }
       ],
       [
         "OS=='mac'",
         {
           "xcode_settings": {
             "GCC_ENABLE_CPP_EXCEPTIONS": "YES",
             "CLANG_CXX_LIBRARY": "libc++",
             "MACOSX_DEPLOYMENT_TARGET": "10.7"
           }
         }
       ]
     ]
   }
   ```

   - 비활성화

   ```json
   {
     "defines": ["NAPI_DISABLE_CPP_EXCEPTIONS"]
   }
   ```

4. `addon`이 `macOS`를 지원하도록 하려면 다음 설정 `binding.gyp`에 추가

   ```json
   {
     "conditions": [
       [
         "OS=='mac'",
         {
           "cflags+": ["-fvisibility=hidden"],
           "xcode_settings": {
             "GCC_SYMBOLS_PRIVATE_EXTERN": "YES" // -fvisibility=hidden
           }
         }
       ]
     ]
   }
   ```

5. 모듈 코드에 `napi.h` 선언. `ABI` 안정성 보장을 위해 `node.h`, `nan.h`, `v8.h`는 선언하지 않는 것이 좋음.

   ```cpp
   #include <napi.h>
   ```

6. `binding.gyp`에 `targets` 추가 후 `sources`에 추가한 파일 명으로 모듈 파일 생성 후 모듈 코드 작성.

   - `binding.gyp`

   ```json
   {
     "targets": [
       {
         // myModule is the name of your native addon
         "target_name": "myModule",
         "sources": ["src/my_module.cc"]
         // ...
       }
     ]
   }
   ```

   - `my_module.cc`

   ```cpp
   #include <napi.h>

   // ...

   /**
    * This code is our entry-point. We receive two arguments here, the first is the
    * environment that represent an independent instance of the JavaScript runtime,
    * the second is exports, the same as module.exports in a .js file.
    * You can either add properties to the exports object passed in or create your
    * own exports object. In either case you must return the object to be used as
    * the exports for the module when you return from the Init function.
    */
   Napi::Object Init(Napi::Env env, Napi::Object exports) {

    // ...

    return exports;
   }

   /**
    * This code defines the entry-point for the Node addon, it tells Node where to go
    * once the library has been loaded into active memory. The first argument must
    * match the "target" in our *binding.gyp*. Using NODE_GYP_MODULE_NAME ensures
    * that the argument will be correct, as long as the module is built with
    * node-gyp (which is the usual way of building modules). The second argument
    * points to the function to invoke. The function must not be namespaced.
    */
   NODE_API_MODULE(NODE_GYP_MODULE_NAME, Init)
   ```

> `C++` 코드는 실행 가능한 형태로 컴파일되어야 함.

---

## `Data Types`

- `node-addon-api`에서 사용하는 `Data Types`.

### `Env`

- 실행되고 있는 환경을 포함하는 불투명한 데이터 구조.
- `Env` 객체는 일반적으로 `Node.js` 런타임 또는 `node-addon-api` 인프라에서 생성 및 전달.

#### 구성

- `constructor`

  - 환경 생성자.
  - 매개변수
    - `napi_env`: `Napi::Env`를 구성할 환경.

  ```cpp
  Napi::Env::Env(napi_env env);
  ```

- `napi_env`

  - 환경을 나타내는 불투명한 데이터 구조를 반환.

  ```cpp
  operator napi_env () const;
  ```

- `Global`

  - `JavaScript` 환경의 전역 객체를 `Napi::Object` 형태로 반환.

  ```cpp
  Napi::Object Napi::Env::Global() const;
  ```

- `Undefined`

  - `JavaScript` 환경의 `undefined`를 `Napi::Value` 형태로 반환.

  ```cpp
  Napi::Value Napi::Env::Undefined() const;
  ```

- `Null`

  - `JavaScript` 환경의 `null`를 `Napi::Value` 형태로 반환.

  ```cpp
  Napi::Value Napi::Env::Null() const;
  ```

- `RunScript`

  - 문자열로 `JavaScript` 코드를 실행하고 결과를 `Napi::Value` 형태로 반환.
  - 매개변수
    - `script`: `JavaScript` 코드가 포함된 문자열.(`Napi::String`, `const char *`, `const std::string &`)

  ```cpp
  Napi::Value Napi::Env::RunScript(Napi::String script);
  ```

### `CallbackInfo`

- 실행 중인 `JavaScript` 함수 메세지 요청의 구성 요소를 나타내는 객체.
- `Node.js` 런타임 또는 `node-addon-api`의 구조에 의해 일반적으로 전달.
- `Napi::CallbackInfo` 객체는 호출자에 의해 전달된 매개변수가 포함.
- 매개변수의 수는 `Length` 메서드에 의해 반환.
- `operator[]` 메서드를 사용하여 각 개별 인수에 액세스.

#### 구성

- `constructor`

  - 함수 메세지 요청 구성 요소 생성자.
  - 매개변수
    - `napi_env`: `Napi::CallbackInfo`를 구성할 환경.
    - `napi_callback_info`: `Napi::CallbackInfo`를 구성할 데이터 구조.

  ```cpp
  Napi::CallbackInfo::CallbackInfo (napi_env env, napi_callback_info info);
  ```

- `Env`

  - 메세지 요청이 이루어지는 환경 객체 `Napi::Env`를 반환.

  ```cpp
  Napi::Env Napi::CallbackInfo::Env() const;
  ```

- `Length`

  - 메세지 요청에 전달된 매개변수의 수 반환.

  ```cpp
  size_t Napi::CallbackInfo::Length() const;
  ```

- `operator []`

  - 메세지 요청에 전달된 해당 인덱스의 매개변수 `Napi::Value`를 반환.

  - 매개변수
    - `index`: 0부터 시작하는 매개변수의 인덱스.

  ```cpp
  const Napi::Value operator [](size_t index) const;
  ```

- `This`

  - 메세지를 호출한 환경에서 `JavaScript`의 `this` 값 `Napi::Value`를 반환.

  ```cpp
  Napi::Value Napi::CallbackInfo::This() const;
  ```

### `Value`

- `JavaScript` 값의 `C++` 표현.
- `JavaScript` 값인 `number`, `boolean`, `string`, `object`에 기초하여 `Napi::Number`, `Napi::Boolean`, `Napi::String`, `Napi::Object`에 기초.
- `napi_value`라는 얇은 래퍼로 둘러싸여 있으며, `C++` 유형으로 변환 가능.

#### 구성

---

#### 참고

- [`node-addon-api` 문서](https://github.com/nodejs/node-addon-api)
- [`node-addon-api` `Value` 문서](https://github.com/nodejs/node-addon-api/blob/main/doc/value.md)
