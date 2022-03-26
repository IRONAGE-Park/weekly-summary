# `@emotion`

## `@emotion`에서 리스트 간 간격 부여

- `Sass`에서는 첫 번째 요소에는 간격을 부여하지 않은 채로 리스트간 간격을 부여하기 위해서는 다음과 같이 부모 선택자(`&`)를 활용하였다.

  ```scss
  .class-name {
    & + & {
      margin-left: 10px;
    }
  }
  ```

- 이 문법은 `styled-components`에서는 정상 적용되었으나, `@emotion`으로 모듈을 변환하는 도중 몇몇 스타일이 제대로 적용되지 않는 문제를 확인할 수 있었다.
- 리스트가 있을 때, 모든 리스트 요소에 같은 스타일을 적용하면 정상이지만, 한 가지가 다른 요소(선택 표시, 토글 등)로 적용될 경우, 그의 앞 뒤 요소에 간격이 부여되지 않는 것을 확인했다.

  - `styled-components`에서는 다음과 같이 `기존 스타일 클래스 네임 + 추가 스타일 클래스 네임`과 같은 방식으로 이전 클래스, 추가 클래스를 분할하여 상속받는 방식으로 관리한다.
    - `SCSS` 구조
      ```scss
      .item {
        & + & {
          margin-top: 12px;
        }
      }
      .selected {
        border: solid 1px #06f;
      }
      ```
    - 렌더 컴포넌트 형태
      ```html
      <li class="item"></li>
      <li class="item selected"></li>
      <li class="item"></li>
      <li class="item"></li>
      ```
      > 따라서, `.item` 간의 `& + &` 문법에는 영향을 주지 않아 그대로 적용이 잘 되었다.
  - 하지만 `@emotion`에서는 `새로운 스타일 클래스 네임`과 같은 방식으로 추가된 클래스에 기존 스타일 요소를 모두 합친 새로운 이름의 클래스를 생성하는 방식으로 관리한다.
    - `SCSS` 구조
      ```scss
      .item {
        & + & {
          margin-top: 12px;
        }
      }
      .item-selected {
        border: solid 1px #06f;
        & + & {
          margin-top: 12px;
        }
      }
      ```
    - 렌더 컴포넌트 형태
      ```html
      <li class="item"></li>
      <li class="item-selected"></li>
      <li class="item"></li>
      <li class="item"></li>
      ```
      > 따라서, `.item` 사이에 `.item-selected`가 포함되어 `& + &` 적용되지 않았다.

- `@emotion`의 [`Styled Components`](https://emotion.sh/docs/styled) 사용 방식에서 [`Targeting anotion emotion component`]를 사용하기 위해 적용한 [`@emotion/babel-plugin`](https://emotion.sh/docs/@emotion/babel-plugin)에 의한 문제라고 생각했으나, 해당 모듈을 제거한 후에도 똑같은 현상이 유지 됐다.
- 그 후, [`@emotion Github issues`](https://github.com/emotion-js/emotion/issues/1922)에 해당 문제가 올라와있었고, 수정하지 않겠다는 답변을 확인 후 작성되어 있는 대안을 적용하였다.

  ```scss
  &:not(:first-child) {
    margin-left: 12px;
  }
  ```

- 첫 번째 요소가 아닌 요소에 모든 스타일을 적용하는 것이었는데, 잘 적용이 되는 것을 확인하였지만 위의 최종 `SCSS` 구조를 비교해봐도 `@emotion`의 방식이 좋아보이진 않는데 뭔가 다른 이유나 이슈가 있는지 궁금하다.
