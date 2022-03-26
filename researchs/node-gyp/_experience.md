# `node-gyp`의 사용 경험

- `node-gyp`로 네이티브 모듈을 빌드한 후, 해당 모듈을 실행하는 `Node.js`에서 `try ... catch ...`로 에러 핸들링을 하여도, 네이티브 모듈 내에서 에러가 발생하는 경우에는 프로세스(애플리케이션)이 비정상 강제 종료.

- 네이티브 모듈은 `webpack`으로 번들링하는 과정이 까다로움.
  - `node-loader`를 사용하여 `.node` 파일을 해석하여도 제대로 사용할 수 없음.
  - `TypeScript` 적용의 경우, `.d.ts`에 `declare module`을 사용하면 에러 통과 가능.
  - `node_modules` 폴더 내에 모듈과 해당 모듈을 호출하는 인터페이스 역할의 `index.js`, `type.d.ts`를 작성하면 사용 가능.
    ```ts
    import NativeModule from "native_module";
    ```
