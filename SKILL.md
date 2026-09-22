---
name: langchain-code-guide
description: Explain, translate, and safely edit LangChain or LangGraph Python/TypeScript code in beginner-friendly natural language. Use when a user wants to understand agent, chain, tool, state, node, edge, memory, or checkpoint code, or wants to change that code by describing the desired behavior instead of naming APIs.
---

# LangChain / LangGraph Code Guide

Turn framework-heavy code into an accurate mental model that a newcomer can understand and modify. “Translate” normally means explaining code semantics in natural language; treat Python↔TypeScript conversion as a separate porting request only when the user asks for it.

## Establish the real code context

- Inspect the target file plus its imports, state/types, tool definitions, prompts, model construction, and nearby tests. Do not explain an isolated call as if it were the whole agent.
- Determine Python versus TypeScript and infer the installed LangChain/LangGraph generation from dependency manifests and imports. Do not silently rewrite current code into a different API generation.
- When only a pasted snippet is available, explain it directly and label any behavior that depends on omitted code or package versions.
- Consult current official documentation when exact API behavior, deprecation status, or version compatibility affects the answer. Separate documented behavior from inference about the user's program.

## Build a beginner-friendly mental model

Start at the user's altitude and use their language; default to Chinese when the request is Chinese. Preserve exact code identifiers in backticks so the explanation remains searchable.

Explain the smallest useful set of layers:

1. What the program is trying to achieve in one sentence.
2. What goes in, what comes out, and what external side effects can occur.
3. The execution path in actual runtime order, not source-file order when those differ.
4. The responsibility of each important component in plain language.
5. Where a beginner can safely edit behavior and what changes are structurally risky.

Prefer a short flow such as `用户输入 → 提示词 → 模型 → 工具 → 状态更新 → 回复` before details. Use a table only when several code elements need exact plain-language mappings. Avoid explaining every syntax token unless the user asks for line-by-line teaching.

Translate framework concepts with concrete roles rather than jargon alone:

- LangChain: prompts, models, runnables/chains, agents, tools, structured output, message history, callbacks, and output parsers.
- LangGraph: state schema, reducers, nodes, edges, conditional routing, `START`/`END`, compilation, invocation/streaming, commands, interrupts, checkpoints, and stores.

For LangGraph, always distinguish graph construction from graph execution. Show which node reads or writes each state field and how the next node is chosen. For LangChain agents, show the loop between model decisions, tool calls, tool results, and the final answer.

## Turn natural-language changes into code

When the user describes desired behavior such as “先确认再调用工具” or “记住上一轮结果”:

- Translate the request into a compact behavior contract: trigger, new behavior, unchanged behavior, and observable success condition.
- Identify the narrowest correct edit point. Reuse the project's existing patterns and installed APIs; do not introduce a new abstraction or dependency just to make the explanation look simpler.
- If repository files are available and the user asked to change/build, implement the edit, update focused tests, and run proportionate verification. Do not stop at a suggested snippet.
- If only a snippet is available, provide a complete revised snippet or a tight before/after patch, then explain the changed lines and how to test them.
- Preserve prompts, tool schemas, state types, reducers, routing semantics, async behavior, streaming, and persistence unless the requested behavior requires changing them.
- Flag any change that can cause external writes, repeated tool execution, infinite graph loops, lost conversation state, incompatible serialized checkpoints, or accidental secret exposure.
- For LangGraph requests to confirm before a tool call or external action, prefer the framework's human-in-the-loop `interrupt`/resume flow with a suitable checkpointer when supported by the installed version. Use blocking `input()` only for an explicitly local interactive CLI example, label that limitation, and never hide a blocking prompt inside a server, async, streaming, or checkpointed node.

## Make edits approachable

For each meaningful edit, tell the user:

- **改哪里**: the file, symbol, node, tool, or prompt.
- **为什么**: the behavioral reason, not merely the API name.
- **改了会怎样**: the new runtime path in plain language.
- **怎么验证**: one concrete input and the expected observable result.

When helpful, label zones as “safe to tune” (prompt wording, model options, simple routing thresholds) versus “change carefully” (state schemas, reducers, tool argument schemas, checkpoint identity, recursion limits). These labels are guidance, not a substitute for inspecting actual callers and tests.

For a beginner-facing code-change answer, a compact sequence such as “这段代码在做什么 → 运行顺序 → 状态怎么变化 → 怎么改 → 完整改后代码 → 怎么验证” is a useful default. Omit sections that add no value instead of filling a rigid template.

## Accuracy boundaries

- Never claim a node, tool, memory store, or checkpoint is active merely because it is defined; trace whether it is wired into the compiled/invoked program.
- Distinguish model behavior requested by a prompt from behavior enforced by code or schemas.
- Distinguish conversation memory, graph state, checkpoint persistence, and long-term stores; do not call all of them “memory.”
- Explain failures at the boundary where they occur: model output, tool schema validation, tool execution, state merge/reducer, routing, persistence, or transport.
- Do not hide framework terms completely. Introduce each important term once in plain language, then keep the exact term beside it so the user can recognize it in code and documentation.

End with the shortest useful next action: the exact setting to tune, the test to run, or the next file to inspect. Do not add a generic tutorial when the user's code and goal are already clear.
