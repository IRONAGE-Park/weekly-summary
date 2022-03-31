# 2022/03 5주차 주간 리포트

## 주간 작업 목록

---

- [`Final Cut Pro` 기능, 스크립트, 플러그인 조사 ✅](#final-cut-pro-기능-스크립트-플러그인-조사-)
- [`Final Cut Pro` 경쟁사 조사 ✅](#final-cut-pro-경쟁사-조사-)
- [`macOS`에서 `Adobe Camera Raw` 전체 호환 ✅](#macos에서-adobe-camera-raw-전체-호환-)
- [`Windows` 한/영 버그 수정 조사 ✅](#windows-한영-버그-수정-조사-)
- [`Windows`에서 `Typing` 기능 `HUX8` 입력 버그 조사 ✅](#windows에서-typing-기능-hux8-입력-버그-조사-)
- [`Grid Pro` 환경 세팅 ✅](#grid-pro-환경-세팅-)

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

- `macOS`에서 `Adobe Camera Raw`의 전체 호환 작업을 마무리하였습니다.
- `Adobe Photoshop`에서 실행하는 `Camera Raw` 필터 뿐만 아니라 `Adobe Bridge`에서 실행하는 `Camera Raw` 필터도 사용 가능합니다.

  ![Bridge_지원](./assets/Bridge_지원.gif)

- 만약 `Adobe Photoshop`의 `Camera Raw`와 `Adobe Bridge`의 `Camera Raw`가 동시에 켜져 있을 때는 다음과 같이 동작합니다.

  1. 동시에 켜진 상태에서 기능을 처음 실행 - `Adobe Photoshop`에서 동작.
  2. `Adobe Photoshop`에서 기능 1회 이상 실행 후, `Adobe Bridge`가 켜고 기능을 실행 - `Adobe Photoshop`에서 동작.
  3. `Adobe Bridge`에서 기능 1회 이상 실행 후, `Adobe Photoshop`가 켜고 기능을 실행 - `Adobe Bridge`에서 동작.

  - 일반적으로 `Adobe Photoshop`이 우선 순위가 높다고 생각하시면 편할 듯 합니다.

- 또한, 실행 중인 `Camera Raw`가 꺼지면 나머지 `Camera Raw`에서 기능이 실행됩니다.

  ![Photoshop_Bridge_전환](./assets/Photoshop_Bridge_전환.gif)

#### 고려 사항

- 현재 `Adobe Bridge`에서 정상적으로 작동하려면 프로그램 자동 전환 기능을 `off` 한 후 사용하여야 합니다.
- `Camera Raw`를 또 다른 지원 프로그램으로 묶어서 관리할 지 고민입니다.

---

## `Windows` 한/영 버그 수정 조사 ✅

#### 작업 상세 설명

#### 고려 사항

---

## `Windows`에서 `Typing` 기능 `HUX8` 입력 버그 조사 ✅

#### 작업 상세 설명

#### 고려 사항

---

## `Grid Pro` 환경 세팅 ✅

#### 작업 상세 설명

- `Grid Pro`를 지원하기 위해 `INVAIZ Studio`에서 다시 `Grid Pro` 모델을 추가하고, 데이터를 추가할 수 있도록 버그가 있는 소스를 수정하였습니다.

  ![Grid_Pro_모델_추가](./assets/Grid_Pro_모델_추가.gif)

  ![Grid_Pro_미리보기_호버](./assets/Grid_Pro_미리보기_호버.gif)

- `Tray` 및 메뉴에서 상태를 설정할 수 있는 기능을 `Grid Pro` 모델도 추가하였습니다.

  ![Grid_Pro_메뉴](./assets/Grid_Pro_메뉴.gif)

#### 고려 사항

- `Drag & Drop` 요소를 새로 그려야하는데 기존 `Grid10`과 중복되지 않을 것 같은 소스 코드가 많아 매핑 기능에 시간이 어느정도 쓰일 것으로 예상됩니다.

---

## 전달 사항

### 이번 주 추가 리스트

- `Grid Pro` 지원

### 이번 주 구현 리스트

- `macOS`에서 `Camera Raw` `Bridge`, `ACR` 실행 환경 구분

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
- `Grid Pro` 지원
