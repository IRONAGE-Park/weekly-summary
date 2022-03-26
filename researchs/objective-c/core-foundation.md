# `Core Foundation`

> `Objective-C`의 `Core Foundation` 프레임워크에 관한 조사.

- `Foundation` 프레임워크와 원할하게 연결된 `Low-level`의 함수, 데이터 유형.
- `Application Service`, 애플리케이션 환경 등 기본적인 소프트웨어 서비스를 제공하는 프레임워크.
- 공통 데이터 유형 추상화, 유니코드 문자열 지원, 다양한 유틸리티 기능 제공.

- 참고: [Apple Developer - `Core Foundation` 디자인 개념](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFDesignConcepts/CFDesignConcepts.html#//apple_ref/doc/uid/10000122i)

## 목차

- [`CFType`](#cftype)
- [`CFString`](#cfstring)
- [`CFArray`](#cfarray)
- [`CFDictionary`](#cfdictionary)
- [`Foundation`과 연결](#foundation과-연결)

---

## `CFType`

- `Core Foundation`의 불투명 유형 데이터 타입.

### 주요 기능

- `Core Foundation` 타입의 조상.
- 다형성 함수
- 데이터 타입 유지 및 해제, 비교 및 검사, 타입에 대한 설명을 얻고 할당자를 가져옴.
- 참조 타입으로 `CFTypeRef` 사용.

### 사용 예제

- `CFRelease`

  - `Core Foundation` 데이터를 해제함.
  - 매개변수
    - `cf`: `NULL`이 아닌 `Core Foundation Type`.

  ```objective-c
  CFTypeRef cf; // CFStringRef도 가능
  CFRelease(cf);
  ```

- `CFRetain`

  - `Core Foundation` 데이터를 유지함.
  - `Core Foundation` 데이터를 다른 곳에서 받고 생성하거나 복사하지 않았을 때, 해당 데이터를 유지해야 함.(안하면 참조 불가)
  - 클로저의 역할을 대신하기 위함.
  - 매개변수
    - `cf`: `NULL`이 아닌 `Core Foundation Type`.

  ```objective-c
  CFTypeRef cf; // CFStringRef도 가능
  CFRetain(cf);
  ```

#### 참고

- [Apple Developer - `CFType`](https://developer.apple.com/documentation/corefoundation/cftype?language=objc)
- [Apple Developer - `CFRelease`](https://developer.apple.com/documentation/corefoundation/1521153-cfrelease?language=objc)
- [Apple Developer - `CFRetain`](https://developer.apple.com/documentation/corefoundation/1521269-cfretain?language=objc)

---

## `CFString`

- 변경할 수 없는 문자열 타입.
- 변경하기 위해선 `CFMutableString` 사용.
- `NSString`과 호환.

### 주요 기능

- 문자열 조작 및 변환 기능 모음.
- 원할한 유니코드 지원 제공, `Cocoa Framework`와 `C/C++` 기반 프로그램 간의 데이터 공유를 용이하게 함.
- 참조 타입으로 `CFStringRef` 사용.

### 사용 예제

- `CTSTR`

  > `#define`

  - 상수 문자열을 컴파일 단계에 `CFStringRef`로 변환.
  - 변경할 수 없는 문자열 혹은 에러가 발생하면 `NULL`.
  - 매개변수
    - `cStr`: 문자열이 생성될 상수, `C` 문자열(큰 따옴표)

  ```objective-c
  CFStringRef string = CFSTR("string");
  ```

- `NSString`을 `CFStringRef`로 변환

  - `(__bridge CFStringRef)ns_string_value` 형태로 사용.

  ```objective-c
  NSString *ns_string_value = @"nsstring";
  CFStringRef string = (__bridge CFStringRef)ns_string_value;
  ```

- 숫자 타입을 `CFStringRef`로 변환

  - `NSString`을 거친 후 변환

  ```objective-c
  double number = 5.3f;
  NSString *number_to_ns_string = [NSString stringWithFormat:@"%f", number];
  CFStringRef number_to_string = (__bridge CFStringRef)number_to_ns_string;
  ```

- `CFStringRef`를 숫자 타입으로 변환

  - `CFStringGetIntValue`

    > 함수

    - `CFStringRef`이 나타내는 `int` 값을 반환.
    - 오류가 있는 경우 `0` 반환.
    - 매개변수
      - `str`: 숫자를 표현하는 `CFStringRef`

    ```objective-c
    SInt32 value = CFStringGetIntValue(CFSTR("-123"));
    ```

  - `CFStringGetDoubleValue`

    > 함수

    - `CFStringRef`이 나타내는 `double` 값을 반환.
    - 오류가 있는 경우 `0.0` 반환.
    - 매개변수
      - `str`: 숫자를 표현하는 `CFStringRef`

    ```objective-c
    double value = CFStringGetDoubleValue(CFSTR("0.123"));
    ```

#### 참고

- [Apple Developer - `CFString`](https://developer.apple.com/documentation/corefoundation/cfstring?language=objc)
- [Apple Developer - `CFSTR`](https://developer.apple.com/documentation/corefoundation/cfstr?language=objc)
- [Apple Developer - `stringWithFormat`](https://developer.apple.com/documentation/foundation/nsstring/1497275-stringwithformat?language=objc)
- [Apple Developer - `CFStringGetIntValue`](https://developer.apple.com/documentation/corefoundation/1541939-cfstringgetintvalue?language=objc)
- [Apple Developer - `CFStringGetDoubleValue`](https://developer.apple.com/documentation/corefoundation/1543293-cfstringgetdoublevalue?language=objc)

---

## `CFArray`

- 변경할 수 없는 정렬된 컬렉션(배열) 타입.
- 변경하기 위해선 `CFMutableArray` 사용.
- `NSArray`와 호환.

### 주요 기능

- 정적 배열을 생성하고 관리.
- 참조 타입으로 `CFArrayRef` 사용.

### 사용 예제

- `CFArrayCreate`

  > 함수

  - 주어진 배열로 새로운 `CFArrayRef` 생성.
  - 매개변수
    - `allocator`: `CFAllocatorRef`. `NULL` 혹은 `kCFAllocatorDefault` 사용 가능.
    - `**values`: 새로운 `CFArrayRef`를 만들기 위한 배열.
    - `numValues`: `**values`에서 새로운 `CFArrayRef`로 복사할 값의 갯수 `CFIndex`.
    - `*callBacks`: 각 값에 사용할 배열의 콜백 함수. `kCFTypeArrayCallBacks` 사용 가능.

  ```objective-c
  CFStringRef = strs[1];
  strs[0] = CFSTR("CFArray");

  CFArrayRef callbackArray = CFArrayCreate(NULL, (const void **)strs, 1, &kCFTypeArrayCallBacks);
  ```

- `CFArrayGetCount`

  > 함수

  - `CFArrayRef`의 값의 갯수를 나타내는 `CFIndex` 반환.
  - 매개변수
    - `theArray`: 갯수를 알아낼 `CFArrayRef`.

  ```objective-c
  CFStringRef = strs[1];
  strs[0] = CFSTR("CFArray");

  CFArrayRef callbackArray = CFArrayCreate(NULL, (const void **)strs, 1, &kCFTypeArrayCallBacks);
  CFIndex count = CFArrayGetCount(callbackArray);
  ```

- `CFArrayGetValueAtIndex`

  > 함수

  - `CFArrayRef`의 주어진 인덱스에 해당하는 값(`const void *`) 반환.
  - 인덱스가 `CFArrayRef`의 길이를 초과하면 에러 발생.
  - 매개변수
    - `theArray`: 값을 가져올 `CFArrayRef`.
    - `idx`: 검색할 `CFIndex`.

  ```objective-c
  CFStringRef = strs[1];
  strs[0] = CFSTR("CFArray");

  CFArrayRef callbackArray = CFArrayCreate(NULL, (const void **)strs, 1, &kCFTypeArrayCallBacks);

  CFStringRef str = CFArrayGetValueAtIndex(callbackArray, 0);
  ```

#### 참고

- [Apple Developer - `CFArray`](https://developer.apple.com/documentation/corefoundation/cfarray?language=objc)
- [Apple Developer - `CFArrayCreate`](https://developer.apple.com/documentation/corefoundation/1388741-cfarraycreate?language=objc)
- [Apple Developer - `CFArrayTypeCallBacks`](https://developer.apple.com/documentation/corefoundation/kcftypearraycallbacks?language=objc)
- [Apple Developer - `kCFTypeArrayCallBacks`](https://developer.apple.com/documentation/corefoundation/cfarraycallbacks?language=objc)
- [Apple Developer - `CFArrayGetCount`](https://developer.apple.com/documentation/corefoundation/1388772-cfarraygetcount?language=objc)
- [Apple Developer - `CFArrayGetValueAtIndex`](https://developer.apple.com/documentation/corefoundation/1388767-cfarraygetvalueatindex?language=objc)

---

## `CFDictionary`

### 주요 기능

### 사용 예제

#### 참고

- [Apple Developer - `CFDictionary`](https://developer.apple.com/documentation/corefoundation/cfdictionary?language=objc)

---

## `Foundation`과 연결

- [Apple Developer - `CF : NS Bridges`](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Toll-FreeBridgin/Toll-FreeBridgin.html#//apple_ref/doc/uid/TP40010810-CH2)
- [Naver Blog - `\_\_bridge` 형변환](https://m.blog.naver.com/ciscovoip/222049735477)

---
