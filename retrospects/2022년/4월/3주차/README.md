# (기술) `Vite`

최근 각광받는 `JavaScript` 번들링 도구인 `Vite`에 대해서 조사를 해보았다.

![vite-logo](./assets/vite-logo.svg)

`Vite`는 프랑스어로 "빠르다(Quick)"을 의미하며, `/vit/`이라고 발음한다.  
빠르고 간결한 모던 웹 프로젝트 개발에 초점을 맞춰 탄생한 새로운 `JavaScript` 번들링 도구로 주목받고 있다.

## 번들링 도구

번들링(`Bundling`)은 빌드 툴 체인(`Build Tool Chain`)의 종류 중 하나로, 빌드 툴 체인이란, 소스를 빌드하는 과정 전반에 걸쳐 필요한 도구를 말한다.

번들링은 여러 개로 흩어져 있는 파일들을 압축, 난독화 등을 하여 하나의 파일로 모아주는 것을 말한다.

웹 브라우저에서 `JavaScript`의 역할이 커져감에 따라, `Script` 파일들의 크기가 점점 커지고 복잡해지면서 단순히 하나의 파일이나 클래스로 관리하기 어려워지고, 이에 모듈(`module`)이라고 하는 분리된 단위로 관리하기 시작했다.  
하지만 이 분리된 모듈들을 하나의 `HTML` 파일에서 각각 불러와서 사용하기엔 그만큼의 `HTTP` 요청이 발생하므로 네트워크 성능의 저하가 발생할 수 있고, 모듈 간의 전역 변수 충돌 등의 문제가 발생할 수 있으므로 이를 최종적으로 하나의 파일로 통합하여 배포하는 방식을 채택하게 됐고, 여기서 필요한 도구가 번들링 도구이다.

번들링 도구의 종류로는 [`RequireJS`](https://requirejs.org/), [`Browserify`](https://browserify.org/), [`Rollup`](https://rollupjs.org/guide/en/), [`Parcel`](https://ko.parceljs.org/), [`Webpack`](https://webpack.js.org/) 등이 있는데, 이 중 가장 인기가 많은 도구는 `Webpack`이다.

### `Webpack`

![webpack-logo](./assets/webpack-logo.svg)

`Webpack`은 모던 `JavaScript` 애플리케이션을 위한 정적 모듈 번들러로, 다양한 플러그인과 로더의 지원, 상세한 설정을 통해 개발자가 원하는 모습으로 코드를 번들링해줄 수 있는 강력한 도구이다.

굉장히 많은 곳에서 사용하고 있으며, `React`, `Vue`, `Angular`, `Node.js`, `Svelte` 등 다양한 프레임워크로의 번들링 또한 가능하며 `Babel`, `TypeScript`의 트랜스파일링 지원도 강력하다.

이 중 `React`와 `Vue`는 기본 프로젝트 생성 도구에 `Webpack`이 포함되어 있을 정도로 인정받은 현황이다.

## `Vite`의 장점

`Vite`는 `dependencies`, `source code` 두 가지 카테고리로 나누어 개발 서버를 실행하는 방식으로 기존보다 매우 빠른 구동 속도를 강조하고 있다.

> 다른 빌드 도구와의 차이점: https://vitejs-kr.github.io/guide/comparisons.html

또한, `Rollup`의 플러그인 인터페이스에서 특정 옵션을 추가한 형태로 구성하여 다양한 플러그인을 제공받을 수 있고, 손 쉽게 플러그인을 만들어 확장할 수 있다.

### 기존의 문제점

기존의 도구들은 거대한 프로젝트에서 `Cold-Starting`(최초로 실행되어 이전에 캐싱한 데이터가 없는 경우) 방식으로 병목 현상(개발 서버를 실행하는 데에 굉장히 오래 걸림)으로 인해 속도가 아쉬운 단점이 있었다.

또한 `Webpack` 같은 경우에는 초기 설정이 굉장히 어렵기 때문에 러닝커브가 가파르다는 단점도 있다. 거기다가 `Webpack`의 문서는 그다지 친절한 편은 아니며, 버전에 따라 다른 종속성 충돌 등의 문제가 발생할 수도 있다.

### 러닝 커브

`Vite`는 설정하기가 몹시 간편하다.

- `Vite` 설정

  ```js
  import { defineConfig } from "vite";
  import react from "@vitejs/plugin-react";

  // https://vitejs.dev/config/
  export default defineConfig({
    plugins: [react()],
  });
  ```

- `Webpack` 설정

  ```js
  const path = require("path");

  const HtmlWebpackPlugin = require("html-webpack-plugin");

  module.exports = {
    entry: {
      "js/app": ["./src/App.jsx"],
    },
    output: {
      path: path.resolve(__dirname, "dist/"),
      publicPath: "/",
    },
    module: {
      rules: [
        {
          test: /\.(js|jsx)$/,
          use: ["babel-loader"],
          exclude: /node_modules/,
        },
      ],
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: "./src/index.html",
        filename: "index.html",
      }),
    ],
  };
  ```

당장 위 두 개의 간단한 `React` 애플리케이션 설정만 비교하여도 설정의 난이도를 체감할 수 있을 것이다.

### `ESBuild`

개발 시 그 내용이 바뀌지 않을 `dependencies` 소스들은 매번 번들링 과정을 거치지 않고 "사전 번들링"을 통해 그 속도를 축약시켰다.

또한 이 사전 번들링에는 [`ESbuild`](https://esbuild.github.io/)라는 도구를 활용하는데, `Go`로 작성된 `ESBuild`는 기존 번들러 대비 10~100배 빠른 번들링 속도를 가져올 수 있다.

### `Native ESM`

`JSX`, `CSS`, 컴포넌트와 같이 컴파일링이 필요하고 수정이 매우 잦은 소스들은 [`Native ESM`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)을 활용해 브라우저를 곧 번들러로 동작시켜 브라우저의 판단 아래 특정 모듈에 대한 소스 코드를 요청하면 이를 전달하는 방식으로 제공하고 있다.

이를 통해 `HMR` 도한 지원하고 있으며, 전 과정에서 완벽하게 `ESM`을 이용하기 때문에 앱 사이즈가 커져도 갱신 시간에는 영향을 끼치지 않음.

### `HTTP` 헤더를 통한 캐싱

필요에 따라 소스 코드는 캐시되도록 함으로써, 한 번의 요청이라도 덜 하도록 조정. 속도를 더욱 향상시켰다.

### 지속적, 빠르게 업데이트되는 도구

`Native ESM`은 비교적 최근에 지원되기 시작했기 때문에 실제 배포시 오류가 발생할 수 있는 문제로 인해 번들링 과정을 거쳐야 하며, 번들링 시에 `ESBuild`를 사용하기엔 `Code Splitting` 및 `CSS` 처리의 미흡함으로 인해 계속해서 업데이트를 진행하고 있다.

## 적용

`Webpack` 같은 경우에는, 어떠한 프레임워크(`React` 등)에서 설정을 제공하지 않는 이상 직접 설정 파일을 작성하며 적용해야 하는 어려움이 있었는데, `Vite`는 `Vite` 스스로가 여러가지 웹 프레임워크에 대한 기본 설정을 제공하기 때문에 간편하게 사용이 가능하다.

또한, `Vite`가 제공하는 템플릿 이외에 [`Awesome-Vite`](https://github.com/vitejs/awesome-vite#templates)라는 프로젝트로 여러가지 템플릿(`boilerplate`)을 확인할 수 있다.

### `Vite` 프로젝트 설치

- 프로젝트 설치 명령어

  - 기본 명령어

    ```shell
    # npm
    npm create vite@latest

    # yarn
    yarn create vite

    # pnpm
    pnpm create vite
    ```

  - 명시 명령어

    ```shell
    # npm 6.x
    npm create vite@latest <projectname> --template <template>

    # npm 7+, '--'를 반드시 붙여주세요
    npm create vite@latest <projectname> -- --template <template>

    # yarn
    yarn create vite <projectname> --template <template>

    # pnpm
    pnpm create vite <projectname> -- --template <template>
    ```

- 지원하는 `<template>`

  | `JavaScript` | `TypeScript` |
  | :----------: | :----------: |
  |  `vanilla`   | `vanilla-ts` |
  |    `vue`     |   `vue-ts`   |
  |   `react`    |  `react-ts`  |
  |   `preact`   | `preact-ts`  |
  |    `lit`     |   `lit-ts`   |
  |   `svelte`   | `svelte-ts`  |

## 결론

`Vite`는 최신 기술이니만큼 기존 `Webpack`을 뛰어넘을 새로운 번들링 도구로써 활약할 것이 예상된다. 지금 당장은 `Webpack`에 대한 적용이 좀 더 편한 상태일진 모르지만, `Vite`를 프로젝트에 적용해보고, 상세한 설정을 건드리면서 `Webpack` -> `Vite`로 프로젝트를 변경하여 적용했을 때, 두 번들링 도구를 직접 비교해보면 몸소 장단점을 체감할 수 있을 것 같다.

이러한 장단점을 경험해본 후 적당한 프로젝트에 녹아낼 수 있게 되면 생산성 향상에 크게 기여할 것으로 기대된다.
