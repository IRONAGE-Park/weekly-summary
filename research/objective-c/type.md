# `Type`

> `Objective-C`의 자료형(`type`)에 관한 조사.

## 목차

- [`NSURL`](#nsurl)
- [`NSArray`](#nsarray)

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

## `NSArray`

- 정적으로 정렬된 객체의 컬렉션.

### 주요 기능

- 배열. 정렬된 객체의 컬렉션을 관리.

### 사용 예제

- 리터럴을 사용하여 생성

  ```objective-c
  NSArray *array = @[someObject, @"Hello World!", @42];
  ```
