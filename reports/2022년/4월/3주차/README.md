# 2022/04 3주차 주간 리포트

## 주간 작업 목록

---

- [릴리즈 노트 디자인 업데이트 ✅](#릴리즈-노트-디자인-업데이트-)
- [`Final Cut Pro` 동작 코드 업데이트 ✅](#final-cut-pro-동작-코드-업데이트-)
- [`Final Cut Pro` 단축키 조사 ✅](#final-cut-pro-단축키-조사-)
- [`INVAIZ Studio` 전체 설정 내보내기/불러오기 ✅](#invaiz-studio-전체-설정-내보내기불러오기-)
- [디테일 수정 ✅](#디테일-수정-)

---

## 릴리즈 노트 디자인 업데이트 ✅

#### 작업 상세 설명

- 기존 릴리즈 노트는 디자인적으로 가독성이 떨어지는 것 같아 디자인을 변경하였습니다.
  ![릴리즈_노트_디자인_업데이트](./assets/릴리즈_노트_디자인_업데이트.gif)
- 좌측에는 릴리즈 노트의 버전이 나열되어 있고, 중간 영역에 내용, 우측 영역에 요약 네비게이션을 배치하였습니다.

#### 고려 사항

- 우측 요약 영역에서, 현재 보고 있는 화면을 강조하는 기능을 개발 중에 있으나 현재 데이터 형식의 한계로 구현이 난해합니다.

---

## `Final Cut Pro` 동작 코드 업데이트 ✅

#### 작업 상세 설명

- `Final Cut Pro`에서 정확하게 영역에 접근하지 못하는 문제를 어느정도 수정하였습니다.
- 동적으로 변하는 인덱스를 통해 접근하는 것이 아니라, 각 요소 간의 상대 위치를 파악하여 접근할 수 있도록 하였습니다.

  ![Final_Cut_Pro_안정성](./assets/Final_Cut_Pro_안정성.gif)

- 각 요소에 접근할 수 있으나, `Hue` 패널의 경우 일반적인 접근 요소를 통해 값을 조작해도 반영되지 않아 다른 방법이 있는지 고안 중에 있습니다.
- `Adobe Camera Raw`에서는 모두 `TextField`를 통해 접근하여 문자열을 다루었지만, `Final Cut Pro` 같은 경우에는 `Slider`, `TextField` 등 다양하게 접근하며, 문자열과 숫자 타입을 모두 다루어야 하므로 까다로울 것으로 예상됩니다.
- `Final Cut Pro`의 정보를 조사하였습니다. 자세한 내용은 [여기](../../../../researchs/finalcutpro)에서 확인 가능합니다.
- `L` 사에서 `Color Wheel` 패널을 사용하기 위해, 해당 패널을 생성해주는 기능이 있는데, 기능이 단축키인 것으로 확인되었으며 현재 어떤 패널을 활성화시켜놓은 상태인지 확인하는 로직이 없는지 다른 패널이 열려있을 경우 기능이 동작하지 않는 것을 확인했습니다.
- `TextField`, `Slider`와 더불어 패널을 조작하는 방식이 굉장히 특이하여 `Grid10` 기기 자체의 한계로 조작이 어려울 것으로 예상됩니다.

#### 고려 사항

- 기능 조작은 금방 업데이트할 수 있을 것으로 예상되지만, 자세한 사용 방법 등 디테일적인 요소에 미숙함이 보일 것 같습니다.
- 단축키는 `Command Set`에서 모두 제공하는 것으로 보이며, `Command Set`에 있는 모든 기능을 매핑하고, 번역하는 작업에 시간이 많이 소요될 것으로 예상됩니다.

---

## `Final Cut Pro` 단축키 조사 ✅

#### 작업 상세 설명

- `Final Cut Pro`가 설치된 `Mac`에서는 `/Users/{USER_NAME}/Library/Preferences`의 `com.apple.FinalCut.plist` 파일로부터 현재 `Final Cut Pro` 애플리케이션 상태와 관련된 값을 읽거나 쓸 수 있음.
  - 현재 활성화된 `Command Set`
    ![plist_Active_Command_Set](./assets/plist_Active_Command_Set.png)
  - `L` 사에서는 `FFDefaultColorCorrectionID`라는 값을 사용하는데, 정확한 용도는 모르나 여기서 데이터를 확인 가능.
    ![plist_FFDefaultColorCorrectionID](./assets/plist_FFDefaultColorCorrectionID.png)
- 커스텀 `Command Set` 파일의 형태와 주입(`Final Cut Pro`에 삽입)하는 방법을 발견.
  - `Command Set` 폴더 위치
    ![Command_Sets_폴더](./assets/Command_Sets_폴더_위치.png)
    - 위치: `/Users/{USER_NAME}/Library/Application Support/Final Cut Pro/Command Sets`
  - `Default Command Set` 구조
    ![Default_Command_Set](./assets/Default_Command_Set.png)
  - `INVAIZ Command Set` 구조
    ![INVAIZ_Command_Set](./assets/INVAIZ_Command_Set.png)
  - 적용된 커스텀 `Command Set`
    ![적용된_커스텀_Command_Set](./assets/적용된_커스텀_Command_set.png)
- 해당 커스텀 `Command Set`과, `Command Set`의 설정 상태를 읽어오는 `plist`를 사용하면 커스텀 단축키를 사용하는 사용자마다 단축키가 다른 문제점을 해결할 수 있고, 현재 `Final Cut Pro` 기능에 해당하는 단축키를 하나하나 매핑 시킬 필요 없이, 현재 `Command Set`에 지정된 해당 기능의 단축키를 읽어와 실행시키는 방법을 사용할 수 있어, `Command Set`으로 설정 가능한 단축키는 모두 지원 가능할 것으로 보임.

#### 고려 사항

- `Command Set` 이상의 단축키가 있는지 확인되지 않았습니다.
- 또한, `Plugin` 시스템(`FxPlug`)이 `Adobe`처럼 있는 것 같은데, 모션 플러그인 등의 기능도 추후 고려해볼 만한 것으로 생각됩니다.

---

## `INVAIZ Studio` 전체 설정 내보내기/불러오기 ✅

#### 작업 상세 설명

- `INVAIZ Studio`에서 관리하는 모든 설정 값을 불러오거나 내보낼 수 있는 창구를 만들었습니다.
  ![전체_설정_창구](./assets/전체_설정_창구.gif)
- 전체 설정 내보내기 기능을 통해 내보낼 수 있는 설정 값들은 다음과 같습니다.
  - 커스텀 파일 설정
  - 전체 프리셋(워크셋) 설정
  - 테마 설정
  - PC 부팅 시 프로그램 자동 실행 설정
  - 플러그인 통신 설정
  - 언어 설정
  - 오버레이 설정
- 내보내기한 파일은 저장한 경로에 `.invst` 확장자를 가진 파일로 암호화 후 저장합니다.
- 불러오기 시 `.invst` 확장자를 가진 파일을 불러올 수 있으며, 이전에 사용하고 있던 설정 파일에 덮어 씌워지는 것이 아니라, 전부 삭제된 후 새롭게 설정되게 됩니다.

#### 고려 사항

- 덮어쓰기 기능은 현재 버전간 호환 및 `OS` 종속성 문제로 인해 개발이 지연되고 있습니다.

---

## 디테일 수정 ✅

#### 작업 상세 설명

- 자동 업데이트 알림 창 언어팩 설정
  - `Windows`에서 발생하는 자동 업데이트 시의 알림 창 또한 언어 팩이 적용될 수 있도록 설정하였습니다.
    ```json
    {
      "AUTO_UPDATE": {
        "message": {
          "ko": "새로운 업데이트가 발견되었습니다. INVAIZ Studio Basquiat을 업데이트 하시겠습니까?",
          "en": "A new update has been found. Update INVAIZ Studio Basquiat?",
          "ja": "新しいアップデートが見つかりました。 INVAIZスタジオバスキアを更新しますか?"
        },
        "update": {
          "ko": "업데이트",
          "en": "Update",
          "ja": "アップデート"
        },
        "cancel": {
          "ko": "취소",
          "en": "Cancel",
          "ja": "キャンセル"
        }
      }
    }
    ```
  - 위와 같이 번역되어 사용자들에게 맞은 언어팩으로 제공됩니다.
- 프로그램 추가 목록 동적 데이터 관리 수정
  - `INVAIZ Studio`가 켜진 이후에 실행된 프로세스가 포커스 된 후에 `INVAIZ Studio` 프로그램 추가 설정에서 감지되지 않는 버그를 수정하였습니다.
  - 기존 로직의 분기 처리에서 에러가 있는 것으로 확인되었습니다.

---

## 전달 사항

![추가버전_런칭](./assets/추가버전_런칭.png)

> 2022.04.20(수) `INVAIZ Studio` 2.1.2 버전(추가버전) 런칭.

### 이번 주 추가 리스트

- 커스텀 스크롤 구현
- 릴리즈 노트 디자인 업데이트
- 전체 설정 내보내기/불러오기

### 이번 주 구현 리스트

- 릴리즈 노트 디자인 업데이트
- 전체 설정 내보내기/불러오기

### 현재 구현이 필요한 기능

- 자동 업데이트 환경 구성
- 목록 휴지통 기능 구현 - Design 설계 중.
- `Func` 형식에 `id` 추가
- `Func` 형식에서 `sendCepScript`의 경우 `fcode`에 `id` 값 매핑 후 실행
- 매크로 여러 개 클릭하여 한 번에 복사 / 붙여넣기
- 모든 데이터 구조 `id` 형식 변경 `number` -> `string`
- `macOS`에서 설치 시 `CEP` 프로그램 종료 시키기
- 오버레이 회전 기능 구현
- 커스텀 기능 목록에서 `Drag & Drop` 기능 구현
- 그룹 버튼으로 프리셋 변경 모드 설정 기능 추가
- `Final Cut Pro` 지원
- `Tooltip` 스타일 적용
- 프로그램 추가 후 제거 시 다시 추가할 수 없는 버그
- `Grid Pro` 지원
- 커스텀 스크롤 구현
