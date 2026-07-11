# 从旧知识库迁移到 AI Content Knowledge Base

本指南适用于把已有的 Obsidian Vault、Markdown 笔记目录、文章仓库、课程目录或混合型个人知识库迁移到 `ai-content-kb`。

迁移目标不是把所有文件机械地塞进新目录，而是为每份内容明确：

```text
它是谁写的？
它是不是外部来源？
它是否已经发布？
它是否经过人工审核？
它应该作为原文、知识索引，还是机器元数据存在？
```

## 一、迁移原则

### 1. 旧库先只读，不原地改造

迁移期间保留旧知识库原样。不要一开始就移动目录、批量改 frontmatter、重写所有 `[[wikilink]]` 或删除重复文件。

正确顺序是：

```text
只读盘点 -> 生成映射 -> 小批量复制 -> 验证 -> 扩大迁移 -> 最后切换
```

### 2. 复制优先于移动

在新库完成验收前，旧库是回滚基础。迁移操作默认使用复制，不使用移动，也不使用带 `--delete` 的同步命令。

### 3. 先判断内容角色，再判断主题

同一个“AI”目录里可能同时存在个人笔记、网页剪藏、已发布文章和 AI 草稿。它们不能仅因为主题相同就进入同一目标目录。

### 4. AI 结果先进入 staging

AI 生成的概念页、实体页、路径映射、关系边和清理建议都先进入 `.kb/staging/`。未经审核，不能直接成为正式 Wiki 或图谱。

### 5. 小批量验证后再扩大

建议首批只迁移 20～50 个有代表性的 Markdown 文件，同时覆盖原创笔记、外部来源、已发布作品、附件和双链。

## 二、旧目录到新目录的映射

| 旧知识库内容 | 推荐目标 | 判断标准 |
|---|---|---|
| 日记、随手记、原创想法 | `raw/notes/` | 由知识库所有者原创，尚未发布 |
| 口述记录和自己的会议复盘 | `raw/voice/` 或 `raw/notes/` | 主要表达所有者自己的经历与判断 |
| 自己完成的调研结论 | `raw/research/` | 重点是个人分析，而非外部材料原文 |
| 正在发展的个人草稿 | `raw/drafts/` | 已决定继续创作，但未达到发布状态 |
| 网页剪藏、第三方文章 | `sources/clips/` | 外部作者或机构的内容 |
| 论文 | `sources/papers/` | 可引用研究材料 |
| 书摘 | `sources/books/` | 外部出版物摘录或阅读笔记中的引用部分 |
| 第三方报告 | `sources/reports/` | 外部机构发布的报告 |
| 外部视频、播客转写 | `sources/media/` | 外部媒体材料；不包含音视频二进制 |
| 已发布文章、课程和脚本 | `products/` | 已由所有者审核、发布或交付 |
| 已审核概念页和实体页 | `wiki/` | 用于导航和综合，并且能够引用原文 |
| 无法确认来源或审核状态的总结 | `.kb/staging/wiki/` | 先作为候选，不直接进入正式 Wiki |
| AI 未审核草稿 | `.kb/staging/drafts/` | 尚未由人确认 |
| 旧库中的关系数据和自动生成索引 | `.kb/staging/links/` 或 `.kb/staging/migration/` | 先校验 schema、路径和证据 |

### 混合目录怎么处理

不要把混合目录整体映射。例如旧库的 `AI/` 目录可能需要拆分为：

```text
AI/我的想法.md              -> raw/notes/
AI/某公司技术博客.md        -> sources/clips/
AI/已发布的RAG文章.md       -> products/articles/
AI/RAG概念总结.md           -> .kb/staging/wiki/ 或 wiki/concepts/
```

如果无法确定内容角色，标记为 `needs_review`，不要猜测。

## 三、迁移前准备

### 1. 备份旧知识库

至少保留一份独立备份。如果旧库已经使用 Git，先确认工作区状态并创建一个迁移前提交或 tag。

```bash
git status
git tag before-ai-content-kb-migration
```

如果旧库没有 Git，可以复制到一个只读备份目录。迁移完成前不要清理备份。

### 2. 检查敏感与不可公开内容

如果新库会公开到 GitHub，迁移前识别：

- 私人日记、健康、财务和客户信息；
- API Key、Cookie、Token、`.env` 和私有链接；
- 无权公开的网页全文、书籍、论文附件和课程材料；
- Obsidian 插件缓存、工作区状态、回收站和日志；
- 包含用户名的绝对路径和本机挂载配置。

公开库应优先保存外部材料的元数据、来源链接和原创摘要，而不是完整版权内容。

### 3. 建立迁移记录

为每个旧文件记录一行迁移信息。推荐放在：

```text
.kb/staging/migration/migration-map.csv
```

建议字段：

```csv
old_path,new_path,role,sha256,status,review_note
```

其中 `status` 可以使用：

```text
inventoried
needs_review
approved
copied
validated
skipped
```

迁移映射中不要提交外部绝对路径到公开仓库。公开前可以保留相对标识，或将本地映射放在被忽略的 `.kb/metadata/local-migration-map.csv`。

## 四、使用 Codex 迁移

打开新 clone 的 `ai-content-kb` 项目后，可以分阶段给 Codex 下达任务。

### 阶段 1：只读盘点

```text
迁移旧知识库：只读盘点我提供的旧知识库目录。
不要移动、复制、删除或修改任何旧文件。
统计 Markdown、附件、目录、wikilink、标签、frontmatter 和疑似敏感文件。
按 raw、sources、products、wiki、needs_review 给出初步分类。
将报告写入 .kb/reports/migration-inventory.md。
```

盘点报告至少应包含：

- 文件和目录数量；
- 文本与二进制类型；
- 主要目录及样本；
- 来源身份不明确的目录；
- 已发布内容的位置；
- 附件引用方式；
- 断链和 unresolved wikilink；
- 重复文件和疑似重复概念；
- 私密信息与版权风险；
- 建议的首批试迁范围。

### 阶段 2：生成映射，不复制

```text
根据 migration-inventory.md 生成迁移映射草稿。
逐项给出 old_path、建议 new_path、内容角色、判断依据和风险。
无法确定的标记 needs_review。
写入 .kb/staging/migration/，不要开始复制。
```

审核重点：

- 个人判断是否误归到外部来源；
- 外部剪藏是否误归到 `raw/`；
- 已发布内容是否误当作普通素材；
- 旧 Wiki 是否有可靠引用；
- AI 草稿是否被误认为已审核内容。

### 阶段 3：试迁一小批

```text
迁移旧知识库：根据已审核映射试迁第一批 30 个文件。
只复制，不移动或删除旧文件。
保留正文和必要 frontmatter；不要批量插入新 wikilink。
AI 生成的 Wiki 和关系候选写入 staging。
完成后生成 .kb/reports/migration-batch-001.md。
```

首批样本建议包括：

- 5～10 条原创笔记；
- 5～10 份外部来源；
- 3～5 篇已发布作品；
- 3～5 个旧 Wiki 或 MOC 页面；
- 带图片或附件的文件；
- 包含别名、标签和复杂双链的文件。

### 阶段 4：验证试迁结果

```text
检查知识库：验证 migration-batch-001。
比较迁移前后文件 hash、正文、链接、附件、frontmatter 和文件数量。
检查错误分类、重复 alias、断链、无来源 Wiki 页面和隐私风险。
先输出报告，不自动修复。
```

只有当试迁的分类准确率、链接完整性和人工审核成本可接受时，才扩大批次。

### 阶段 5：分批扩大

按内容角色或主题分批，每批完成：

```text
审核映射 -> 复制 -> 生成 staging -> 验证 -> 人工确认 -> Git commit
```

不要把整个旧库作为一个 commit。推荐提交信息：

```text
Migrate raw notes batch 001
Migrate external clips batch 001
Migrate reviewed wiki concepts batch 001
```

### 阶段 6：切换和收尾

满足以下条件后再把新库作为主知识库：

- 关键内容均有迁移记录；
- 抽样文件 hash 或正文一致；
- Obsidian 关键链接和附件可打开；
- 正式 Wiki 页面能够回到原文；
- 未审核内容仍在 staging；
- 发布目录的构建流程未受影响；
- 私密内容没有进入公开仓库；
- 至少完成一次完整备份和恢复测试。

切换后，旧库仍建议保留只读一段时间，再决定是否归档。

## 五、手动复制方法

在映射已经审核的前提下，可以先 dry run：

```bash
rsync -av --dry-run /path/to/approved-old-batch/ raw/notes/
```

确认输出后再移除 `--dry-run`：

```bash
rsync -av /path/to/approved-old-batch/ raw/notes/
```

注意：

- 不要使用 `--delete`；
- 不要把未经分类的整个旧库同步到一个目标目录；
- 路径中有空格时要使用引号；
- 复制后检查文件数量与 hash；
- 外部产品仓库可以继续独立维护，通过本地挂载或引用接入 `products/`。

## 六、Obsidian 专项处理

### `.obsidian/` 配置

不要直接复制整个旧 `.obsidian/`。以下内容通常属于本机状态，应谨慎处理或忽略：

```text
workspace.json
workspace-mobile.json
插件缓存
插件登录状态
最近打开文件
```

主题、快捷键和图谱颜色等可移植配置可以逐项选择。新项目已经提供一个通用 `.obsidian/graph.json`。

### Wikilink

- 先保留原链接文本；
- 生成断链报告；
- 建立 canonical concept 和 alias 映射；
- 只修复已确认的目标；
- 不要一次性把所有普通文本替换成 wikilink。

### 附件

- 检查 Markdown 中的相对路径和 `![[attachment]]`；
- 保持与正文相对关系，或按内容角色迁移附件；
- 音频和视频二进制不要进入 vault；
- 公开仓库迁移图片前检查版权与隐私。

### 图谱

Obsidian 图谱来自 Markdown 链接和标签。`.kb/links/*.yaml` 不会自动显示为 Obsidian 连线。迁移后优先修复高价值 Wiki 页面之间的可视化链接，不追求一次性恢复所有历史边。

## 七、常见迁移错误

### 把所有旧笔记都放进 `raw/`

外部剪藏不是个人原创。先判断作者和来源。

### 把旧 Wiki 直接视为事实源

旧 Wiki 可能是无来源总结。缺少引用的页面应先进入 `.kb/staging/wiki/`。

### 一次性重写所有路径和双链

这种操作难以审核和回滚。先生成映射和断链报告，再分批修复。

### 迁移二进制媒体和缓存

视频、音频、缓存和渲染产物会快速扩大仓库，也可能包含隐私。只保留转写、元数据和外部地址。

### 迁移后立即删除旧库

至少等到文件、链接、附件、查询和备份全部验收后再归档旧库。

## 八、迁移验收清单

- [ ] 旧知识库有独立备份并保持只读。
- [ ] 每个迁移文件都有 old path、new path、角色和状态记录。
- [ ] `raw/` 中没有明显外部剪藏。
- [ ] `sources/` 中的材料保留来源信息。
- [ ] `products/` 只包含已审核或已发布内容。
- [ ] 无来源旧 Wiki 页面仍在 staging。
- [ ] AI 生成内容没有直接进入正式层。
- [ ] 关键 Markdown 链接、wikilink 和附件路径有效。
- [ ] alias 没有明显重复或循环引用。
- [ ] 抽样文件正文或 hash 与旧库一致。
- [ ] 没有迁移密钥、私密路径、缓存和无权公开的材料。
- [ ] 每批迁移都有独立 Git commit。
- [ ] 可以用真实问题查询并回到原文。

## 九、推荐的第一个迁移提示词

```text
迁移旧知识库：先进行只读盘点。

旧知识库位于：<提供路径或附件>

要求：
1. 不修改、移动、复制或删除旧文件；
2. 按 raw、sources、products、wiki、needs_review 分类；
3. 统计 Markdown、附件、wikilink、标签和 frontmatter；
4. 标记隐私、版权、重复内容、断链和来源不明问题；
5. 推荐 20～50 个文件作为首批试迁样本；
6. 将报告写入 .kb/reports/migration-inventory.md；
7. 只生成报告，等待我审核后再继续。
```
