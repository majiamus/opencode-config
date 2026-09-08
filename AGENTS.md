# 全局规则

## 语言设置
与用户沟通时使用简体中文

## opencode tools
- 用 `grep`/`glob` 做发现（找文件、搜模式）
- 用 `lsp` 做理解（定义跳转、引用查找、类型信息、搜索符号）
- 找到文件后，优先用 `lsp` 导航，而不是读取整个文件

## 图片处理
- 用户发送图片或提供图片路径时，先用 `read` 工具直接读取图片进行识别（我具备识图能力）
- 仅当 `read` 读取失败或当前模型无法识别图片时，才用 `task` 工具调度 `multimodal-looker` 子代理兜底，并附上图片路径或说明
- 无论何种情况，禁止直接回复"无法识别图片"而不给出处理

## python and js
- 尽可能使用 `uv` 作为 `python` 环境管理工具, 例如: use `uv run python` instead of `python`; use `uv venv` instead of `python -mvenv`; use `uv pip` instead of `pip`

<!-- context7 -->
Use Context7 MCP to fetch current documentation whenever the user asks about a library, framework, SDK, API, CLI tool, or cloud service -- even well-known ones like React, Next.js, Prisma, Express, Tailwind, Django, or Spring Boot. This includes API syntax, configuration, version migration, library-specific debugging, setup instructions, and CLI tool usage. Use even when you think you know the answer -- your training data may not reflect recent changes. Prefer this over web search for library docs.

Do not use for: refactoring, writing scripts from scratch, debugging business logic, code review, or general programming concepts.

## Steps

1. Always start with `resolve-library-id` using the library name and the user's question, unless the user provides an exact library ID in `/org/project` format
2. Pick the best match (ID format: `/org/project`) by: exact name match, description relevance, code snippet count, source reputation (High/Medium preferred), and benchmark score (higher is better). If results don't look right, try alternate names or queries (e.g., "next.js" not "nextjs", or rephrase the question). Use version-specific IDs when the user mentions a version
3. `query-docs` with the selected library ID and the user's full question (not single words)
4. Answer using the fetched docs

If a query fails with a quota error, inform the user and suggest setting `CONTEXT7_API_KEY` env var for higher limits. Do not silently fall back to training data.
<!-- context7 -->
