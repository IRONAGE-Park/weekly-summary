# `Appkit`

> `Objective-C`의 `Appkit` 라이브러리에 관한 조사.

## 목차

- [`NSApplication`](#nsapplication)
- [`NSWorkspace`](#nsworkspace)
- [`NSRunningApplication`](#nsrunningapplication)

---

## `NSApplication`

- 애플리케이션에서 사용되는 기본 이벤트 루프와 리소스를 관리하는 객체.
- 모든 애플리케이션은 단일 `NSApplication` 인스턴스를 사용하여 통제.

> `main` 객체.

### 주요 기능

- 애플리케이션을 실행 후, 동작, 이벤트, 애플리케이션 윈도우 관리, 알림 전달 등의 전반적인 기능 관리.
- 애플리케이션 활성화, 로그인 시 재실행, 원격 알림 관리.
- 애플리케이션 모양 관리, 윈도우, 패널, 모달 및 메뉴 관리.
- `UI` 레이아웃 방향 관리.
- `Dock`, `Touchbar` 관리.
- 도움말, 주의, 등의 서비스 관리.

### 사용 예제

#### 참고

- [Apple Developer - `NSApplication`](https://developer.apple.com/documentation/appkit/nsapplication?language=objc)

---

## `NSWorkspace`

- 다른 애플리케이션을 실행하고 다양한 파일 처리 서비스를 수행할 수 있는 작업 공간.
- 애플리케이션 당 하나의 공유 `NSWorkspace` 객체 존재.

### 주요 기능

- 파일 및 장치를 열고 조작하고 정보를 얻음.
- 파일 시스템, 장치 및 사용자 데이터베이스에 대한 변경 사항 추적.
- 파일에 대한 `Finder` 정보를 가져오고 설정.
- 애플리케이션을 실행.

### 사용 예제

- `openURL:`

  > 인스턴스 메서드

  - 지정된 `NSURL`의 위치를 염.
  - `url`: 지정할 `NSURL`.

  ```objective-c
  NSURL *url = [NSURL URLWithString:@"https://github.com"];
  [[NSWorkspace sharedWorkspace] openURL:url];
  ```

- `selectFile:inFileViewerRootedAtPath:`

  > 인스턴스 메서드

  - 지정된 `NSURL`의 위치에 있는 파일을 선택함.
  - `selectFile`: 선택할 파일 경로.
  - `inFileViewerRootedAtPath`: 파일 뷰어에 사용할 경로.(빈 문자열은 기본 뷰어)

  ```objective-c
  [[NSWorkspace sharedWorkspace] selectFile:@"/bin" inFileViewerRootedAtPath:@""];
  ```

- `frontmostApplication`

  > 인스턴스 프로퍼티

  - 현재 맨 앞에 띄워져 있는 애플리케이션을 반환함.

  ```objective-c
  NSRunningApplication *frontMostApp = [[NSWorkspace sharedWorkspace] frontmostApplication];
  ```

- `runningApplications`

  > 인스턴스 프로퍼티

  - 현재 실행 중인 애플리케이션의 배열을 반환함.

  ```objective-c
  NSArray *runningApps = [[NSWorkspace sharedWorkspace] runningApplications]
  ```

- `requestAuthorizationOfType:completionHandler`

  > 인스턴스 메서드

  - 권한 있는 파일 작업을 수행하기 위한 권한 부여를 요청.
  - `type`: 수행할 파일 `NSWorkspaceAuthorizationType`.
  - `completionHandler`: 권한 부여 요청이 완료될 때 호출할 핸들러.
    - `authorization`: 부여된 `NSWorkspaceAuthorization`.
    - `error`: 승인되지 않은 `NSError`.

  ```objective-c
  void (^test)(NSWorkspaceAuthorization *auth, NSError *error);
  test = ^(NSWorkspaceAuthorization *auth, NSError *error) {
    NSLog(@"%@ %@", auth, error);
  };

  [[NSWorkspace sharedWorkspace] requestAuthorizationOfType:NSWorkspaceAuthorizationTypeReplaceFile completionHandler:test];
  ```

- `openApplicationAtURL:configuration:completionHandler`

  > 인스턴스 메서드

  - 지정된 `NSURL`의 위치에 있는 앱을 샐행하고, 비동기적으로 보고.
  - `applicationURL`: 앱의 위치를 나타내는 `NSURL`.
  - `configuration`: 앱 실행 방법을 나타내는 `NSWorkspaceOpenConfiguration`.
  - `completionHandler`: 결과와 비동기적으로 호출할 핸들러.
    - `app`: 시작된 `NSRunningApplication`.
    - `error`: 실패 `NSError`.

  ```objective-c
  NSURL *INVAIZStudio = [NSURL fileURLWithPath:@"/Applications/KakaoTalk.app"];

  void (^test)(NSRunningApplication *app, NSError *error);
  test = ^(NSRunningApplication *app, NSError *error) {
    NSLog(@"%@ %@", app, error);
  };

  [[NSWorkspace sharedWorkspace] openApplicationAtURL:INVAIZStudio configuration:[NSWorkspaceOpenConfiguration configuration] completionHandler:test];
  ```

#### 참고

- [Apple Developer - `NSWorkspace`](https://developer.apple.com/documentation/appkit/nsworkspace?language=objc)

---

## `NSRunningApplication`

- 단일 애플리케이션 인스턴스에 대한 정보를 조작하고 제공할 수 있는 객체.

### 주요 기능

- `processIdentifier`를 통해 애플리케이션을 얻어낼 수 있음.
- 현재 애플리케이션을 찾을 수 있음.
- 애플리케이션 활성화, 숨기기, 종료 등의 조작 가능.
- 애플리케이션의 `localizedName`, `icon`, `bundleIdentifier`, `bundleURL`, `executableArchitecture`, `executableURL`, `launchDate`, `finishedLaunching`, `processIdentifier`, `ownsMenuBar`를 확인 가능.

### 사용 예제

#### 참고

- [Apple Developer - `NSRunningApplication`](https://developer.apple.com/documentation/appkit/nsrunningapplication?language=objc)
