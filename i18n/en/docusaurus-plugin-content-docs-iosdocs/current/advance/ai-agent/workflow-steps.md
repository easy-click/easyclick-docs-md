---
title: Workflow step reference
description: All AI Agent workflow step types, parameters, and usage
sidebar_label: Workflow step reference
keywords:
 - workflow steps
 - fetch_nodes
 - OCR
 - image match
 - IME
 - BLE
---

# Workflow step reference

A workflow is made of **steps** run in order. When you **insert a step** in the editor, the table below matches the menu items. Use this page to **look up parameters** — you do not need to read it cover to cover.

:::tip Common concepts

- **Save to variable** (`saveAs`): store a step’s result under a name; later steps reference it with `{{name}}` or **read from previous step**
- **Variable pool**: the editor toolbar shows **flow config** and **step results**; after a trial run you can see actual values
- **Skip condition**: skip this step when the condition is met
- **Failure handling**: on failure, jump to another step or run a recovery action

:::

:::tip Environment & licensing

Whether steps succeed depends on [Prerequisites & licensing](./prerequisites): **USB device license**, **automation IPA environment** (most UI steps), **Bluetooth/OTG** (`ble_*` steps). If a trial run fails, check environment first, then parameters.

:::

## 1. Environment & App

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `device_auth_ok` | Device license check | Check USB / screen license; also runs automatically before the flow starts | `type`: 1=USB license, 2=screen license |
| `ensure_auto_env` | Start automation environment | Start device automation service and poll until ready | — |
| `open_app` | Open app | Launch app by Bundle ID | `bundleId`; optional `resetUsb` |
| `home` | Go home | Simulate Home key | — |

## 2. Wait & logging

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `wait` | Wait | Fixed delay in milliseconds | `duration` (ms) |
| `log` | Log message | Output to run log | `message` or `messageFromVar` |

## 3. UI & gestures (automation environment required)

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `fetch_nodes` | Node capture | Dump UI tree and parse into node list | `match.dumpXml` filter; `saveAs` |
| `vlm_locate` | VLM · visual locate | Vision LLM locates element coordinates on screenshot | `intent` (Chinese description); `saveAs`; optional `imageFromParam` |
| `capture_screen_no_auto` | Non-automation screenshot | Bridge full-screen capture | `saveAs` |
| `capture_screen_auto` | Automation screenshot | Agent full-screen capture; requires `ensure_auto_env` | `saveAs` |
| `click_point` | Tap | Coordinate tap | `x`,`y` or `fromParam` |
| `swipe_to_point` | Swipe | Swipe from start to end | `startX`,`startY`,`endX`,`endY`,`duration` |
| `long_click_point` | Long press | Long press at coordinates | `x`,`y`,`duration` |
| `double_click_point` | Double tap | Double tap at coordinates | `x`,`y` |
| `multi_touch` | Multi-touch | Multi-finger paths | `fingers` array |
| `input_text` | Type text | Type into focused control | `content` |

### Screenshot & OCR steps

| Step op | When to use |
|---------|--------|
| `capture_screen_no_auto` | Full-screen capture without automation |
| `capture_screen_auto` | Full-screen capture with automation |
| `ocr_screen_no_auto` | OCR without automation |
| `ocr_screen_auto` | OCR with automation |

Names match **AI chat** — you can say “non-automation screenshot”, “automation OCR”, etc.

### OCR engine types (`ocrType`)

| ocrType | Description |
|---------|------|
| `paddleOcrNcnnV5` (default) | Local NCNN; balanced speed and accuracy |
| `v5` / `paddleOcrOnnxV5` | ONNX v5 |
| `v4` / `paddleOcrOnnxV4` | ONNX v4 |
| `ocrLite` | OcrLite |

On the same device, **each ocrType is cached separately**; after a formal task ends the engine is released, so the next OCR may re-init (first packet slightly slower is normal). Editor **OCR preview** keeps the engine loaded for continuous debugging.

Optional parameters: `padding`, `maxSideLen`, `region` (recognition area).

### Default `fetch_nodes` dumpXml filter

When `match.dumpXml` is not configured:

- `visibleFilter=1`, `labelFilter=1`, `boundsFilter=1`
- `maxDepth=50`
- `excludedAttributes=visible,selected,enable,accessible`
- `maxChildCount=200`

For **VLM semantic locate**, use the dedicated **`vlm_locate`** step (menu **VLM · visual locate**), not mixed into `fetch_nodes`.

### vlm_locate

| Parameter | Description |
|------|------|
| `intent` | Chinese description of target, e.g. “settings button top-right” |
| `saveAs` | Write coordinates and other results |
| `imageFromParam` | Optional; reference previous screenshot; empty = capture live |

Configure the VLM model first in [Config & data directories](./config) (`vlm_config.json`).

## 4. Connection recovery

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `reset_usb_conn` | Reset USB | Software usbmuxd reset | — |
| `reconnect_usb` | USB reconnect flash | Same as unplug/replug cable; **Windows only** | After run, suggest `wait` 5–10 seconds |

Use when automation won’t start, app won’t open, or other connection issues.

## 5. Photo album

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `album_request_auth` | Request album permission | Trigger album permission dialog | — |
| `album_delete_photos` | Clear photos | Delete all photos in album (irreversible) | — |
| `album_delete_videos` | Clear videos | Delete all videos in album | — |
| `album_insert_img_folder` | Import images | Scan local folder and write images to device album | `folderPath` (absolute path) |
| `album_insert_video_folder` | Import videos | Scan local folder and write videos to device album | `folderPath` |

:::warning

Album clear operations are **irreversible** — confirm device and paths before trial run.

:::

## 6. IME input method

Each IME step auto-checks input method availability (equivalent to `ime_ok`).

| Step op | Name | Description |
|---------|------|------|
| `ime_ok` | Check available | Check IME ready (legacy flows; can omit in new flows) |
| `ime_input` | Type text | Input via IME |
| `ime_paste` | Paste | Empty content uses clipboard |
| `ime_press_del` | Delete key | Simulate backspace |
| `ime_press_enter` | Enter | Simulate Enter |
| `ime_copy_to_clipboard` | Copy to clipboard | Copy input field content |
| `ime_get_text` | Read input field | Read current field text |
| `ime_get_clipboard` | Read clipboard | Read device clipboard |
| `ime_set_clipboard` | Set clipboard | Set clipboard content |
| `ime_open_url` | Open URL | Open URL via IME |
| `ime_remove_all_content` | Clear input field | Clear all content |
| `ime_change_keyboard` | Switch keyboard | Switch input method keyboard |
| `ime_dismiss` | Hide keyboard | Dismiss keyboard |

## 7. BLE Bluetooth peripherals

For BLE keyboard, mouse, and other external input devices. Complete [BLE tutorial](/iosdocs/advance/ios-usb-ble) and, if needed, [OTG tutorial](/iosdocs/advance/ios-usb-otg) binding and testing first.

| Step op | Name | Description |
|---------|------|------|
| `ble_click` | BLE tap | Coordinate tap |
| `ble_double_click` | BLE double tap | Coordinate double tap |
| `ble_swipe` | BLE swipe | start/end coordinates + duration |
| `ble_touch_down` / `ble_touch_up` / `ble_touch_move` | Down / up / move | Combine for drag |
| `ble_press` | BLE long press | x, y, delay |
| `ble_key_press` | BLE key | prefix + code |
| `ble_key_press_char` | BLE character | Character input |
| `ble_system_key` | System key | Home, volume, etc. |
| `ble_toggle_keyboard` | Toggle soft keyboard | Show/hide soft keyboard |
| `ble_reset` | BLE reset | Reset peripheral |
| `ble_set_wifi` | BLE WiFi provisioning | SSID + password |

## 8. OCR & data extraction

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `ocr_screen_no_auto` | Non-automation OCR | Bridge capture + recognize | `ocrType`; `saveAs`; optional region |
| `ocr_screen_auto` | Automation OCR | Agent capture + recognize; requires `ensure_auto_env` | Same as left |
| `pick_node` | Node data extraction | Extract fields, regions, coordinates from node variable | conditions, `saveAs` |
| `pick_ocr` | OCR data extraction | Pick line, field, center from OCR result | match rules, `saveAs` |
| `pick_match` | Match data extraction | Center or best match from template/color-offset result | `fromParam`; `pick.mode`; `saveAs` |

`pick_*` steps support **quick templates** (buttons on step cards) and **last trial run preview**.

## 9. Image/color recognition

See [Image/color matching](./image-match) for detailed usage.

| Step op | Name | Description |
|---------|------|------|
| `match_template_no_auto` | Non-automation template match | Bridge capture + OpenCV template match |
| `match_template_auto` | Automation template match | Agent capture + template match |
| `find_image_by_color_no_auto` | Non-automation color-offset match | Bridge + findImageByColorEx |
| `find_image_by_color_auto` | Automation color-offset match | Agent + findImageByColorEx |

Main parameters: `templatePath` (relative to working directory, e.g. `assets/templates/xxx.png`), `imageFromParam`, `region`, `threshold` / `weakThreshold`, `saveAs`.

Typical chain: `match_template_*` → `pick_match` (mode=center) → `click_point`.

## 10. Local files

File steps operate on the **control host** filesystem (not files on the device).

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `save_file` | Save file | Write variable content to file | `baseDir`: runtime_data / external; `fromParam`; `filename`; `format` |
| `read_file` | Read file | Read JSON/text | `baseDir`: readdata / external; `filename`; `saveAs` |
| `delete_file` | Delete file | Delete file or directory recursively | `baseDir` + relative path, or external absolute path |
| `list_dir` | List directory | Return path array | `folderPath`; `recursive` |
| `file_exists` | Path exists | Write `{exists, path, isDir?}` | `path` absolute path |

:::tip Working directory often not required

If **flow config** has no working directory, save/delete use Agent default **`runtime_data/{deviceId}/`**, read uses **`runtime_data/readdata/`** (shared, not per device). With **working directory** set: save/delete `{workDir}/{deviceId}/`, read `{workDir}/`.

:::

**Directory notes (`baseDir`):**

| baseDir value | Path |
|-----------|------|
| runtime_data | `{dataDir}/runtime_data/{deviceId}/` (default save, delete) |
| readdata | `{dataDir}/runtime_data/readdata/` (default read, shared) |
| external | User-specified absolute path on PC |
| audit | `{dataDir}/audit/{taskId}/{deviceId}/{workflowId}/` |

Legacy `datatmp` and `{dataDir}/datatmp/` still work; new workflows should use `runtime_data`. Save paths **do not include taskId**.

## 11. External integration

| Step op | Name | Description | Main parameters |
|---------|------|------|-----------|
| `call_http` | HTTP request | Call registry endpoint or specified URL | `endpoint`/`url`; `method`; `body`; `saveAs` |
| `run_cli` | Run CLI | Execute registry command | `cliId`; `args`; `saveAs` |
| `call_mcp` | Run MCP | Call registry MCP Tool | `server`; `tool`; `arguments`; `saveAs` |

Legacy `pick_cli` / `pick_mcp` **auto-migrate** to new step format when opened.

See [External integration](./external-integration).

## 12. Flow control

| Step op | Name | Description |
|---------|------|------|
| `branch` | Conditional branch | if/else with two nested step groups |
| `switch` | Multi-way branch | Match cases in order, else default |
| `pause_human` | Human pause | Task pauses; wait for **Continue** in task panel |
| `invoke_workflow` | Invoke subflow | Run another workflow synchronously; pass params, write back variables |

### invoke_workflow (subflow)

| Parameter | Description |
|------|------|
| `workflowId` | Subflow ID |
| `params` / input params | Key-values passed to subflow |
| `exportParams` / write back | Which subflow variables return to parent |
| `saveAs` | Optional; record invoke success `{ workflowId, status }` |
| `returnMode` | Result content: status only / includes write-back / subflow params snapshot |

On subflow **failure** or **human pause**, internal variables are **not** written back by default (unless configured to write on failure).

### branch / switch conditions

- Supports **when** expressions (evaluated after template expansion)
- Can combine with `dumpXml` node existence (configure pick conditions in editor)

## 13. Insert-step menu categories

Editor **Add step** groups by category:

**Common** · **Proxy events** · **OCR** · **VLM · visual locate** · **Image/color** · **Album** · **IME input** · **BLE Bluetooth** · **Link management** · **Data processing** · **Files** · **HTTP/CLI/MCP** · **Sub-workflow (FSM)**

## Variables & templates

- **Flow config**: global defaults (UI merges former `globals` / `fsm.context`; **workflow default** vs. **this flow startup** scopes)
- Task params override startup values via trial run **Params** or invoke **input params**
- String fields support `{{var}}`, `{{stepVar.field}}` template syntax
- **Variable pool** (editor toolbar): view config and step results; after trial run switch to **Run** view for actual values; click to highlight canvas references

## Validation rules (before save / trial run)

Editor **Validate** checks for example:

- Workflow ID, FSM initial/final reachability
- Album steps have `folderPath`
- Match steps have template path or Base64
- File steps in external mode specify directory
- Invoke has no circular dependency
- Global variable name conflicts, reserved words

On failure, the first error is shown in the UI.
