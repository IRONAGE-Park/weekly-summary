# 2022/07 4주차 주간 리포트

## 주간 작업 목록

---

- [프리셋/커스텀 파일 불러오기를 위한 확장자 동일화 ✅](#프리셋커스텀-파일-불러오기를-위한-확장자-동일화-)
- [플러그인 구독 결제 프록시 서버 코드 안정화 ✅](#플러그인-구독-결제-프록시-서버-코드-안정화-)
- [일본어 설정 `macOS`에서 `INVAIZ Studio Basquiat` 실행 불가 버그 수정 ✅](#일본어-설정-macos에서-invaiz-studio-basquiat-실행-불가-버그-수정-)
- [첫 실행 시 `OS` 언어 설정으로 설정되지 않는 버그 수정 ✅](#첫-실행-시-os-언어-설정으로-설정되지-않는-버그-수정-)
- [7월 현황 및 8월 계획 작성 ✅](#7월-현황-및-8월-계획-작성-)

---

## 프리셋/커스텀 파일 불러오기를 위한 확장자 동일화 ✅

#### 작업 상세 설명

- 기존에는 그저 상황에 따라 `string` 자료형을 통해 확장자를 설정하여 통일성을 해치기 쉬웠으며, 어떤 파일에 어떤 확장자가 적용되는지, 어디서 확장자를 열람할 수 있는지 어려움이 있었습니다.
- 실제로, 커스텀 파일이 로컬에 저장될 때 `.ivz` 확장자를 통해 저장을 해야 했으나, 프리셋 파일 확장자와 동일한 `.inz`로 저장되고 있었습니다.
  - 로컬에 저장되는 것이기 때문에 사용자들에게 실질적인 피해는 없었지만, 추후 버그 등의 이유로 파일을 요청하거나 했을 때 불편함이 있을 수 있었습니다.
- 이에, 확장자를 일관성있게 다룰 수 있는 파일을 생성하고, 해당 파일에 접근하는 방식으로 관리할 수 있게 하였습니다.
- 확장자 설명
  - `.inz`: 프리셋 파일
  - `.ivz`: 커스텀 파일
  - `.invst`: 전체 설정 파일
  - `.binz`: 목록 휴지통 파일(현재는 사용하지 않음)

#### 고려 사항

- 추후 새로운 확장자가 더 생기거나, 특정 확장자에 아이콘을 입히기 유리합니다.
- 또한, 확장자에 변경이 필요할 경우에도 필터를 적용하기 용이합니다.

---

## 플러그인 구독 결제 프록시 서버 코드 안정화 ✅

#### 작업 상세 설명

- 간단하게 작성했던 프록시 서버 코드에 로직을 좀 더 분리하고, 레이어를 구분하여 견고성을 높였습니다.
- 기존에는 주먹구구식으로 개발을 했기 때문에 의존성이 많은 코드들이었으나, 이를 최대한 분리하는 리팩터링을 진행했습니다.
- `JWT`를 통해 로그인 / 인증을 합니다.
- 에러 메세지의 종류를 조금 더 세분화하였습니다.
- 구독의 자세한 정보를 가지고 올 수 있는 `API`를 작성하였습니다.

#### 고려 사항

- 로그인 방식에서 현재 취약점이 발견되었습니다.

---

## 일본어 설정 `macOS`에서 `INVAIZ Studio Basquiat` 실행 불가 버그 수정 ✅

#### 작업 상세 설명

- `OS` 언어 설정이 한글 / 영어인 상태에서는 이상없이 정상적으로 동작하였으나, 일본어로 변경하게 되면 실행이 안되는 버그가 발생하였습니다.
- `Windows`에서는 발생하지 않는 것으로 확인되었으며, `macOS`에서만 발생하는 버그입니다.
- 로그가 따로 나오지 않아 버그 발생 지점을 추적하는데 시간이 많이 소요되었습니다.
- 확인 결과 `node-window-manager`라는 외부 모듈에서 `macOS` 부분 소스 코드에 결함이 있었습니다.
  - 이전에 일본어 설정으로 실행 및 테스트했을 때는 문제 없이 동작한 것으로 보아, `macOS`의 버전 업데이트로 인해 정책 등이 변경된 것으로 판단됩니다.
- 이에 해당 예외 부분을 처리하여 정상적으로 로드되도록 수정하였습니다.

#### 고려 사항

- 일시적인 처방으로, 프로그램 추가 리스트 부분에서 데이터가 누락되어 특정 프로그램을 커스텀 추가할 수 없는 경우가 발생할 수 있습니다.

---

## 첫 실행 시 `OS` 언어 설정으로 설정되지 않는 버그 수정 ✅

#### 작업 상세 설명

- 첫 실행 시 해당 `OS`의 언어 설정 값으로 `INVAIZ Studio Basquiat`의 언어 설정을 초기화하도록 해놓았었는데, 이 기능이 동작하지 않아 일본 사용자 분들이 한국어 `INVAIZ Studio Basquiat`로 시작하게 되는 불편함이 있었습니다.
- 가장 최근 엔트리 포인트 소스에서 `Application` 객체를 생성하면서 언어팩 로드 시점이 변경되었기 때문으로 확인되었습니다.
- 이를 다시 `OS`의 언어 설정 값을 따르도록 수정하였습니다.

#### 고려 사항

---

## 7월 현황 및 8월 계획 작성 ✅

#### 작업 상세 설명

- 연구소 기업 R&BD에 필요한 7월 성과 현황 및 8월 계획을 작성하였습니다.
- 자세한 내용은 `Notion`에서 확인하실 수 있습니다.

#### 고려 사항

---

## 전달 사항

- 2022/07/28(목) `macOS` `INVAIZ Studio Basquiat` 임시 업데이트 버전 배포
  - 버전은 따로 변경하지 않았으나, 프리셋 불러오기 에러 핸들링, 전체 설정 `Drag & Drop` 불러오기가 적용되어 있습니다.
