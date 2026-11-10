---
name: windows-uia-dispatch
description: Use when automating Windows desktop AI apps via UIA (send prompts, click buttons, collect artifacts) — especially Electron/React apps whose composers and send buttons render asynchronously.
---

# Windows UIA Dispatch

## 何时使用

- 需要程序化驱动 Windows 桌面应用（Electron/React、WebView 壳）。
- 要发送长文本、等待按钮可用、回收产物文件或 UI 文本。

## 输入

- 进程名、窗口标题提示。
- 目标控件的识别方式（AutomationId / Name / ControlType）。
- 产物路径与超时。

## 步骤

1. **找窗口**：按进程名 + 标题，取面积最大的主窗（悬浮小窗会干扰）。
2. **强制前台**：SetForegroundWindow + AttachThreadInput；点击后可能再次失去前台，粘贴前再强制一次。
3. **等页面稳定**：页面切换后控件异步渲染，**轮询**目标控件（10s 级），不要固定 sleep。
4. **输入文本**：点击 → Ctrl+A → Delete → 剪贴板 Ctrl+V；React 受控组件不要用 set_edit_text（不触发 onChange）。
5. **回读校验**：读 composer value，确认包含输入内容（空值时可能返回 placeholder）。
6. **找发送按钮**：每轮重新查找（避免旧 wrapper），等 enabled（30s 级）；invoke 失败回退 click_input。
7. **判定完成**：等产物文件出现且 mtime/size 稳定两轮；或回读 UI 文本；只看「已发送」不算完成。

## 常见坑

- **悬浮小窗/多进程**：取最大主窗。
- **UIPI**：目标应用提权时，普通权限输入会被拦；UIA Invoke 走 broker 可能可用。
- **控件 name 变化**：用 contains/正则，不要死等单一精确字符串。
- **旧产物**：必须有 mtime 基线，否则旧文件会被当成新结果。
- **生成中**：发送按钮变「停止」，必须先等空闲。

## 验收证据

- 产物路径 + mtime + size + 内容哈希。
- UI 转录原文。
- 派单日志（start/end/elapsed/ok）。

## 失效条件

- 应用更新导致控件树大改，需重新探测。
- 锁屏/未登录/窗口不可见时，UIA 不可用。
