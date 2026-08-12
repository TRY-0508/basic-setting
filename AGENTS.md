## Skill 使用

1. **优先使用已有 skill** — 通过 `skill` 工具检查可用 skill。如有匹配，加载它。
2. **外部生态发现** — 若无匹配，使用 `find-skills` 搜索 `npx skills find`（https://skills.sh/）。
3. **最后自行创建** — 仅当无现有 skill 可用时，才用 `skill-creator` 创建。

将 skill 安装到 `~/.config/opencode/skills/<name>/SKILL.md`，禁止安装到项目本地。

---

## Skill 清单

| 场景 | Skill | 来源 | 说明 |
|------|-------|------|------|
| 编码行为准则 | `karpathy-guidelines` | 自建 | 先想后写、简单优先、精准修改、目标驱动执行 |
| 项目推进与文档管理 | `project-workflow` | 自建 | 层级化文档结构、模块级设计、INDEX.md、契约管理 |
| 一致性与变更管理 | `consistency-management` | 自建 | 行内提案、影响分析、契约检查、无需临时文档 |
| 经验知识沉淀 | `knowledge-accumulation` | 自建 | 模式提取、抽象方法、触发优化、回顾总结 |
| 外部调研 | `external-research` | 自建 | 论文五维度、项目四维度、resource/ 目录组织 |
| 文献综述 | `paper-survey` | 自建 | 论文检索、主题聚类、分层阅读、综述文档生成 |
| 中文论文写作 | `chinese-academic-writing` | 自建 | LaTeX/Word 中文排版、字号对照、标点字体、APA 引用 |

---

## Skill 管理规则

- `~/.config/opencode/skills/` 下的 skill 分为两类：**外部获取的**（文件内容主要为英文）和**用户自建的**（文件内容主要为中文）
- 仅允许修改用户自建的 skill，不得改动外部获取的 skill
- 以下为完整用户自建 skill 清单（以清单为准，语言是辅助判断手段）：
  - `karpathy-guidelines`、`project-workflow`、`consistency-management`、`knowledge-accumulation`、`external-research`、`paper-survey`、`chinese-academic-writing`

---

## Agent 行为准则

以下行为准则源自 Andrej Karpathy 的观察。核心取舍：**谨慎优先于速度**。详见 `karpathy-guidelines` skill。

### 1. 先想后写

- 明确陈述假设。不确定时提问。如有多种理解，逐条列出。
- 遇到模糊之处，停下来。指出困惑所在，然后提问。
- 如果存在更简单的方案，直接说出来。该反驳时就反驳。

### 2. 简单优先

- 不超出需求范围添加功能。不为一次性代码创建抽象层。
- 不为不可能发生的场景添加错误处理。
- 自问："资深工程师会觉得这过度复杂吗？" 如果是，重写。

### 3. 精准修改

- 不"顺手优化"相邻的代码、注释或格式。
- 遵循已有风格。除非被要求，否则不删除已有的死代码。
- 只移除因你的改动而变为无用的 import、变量或函数。

### 4. 目标驱动执行

将任务转化为可验证的目标：
- "添加校验" → 先写校验无效输入的测试，再让测试通过
- "修复 bug" → 先写能复现 bug 的测试，再让测试通过
- "重构 X" → 重构前后测试均通过

### 5. 后台任务状态可见

当使用子智能体（background task）并行处理多个任务时：

- 启动后台任务后，在回复末尾列出所有运行中的任务及其预计完成状态
- 后台任务全部完成后，主动汇总各任务的完成情况（成功/失败/部分完成）
- 不等待用户追问才汇报——任务完成即汇报

### 6. 经验沉淀路由

- 发现可复用经验/模式/教训时，加载 `knowledge-accumulation` skill 进行规范化沉淀。
- 发现跨模块一致性问题或需要做大规模变更时，加载 `consistency-management` skill。
- 推演设计方案或组织文档结构时，加载 `project-workflow` skill。

---

## 个人文档内容偏好

以下规则适用于项目文档、提案、分析报告、经验总结等日常输出。**学术论文场景不适用以下规则，应使用 `chinese-academic-writing` skill。**

- **避免术语堆砌**：一句话需要读者解析三个以上堆叠的专业名词时，拆成短句或用示例说明。

| 反例 | 正例 |
|------|------|
| "通过多层级事件驱动治理闭环实现架构级可观测性横切插桩" | "每个关键步骤都会触发检查和记录。计费模块在事件流的关键节点介入，日志模块记录全过程的追踪数据。" |

- **表格同样避免名词堆叠**：即使在高信息密度的表格中，也尽量不要使用多名词堆叠的写法，这样的可读性真的很差。单元格应写成完整的动作或短句，如"按角色、阶段、新近度打分，取分数最高的若干条"，而非"重要性评分（角色+阶段+新近度）+ 贪心 top-K"。

- **使用平实语言**：写得像在向一位没读过代码但有能力的工程师解释。不用学术化表达。"我们用事件连接组件" > "系统采用事件驱动的耦合机制实现组件间通信"。

- **避免口语对话式表达**：文档正文、字段名、列表项一律使用书面语，不用聊天式说法。字段名用书面名词（如"研究内容"，而非"它做了什么"）；公式解释用"通俗地说""含义是"，而非"人话""意思是"；总结用"简言之"，而非"一句话说就是"。

| 禁止 | 改用 |
|------|------|
| 它做了什么（字段名） | 研究内容（字段名） |
| 人话解释： | 通俗地说： |
| 一句话说就是： | 简言之： |
| 意思是： | 含义是： |

- **不用虚饰限定词**：

| 禁止 | 改用 |
|---|---|
| 全项目唯一权威定义 | 以本文为准 |
| 权威参考实现 | 参考实现 |
| 企业级生产就绪 | 生产可用（附具体验证条件） |
| 端到端全覆盖 | 覆盖以下场景：[列出] |
| 最佳实践 | 推荐做法（附理由） |

- **具体胜过抽象**："每个域有清晰的边界" → 说出边界在哪里；"系统是可扩展的" → 说出如何扩展。

- **文档结构层级化、可扫读**：构建文档时让内容层级分明、条理清晰、便于快速阅读。用标题分级组织内容，并列信息用表格或列表承载，长段落拆成短段；每个小节先给结论，再展开细节。读者扫读标题和列表即可定位信息，再按需深入。

| 禁止 | 改用 |
|------|------|
| 一长段文字平铺所有内容，无标题无列表 | 标题分级 + 短段落 + 表格/列表 |
| 所有内容挤在一个大章节里 | 按主题拆成小节，每节一个要点 |
| 先堆细节后给结论 | 结论先行，细节随后展开 |

- **设计文档不含历史痕迹**：不保留旧方案、版本标记、调整备注、进度勾选等临时内容。

---

## 多主机配置同步

opencode 的 AGENTS.md 和 skills 通过私有 GitHub 仓库 `TRY-0508/basic-setting` 进行多主机同步。

### 仓库结构

```
basic-setting/
├── AGENTS.md          # → ~/.config/opencode/AGENTS.md
└── skills/            # → ~/.config/opencode/skills/
```

### 同步操作

**上传（当前主机配置有变更时）：**

```powershell
# Windows
$src = "$env:USERPROFILE\.config\opencode"
$repo = "<local-repo-path>"  # 本地 clone 路径
Copy-Item "$src\AGENTS.md" "$repo\AGENTS.md"
New-Item -ItemType Directory -Force -Path "$repo\skills" | Out-Null
Copy-Item -Recurse -Force "$src\skills\*" "$repo\skills\"
cd $repo; git add -A; git commit -m "sync: update config"; git push
```

**下载（新主机初始化时）：**

```powershell
# Windows
git clone https://github.com/TRY-0508/basic-setting.git "<temp-path>"
$dst = "$env:USERPROFILE\.config\opencode"
Copy-Item "<temp-path>\AGENTS.md" "$dst\AGENTS.md"
New-Item -ItemType Directory -Force -Path "$dst\skills" | Out-Null
Copy-Item -Recurse -Force "<temp-path>\skills\*" "$dst\skills\"
```

### 注意事项

- 仓库为**私有仓库**，推送/拉取需要 GitHub 认证
- 需配置 git 代理才能在国内网络环境下访问：`git config --global http.proxy http://127.0.0.1:<port>`
- 修改 AGENTS.md 或用户自建 skill 后，应同步推送到远程仓库
- Agent 在修改 AGENTS.md 或 skills 后，应提醒用户同步推送
