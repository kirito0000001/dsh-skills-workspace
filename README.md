# DSH Skills Workspace

三个 DeepSeek Harness（DSH）技能的工作副本 —— 平时用来迭代技能本身，改坏了从这里恢复：

**`zhihu` → `wiki-collect` → `wiki-write`**：先查别人走过的路，再把一手材料读成能引用的账本，最后写成别人看得懂的成稿。

本仓库采用 [MIT 许可证](./LICENSE)，文档与脚本均可自由使用、修改、再分发。

---

## 技能一览

| 技能 | 做什么 | 典型触发 |
| --- | --- | --- |
| **`zhihu`** | 动手前先查别人走过的路。固定流程：多轮检索建候选清单 → 统一研读知乎/CSDN/GitHub/官方文档 → 自己实跑验证。 | 「开始做」「动手」「查一下原因」「调研一下」 |
| **`wiki-collect`** | 把一手材料（代码仓库、剧本与设定、旧稿、实验数据）读成《素材账本》+《设定表》+《缺口清单》，每条带可回溯定位、够核对的证据、能判真假的状态。 | 「先采集素材」「把这些材料整理成能引用的」「读一遍这个仓库」 |
| **`wiki-write`** | 把《素材账本》写成成稿：README、技术文章、设定集条目、剧本、小说、学习路径。负责重构、成文与验收。 | 「帮我写一篇」「写个 README」「这段读起来不清楚」「帮我改稿」 |

三者是流水线关系：**检索**归 `zhihu`，**采集**归 `wiki-collect`，**成文**归 `wiki-write`。各守一段，不互相越界。

---

## 目录结构

```
.
├── zhihu/
│   └── SKILL.md                        # 检索与验证流程（单文件技能）
├── wiki-collect/
│   ├── SKILL.md                        # 主入口：路由到 references/ 下对应的 1–2 份
│   ├── references/
│   │   ├── 采集-代码.md                 # 按材料类型分的采集法
│   │   ├── 采集-剧本与设定.md
│   │   ├── 采集-旧稿与数据.md
│   │   ├── 素材账本字段.md              # 账本/设定表/缺口清单的字段契约
│   │   ├── 完成判据.md                  # 交付门
│   │   ├── 冲突裁决.md                  # 材料互相矛盾时怎么判
│   │   ├── 子代理.md                    # 大材料拆给子代理并行读
│   │   ├── 问作者.md                    # 缺信息时该问什么
│   │   └── 规则契约.md                  # 由规则表机械生成的索引
│   └── tools/
│       └── check.mjs                   # 账本机检（定位能否回原文、条数是否够）
└── wiki-write/
    ├── SKILL.md
    ├── references/
    │   ├── 文体-README.md               # 六种文体的骨架与验收差异
    │   ├── 文体-技术文章.md
    │   ├── 文体-设定集.md
    │   ├── 文体-剧本.md
    │   ├── 文体-小说.md
    │   ├── 文体-学习路径.md
    │   ├── 重构.md                      # 定读者与主线、去耦合、拆篇
    │   ├── 验收.md                      # 零上下文复述测试
    │   ├── 词表-中文.md                 # 用词与标点约定
    │   └── 规则契约.md                  # 由规则表机械生成的索引
    └── tools/
        ├── check.mjs                    # 成稿机检（文体结构、角标、禁用句式）
        ├── publish.mjs                  # 生成/校验发布版（发布不变式）
        └── gen-contract.mjs             # 从 references/ 抽规则表重建 规则契约.md
```

---

## 使用方式

DSH 的技能加载器只认 `<skills>/<技能名>/SKILL.md` 这一种路径（恰好两段），并且技能目录内只收 `.md` 文件。所以整个目录是这样落地的：

```bash
git clone https://github.com/kirito0000001/dsh-skills-workspace.git
# 然后把 zhihu/、wiki-collect/、wiki-write/ 三个目录放进你的 DSH skills 目录
```

`.gitignore` 与 `.git/` 放在技能根下不会被当成技能，也不影响加载。

### 依赖

参考文档零依赖；`tools/` 下的脚本需要 **Node.js**（脚本本身零第三方依赖）：

```bash
# wiki-collect：机检账本
node wiki-collect/tools/check.mjs --topic <专题目录> [--expect-entries N]

# wiki-write：生成发布版、校验发布不变式（两文件必须逐字节相同）
node wiki-write/tools/publish.mjs --draft 04_草稿.md
node wiki-write/tools/publish.mjs --verify 04_草稿.md 06_发布版.md

# wiki-write：成稿级检查
node wiki-write/tools/check.mjs --file 06_发布版.md --genre readme|wiki|tech

# wiki-write：规则契约重建（改了规则表之后跑）
node wiki-write/tools/gen-contract.mjs <草稿目录>
```

---

## 说明

- 这两个 wiki 技能按**「日常档 / 改进档」**分档：日常档只交付成品，改进档才额外产出复盘与技能反馈。默认走日常档。
- 仓库内容以中文写成，规则条文带编号（如 `COL-C25`、`WVR-17`、`WRT-TA-C01`），跨多个文件互相引用；改动规则表后建议跑一遍 `gen-contract.mjs` 与 `check.mjs`。
- 这是一份仍在迭代中的工作副本，条文会随实战修订；不保证与任何发布版本一致。

## 许可证

[MIT](./LICENSE) © 2026 晓桀
