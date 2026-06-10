# NMP 论文原理精要

> 来源：*The Null Model Paradigm: Constraint-First Architecture for Reliable LLM Agents*  
> 作者：loweswang  
> 发布：Zenodo正式版（DOI），2026年5月30日

---

## 一、问题诊断：DuMate崩溃的范式意义

### 观察事实
2026年5月30日，DuMate（本地部署的AI办公助手）在消费级设备（i5-7400, 8GB RAM, 120GB SSD, Win10）上执行文件整理任务时：
- 任务：整理约100个文本文件和数十个Word文档
- 指令："按内容和类型整理；汇总相同内容；保留原文件；保存到kb文件夹"
- 结果：连续三次失败，报错Internal Server Error: Bad Gateway

### 核心论点
**这不是实现缺陷，而是范式缺陷。**

当前Agent采用**Full-Model Paradigm（全模型范式）**：让单一LLM同时承担理解意图、规划操作、执行物理动作。LLM满载互联网语料、文化知识、统计模式，却缺乏物理世界所需的骨架结构。

---

## 二、AlphaGo直觉：结构先例

| 组件 | AlphaGo | NMP Agent |
|------|---------|-----------|
| 问题来源 | 对手的棋步 | 人类意图 + 世界状态 |
| 判断引擎 | MCTS + 价值网络 | **Null Model** |
| 知识来源 | 人类棋谱 / 自我对弈 | LLM + 元事实库 |
| 最终权威 | 引擎选择棋步 | **Null Model选择动作** |

**关键洞察**：AlphaGo/AlphaZero的搜索-评估引擎**没有围棋文化、没有人类直觉、没有开局库**——它只有通用的计算骨架（搜索、评估、选择）。人类棋谱只是候选来源，引擎决定哪一步真正执行。

NMP将这一分离推广到物理世界Agent：
- **LLM是开局库（Opening Book）**——提供候选策略
- **Null Model是棋手（Player）**——计算、评估、选择、执行

---

## 三、形式定义

### 知识分层：先验计算能力 vs 后天领域知识

在定义Full Model和Null Model之前，必须先澄清**"知识"的分层**。NMP架构中的"知识"不是单一概念，而是分布在不同层级的不同形态：

| 层级 | 知识形态 | 性质 | 所有者 | 与Null Model的关系 |
|------|---------|------|--------|------------------|
| **L1** | 人类目的 | 不可计算的价值判断 | 人类 | 注入为边界条件 |
| **L2 (Null Model)** | **先验计算能力** | **普遍、领域无关的形式推理** | 架构内置 | **Null Model的本质** |
| **L2 ↔ 元事实库** | **元事实/元规则** | **静态、确定、人类确认** | 人类维护 | Null Model**查询但不拥有** |
| **L3 (LLM)** | 训练知识 | 概率性、动态、模型内部 | LLM权重 | LLM**内部拥有**，Null Model**不可见** |
| **L3 ↔ 外部DB** | 领域数据 | 情境性、查询依赖 | 外部系统 | 经L2过滤后使用 |

**核心区分：**
- **先验计算能力**（A Priori Computational Capacity）：BFS图搜索、约束满足、校验和验证、路径选择——这些是Null Model的"本能"，如同CPU的指令集，不依赖任何领域。
- **后天领域知识**（A Posteriori Domain Knowledge）："C盘满了要清理""这个游戏需要8GB显存"——这些是LLM训练数据中的文化经验，Null Model**绝不内置**。
- **元事实**（Meta-Facts）：病历、性别、规则、档案——这些是**静态确定的事实**，存储在**元事实库**中，Null Model在执行提取动作前**查询**它们以确定"允许提取什么"，但查询后**不保留**、**不学习**、**不内化**。

![三层双视角辩证架构流程图](三层双视角（TDA）辩证架构流程图.png)

*上图展示了NMP的辩证结构：输入层同时驱动LLM（判例库驱动）和空模型（元事实库驱动），两者在"结构化辩论层"形成冲突图，最终由"元受动极层"的裁决AI（动态阈值/有依据随机）做出判断，经非认知结构性熔断器（人工审计/外部锚点拥有最高否决权）后，输出到外挂扬弃记忆，形成闭环反馈。*

---

### Full Model（全模型）
知识层被领域语料、文化经验、价值偏好、自主目标生成机制**最大化饱和**的AI系统。当代LLM（GPT-4、Qwen、Claude）及其衍生Agent是典型全模型。

### Null Model（空模型）
**知识自由的计算判断基底（Knowledge-free computational judgment substrate）**。

"Null"不是空集（∅），而是**撤离（Evacuation）**：
- 系统性地剥离所有**后天偶然知识**（领域语料、文化经验、价值偏好、自主目的）
- 保留完整的**先验算法骨架**（判断、搜索、评估、约束满足、最优路径选择）
- **不包含**领域知识、文化数据、价值偏好、自主目的
- **查询但不拥有**元事实库中的静态规则与事实

**操作输入**：
1. 人类目的（Human Purpose）——L1注入
2. 物理世界状态（Physical-world State）——经元事实库规则过滤后的探针数据
3. 形式约束（物理学、数学、逻辑、因果性）——先验计算能力
4. 元事实查询结果（Meta-Fact Query Results）——**只读边界条件，非知识**

**操作输出**：
- 允许的动作序列（Permitted Action Sequences）
- 执行裁决（Execution Verdicts）

**所有语义解释所需的知识都向Full Model（LLM）查询，但LLM对Null Model的裁决零修改权。**

---

### 元事实库（Meta-Fact Vault / 知识库1）

**定位**：人类确认的静态事实层，空模型在提取任何物理世界数据之前必须查询的**先验约束源**。

**本质**：它不是"数据库"，也不是"隐私工具"——它是**确定性知识的唯一可信来源**。

| 维度 | 元事实库 | LLM训练知识 |
|------|---------|------------|
| **确定性** | 100%确定（人类确认） | 概率性（统计模式） |
| **修改权** | 仅人类可修改 | 模型内部权重，不可直接修改 |
| **可见性** | 对空模型只读，对LLM不可见 | 对LLM内部可见，对空模型不可见 |
| **作用** | 约束提取动作、提供因果映射、存储历史判例 | 提供语言理解、生成候选策略 |
| **内容示例** | 病历、性别、¬Delete(source_file)、设备规格、规则档案 | "C盘满了要清理"、"Python列表推导式语法" |

**关键机制**：
- **提取前调度**：空模型在探针执行**之前**查询元事实库，确定"允许提取什么"
- **隐私是元事实的属性**：元事实库中包含隐私敏感数据（病历、身份），因此"隐私保护"是元事实库的**副作用**，而非其**目的**
- **判例法体系**：元事实库中的历史执行记录（日志）形成**判例法**，用于动态调整阈值和策略

---

### 六步流水线（Six-Step Pipeline）

1. **Problem**：人类以自然语言表达意图
2. **Null Model (Strategy Computation)**：查询元事实库获取提取规则 → 物理资源预算 → 数学任务分解 → 约束锁定 → 候选执行路径生成
3. **Full Model (Cognition)**：LLM执行语义理解与分类，只输出文本标签和知识片段
4. **Database (Knowledge)**：外部检索领域特定信息（经L2约束过滤）
5. **Full Model (Synthesis)**：LLM在Null Model约束下组装文本-only执行蓝图
6. **Null Model (Judgment & Execution)**：评估LLM蓝图的形式约束符合性（对照元事实库规则），选择最优允许路径，强制执行原子性，提供回滚保证
7. **Solution**：物理世界执行结果

---

## 四、三层架构防火墙与输入边界锁

### L1：意图注入（Intent Injection）——人类主权
人类提供目标、价值排序、成功标准。这一层**不可计算、不可委托**。

### L2：Null Model层——计算判断引擎
**唯一拥有物理世界执行权限的层**。它不只是"过滤"LLM输出，而是**从零计算执行策略**。LLM（L3）拥有：
- 零文件系统写入权限
- 零进程生成权限
- 零修改执行计划权限

**四个刚性计算模块**：

1. **Physical Budgeting Module**：监控RAM、磁盘空间、文件句柄、网络状态。在调用任何LLM之前计算安全批次大小（如8GB机器上每批10个文件）。

2. **Mathematical Planning Module**：在复杂度和资源约束下将任务分解为最优原子操作序列。这是**规划计算**，不是知识查询。

3. **Constraint Formalization Module**：将自然语言意图转化为不可变的逻辑断言。例如"不删除原文件"形式化为公理 ¬Delete(S_source)，永久剪枝Delete类中的任何候选操作。

4. **Verification & Rollback Module**：强制执行因果性（每个效果有可追溯的原因）、同一性（复制文件保持校验和一致）、可逆性（每批要么全提交要么全回滚）。

### L3：Full Model层——生成认知
LLM和外部数据库在此运行。它们的唯一权限：
- 读取文件内容（文本-only输入）
- 输出文本标签（如"Contract"、"Meeting Minutes"）
- 生成人类可读摘要

**零文件系统写入权限、零进程生成权限、零修改执行计划权限。**

---

### LLM的"输入边界锁"机制（替代"笼养"表述）

> **旧表述**："The Null Model Paradigm does not reject LLMs; it cages them."
> 
> **精确表述**：LLM不是"被剥夺权限的囚徒"，而是"在封闭考场答题的考生"。

**LLM的约束不是"权限阉割"，而是"输入边界锁定"**：

| 机制 | 说明 | 类比 |
|------|------|------|
| **输入边界** | LLM只能接收空模型过滤后的**相关事实+目的目标**，无法接触原始感知数据 | 考场提供所有必要的题目条件 |
| **目的边界** | LLM的策略必须服务于人类注入的目的，空模型会校验目的对齐性 | 答案必须符合题目要求 |
| **事实边界** | LLM若引用不存在的事实，空模型会标记为"不一致"并降低置信度 | 引用未给定的条件，答案无效 |
| **计算自由** | LLM在边界内拥有完整的推理自由，不受限制 | 考生可以任意推导 |

**关键**：LLM的"自由"在**计算层面**不受限制——它可以做任意复杂的语言推理。它的"约束"体现在**输入层面**——它只能基于空模型给定的**事实集合**和**目的约束**进行推理，不能"自己去找资料"。

**这不是"笼养"，这是"命题作文"**：
- 命题（目的）和素材（事实）由空模型提供
- LLM负责**文笔**（策略生成）
- 空模型负责**审题**（目的校验）和**评分**（事实一致性校验）
- 最终**定稿**（执行）由空模型决定

---

## 五、DuMate案例：反事实分析

### 全模型范式的失败链
1. **无物理预算**：LLM尝试同时处理150+文件，耗尽8GB RAM
2. **无原子性**：操作未分批次；中途崩溃导致文件系统不一致
3. **无回滚**：遇到Bad Gateway时，Agent生成道歉文本而非回滚部分复制
4. **语言歧义**：LLM"统计性理解"了请求，但没有对"保留原文件"的形式锁定

**LLM不是在"被缺失层支撑"，而是在物理世界上裸体翻滚。**

### NMP范式的反事实成功
1. **L2物理模块**扫描F:i：150个文件，总大小X，可用RAM≈4GB（扣除OS占用）。计算安全批次大小为10。
2. **L2查询元事实库**获取提取规则：允许提取文件名、大小、类型；禁止提取文件内容、路径。
3. **L2约束模块**形式化意图：Operation ∈ {Copy}, ¬Delete, ¬Move。
4. **L3 LLM**每批只接收10个文件的文本内容，返回标签（如"Contract"）。
5. **L2**清洗标签（剥离路径遍历字符、阻断保留名称）。
6. **L2**执行Copy(src, dst)，验证校验和同一性（MD5_src = MD5_dst），因果日志记录，继续执行。
7. **如果LLM超时（Bad Gateway）**，L2不会崩溃。它优雅降级：标签默认回退到文件扩展名，复制继续执行，异常记录供人类审查。

---

## 六、形式保证

NMP架构提供全模型范式无法提供的三项结构保证：

1. **Resource Safety（资源安全）**：Null Model在调用LLM之前先预算资源，保证物理耗尽不可能由LLM过度生成导致。

2. **Constraint Immutability（约束不可变性）**：一旦¬Delete(S)在L2中被形式化，任何LLM输出都无法覆盖它——由架构防火墙保证。

3. **Graceful Degradation（优雅降级）**：如果L3失败（超时、幻觉），L2回退到确定性默认值，保证系统始终处于安全状态。

---

## 七、AGI启示

当前工业实践通过**饱和**追求AGI：更多参数、更多数据、更多模态、更多自主行为。NMP认为这是一个**范畴错误**。

AGI不是一个膨胀的、"无所不知"的Full Model；它是一个最小的、"永不跌倒"的Null Model。

安全AGI的路径不是：
```
More Data → More Knowledge → More Autonomy
```

而是：
```
Harder Constraints → Reliable Judgment → Safe Execution
```

---

## 八、核心论断

> "The DuMate failure is not an anecdote; it is a paradigm indicator. LLM agents will continue to collapse in physical environments until the industry recognizes that generation without constraint is hallucination, and execution without judgment is destruction."

> "The Null Model Paradigm does not reject LLMs; it provides them with a structured examination hall. The LLM is not caged; it is given a clear proposition and verified facts, within which it can reason freely. The Null Model is not merely a cage; it is the examiner that sets the proposition, verifies the reasoning, and enforces the final verdict."

> "Intelligence is not the accumulation of knowledge; it is the discipline to know when not to act—and the hard shell that enforces that discipline."
