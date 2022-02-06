# `IME-Mode`

> `JavaScript`에서 `Key` 이벤트와 `IME-Mode` 호환에 관한 조사.

## 목차

- [`IME란?`](#ime란)
- [`IME-Mode란?`](#ime-mode란)
- [`JavaScript`에서 `IME-Mode`](#javascript에서-ime-mode)
- [영문 입력으로 고정시키기 위한 방법?](#영문-입력으로-고정시키기-위한-방법)

---

## `IME란?`

- 입력 방식 편집기(`Input Method Editor`).
- 언어를 입력할 때 특정한 방식의 키보드를 입력하면 해당하는 글자를 띄워주도록 하는 시스템.
- 한국어의 경우 한글 자모를 조합시켜 글자를 만들어줌.

> 같은 키보드를 통해 여러 언어를 입력할 수 있는 메커니즘을 제공함.

---

## `IME-Mode란?`

- 현재 설정된 `IME`의 언어 모드.
- 이 모드를 통해 `OS`는 현재의 입력을 어떤 언어로 치환할 것인지 결정함.
- `Windows`의 `WIN32 API`에서 `IME-Mode` 확인 예시 코드.

  ```cpp
  #include <iostream>
  #include <Windows.h>

  #define ARRSIZE(arr) (sizeof(arr)/sizeof(*(arr)))

  using namespace std;

  #define LID_TRADITIONAL_CHINESE  0x0404
  #define LID_JAPANESE      0x0411
  #define LID_KOREAN        0x0412
  #define LID_SIMPLIFIED_CHINESE  0x0804

  int main() {
    HKL hKeyboardLayout = 0;
    wchar_t localeName[100] = { 0 };
    cout << "Calling GetUserDefaultLocaleName" << endl;

    int ret = GetUserDefaultLocaleName(localeName, ARRSIZE(localeName));
    wcout << localeName << endl;

    if (hKeyboardLayout == 0)
      hKeyboardLayout = GetKeyboardLayout(0);
    if (LOWORD(hKeyboardLayout) == LID_KOREAN)
      cout << "kor";
    else
      cout << "else";

    return 0;
  }
  ```

  ```cpp
  #include <iostream>
  #include <Windows.h>

  int main() {
    while (1) {
      HWND  _curr_window = GetForegroundWindow();
      DWORD _curr_window_procces_id;
      DWORD _curr_window_thread_id = GetWindowThreadProcessId(_curr_window, &_curr_window_procces_id);
      HKL _key_locale = GetKeyboardLayout(_curr_window_thread_id);
      int n = (int)_key_locale;

      std::cout << "Process ID is " << _curr_window_procces_id << " and Thread ID is " << _curr_window_thread_id << std::endl;
      std::cout << "Keyboard layout is " << n << std::endl;
    }
    return 0;
  }
  ```

- `macOS`에서는 확인하는 코드를 찾지 못 함.

#### 참고

- 소스코드 예시: [dydtjr1128's Blog](https://dydtjr1128.github.io/win_api/2019/09/26/get-keyboard-layout.html)

---

## `JavaScript`에서 `IME-Mode`

- `<input type="text" />`의 `keydown` 이벤트에서 `keyCode`를 이용한 코드를 작성하려면 `IME-mode`가 한글일 때 `IME`에서 메세지를 가로채기 때문에 `keyCode`는 `229`가 나오고, `key`는 `Process`가 나옴.

  > `Mozlia`에서는 `keydown`, `keyup` 이벤트 대신에 `input` 이벤트를 사용하는 것을 추천함.

- `IME`는 한글 입력 시 컴포징이라는 단계를 거침.
- `ㄱ`을 입력하면 아직 글자가 완성되지 않았다고 판단하여 글자가 완성될 때까지 조합을 하는데 이것을 컴포지션이라고 부름.
- `JavaScript`에서는 이 컴포징이 진행 중일때도 `keydown`, `keyup` 이벤트를 보냄.

- 한글 입력인지 알 수 있는 로직 소스 코드 예시

  ```js
  input.addEventListener("keyup", (event) => {
    if (event.isComposing) {
      // 한글 입력 중
    }
  });
  ```

> 이렇기 때문에 `keydown`, `keyup`을 통해서 `input`에 각 `character`에 즉각적으로 작동하는 이벤트는 사용하지 않는 것이 좋음.

> 이벤트에서 컴포지팅 메세지를 보내는 것이 고쳐지지 않을 수도 있기 때문.

> 이에 `input` 이벤트. 즉, `element`의 `value`가 `change`될 때마다 발화하는 이벤트를 사용하는 것을 권장.

#### 참고

- `keyCode` 229가 뜰 때: [함께 걷는 개발자 Blog](https://circus7.tistory.com/6)

---

## 영문 입력으로 고정시키기 위한 방법?

- `CSS`에서 제공하는 `ime-mode`라는 프로퍼티는 `IE` 등의 일부 브라우저에서 동작하고, 정식 지원이 아니며 추후 사라질 예정이기 때문에 제외.(출처: https://developer.mozilla.org/en-US/docs/Web/CSS/ime-mode)
- `JavaScript`의 정규식을 사용하거나, `input` 태그에 `pattern` 프로퍼티를 사용하여 제어할 수도 있으나, 이는 이미 한글로 입력된 상황에서 영어로 강제 변환하는 것이기 때문에 근본적으로 영어 입력이 되는 것은 아님.
