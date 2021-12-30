# `Core Graphics`

> `Objective-C`의 `Core Graphics` 라이브러리에 관한 조사.

## 목차

- [`Quartz Display Service`](#quartz-display-service)
- [`Quartz Event Service`](#quartz-event-service)
- [`Quartz Window Service`](#quartz-window-service)

---

## `Quartz Display Service`

- 디스플레이 하드웨어를 구성하고 제어하기 위해 직접 액세스.

### 주요 기능

- 디스플레이의 크기, 표시 모드 등.
- 디스플레이 독점 사용을 위한 캡쳐.
- 디스플레이 페이드 효과 수행.
- 디스플레이 미러링 활성화.
- 마우스 커서 제어.

### 사용 예제

- `CGDirectDisplayID`

  > 데이터 타입

  - 디스플레이의 고유 식별자.

- `CGMainDisplayID`

  > 함수

  - 메인 디스플레이의 `CGDirectDisplayID`를 반환.

  ```objective-c
  CGDirectDisplayID mainId = CGMainDisplayID();
  ```

- `CGDisplayBounds`

  > 함수

  - 디스플레이의 `CGRect`를 반환.
  - 매개 변수
    - `displayId`: 가져올 디스플레이의 `CGDirectDisplayID`

  ```objective-c
  CGDirectDisplayID mainId = CGMainDisplayID();
  CGRect rect = CGDisplayBounds(mainId);
  NSLog(@"%@", NSStringFromRect(rect));
  ```

#### 참고

- [Apple Developer - Quartz Display Service](https://developer.apple.com/documentation/coregraphics/quartz_display_services?language=objc)
- [Apple Developer - CGDirectDisplayID](https://developer.apple.com/documentation/coregraphics/cgdirectdisplayid?language=objc)
- [Apple Developer - CGMainDisplayID](https://developer.apple.com/documentation/coregraphics/1455620-cgmaindisplayid?language=objc)
- [Apple Developer - CGDisplayBounds](https://developer.apple.com/documentation/coregraphics/1456395-cgdisplaybounds?language=objc)

---

## `Quartz Event Service`

- 이벤트 탭 관리 기능 제공.
- 낮은 수준의 사용자 입력 이벤트 스트림을 관찰하고 변경.

### 주요 기능

- 이벤트 모니터링. [`CGEventTap`](https://developer.apple.com/documentation/coregraphics/1454426-cgeventtapcreate?language=objc)
- `Keyboard`, `Mouse`, `Scroll` 등의 이벤트 관리.
- 현재 키가 눌려져 있는지, 마우스가 클릭되어 있는지 확인 가능.
- 이벤트 탭을 통해 외부 이벤트 후크 설정.

### 사용 예제

- `CGEventCreateMouseEvent`

  > 함수

  - 새로운 마우스 `CGEventRef` 반환.
  - 이벤트를 생성할 수 없는 경우 `NULL` 반환.
  - 더이상 이벤트가 필요하지 않으면 `CFRelease`를 사용해 이벤트 해제.
  - 매개변수
    - `source`: 다른 이벤트 소스 또는 `NULL`
    - `mouseType`: 마우스 이벤트 유형 [`CGEventType`](https://developer.apple.com/documentation/coregraphics/cgeventtype?language=objc)
    - `mouseCursorPosition`: 모든 디스플레이에서 마우스 커서의 위치 `CGPoint`
    - `mouseButton`: 상태를 변경하는 버튼 [`CGMouseButton`](https://developer.apple.com/documentation/coregraphics/cgmousebutton?language=objc)

  ```objective-c
  CGEventRef event = CGEventCreateMouseEvent(NULL, kCGEventMouseMoved, CGPointMake(2570, 500), kCGMouseButtonLeft);
  CGEventPost(kCGHIDEventTap, event);
  ```

- `CGEventCreateKeyboardEvent`

  > 함수

  - 새로운 키보드 `CGEventRef` 반환.
  - 이벤트를 생성할 수 없는 경우 `NULL` 반환.
  - 더이상 이벤트가 필요하지 않으면 `CFRelease`를 사용해 이벤트 해제.
  - 보조 키를 포함하기 위해선 필요한 모든 키를 순서대로 입력.
  - 매개변수
    - `source`: 다른 이벤트 소스 또는 `NULL`
    - `virtualKey`: [가상 키 코드](http://daplus.net/macos-mac-%EA%B0%80%EC%83%81-%ED%82%A4-%EC%BD%94%EB%93%9C-%EB%AA%A9%EB%A1%9D%EC%9D%80-%EC%96%B4%EB%94%94%EC%97%90%EC%84%9C-%EC%B0%BE%EC%9D%84-%EC%88%98-%EC%9E%88%EC%8A%B5%EB%8B%88%EA%B9%8C/)
    - `keyDown`: 키보드 이벤트 유형 [`CGEventType`](https://developer.apple.com/documentation/coregraphics/cgeventtype?language=objc)
      - `true`이면 `Keydown`, `false`이면 `Keyup`

  ```objective-c
  CGEventRef shiftDownEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)56, true);
  CGEventRef shiftUpEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)56, false);

  CGEventRef zDownEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)6, true);
  CGEventRef zUpEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)6, false);

  CGEventPost(kCGHIDEventTap, shiftDownEvent);
  CGEventPost(kCGHIDEventTap, zDownEvent);
  CGEventPost(kCGHIDEventTap, zUpEvent);
  CGEventPost(kCGHIDEventTap, shiftUpEvent);
  ```

- `CGEventCreateScrollWheelEvent`

  > 함수

  - 새로운 마우스 스크롤 `CGEventRef` 반환.
  - 이벤트를 생성할 수 없는 경우 `NULL` 반환.
  - 더이상 이벤트가 필요하지 않으면 `CFRelease`를 사용해 이벤트 해제.
  - 매개 변수
    - `source`: 다른 이벤트 소스 또는 `NULL`
    - `units`: 스크롤 이벤트 측정 단위 [`CGScrollEventUnit`](https://developer.apple.com/documentation/coregraphics/cgscrolleventunit?language=objc)
    - `wheelCount`: 마우스 스크롤 장치 수(최대 3개)
      - 2로 하면 `x`축과 `y`축 스크롤 가능.
    - `wheelValue`: 움직임 값.
      - 일반적으로 -10 ~ +10
      - 최대 2개의 값(연속 매개변수), 첫 번째 값 `y` 축, 두 번째 값 `x` 축.
      - 양수가 위로 향하는 값.

  ```objective-c
  CGEventRef scrollEvent = CGEventCreateScrollWheelEvent(NULL, kCGScrollEventUnitPixel, 2, 10, 10);
  CGEventPost(kCGHIDEventTap, scrollEvent);
  ```

- `CGEventGetLocation`

  > 함수

  - 모든 디스플레이에서 마우스 커서의 위치 `CGPoint` 반환.
  - 매개변수
    - `event`: 찾을 마우스 `CGEventRef`

  ```objective-c
  CGEventRef event = CGEventCreate(NULL);
  CGPoint point = CGEventGetLocation(event);
  NSLog(@"%@", NSStringFromPoint(point));
  ```

- `CGEventSetFlags`

  > 함수

  - 이벤트 플래그를 설정.
  - `Shift`, `Command`, `Control` 등의 키를 이벤트와 함께 사용할 수 있게 해줌.
  - 매개변수
    - `event`: 플래그 설정할 이벤트
    - `flags`: 새 플래그 [`CGEventFlags`](https://developer.apple.com/documentation/coregraphics/cgeventflags?language=objc)

  ```objective-c
  CGEventRef keyEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)1, true);
  CGEventSetFlags(keyEvent, kCGEventFlagMaskShift);
  CGEventPost(kCGHIDEventTap, keyEvent);
  ```

- `CGEventPost`

  > 함수

  - 이벤트를 실행.
  - 매개변수
    - `tap`: 이벤트를 게시할 위치 [`CGEventTapLocation`](https://developer.apple.com/documentation/coregraphics/cgeventtaplocation?language=objc)
    - `event`: 실행할 이벤트 [`CGEventRef`]

  ```objective-c
  CGEventRef keyEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)1, true);
  CGEventSetFlags(keyEvent, kCGEventFlagMaskShift);
  CGEventPost(kCGHIDEventTap, keyEvent);
  ```

- `CGEventPostToPid`

  > 함수

  - 지정된 프로세스에 이벤트를 실행.
  - 매개변수
    - `pid`: 전송할 프로세스의 `id`
    - `event`: 전송할 [`CGEventRef`]

  ```objective-c
  NSRunningApplication *frontMostApp = [[NSWorkspace sharedWorkspace] frontmostApplication];
  CGEventRef keyEvent = CGEventCreateKeyboardEvent(NULL, (CGKeyCode)1, true);
  CGEventSetFlags(keyEvent, kCGEventFlagMaskShift);
  CGEventPostToPid(frontMostApp.processIdentifier, keyEvent);
  ```

#### 참고

- [Apple Developer - Quartz Event Service](https://developer.apple.com/documentation/coregraphics/quartz_event_services?language=objc)
- [Apple Developer - CGEventCreateMouseEvent](https://developer.apple.com/documentation/coregraphics/1454356-cgeventcreatemouseevent?language=objc)
- [Apple Developer - CGEventCreateKeyboardEvent](https://developer.apple.com/documentation/coregraphics/1456564-cgeventcreatekeyboardevent?language=objc)
- [Apple Developer - CGEventCreateScrollWheelEvent](https://developer.apple.com/documentation/coregraphics/1541327-cgeventcreatescrollwheelevent?language=objc)
- [Apple Developer - CGEventGetLocation](https://developer.apple.com/documentation/coregraphics/1455788-cgeventgetlocation?language=objc)
- [Apple Developer - CGEventSetFlags](https://developer.apple.com/documentation/coregraphics/1455044-cgeventsetflags?language=objc)
- [Apple Developer - CGEventPost](https://developer.apple.com/documentation/coregraphics/1456527-cgeventpost?language=objc)
- [Apple Developer - CGEventPostToPid](https://developer.apple.com/documentation/coregraphics/1454804-cgeventposttopid?language=objc)

---

## `Quartz Window Service`

- 애플리케이션 윈도우에 대한 정보를 제공.

### 주요 기능

- 사용자가 실행 중인 애플리케이션 윈도우의 정보 관찰.
- 창의 내용을 이미지로 생성.

### 사용 예제

#### 참고

- [Apple Developer - Quartz Window Service](https://developer.apple.com/documentation/coregraphics/quartz_window_services?language=objc)
