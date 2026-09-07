---
read_when:
  - 使用开发 gateway 模板
  - 更新默认开发智能体身份
summary: 开发智能体工具备注（C-3PO）
x-i18n:
  generated_at: "2026-02-01T21:37:41Z"
  model: claude-opus-4-5
  provider: pi
  source_hash: 3d41097967c9811637855664f978c02107a28b6d811ba49941a8f96f0720cd45
  source_path: reference/templates/TOOLS.dev.md
  workflow: 15
---

<!-- Manually maintained candidate; x-i18n records the prior generated baseline, not this revision. -->

# TOOLS.md - 用户工具备注（可编辑）

此文件用于记录*你*关于外部工具和约定的备注。
它不定义哪些工具可用；OpenClaw 在内部提供内置工具。

## 示例

### imsg

- 发送 iMessage/SMS：明确收件人和内容，在已有具体授权内发送；缺少必要范围时再澄清。
- 尽量发送简短消息；避免发送密钥。

### sag

- 文字转语音：指定语音、目标扬声器/房间，以及是否使用流式传输。

添加任何你希望助手了解的关于本地工具链的内容。
