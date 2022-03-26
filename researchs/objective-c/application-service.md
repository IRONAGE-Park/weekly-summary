# `Application Service`

> `Objective-C`의 `Application Service` 라이브러리에 관한 조사.

- `Carbon` 애플리케이션에 필수적인 여러 서비스를 포함하는 애플리케이션 서비스 프레임워크에 대한 `API` 참조를 제공.

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

- `AXUIElementRef`

  > 데이터 타입

  - 애플리케이션에 액세스 가능한 사용자 인터페이스 요소(`AXUIElement`) 참조값
  - 참조값을 통해 인터페이스 요소의 정보, 위치 등 세부 정보를 관리하는 메세지 요청 및 응답, 알림 표시 가능.

  - 문서: [Apple Developer - `AXUIElementRef`](https://developer.apple.com/documentation/applicationservices/axuielementref?language=objc)

- `AXIsProcessTrusted`

  > 함수

  - 현재 실행하는 애플리케이션이 다른 개체에 접근하는 것을 신뢰하는 여부, `Boolean`을 반환.

  ```objective-c
  Boolean isTrusted = AXIsProcessTrusted(); // 접근 신뢰 여부를 가져옴
  ```

  - 문서: [Apple Developer - `AXIsProcessTrusted`](https://developer.apple.com/documentation/applicationservices/1460720-axisprocesstrusted?language=objc)

- `AXUIElementCreateApplication`

  > 함수

  - `Process`의 `ID`를 통해 애플리케이션에 대한 최상위 액세스 가능 `AXUIElementRef`를 만들고 반환.
  - 매개변수
    - `pid`: 액세스 가능 `AXUIElementRef`를 생성할 `Process`의 `ID`

  ```objective-c
  CGWindowListOption listOptions = kCGWindowListExcludeDesktopElements;
  CFArrayRef windowList = CGWindowListCopyWindowInfo(listOptions, kCGNullWindowID);
  // 현재 실행되고 있는 애플리케이션 창 목록을 가져옴
  NSArray *array = CFBridgingRelease(windowList);
  // CFArrayRef를 NSArray로 변환
  for (NSDictionary *info in array) {
    NSNumber *processId = info[(id)kCGWindowOwnerPID]; // process id 가져옴
    NSRunningApplication *app = [NSRunningApplication runningApplicationWithProcessIdentifier:[processId intValue]];
    // process id를 통해 NSRunningApplication 생성
    if (app.active) {
      // 현재 NSRunningApplication이 활성화 되어 있는지 확인
      AXUIElementRef element = AXUIElementCreateApplication(app.processIdentifier);
      // 활성화된 애플리케이션의 AXUIElementRef 생성
    }
  }
  ```

  - 문서: [Apple Developer - `AXUIElementCreateApplication`](https://developer.apple.com/documentation/applicationservices/1459374-axuielementcreateapplication?language=objc)

- `AXUIElementCopyAttributeNames`

  > 함수

  - `AXUIElementRef`가 지원하는 모든 속성의 이름 `CFString` 목록을 `CFArrayRef`를 통해 전달.
  - 속성의 종류

  - 매개변수
    - `element`: 속성을 확인할 `AXUIElementRef`
    - `*names`: 속성의 이름이 포함된 배열을 전달할 `CFArrayRef`(포인터)

  ```objective-c
  pit_t frontMostProcessId = [[[NSWorkspace shardWorkspace] frontmostApplication] processIdentifier];
  AXUIElementRef frontMostElement = AXUIElementCreateApplication(frontMostProcessId);
  // 현재 활성화된 애플리케이션의 AXUIElementRef 생성

  CFArrayRef attributes;
  AXUIElementCopyAtrributeNames(frontMostElement, &attributes);
  // AXUIElementRef가 갖고 있는 모든 속성의 이름 목록을 가져옴
  ```

  - 문서: [Apple Developer - `AXUIElementCopyAttributeNames`](https://developer.apple.com/documentation/applicationservices/1459475-axuielementcopyattributenames?language=objc)

- `AXUIElementCopyAttributeValues`

  > 함수

  - 지정된 인덱스부터 시작하여 `AXUIElementRef`가 갖고 있는 지정된 속성의 값 목록을 `CFArrayRef`를 통해 전달.
  - 매개변수
    - `element`: 속성의 값 목록을 확인할 `AXUIElementRef`
    - `attribute`: 목록을 확인할 속성의 이름을 지정한 `CFStringRef`
    - `index`: 시작할 `CFIndex` | `ìnt`
    - `maxValues`: 최대 가져올 목록의 길이 `CFIndex` | `int`
    - `*values`: 속성의 값 목록이 저장된 배열을 전달할 `CFArrayRef`(포인터)

  ```objective-c
  pit_t frontMostProcessId = [[[NSWorkspace shardWorkspace] frontmostApplication] processIdentifier];
  AXUIElementRef frontMostElement = AXUIElementCreateApplication(frontMostProcessId);
  // 현재 활성화된 애플리케이션의 `AXUIElementRef` 생성

  CFArrayRef childList;
  AXUIElementCopyAttributeValues(frontMostElement, CFSTR("AXChildren"), 0, 9999, &childList);
  // AXUIElementRef의 AXChildren 속성의 값 목록을 가져옴
  ```

  - 문서: [Apple Developer - `AXUIElementCopyAttributeValues`](https://developer.apple.com/documentation/applicationservices/1462060-axuielementcopyattributevalues?language=objc)

- `AXUIElementCopyAttributeValue`

  > 함수

  - `AXUIElementRef`가 갖고 있는 지정된 속성의 값을 `CFTypeRef`를 통해 전달.
  - 매개변수
    - `element`: 속성의 값을 확인할 `AXUIElementRef`
    - `attribute`: 값을 확인할 속성의 이름을 지정한 `CFStringRef`
    - `*value`: 속성의 값을 전달할 `CFTypeRef`(포인터)

  ```objective-c
  pit_t frontMostProcessId = [[[NSWorkspace shardWorkspace] frontmostApplication] processIdentifier];
  AXUIElementRef frontMostElement = AXUIElementCreateApplication(frontMostProcessId);
  // 현재 활성화된 애플리케이션의 `AXUIElementRef` 생성

  CFTypeRef role;
  AXUIElementCopyAttributeValue(child, CFSTR("AXRole"), &role);
  // AXRole 속성 값을 가져옴
  ```

  - 문서: [Apple Developer - `AXUIElementCopyAttributeValue`](https://developer.apple.com/documentation/applicationservices/1462085-axuielementcopyattributevalue?language=objc)

- `AXUIElementSetAttributeValue`

  > 함수

  - `AXUIElementRef`가 갖고 있는 지정된 속성의 값을 `CFTypeRef`로 설정(변경)
  - 매개변수
    - `element`: 속성의 값을 설정할 `AXUIElementRef`
    - `attribute`: 값을 설정할 속성의 이름을 지정한 `CFStringRef`
    - `value`: 설정할 속성의 값 `CFTypeRef`

  ```objective-c
  pit_t frontMostProcessId = [[[NSWorkspace shardWorkspace] frontmostApplication] processIdentifier];
  AXUIElementRef frontMostElement = AXUIElementCreateApplication(frontMostProcessId);
  // 현재 활성화된 애플리케이션의 `AXUIElementRef` 생성

  CFArrayRef childList;
  AXUIElementCopyAttributeValues(frontMostElement, CFSTR("AXChildren"), 0, 9999, &childList);
  // AXUIElementRef의 AXChildren 속성의 값 목록을 가져옴

  if (childList != NULL) {
    // 목록이 존재할 때 실행
    CFIndex count = CFArrayGetCount(childList); // 목록의 길이 가져옴
    for (int i = 0; i < count; i++) {
      CFTypeRef child = CFArrayGetValueAtIndex(childList, i); // 목록에서 해당 인덱스의 AXChildren 값
      CFTypeRef role;
      AXUIElementCopyAttributeValue(child, CFSTR("AXRole"), &role);
      NSString *nsRole = CFBridgingRelease(role);
      // 해당 인덱스의 AXChildren의 AXRole 값을 CFTypeRef로 가져온 후, NSString으로 변환

      Boolean isTextField = [nsRole containsString:@"AXTextField"];
      if (isTextField) {
        // AXRole의 이름이 AXTextField일 때 실행
        CFStringRef attribute_name = CFSTR("AXValue");
        CFTypeRef value;
        AXUIElementCopyAttributeValue(child, attribute_name, &value);
        // AXTextField의 AXValue 속성의 값 가져옴

        NSString *updateValue = [NSString stringWithFormat:@"%@ updated.", value];
        AXUIElementSetAttributeValue(child, attribute_name, (__bridge CFStringRef)updateValue);
        // AXTextField의 기존 AXValue 속성의 값 문자열에, " updated."를 추가한 값을
        // AXTextField의 AXValue 속성의 값으로 설정
        // => 기존 값에 " updated." 추가하여 갱신
      }
    }
  }
  ```

  - 문서: [Apple Developer - `AXUIElementSetAttributeValue`](https://developer.apple.com/documentation/applicationservices/1460434-axuielementsetattributevalue?language=objc)

#### 참고

- [Apple Developer - `AXUIElement.h`](https://developer.apple.com/documentation/applicationservices/axuielement_h?language=objc)
- [Apple Developer - 접근성 프로그래밍 가이드](https://developer.apple.com/library/archive/documentation/Accessibility/Conceptual/AccessibilityMacOSX/index.html#//apple_ref/doc/uid/TP40001078)

---
