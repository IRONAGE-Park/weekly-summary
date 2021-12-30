# `Foundation`

> `Objective-C`의 `Foundation` 라이브러리(기본 기능 계층)에 관한 조사.

## 목차

- [`NSLog`](#nslog)
- [`NSURL`](#nsurl)
- [`NSArray`](#nsarray)
- []

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

## `NSArray`

- 정적으로 정렬된 객체의 컬렉션.

### 주요 기능

- 배열. 정렬된 객체의 컬렉션을 관리.

### 사용 예제

- 리터럴을 사용하여 생성

  ```objective-c
  NSArray *array = @[someObject, @"Hello World!", @42];
  ```

#### 참고

- [Apple Developer - NSArray](https://developer.apple.com/documentation/foundation/nsarray?language=objc)
