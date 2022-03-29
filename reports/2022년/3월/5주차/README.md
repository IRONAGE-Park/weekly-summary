# 2022/03 5주차 주간 리포트

## 주간 작업 목록

---

- [`Final Cut Pro` 기능, 스크립트, 플러그인 조사 ✅](#final-cut-pro-기능-스크립트-플러그인-조사-)
- [`Final Cut Pro` 경쟁사 조사 ✅](#final-cut-pro-경쟁사-조사-)
- [`macOS`에서 `Adobe Camera Raw` 전체 호환 ✅](#macos에서-adobe-camera-raw-전체-호환-)
- [`Windows` 한/영 버그 수정 조사 ✅](#windows-한영-버그-수정-조사-)
- [`Windows`에서 `Typing` 기능 `HUX8` 입력 버그 조사 ✅](#windows에서-typing-기능-hux8-입력-버그-조사)

---

## `Final Cut Pro` 기능, 스크립트, 플러그인 조사 ✅

#### 작업 상세 설명

- `Final Cut Pro` 업데이트를 위해 지원할 수 있는 기능과 강제 입력 외에 사용할 수 있는 스크립트, 플러그인이 존재하는지 조사하였습니다.
- 우선 조작할 수 있는 값이나, 수치 조작 패널 등의 위치 등을 조사하였습니다.
- 수치 조작 패널은 총 4가지가 있는 것으로 확인되었습니다.

  - `Color Board`

    ![Color_Board](./assets/Color_Board.png)

  - `Color Curves`

    ![Color_Curves](./assets/Color_Curves.png)

  - `Color Wheels`

    ![Color_Wheels](./assets/Color_Wheels.png)

  - `Hue/Saturation Curves`

    ![Hue_Saturation_Curves](./assets/Hue_Saturation_Curves.png)

- 단축키의 경우, 사용자마다 다르게 설정할 수 있는 듯 하며 이를 파일화 할 수도 있는 것 같습니다.
- 이러한 단축키 설정 값을 `Commands`라고 합니다.

  ![Commands_설정](./assets/Commands_설정.png)

#### 고려 사항

---

## `Final Cut Pro` 경쟁사 조사 ✅

#### 작업 상세 설명

- `Final Cut Pro` 본격 지원에 앞서 경쟁사 `L`사의 기능을 분석하였습니다.
- `L`사는 대체로 단축키를 강력하게 지원하고 있으며, 수치 조작의 경우 `Camera Raw`를 조작하는 것과 마찬가지로 직접 `TextField`에 접근하여 값을 조작하는 듯 합니다.
- 따로 스크립트나 플러그인은 사용하지 않는 것으로 확인되었습니다.
- 지원하는 단축키 기능에 비하여 다이얼의 기능은 빈약한 듯 합니다.

  - 다이얼 지원 기능

    ![Loupedeck_Functions_Color_Grading](./assets/Loupedeck_Functions_Color_Grading.gif)

    ![Loupedeck_Functions_Dials](./assets/Loupedeck_Functions_Dials.gif)

  - 버튼 지원 기능

    ![Loupedeck_Functions_Commands_ShortCuts](./assets/Loupedeck_Functions_Commands_ShortCuts.gif)

- `L`사 만의 커스터 마이징 `Commands`를 자동으로 로드해둔 것을 확인하였습니다.

  ![Loupedeck_Commands](./assets/Loupedeck_Commands.png)

- 또한 다이얼 기능의 경우 수치 조작 패널 종류가 `Color Board`, `Color Curves`, `Color_Wheels`, `Hue/Saturation Curves` 등이 있는 것으로 확인되었는데, 그 중 `Color_Wheels`만 제대로 지원하고 있습니다.

#### 고려 사항

---

## `macOS`에서 `Adobe Camera Raw` 전체 호환 ✅

#### 작업 상세 설명

#### 고려 사항

---

## `Windows` 한/영 버그 수정 조사 ✅

#### 작업 상세 설명

#### 고려 사항

---

## `Windows`에서 `Typing` 기능 `HUX8` 입력 버그 조사 ✅

#### 작업 상세 설명

#### 고려 사항

---

## 전달 사항

### 이번 주 추가 리스트

### 이번 주 구현 리스트

### 현재 구현이 필요한 기능

- 자동 업데이트 환경 구성
- 목록 휴지통 기능 구현 - Design 설계 중.
- `Func` 형식에 `id` 추가
- `Func` 형식에서 `sendCepScript`의 경우 `fcode`에 `id` 값 매핑 후 실행
- 매크로 여러 개 클릭하여 한 번에 복사 / 붙여넣기
- 모든 데이터 구조 `id` 형식 변경 `number` -> `string`
- `macOS`에서 설치 시 `CEP` 프로그램 종료 시키기
- `Windows` 한글로 키 입력 시 종료되는 버그
- 오버레이 회전 기능 구현
- 커스텀 기능 목록에서 `Drag & Drop` 기능 구현
- 그룹 버튼으로 프리셋 변경 모드 설정 기능 추가
- `Final Cut Pro` 지원
- `Tooltip` 스타일 적용
- 프로그램 추가 후 제거 시 다시 추가할 수 없는 버그
- `macOS`에서 `Camera Raw` `Bridge`, `ACR` 실행 환경 구분
