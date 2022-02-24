# `React Router`의 사용 경험

- [React Router](https://reactrouter.com)
- [v6 docs](https://reactrouter.com/docs/en/v6)
- [v5 docs](https://v5.reactrouter.com/web/guides/quick-start)

## `v5`에서 `v6` 버전으로 마이그레이션

### 변경 사항

- `Redirect` 컴포넌트를 `Switch` 컴포넌트 내에서 사용할 수 없도록 제거
- `Switch` 컴포넌트를 `Routes` 컴포넌트로 변경
- `Routes`, `Route`, `Link`가 모두 상대 경로를 사용
- `Route`의 `exact` `props` 제거
  - 와일드카드인 `*`을 사용해 모든 하위 경로 매칭 가능
  - 중첩 구조를 렌더링하기 위해선 `children` 컴포넌트에서 `<Outlet />` 렌더
- `Route`에서 `element` `props`를 통해 `JSX` 문법 전달 가능
- `Route`에서 `path` `props` 정규 표현식 지원 종료
- `react-router-config` 모듈을 `useRoutes`를 통해 구현
- `useHistory`를 `useNavigate`로 변경
- `Link` 태그에 `component` `props` 제거
- `NavLink`의 `exact` `props`를 `end`로 변경
- `NavLink`에서 `activeClassName`, `activeStyle` `props` 제거
  - `style` 내에서 `isActive`가 포함된 객체를 매개변수로 받는 함수 전달하여 사용 가능
- `SSR`에 사용되는 `StaticRouter`의 위치가 `react-router-dom/server`로 이동

## 마이그레이션 하기 위한 일반적인 프로세스

- [공식 문서](https://reactrouter.com/docs/en/v6/upgrading/v5)

1. `React v16.8` 이상으로 업그레이드
2. `React Router v5.1`으로 업그레이드
   - `<Switch />` 내의 `<Redirect />` 제거
   - `<Switch />` 를 `<Routes />`로 변경
3. `React Router v6`으로 업그레이드

### 경로 관리

#### `v5`

- 기존 `React Router`를 사용할 때, 전역적으로 경로를 관리하기 위해 `Paths.ts` 파일을 생성 후, 해당 파일에 프로젝트 내의 모든 경로 데이터를 기입하여 사용하였다.

  - `Paths.ts`

  ```ts
  const Paths = {
    index: "/",
    signin: "/signin",
    signup: "/signup",
    main: {
      home: {
        index: "/home",
      },
      collections: {
        function_executed: "/collections/function_executed",
        personal_executed: "/collections/personal_executed",
      },
      community: {
        index: "/community",
      },
    },
    api: process.env.REACT_APP_API_URL,
  };
  ```

- 위와 같은 파일을 작성하고, 경로를 사용하는 컴포넌트 및 객체(`Route`, `Link`, `history` 등)에서 불러와 사용하면, 혹여나 경로가 변경될 경우 리팩터링하기 편리한 방법이다.
- 또한 `API` 요청 또한, 외부가 아닌 전용 서버의 경우 일반적으로 `development` 모드의 서버 주소와 `production` 모드의 서버 주소가 다를 수 있으므로 편하게 관리할 수 있다.
- 단점으로는 절대 경로만 기입하여 작성하기 때문에 상대 경로에 대한 지원이 미흡하다는 점이 있으며, 이로 인해 똑같은 주소를 중복하여 작성해야 한다.

#### `v6`

- `v6` 부터는 `Route`에서 절대 경로를 지원하지 않는 듯 했다.
- 따라서 기존에 사용하던 방식으로는 중첩 라우팅을 구현할 수 없었고 루트 컴포넌트에서 모든 라우팅을 진행해야 절대 경로를 사용할 수 있었다.
- 상대 경로를 사용하면 확실히 상기 기술한 단점을 해결할 수 있지만, 도저히 리팩터링하기 위해 하나로 관리하는 방법이 생각나지 않았다.
  - 같은 경로를 여러 곳에서 호출하면 상대 경로가 달라질 수 있기 때문이다.
- 이를 `useRoutes`를 사용하여 해결할 수 있을거 같아 새로운 구조를 작성하였다.

  - `Pathts.tsx`

  ```tsx
  const Paths = {
    index: "/",
    signin: "/signin",
    signup: "/signup",
    main: {
      home: {
        index: "/home",
      },
      collections: {
        function_executed: "/collections/function_executed",
        personal_executed: "/collections/personal_executed",
      },
      community: {
        index: "/community",
      },
    },
    api: process.env.REACT_APP_API_URL,
  };

  export const RouteObjects: RouteObject[] = [
    {
      path: Paths.index,
      element: <Outlet />,
      children: [
        {
          path: Paths.signin,
          element: <SignInPage />,
        },
        {
          path: Paths.signup,
          element: null,
        },
        {
          path: Paths.index,
          element: (
            <>
              <MainRouter />
              <Outlet />
            </>
          ),
          children: [
            { index: true, element: <MainPage /> },
            { path: Paths.main.home.index, element: <HomePage /> },
            { path: Paths.main.community.index, element: <CommunityPage /> },
            {
              path: Paths.main.collections.function_executed,
              element: <FunctionExecutedCollectionsPage />,
            },
            {
              path: Paths.main.collections.personal_executed,
              element: <PersonalExecutedCollectionsPage />,
            },
          ],
        },
      ],
    },
  ];
  ```

  - `App.tsx`

  ```tsx
  const App = () => {
    const routes = useRoutes(RouteObjects);

    return (
      <StyleApp>
        <Header />
        <DialogPortals children={<DialogContainer />} tagId="dialog" />
        <StyleMain>{routes}</StyleMain>
      </StyleApp>
    );
  };
  ```

- `exact`가 없는 상태에서, 중첩 구조의 하위 컴포넌트를 렌더하기 위해선 `<Outlet />` 컴포넌트를 렌더해야 한다.
- `Link`, `Route` 태그 모두 절대 경로를 사용하여 `Paths`를 전역적으로 동시에 관리할 수 있게 되었다.

> 하지만 `RouteObjects`를 보면 최상단과 곳곳에 `<Outlet />`를 호출한 부분이 마음에 들지 않는다.
