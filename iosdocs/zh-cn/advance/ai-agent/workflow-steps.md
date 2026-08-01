---
title: 工作流步骤参考
description: AI 智能体工作流全部步骤类型、参数与使用说明
sidebar_label: 工作流步骤参考
keywords:
  - 工作流步骤
  - fetch_nodes
  - OCR
  - 找图
  - IME
  - BLE
---

# 工作流步骤参考

工作流由多个 **步骤** 按顺序组成。在编辑器里 **插入步骤** 时，下表对应菜单里的各项。本节供 **查阅参数** 用，不必通读。

:::tip 几个常用概念

- **保存到变量**（`saveAs`）：某步的结果存个名字，后面步骤用 `{{名字}}` 或 **从上一步读取** 引用
- **变量池**：编辑器工具栏可查看 **流程配置** 与 **步骤结果**，试跑后可看实际值
- **跳过条件**：满足条件时跳过这一步
- **失败处理**：这一步失败时跳转到别的步骤或执行补救

:::

:::tip 环境与授权

步骤能否成功，取决于 [环境与授权](./prerequisites)：**USB 设备授权**、**自动化 IPA 环境**（多数 UI 步骤）、**蓝牙/OTG**（`ble_*` 步骤）。试跑失败时先查环境，再查参数。

:::

## 一、环境与 App

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `device_auth_ok` | 设备授权检查 | 检查 USB/投屏授权是否有效；流程开始前也会自动检查 | `type`：1=USB授权，2=投屏授权 |
| `ensure_auto_env` | 启动自动化环境 | 启动设备自动化服务并轮询就绪 | — |
| `open_app` | 打开应用 | 按 Bundle ID 启动 App | `bundleId`；可选 `resetUsb` |
| `home` | 返回主页 | 模拟 Home 键 | — |

## 二、等待与日志

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `wait` | 等待 | 固定毫秒延迟 | `duration`（毫秒） |
| `log` | 打印日志 | 输出到运行日志 | `message` 或 `messageFromVar` |

## 三、界面与手势（需自动化环境）

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `fetch_nodes` | 节点抓取 | dump UI 树并解析为节点列表 | `match.dumpXml` 过滤；`saveAs` |
| `vlm_locate` | VLM·视觉定位 | 视觉大模型在截图中定位元素坐标 | `intent` 中文描述；`saveAs`；可选 `imageFromParam` |
| `capture_screen_no_auto` | 无自动化截图 | Bridge 直截全屏 | `saveAs` |
| `capture_screen_auto` | 自动化截图 | Agent 截全屏，需 ensure_auto_env | `saveAs` |
| `click_point` | 点击 | 坐标点击 | `x`,`y` 或 `fromParam` |
| `swipe_to_point` | 滑动 | 起点滑到终点 | `startX`,`startY`,`endX`,`endY`,`duration` |
| `long_click_point` | 长按 | 指定坐标长按 | `x`,`y`,`duration` |
| `double_click_point` | 双击 | 指定坐标双击 | `x`,`y` |
| `multi_touch` | 多点触控 | 多指轨迹 | `fingers` 数组 |
| `input_text` | 输入文字 | 向焦点控件输入 | `content` |

### 截图与 OCR 步骤

| 步骤 op | 何时用 |
|---------|--------|
| `capture_screen_no_auto` | 无自动化全屏截图 |
| `capture_screen_auto` | 自动化全屏截图 |
| `ocr_screen_no_auto` | 无自动化 OCR |
| `ocr_screen_auto` | 自动化 OCR |

AI 对话里名称一致，直接说「无自动化截图」「自动化 OCR」等即可。

### OCR 引擎类型（`ocrType`）

| ocrType | 说明 |
|---------|------|
| `paddleOcrNcnnV5`（默认） | 本地 NCNN，速度与精度均衡 |
| `v5` / `paddleOcrOnnxV5` | ONNX v5 |
| `v4` / `paddleOcrOnnxV4` | ONNX v4 |
| `ocrLite` | OcrLite |

同一台设备 **不同 ocrType 会分别缓存**；正式任务结束后引擎会释放，下次 OCR 可能重新初始化（首包略慢属正常）。编辑器 **OCR 预览** 默认保持引擎以便连续调试。

可选参数：`padding`、`maxSideLen`、`region`（识别区域）。

### fetch_nodes 默认 dumpXml 过滤

未配置 `match.dumpXml` 时使用：

- `visibleFilter=1`, `labelFilter=1`, `boundsFilter=1`
- `maxDepth=50`
- `excludedAttributes=visible,selected,enable,accessible`
- `maxChildCount=200`

**VLM 语义定位** 请用独立步骤 **`vlm_locate`**（菜单 **视觉定位 (VLM)**），不要在 `fetch_nodes` 里混用。

### vlm_locate

| 参数 | 说明 |
|------|------|
| `intent` | 中文描述目标，如「右上角设置按钮」 |
| `saveAs` | 写入坐标等结果 |
| `imageFromParam` | 可选，引用上一步截图；空则实时截屏 |

需先在 [配置与数据目录](./config) 中配置 VLM 模型（`vlm_config.json`）。

## 四、连接恢复

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `reset_usb_conn` | 重置 USB | 软件层 usbmuxd 重置 | — |
| `reconnect_usb` | 闪断 USB | 等同拔插数据线，**仅 Windows** | 执行后建议 `wait` 5–10 秒 |

适用于无法开自动化、无法打开 App 等连接异常场景。

## 五、相册

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `album_request_auth` | 请求相册权限 | 触发相册授权弹窗 | — |
| `album_delete_photos` | 清空图片 | 删除相册全部图片（不可恢复） | — |
| `album_delete_videos` | 清空视频 | 删除相册全部视频 | — |
| `album_insert_img_folder` | 导入图片 | 扫描本机文件夹图片写入设备相册 | `folderPath`（绝对路径） |
| `album_insert_video_folder` | 导入视频 | 扫描本机文件夹视频写入设备相册 | `folderPath` |

:::warning

相册清空操作 **不可恢复**，试跑前请确认设备与路径。

:::

## 六、IME 输入法

各 IME 步骤执行前会自动检查输入法可用（等效 `ime_ok`）。

| 步骤 op | 名称 | 说明 |
|---------|------|------|
| `ime_ok` | 检查可用 | 检查 IME 是否就绪（兼容旧流程；新流程可省略） |
| `ime_input` | 输入文字 | 通过 IME 输入 |
| `ime_paste` | 粘贴 | content 为空则用剪贴板 |
| `ime_press_del` | 删除键 | 模拟退格 |
| `ime_press_enter` | 回车 | 模拟回车 |
| `ime_copy_to_clipboard` | 复制到剪贴板 | 复制输入框内容 |
| `ime_get_text` | 读取输入框 | 读取当前框文字 |
| `ime_get_clipboard` | 读剪贴板 | 读取设备剪贴板 |
| `ime_set_clipboard` | 设剪贴板 | 设置剪贴板内容 |
| `ime_open_url` | 打开链接 | 通过 IME 打开 URL |
| `ime_remove_all_content` | 清空输入框 | 清空全部内容 |
| `ime_change_keyboard` | 切换键盘 | 切换输入法键盘 |
| `ime_dismiss` | 隐藏键盘 | 收起键盘 |

## 七、BLE 蓝牙外设

适用于 BLE 键盘、鼠标等外接输入设备。使用前请完成 [蓝牙 BLE 教程](/iosdocs/zh-cn/advance/ios-usb-ble) 与（如需要）[OTG 教程](/iosdocs/zh-cn/advance/ios-usb-otg) 中的绑定与测试。

| 步骤 op | 名称 | 说明 |
|---------|------|------|
| `ble_click` | BLE 点击 | 坐标点击 |
| `ble_double_click` | BLE 双击 | 坐标双击 |
| `ble_swipe` | BLE 滑动 | start/end 坐标 + duration |
| `ble_touch_down` / `ble_touch_up` / `ble_touch_move` | 按下/抬起/移动 | 配合实现拖拽 |
| `ble_press` | BLE 长按 | x, y, delay |
| `ble_key_press` | BLE 按键 | prefix + code |
| `ble_key_press_char` | BLE 字符 | 字符输入 |
| `ble_system_key` | 系统键 | Home、音量等 |
| `ble_toggle_keyboard` | 切换软键盘 | 显隐软键盘 |
| `ble_reset` | BLE 重置 | 重置外设 |
| `ble_set_wifi` | BLE 配网 | SSID + 密码 |

## 八、OCR 与数据提取

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `ocr_screen_no_auto` | 无自动化 OCR | Bridge 截屏并识别 | `ocrType`；`saveAs`；可选 region |
| `ocr_screen_auto` | 自动化 OCR | Agent 截屏并识别，需 ensure_auto_env | 同左 |
| `pick_node` | 节点数据提取 | 从节点变量提取字段、区域、坐标 | 条件、`saveAs` |
| `pick_ocr` | OCR 数据提取 | 从 OCR 结果选行、字段、中心点 | 匹配规则、`saveAs` |
| `pick_match` | 找图数据提取 | 从模板/偏色找图结果取中心点或最佳匹配 | `fromParam`；`pick.mode`；`saveAs` |

`pick_*` 步骤支持 **快捷模板**（编辑器步骤卡片上的按钮）和 **上次试跑预览**。

## 九、图色识别

详细用法见 [图色找图](./image-match)。

| 步骤 op | 名称 | 说明 |
|---------|------|------|
| `match_template_no_auto` | 无自动化找图 | Bridge 截屏 + OpenCV 模板匹配 |
| `match_template_auto` | 自动化找图 | Agent 截屏 + 模板匹配 |
| `find_image_by_color_no_auto` | 无自动化偏色找图 | Bridge + findImageByColorEx |
| `find_image_by_color_auto` | 自动化偏色找图 | Agent + findImageByColorEx |

主要参数：`templatePath`（相对工作目录，如 `assets/templates/xxx.png`）、`imageFromParam`、`region`、`threshold` / `weakThreshold`、`saveAs`。

典型链路：`match_template_*` → `pick_match`（mode=center）→ `click_point`。

## 十、本机文件

文件步骤操作 **中控主机** 文件系统（非设备内文件）。

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `save_file` | 保存文件 | 变量内容写入文件 | `baseDir`：runtime_data / external；`fromParam`；`filename`；`format` |
| `read_file` | 读取文件 | 读 JSON/文本 | `baseDir`：readdata / external；`filename`；`saveAs` |
| `delete_file` | 删除文件 | 删文件或递归删目录 | `baseDir` + 相对路径，或 external 绝对路径 |
| `list_dir` | 列出目录 | 返回路径数组 | `folderPath`；`recursive` |
| `file_exists` | 路径是否存在 | 写入 `{exists, path, isDir?}` | `path` 绝对路径 |

:::tip 默认不必设置工作目录

未在 **流程配置** 里设置工作目录时，保存/删除走 Agent 默认目录 **`runtime_data/{deviceId}/`**，读取走 **`runtime_data/readdata/`**（共享，不按设备分子目录）。设置了 **工作目录** 后，保存/删除为 `{workDir}/{deviceId}/`，读取为 `{workDir}/`。

:::

**目录说明（`baseDir`）：**

| baseDir 值 | 路径 |
|-----------|------|
| runtime_data | `{dataDir}/runtime_data/{deviceId}/`（保存、删除默认） |
| readdata | `{dataDir}/runtime_data/readdata/`（读取默认，共享） |
| external | 用户指定的本机绝对路径 |
| audit | `{dataDir}/audit/{taskId}/{deviceId}/{workflowId}/` |

旧版 `datatmp` 与 `{dataDir}/datatmp/` 仍兼容；新工作流请使用 `runtime_data`。保存路径 **不含 taskId**。

## 十一、外部集成

| 步骤 op | 名称 | 说明 | 主要参数 |
|---------|------|------|-----------|
| `call_http` | HTTP 请求 | 调用注册表端点或指定 URL | `endpoint`/`url`；`method`；`body`；`saveAs` |
| `run_cli` | CLI 执行 | 执行注册表命令 | `cliId`；`args`；`saveAs` |
| `call_mcp` | MCP 执行 | 调用注册表 MCP Tool | `server`；`tool`；`arguments`；`saveAs` |

旧版 `pick_cli` / `pick_mcp` 打开时会 **自动迁移** 为新步骤格式。

详见 [外部集成](./external-integration)。

## 十二、流程控制

| 步骤 op | 名称 | 说明 |
|---------|------|------|
| `branch` | 条件分支 | if/else 两组嵌套步骤 |
| `switch` | 多路分支 | 按顺序匹配 case，否则 default |
| `pause_human` | 人工暂停 | 任务暂停，等待人工在任务面板 **继续** |
| `invoke_workflow` | 调用子流程 | 同步执行另一工作流，可传参、写回变量 |

### invoke_workflow（子流程）

| 参数 | 说明 |
|------|------|
| `workflowId` | 子流程 ID |
| `params` / 传入参数 | 传给子流程的键值 |
| `exportParams` / 写回变量 | 子流程里哪些变量带回父流程 |
| `saveAs` | 可选，记录调用成功标记 `{ workflowId, status }` |
| `returnMode` | 结果标记内容：仅状态 / 含写回变量 / 子流程 params 快照 |

子流程 **失败** 或 **人工暂停** 时，默认 **不会** 写回内部变量（除非配置了失败时也写入）。

### branch / switch 条件

- 支持 **when** 表达式（模板展开后求值）
- 可结合 `dumpXml` 节点存在性判断（编辑器内配置 pick 条件）

## 十三、步骤插入菜单分类

编辑器「添加步骤」按类别分组：

**常用** · **代理事件** · **OCR识别** · **视觉定位 (VLM)** · **图色识别** · **相册** · **输入法输入** · **BLE 蓝牙** · **链接管理** · **数据处理** · **文件** · **HTTP/CLI/MCP** · **子工作流 (FSM)**

## 变量与模板

- **流程配置**：全局默认值（界面合并了原 `globals` / `fsm.context` 等概念，分 **工作流默认** 与 **本流程启动** 两种范围）
- 任务参数通过试跑 **Params** 或 invoke **传入参数** 覆盖启动值
- 字符串字段支持 `{{var}}`、`{{stepVar.field}}` 模板语法
- **变量池**（编辑器工具栏）：查看配置项、步骤结果；试跑后切 **运行** 视图看实际值；点击可高亮画布引用

## 校验规则（保存/试跑前）

编辑器 **校验** 会检查例如：

- 工作流 ID、FSM initial/final 可达性
- 相册步骤是否填写 `folderPath`
- 找图步骤是否提供模板路径或 Base64
- 文件步骤 external 模式是否指定目录
- invoke 是否形成循环依赖
- 全局变量名冲突、保留字

失败时在界面展示第一条错误信息。
