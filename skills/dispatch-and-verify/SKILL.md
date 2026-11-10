---
name: dispatch-and-verify
description: Use when orchestrating multiple AI tools or agents to complete tasks — split work into task cards, dispatch to capability-matched members, verify artifacts (not self-reports), and prevent false success.
---

# Dispatch and Verify

## 何时使用

- 有多个 AI 工具/成员（CLI、桌面 GUI、云 agent）可执行任务。
- 任务需要可追责、可复现，且不能把「消息已发出」当作完成。

## 输入

- 目标与验收标准。
- 成员能力矩阵：执行方式、可写目录、能否回读产物。
- 任务卡：编号、任务、验收标准、负责人。

## 步骤

1. **拆卡**：一张卡只给一个负责人；写清产物路径与验收标准。
2. **选人**：按能力矩阵派单，不按「谁最近闲着」。
3. **派单**：记录开始时间与基线（旧产物 mtime/哈希）。
4. **执行**：成员只写自己的目录；公共文件由调度者单写。
5. **回收**：等待产物出现且稳定；GUI 成员还要回读界面文本。
6. **验收**：验收人 ≠ 负责人，逐条核对证据（文件 + 行号/哈希/命令/转录）。
7. **留痕**：成功/失败/缺陷类型写入质量台账。

## 常见坑

- **假成功**：退出码 0、消息已发送、旧产物存在，都不等于完成。
- **GUI 竞态**：页面切换后控件延迟渲染；固定 sleep 不可靠，改轮询。
- **控件失效**：按钮可能 disabled 或换名；每轮重新查找，避免持有旧 wrapper。
- **共享写冲突**：成员只写自己的目录；公共文件单写者。
- **超时误判**：长任务需要更长超时与稳定窗口，不能只等一次。
- **旧产物冒充**：没有 mtime/哈希基线时，必须要求 mtime 晚于派单时间。

## 验收证据

- 产物路径 + mtime + 大小 + 关键行；必要时 SHA256。
- 复跑命令与输出。
- GUI 转录原文。

## 失效条件

- 成员无法回读产物且无 UI 转录时，不能自动判定完成。
- 任务涉及密钥/敏感数据时，不得派给云端成员。
