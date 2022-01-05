# `Application Service`

> `Objective-C`의 `Application Service` 라이브러리에 관한 조사.

## 목차

- [`AXUIElement.h`](#axuielementh)

---

## `AXUIElement.h`

- `macOS`에서 실행되는 접근 가능한 애플리케이션과 통신하고 제어.
- 애플리케이션에서 액세스 가능한 각 사용자 인터페이스 요소는 `CFTypeRef`인 `AXUIElementRef`로 표시.
- `AXUIElementRef`는 `Core Foundation`의 모든 함수와 함께 사용 가능.

> `<AvailabilityMacros.h>`, `<CoreFoundation/CoreFoundation.h>`, `<ApplicationServices/ApplicationServices.h>`에 포함되어 있음.

### 함수

- 함수에서 반환하는 오류 코드
  - `kAXErrorSuccess`: 성공 시 반환.
  - `kAXErrorInvalidUIElement`: 전달된 `AXUIElementRef`가 잘못됨.
  - `kAXErrorIllegalArgument`: 인수 중 하나가 잘못됨.
  - `kAXErrorCannotComplete`: 메세지에 문제가 있음.(액세스 실패, 응답 없음 등)
  - `kAXErrorAPIDisabled`: 접근성 `API`가 비활성화 됨.

### 사용 예제

- `AXUIElementCreateApplication`

  > 함수

  - `Process`의 `ID`를 통해 애플리케이션에 대한 최상위 액세스 가능 `AXUIElementRef`를 만들고 반환.
  - 매개변수
  - `pid`: 액세스 가능 `AXUIElementRef`를 생성할 `Process`의 `ID`

  ```objective-c
  CGWindowListOption listOptions = kCGWindowListExcludeDesktopElements;
  CFArrayRef windowList = CGWindowListCopyWindowInfo(listOptions, kCGNullWindowID);
  NSArray *array = CFBridgingRelease(windowList);
  for (NSDictionary *info in array) {
    NSNumber *processId = info[(id)kCGWindowOwnerPID];
    NSRunningApplication *app = [NSRunningApplication runningApplicationWithProcessIdentifier:[processId intValue]];
    if (app.active) {
      AXUIElementRef element = AXUIElementCreateApplication(app.processIdentifier);
    }
  }
  ```

  - 문서: [`Apple Developer - AXUIElementCreateApplication`](https://developer.apple.com/documentation/applicationservices/1459374-axuielementcreateapplication?language=objc)

#### 참고

- [`Apple Developer - AXUIElement.h`](https://developer.apple.com/documentation/applicationservices/axuielement_h?language=objc)
- [`Apple Developer - 접근성 프로그래밍 가이드`](https://developer.apple.com/library/archive/documentation/Accessibility/Conceptual/AccessibilityMacOSX/index.html#//apple_ref/doc/uid/TP40001078)

---
