# `Foundation`

> `Objective-C`의 `Foundation` 프레임워크에 관한 조사.

- 데이터 유형, 운영체제 서비스 접근 등의 애플리케이션 기본 기능 계층 정의를 제공하는 프레임워크.
- 데이터 저장 및 지속성, 텍스트 처리, 날짜 및 시간 계산, 정렬 및 필터링, 네트워킹 기능 제공.
- `Foundation`에서 정의한 클래스, 프로토콜 및 데이터 유형은 `macOS`, `iOS`, `watchOS` 및 `tvOS` `SDK` 전체에서 사용.

## 목차

- [`NSString`](#nsstring)
- [`NSArray`](#nsarray)
- [`NSURL`](#nsurl)
- [`NSLog`](#nslog)
- [`Core Foundation`과 연결](#core-foundation과-연결)

---

## `NSString`

- 변경할 수 없는 일반 텍스트 유니코드 문자열 타입.
- 변경하기 위해선 `NSMutableString` 사용.
- `CFString`과 호환.

### 주요 기능

- 문자열 조작 변환 및 검색, 비교 기능 모음
- `Foundation` 및 `Cocoa Framework` 전반에 걸쳐 사용되며, 애플리케이션의 모든 텍스트 및 언어 기능의 기초 역할.
- `UTF-16` 코드 단위의 유니코드 호환 텍스트 문자열을 암호화.

### 사용 예제

- 리터럴을 사용하여 생성

  ```objective-c
  NSString *string = @"test";
  ```

- `stringWithFormat`

  > 타입(정적) 메서드

  - 지정된 형식 문자열을 사용하여 새로운 `NSString` 생성.
  - 매개변수
    - `format`: 형식 문자열
    - `...param`: 형식 지정자에 들어갈 인수 목록

  ```objective-c
  NSString *string = [NSString stringWithFormat:@"%@, %f", object, 3.2f];
  ```

- `stringWithUTF8String`

  > 타입(정적) 메서드

  - 지정된 `C/C++` 문자열을 사용하여 새로운 `NSString` 생성.
  - 매개변수
    - `cstring`: `C/C++` 문자열

  ```objective-c
  NSString *string = [NSString stringWithUTF8String];
  ```

- `length`

  > 인스턴스 프로퍼티

  - 현재 `NSString` 인스턴스의 문자열 길이 `NSUInteger`.

  ```objective-c
  NSString *string = @"Hello World!";
  NSUInteger length = [string length];
  ```

- `UTF8String`

  > 인스턴스 프로퍼티

  - `NULL`로 끝나는 `UTF8` 표현의 `C/C++` 문자열.

  ```objective-c
  NSString *string = @"Hello World!";
  char *UTF8String = [string UTF8String];
  ```

- `containsString`

  > 인스턴스 메서드

  - 대/소문자를 구분하고 지역 언어를 인식하지 않는 검색 수행 후 지정된 문자열이 포함되었는지 나타내는 `BOOL` 반환.
  - 매개변수
    - `str`: 검색할 `NSString`.

  ```objective-c
  NSString *string = @"Hello World!";
  BOOL *isContain = [string containsString:@"Hello"];
  ```

#### 참고

- [Apple Developer - NSString](https://developer.apple.com/documentation/foundation/nsstring?language=objc)
- [Apple Developer - stringWithFormat](https://developer.apple.com/documentation/foundation/nsstring/1497275-stringwithformat?language=objc)
- [Apple Developer - stringWithUTF8String](https://developer.apple.com/documentation/foundation/nsstring/1497379-stringwithutf8string?language=objc)
- [Apple Developer - length](https://developer.apple.com/documentation/foundation/nsstring/1414212-length?language=objc)
- [Apple Developer - UTF8String](https://developer.apple.com/documentation/foundation/nsstring/1411189-utf8string?language=objc)
- [Apple Developer - containsString](https://developer.apple.com/documentation/foundation/nsstring/1414563-containsstring?language=objc)

---

## `NSArray`

- 정적으로 정렬된 객체의 컬렉션.
- `CFArray`와 호환.

### 주요 기능

- 정적 배열. 정렬된 객체의 컬렉션을 관리.

### 사용 예제

- 리터럴을 사용하여 생성

  ```objective-c
  NSArray *array = @[someObject, @"Hello World!", @42];
  ```

#### 참고

- [Apple Developer - NSArray](https://developer.apple.com/documentation/foundation/nsarray?language=objc)

---

## `NSURL`

- 원격 서버의 항목이나 로컬 파일의 경로와 같은 리소스의 위치를 나타내는 객체.

### 주요 기능

- 로컬 파일의 속성을 직접 조작할 수 있음.
- `NSURL` 객체를 다른 `API`에 전달하여 해당 `URL`의 내용 검색 가능.
- `NSString`을 사용하여 객체를 초기화.
- 로컬 파일 참조 및 애플리케이션 간 통신을 위해 사용.

### 사용 예제

- `URLWithString`

  > 타입(정적) 메서드

  - 제공된 `NSString`로 초기화된 `NSURL` 객체를 생성.

  ```objective-c
  NSURL *url = [NSURL URLWithString:@"https://github.com"];
  ```

#### 참고

- [Apple Developer - NSURL](https://developer.apple.com/documentation/foundation/nsurl?language=objc)

---

## `NSLog`

- 메세지를 기록하는 함수.

### 주요 기능

- 오류 메세지를 기록.
- 디버깅 용.

### 사용 예제

```objective-c
NSLog(@"format1, format2", data1, data2);
```

#### 참고

- [Apple Developer - NSLog](https://developer.apple.com/documentation/foundation/1395275-nslog/)
- [Apple Developer - 지원되는 형식 지정자](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFStrings/formatSpecifiers.html#//apple_ref/doc/uid/TP40004265)

---

## `Core Foundation`과 연결

- [Apple Developer - CF : NS Bridges](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Toll-FreeBridgin/Toll-FreeBridgin.html#//apple_ref/doc/uid/TP40010810-CH2)
- [Naver Blog - \_\_bridge 형변환](https://m.blog.naver.com/ciscovoip/222049735477)

---
