# LangChain / LangGraph Code Guide

一个面向新手的 Codex Skill：用自然语言解释、翻译并安全修改 LangChain 与 LangGraph 的 Python/TypeScript 代码。

## 能做什么

- 把复杂代码翻译成容易理解的运行流程
- 解释 Agent、Chain、Tool、State、Node、Edge、Memory 和 Checkpoint
- 区分 LangGraph 的“构建图”和“运行图”
- 把“调用工具前先确认”“增加记忆”等自然语言需求转换成代码修改
- 标出适合新手调整的位置以及需要谨慎修改的位置
- 有完整项目时直接修改并验证；只有代码片段时提供完整修改版

## 安装

将本仓库克隆到 Codex Skill 目录：

```powershell
git clone https://github.com/xucong017-netizen/langchain-code-guide.git "$env:USERPROFILE\.codex\skills\langchain-code-guide"
```

如果该目录已经存在，请先备份或使用其他目录名。

## 使用

```text
$langchain-code-guide 请用新手能看懂的中文解释这段 LangGraph 代码。
```

```text
$langchain-code-guide 把这个智能体改成调用 search 工具前先让用户确认。
```

Skill 也支持在相关 LangChain/LangGraph 请求中被自动选择。

## 文件

- `SKILL.md`：核心工作流和准确性约束
- `agents/openai.yaml`：Codex UI 展示信息和默认提示词
