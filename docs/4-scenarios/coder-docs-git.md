---
title: B3 文档与 Git
subtitle: 自动生成 README、commit、PR
course: OpenCode 中文实战课
stage: 第四阶段
lesson: "B3"
duration: 15 分钟
practice: 20 分钟
level: 进阶
description: 用 AI 自动生成 README 文档、Git commit 信息、Pull Request 描述，提升协作效率。
tags:
  - 文档
  - Git
  - README
  - commit
prerequisite:
  - B1 开发日常
---

# B3 文档与 Git

> 💡 **一句话总结**：用 AI 自动生成 README、commit 消息、PR 描述。

## 📝 课程笔记

本课核心知识点整理：

<img src="/images/4-scenarios/coder-docs-git-notes.mini.jpeg"
     alt="B3 文档与 Git学霸笔记"
     data-zoom-src="/images/4-scenarios/coder-docs-git-notes.jpeg" />

---

## 学完你能做什么

- 自动生成项目 README
- 让 AI 写 commit 消息
- 生成 PR 描述
- 用 /undo /redo 与 Git 联动

---

## 你现在的困境

- README 写得很痛苦，或者干脆不写
- commit 消息随便写，后来看不懂
- PR 描述不知道怎么写才规范

---

## 什么时候用这一招

- 当你需要：高效完成文档和 Git 协作任务
- 而且不想：在这些"杂事"上花太多时间

---

## 🎒 开始前的准备

> 确保你已经完成以下事项：

- [ ] 完成了 [B1 开发日常](./coder-daily)
- [ ] 项目已初始化 Git（`git init`）

---

## 核心思路

### 文档自动化场景

| 文档类型 | 生成时机 | AI 辅助方式 |
|---------|---------|-----------|
| README | 项目初期/重大更新 | 分析项目结构生成 |
| API 文档 | 接口开发完成 | 从代码提取生成 |
| Commit 消息 | 每次提交 | 分析变更生成 |
| PR 描述 | 提交 PR | 汇总 commit 生成 |

---

## 跟我做

### 第 1 步：生成 README

**为什么**
好的 README 是项目的门面。

切换到 Build Agent：

```
@explore 分析项目结构，生成一个专业的 README.md：

包含以下部分：
1. 项目名称和简介
2. 功能特性（Feature list）
3. 快速开始（安装和运行）
4. 使用示例
5. 配置说明
6. 贡献指南
7. 许可证

保存为 README.md
```

### 第 2 步：生成 commit 消息

**为什么**  
规范的 commit 消息便于追溯。

先查看变更：

> 在 OpenCode TUI 里，以 `!` 开头的消息会执行 shell 命令，并把输出带进对话：https://opencode.ai/docs/tui

```
!git diff
```

然后让 AI 生成：

```
根据以上变更，生成符合 Conventional Commits 规范的 commit 消息：
- 格式：type(scope): description
- type：feat/fix/docs/style/refactor/test/chore
- 简洁明了，不超过 50 字符
```

执行提交：

```
!git add . && git commit -m "feat(auth): add email verification on registration"
```

### 第 3 步：生成 PR 描述

<AdInArticle />

**为什么**  
清晰的 PR 描述帮助 reviewer 理解。

> 获取最近提交列表（示例）：

```
!git log --oneline -10
```

```
根据以上 commit 历史，生成 PR 描述：

## 变更概述
（一句话总结）

## 变更详情
（分点列出主要修改）

## 测试情况
（测试了什么）

## 相关 Issue
（关联的 issue 编号）
```

### 第 4 步：使用 /undo 撤销

**为什么**  
AI 修改错了可以撤销。

如果 AI 的修改不满意：

```
/undo
```

**你应该看到**：最近一次对话变更被撤销，相关文件改动也会被回滚（需要 Git 仓库）。更多细节见 https://opencode.ai/docs/tui#undo 或 [附录/命令速查](../appendix/commands)。


### 第 5 步：补充代码注释

**为什么**  
好的注释是活文档。

```
@src/services/payment.ts 为这个文件添加 JSDoc 注释：
- 每个导出函数都加
- 包含参数说明和返回值
- 包含使用示例
```

---

## 📋 魔法 Prompt

### 📄 README 生成
> 预期效果：生成专业的项目 README，让新人快速上手

````
## 角色
你是技术文档工程师，擅长写让人一看就懂的项目文档。

## 任务
为项目生成专业的 README.md。

## 输入信息
### 必填项
- 项目名称：【名称】
- 一句话描述：【描述】
- 技术栈：【技术栈】

### 选填项
- 目标用户：【谁会用这个项目?】
- 项目结构：@explore 或 [粘贴项目结构]

## 输出规范
包含以下部分：
1. **项目标题和 Badge**
2. **项目简介**（3-5 句话）
3. **功能特性**（用列表）
4. **快速开始**
   - 环境要求
   - 安装步骤（可直接复制运行）
   - 运行命令
5. **配置说明**（环境变量、配置文件）
6. **API 文档**（如适用）
7. **常见问题**
8. **贡献指南**
9. **许可证**

## 约束条件
- ✅ 快速开始的命令要能直接复制运行
- ✅ 配置说明要完整，包含所有必填项
- ✅ 使用合适的 Badge（build status、version 等）
- ❌ 避免过于冗长，保持简洁
- ❌ 避免遗漏关键配置
````

### 🔧 Commit 消息生成
> 预期效果：生成规范的 Conventional Commits 格式消息

```
## 角色
你是代码规范专家，擅长写规范的 Commit 消息。

## 任务
根据代码变更生成符合 Conventional Commits 规范的提交消息。

## 输入信息
### 必填项
- 变更内容：!git diff 或 [粘贴变更描述]

### 选填项
- 关联 Issue：【Issue 编号?】

## 输出规范
格式：`<type>(<scope>): <description>`

### type 选择
- feat: 新功能
- fix: Bug 修复
- docs: 文档更新
- style: 代码格式（不影响逻辑）
- refactor: 重构（不是新功能也不是修复）
- test: 测试相关
- chore: 构建/工具/依赖

### 规范
- description 不超过 50 字符
- 使用祈使语气（add, fix, update，不是 added, fixed）
- 如果变更复杂，添加 body 说明详情
- 如有关联 Issue，在 footer 添加 `Closes #123`

## 约束条件
- ✅ type 要准确反映变更类型
- ✅ description 要具体，说明改了什么
- ❌ 避免含糊的描述如 "fix bug"、"update code"
- ❌ 避免在 description 中重复 type
```

### 📝 PR 描述生成
> 预期效果：生成清晰的 PR 描述，帮助 Reviewer 理解变更

```
## 角色
你是技术写作专家，擅长写清晰的技术文档。

## 任务
根据 commit 历史生成 PR 描述。

## 输入信息
### 必填项
- Commit 历史：!git log --oneline -10 或 [粘贴 commit 列表]

### 选填项
- PR 目的：【这个 PR 要解决什么问题?】
- 关联 Issue：【Issue 编号?】

## 输出规范
```markdown
## 变更概述
（一句话总结本 PR 的目的）

## 变更详情
- 详细修改点 1
- 详细修改点 2
- ...

## 测试情况
- [ ] 单元测试通过
- [ ] 集成测试通过
- [ ] 手动测试通过

## 截图/录屏
（如有 UI 变更，附上截图）

## 相关 Issue
Closes #【Issue 编号】

## Reviewer 注意事项
（需要特别关注的地方）
```

## 约束条件
- ✅ 概述要一句话说清楚
- ✅ 变更详情要具体，不只是复制 commit message
- ✅ 如有 UI 变更，提醒附上截图
- ❌ 避免遗漏关联的 Issue
- ❌ 避免变更详情过于笼统


---

## 检查点 ✅

> 全部通过才能继续

- [ ] 生成了 README.md
- [ ] 用 AI 生成了 commit 消息
- [ ] 知道如何生成 PR 描述
- [ ] 知道 /undo 与 Git 的联动

---

## 踩坑提醒

| 现象 | 原因 | 解决 |
|-----|-----|-----|
| README 不准确 | AI 分析不全 | 先用 @explore 让 AI 理解项目 |
| commit 格式不对 | 没指定规范 | 明确要求 Conventional Commits |
| /undo 不生效 | 项目不是 Git 仓库 | 先 `git init` |

---

## 本课小结

你学会了：

1. 自动生成项目 README
2. 生成规范的 commit 消息
3. 生成 PR 描述
4. /undo /redo 与 Git 联动

---

## 下一课预告

> 下一课我们将学习 CI/CD 集成，让 AI 在流水线中工作。

---

📚 **更多完整模板**：[Prompt 模板库](../appendix/prompts)
