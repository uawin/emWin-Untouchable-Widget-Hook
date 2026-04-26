# emWin Untouchable

**Backporting Modality to Older Versions of emWin (STemWin)**

The version 5.50 of **emWin** (GUI library by Segger for embedded applications) introduced a new function `WM_SetUntouchable`. This function allows making a window "untouchable" for PID messages (touch/mouse events). This is essential for creating overlays like screen keypads while avoiding unwanted focus switches to windows in the background.

### Problem

However, this function is absent in older versions of emWin and STemWin (like **v5.44**, which is still standard for many STM32 projects).

### The Solution

This small, header-only library containing only 2 files of source code (.h and .c) allows you to create "untouchable" windows and widgets in emWin versions prior to 5.50. It uses a **callback-hooking mechanism** and works even if you are using a **precompiled binary library** instead of source code.

### Key Features

* **Compatibility:** Designed for emWin versions before 5.50.
* **Non-Invasive:** Does not require emWin source code; works via `User Data` and `Extra Bytes`.
* **Recursive Control:** Toggle "untouchable" behavior for a parent window and all its children with a single call.
* **Extensible:** Easily add support for any missed widgets (the core templates for common widgets like `BUTTON`, `EDIT`, `FRAMEWIN`, `MULTIPAGE` etc. are already included).
* **Minimal Footprint**: Uses only single callback for ALL emWin's widgets and objects.

### Usage Example

Instead of standard creation functions, use the `Untouchable` equivalents:

```c
#include "Untouchable.h"

void CreateMyUI(void) {
    WM_HWIN hBtn;
    
    // Create a button with "untouchable" support
    hBtn = BUTTON_CreateUntouchable(10, 10, 80, 30, hParent, WM_CF_SHOW, 0, ID_BTN_1);
    
    // Make the button (and its children, if any) "untouchable"
    setUntouchable(hBtn, 1); 
}
```
### Supported Creation Methods

* `FRAMEWIN_CreateUntouchable` / `FRAMEWIN_CreateUntouchableIndirect`
* `WINDOW_CreateUntouchable` / `WINDOW_CreateUntouchableIndirect`
* `BUTTON_CreateUntouchable` / `BUTTON_CreateUntouchableIndirect`
* `EDIT_CreateUntouchable` / `EDIT_CreateUntouchableIndirect`
* `MULTIPAGE_CreateUntouchable` / `MULTIPAGE_CreateUntouchableIndirect`
* ...and many others (`Dropdown`, `Listview`, `Radio`).

### Modality Control

```c
setUntouchable(hWin, 1); // Enable
setUntouchable(hWin, 0); // Disable
```

---
*Note: This library was created to solve architectural problems in complex industrial UIs.*
