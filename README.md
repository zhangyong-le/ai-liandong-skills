# AI联动 Skills

可复用的 AI 工作方法库：多成员派单、验收闭环、Windows UIA 自动化、跨源数据核对。

每个技能一个目录：`skills/<name>/SKILL.md`。

## 原则

- 任务先拆卡，再派给能力匹配的成员。
- 「已发送」≠「已完成」≠「已验收」：三级状态分开判定。
- 验收人 ≠ 负责人；只认产物、日志、可复现证据。
- 示例均已脱敏：不含密钥、账号、交易台账、个人数据。

## 技能索引

| Skill | 用途 | 状态 |
|---|---|---|
| [dispatch-and-verify](skills/dispatch-and-verify/SKILL.md) | 多成员派单与验收闭环，含假成功防护 | draft |
| [windows-uia-dispatch](skills/windows-uia-dispatch/SKILL.md) | Windows UIA 桌面 AI 应用自动化：composer 竞态、按钮重查、产物回收 | draft |

## SKILL.md 约定

```markdown
---
name: <kebab-case>
description: <何时使用，一句话>
---

# 标题

## 何时使用
## 输入
## 步骤
## 常见坑
## 验收证据
## 失效条件
```

## 来源

本库沉淀自本机 AI联动 的实践；路径与成员名已做泛化。
