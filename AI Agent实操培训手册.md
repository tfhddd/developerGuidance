# 🚀任务一：【起手式】基于 Agent + Skill 的表达式代码生成（约 60 分钟）
### 目标：通过 AI Agent 对话，完成鲲鹏大数据算子表达式 `ConcatFunction` 的代码设计与生成。
### 我们要做什么
OmniOperator 是鲲鹏开源的高性能大数据 SQL 计算引擎库，使用 C++ 原生代码实现各类算子与表达式函数，通过 SIMD 向量化、LLVM JIT 编译等技术充分发挥 ARM64 硬件性能。其中，**向量化表达式**模块负责对列式数据批量执行 SQL 函数（如 `abs`、`substr` 等）。

今天我们要实现的是 `concat`**（字符串拼接）函数**——给定 2 到 10 个字符串参数，将它们按顺序拼接为一个字符串返回。

这是一项真实的开发任务：需要遵循项目 C++17 规范和命名约定，编写头文件、实现文件、单元测试，并在函数注册表中完成注册。

### 为什么用 Agent + Skill 来做
现在开发者写代码已经普遍会用 AI 辅助了——代码补全、片段生成、问答纠错等。但这种方式下，AI 只能帮开发者"补"当前这一行、这一段，开发者仍然需要自己串联整个开发流程：阅读项目源码、理清接口规范、逐文件编写、补充测试用例……所谓 AI 加持下的"**古法编程**"。

**Agent + Skill** 则是把 AI 的角色从"编程助手"升级为"能独立干活的开发者"：

+ **Agent（智能体）**：能读懂整个项目代码、理解需求、自主规划任务并执行。不是问一句答一句，而是开发者给一个需求，Agent 能自己分析项目 → 设计方案 → 编写代码 → 完成测试，全程自主完成。
+ **Skill（技能）**：一份结构化的"能力说明书"（`SKILL.md` 文件），告诉 Agent 在特定场景下该怎么做。可以类比为**给新员工写操作手册**——手册写得好，新员工（Agent）就能独立完成工作。

### 本次任务中 Agent 是怎么工作的
在本任务的工作区中，已经预置了本次任务相关的 AI工程文件，这些文件定义了 OmniOperator 的代码规范、项目结构和开发模式。当你在 Agent 对话框中输入需求时，Agent 会：

1. **加载 Skill**：自动匹配并读取相关的 Skill 文件，理解项目的编码规范和框架模式
2. **分析项目**：浏览现有代码结构，找到可参考的实现案例
3. **设计方案**：输出一份设计方案文档，等待你审核确认
4. **生成代码**：根据审核通过的方案，生成 `.h`、`.cpp`、测试文件和注册代码

### 你将体验到的 AI 作业范式
通过本任务，你将亲身感受以下关键范式：

| 范式 | 说明 |
| --- | --- |
| **需求 → 方案 → 代码** | Agent 不是一次性输出代码，而是先出方案、人工审核、再写代码，保持人在回路 |
| **Skill 功能** | Skill 文件如何定义，Skill 越完善，Agent 输出越靠谱 |
| **上下文感知** | Agent 会读取项目现有代码作为参考，生成的代码与项目风格保持一致 |
| **人机协作** | 开发者负责审核决策，Agent 负责执行实现，各司其职 |


---

## 1.1 打开工作环境（2 分钟）
**操作内容：**启动 Trae CN IDE 并打开任务一专用工作区。

**操作步骤：**

1. 双击桌面快捷方式 **Trae CN (任务一)**，系统将自动以 `TASK1` 文件夹作为工作区打开。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774856279622-db9e2c15-63ea-459f-9d35-73088b17bd5d.png" width="2391" title="" crop="0,0,1,1" id="ua11b0c6e" class="ne-image">

**易发问题**：

+ **工作区未显示 **`TASK1`** 文件夹**：确认双击的是"任务一"快捷方式而非"任务二"。

---

## 1.2 了解工程结构与 Skill 配置（5 分钟）
**操作内容**：浏览项目目录，理解 Agent + Skill 的工程组织方式。

**操作步骤**：

1. 查看以下文件，了解当前 AI 工程范式：
    - `OmniOperator/AGENTS.md` — 适配 Agent 作业的仓库文件
    - `.trae/skills/` 目录下的 Skill 定义文件

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1775885523821-d874bc72-e682-42ac-ae51-e7102a62e9ad.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_0%2Cw_1920%2Ch_860" width="1920" title="" crop="0,0,1,0.717" id="u0ab153cd" class="ne-image">

---

## 1.3 通过 Agent 对话生成表达式代码（约 10 分钟）
### 步骤一：打开 Agent 对话窗口
**操作内容**：打开 Agent 对话窗口，选择 AI 模型，为代码生成做好准备。

**操作步骤**：

1. 若 Agent 对话窗口未显示，请参考《Trae 基本使用指南》第 2 节进行打开。
2. 在模型选择区域，可以将当前模型切换为最新的 **GLM-5.1**。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774861024442-5fe3850f-6ca9-48c6-aa7d-4f12486ab3e2.png" width="1103" title="" crop="0,0,1,1" id="u27d2ebe0" class="ne-image">

**易发问题**：

+ **找不到模型切换入口**：模型选择区域位于 Agent 对话窗口的右下角。

### 步骤二：输入开发指令
**操作内容**：向 Agent 发送 concat 函数的开发需求，启动代码生成流程。

**操作步骤**：

在 Agent 对话框中输入以下指令，可直接复制粘贴

```plain
为 OmniOperator 实现 concat 函数，规格如下：

函数签名:
- 函数名: concat
- 输入: 2-10 个 varchar/string 参数
- 输出: varchar
- 可变参数: 支持 2-10 个参数

功能规范:
- 按参数顺序连接字符串
- NULL 处理: 任一输入参数为 NULL 则返回 NULL
- 向量编码支持: 常量向量、扁平向量、字典向量

交付件:
1. 函数实现文件 (.h/.cpp)
2. 在对应的 Register*.cpp 文件中注册函数
3. 单元测试，覆盖基本场景、NULL 处理和边界情况

约束条件:
- 本地无编译环境，仅完成代码实现
- 遵循 OmniOperator 现有代码风格和模式
- 参考 Velox 中的 concat 实现作为指导 

注意先生成设计文档待我审核同意后再进行代码生成
```

+ 确认指令内容无误后，按 **Enter** 键提交，Agent 将开始执行。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774861318960-f4a4dd4a-d423-469c-b599-68539d0e5de1.png" width="1112" title="" crop="0,0,1,1" id="u2ca72511" class="ne-image">**易发问题**：

+ **Agent 输出内容与指令不符**：可能是上下文窗口被其他内容干扰，建议点击对话窗口的"新建对话"按钮重新开始。

---

## 1.4 审核设计方案（5 分钟）
**操作内容**：审阅 Agent 自动生成的设计方案，确认技术方案合理后授权继续执行。

**操作步骤**：

Agent 在完成设计方案后会自动暂停，等待人工审核。

1. 资源管理器中会自动打开 Agent 生成的设计方案改动文件（通常命名为 `XX_design.md`）。
2. 审阅设计方案内容，如确认函数接口、数据结构、处理逻辑等是否符合预期。
3. 审核完成后，在 Agent 对话框中点击对应按钮，批准 Agent 开始实现代码
    - 若需修改，请描述具体修改意见后提交。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774865386043-836ae055-529f-49c9-b209-01e74419853a.png" width="2560" title="" crop="0,0,1,1" id="uae88729b" class="ne-image">**易发问题**：

+ **Agent 没有暂停等待审核，直接开始写代码**：部分模型行为可能跳过审核环节直接执行。若遇到此情况，可以中断对话，然后点击本轮会话刚开始处的 "**回退到本轮对话发起前**" 按钮

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774870096611-02866122-0d0d-4d23-8af3-4afb629f595b.png" width="778" title="" crop="0,0,1,1" id="xD4l0" class="ne-image">

+ **找不到设计方案文件**：设计方案文件通常生成在项目根目录下。
+ **不确定方案是否合理**：重点关注以下几点——函数签名是否匹配指令规格、NULL 处理策略是否正确、是否覆盖了三种向量编码类型。

---

## 1.5 等待代码生成并核验结果（约 10 分钟）
**操作内容**：等待 Agent 完成代码生成，并在资源管理器中核验交付件的完整性。

**操作步骤**：

1. Agent 将根据审核通过的设计方案，自动生成实现代码与单元测试。
2. 等待 Agent 完成全部代码生成。
3. 代码生成完成后，在资源管理器中核验以下交付件，**一般是以下形式**：

| 序号 | 文件 | 说明 |
| --- | --- | --- |
| 1 | `ConcatFunction.h` | 函数声明头文件 |
| 2 | `ConcatFunction.cpp` | 函数实现源文件 |
| 3 | `ConcatTest.cpp` | 单元测试文件 |
| 4 | `RegisterString.cpp`（新增行） | 函数注册入口 |


4. 在 Agent 对话框里点击 "全部保留" 即可将生成的代码保留到文件

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774868288398-4898ff3d-9e3d-43fe-b2f3-d0b73846f0f1.png" width="2560" title="" crop="0,0,1,1" id="u6b448018" class="ne-image">

**易发问题**：

+ **代码生成过程中 Agent 中断或报错**：可能是模型上下文过长导致。在对话框中输入"请继续完成代码生成"，Agent 通常能从中断处恢复。
+ **部分文件未生成（如缺少单元测试文件）**：在对话框中明确指出缺失的文件，例如"请补充 ConcatTest.cpp 单元测试文件"，Agent 会针对性补全。
+ `RegisterString.cpp`** 中没有新增注册代码**：Agent 可能遗漏了注册步骤，请在对话框中提示"请在 RegisterString.cpp 中注册 concat 函数"。
+ 如果生成代码质量非常错乱，可以点击本轮会话刚开始处的 "**回退到本轮对话发起前**" 按钮

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774870096611-02866122-0d0d-4d23-8af3-4afb629f595b.png" width="778" title="" crop="0,0,1,1" id="u30f0efbf" class="ne-image">

---

## 1.6 阶段小结与讨论（控制在 30 分钟内）
+ 总结任务一中 Agent + Skill 的协作流程。
    - Skill 如何指导 Agent 理解项目结构和编码规范
    - Agent 从需求分析 → 方案设计 → 代码生成的全流程
+ 讨论 Agent + Skill 这种 AI 作业范式，以及改进和未来发展方向。

---

# 🎯任务二：【见真章】端到端特性交付（Skill 编写 + 远程构建）（约 60 分钟）
### 目标：亲手编写 `remote-build` Skill，并完成远程同步、编译、UT 测试、PR 提交的完整交付流程。
### 我们要做什么
任务一中，我们体验了** 使用 **已写好的 Skill 来驱动 Agent 生成代码。在实际工作中，需要根据具体场景和作业流程，编写对应的 Skill，让 Agent 能更好的按照规范执行这些任务。

任务二的核心就是：**亲手编写一份 Skill，让 Agent 学会"远程构建"**。

### 这次的 Skill 要写什么
鲲鹏大数据平台的编译环境部署在远程服务器上（本地没有编译器），因此代码写完后需要：把代码同步到远程服务器 → 在远程服务器上编译 → 在远程服务器上运行单元测试。需要根据项目实际情况，编写一份专属的 Skill，把这个三步流程"教"给 Agent，让它能自主完成：

```plain
工具层（已有）：rsync_sync.sh、remote_exec.sh 等脚本 —— 封装了底层远程操作
     ↑
Skill 层（你来写）：SKILL.md —— 告诉 Agent 何时用、怎么用这些脚本
     ↑
Agent 层：读取 Skill，自主执行同步 → 编译 → 测试
```

可以类比为：脚本文件是**工具箱里的工具**，Skill 文件是**操作手册**，告诉 Agent 什么时候该拿哪把工具、按什么顺序操作。

### 你将学到什么
| 知识点 | 说明 |
| --- | --- |
| **Skill 的完整结构** | frontmatter（名称/触发描述）+ 适用场景 + 工具清单 + 工作流程 |
| **触发描述的写法** | description 中的关键词如何影响 Agent 的匹配精度 |
| **工作流程的定义** | 如何用"场景 → 命令"的形式描述 Agent 该做什么 |
| **从工具到能力的转化** | 如何把已有脚本封装为 Agent 可理解、可执行的 Skill |


> **说明**：为降低实操耗时，任务二的工作区（`TASK2`）已预置了一份**已上库验证的代码**，而非直接使用任务一生成的代码。这样可以避免任务一结果异常影响后续流程，确保每位学员都能顺利完成端到端交付体验。
>

---

## 2.1 打开工作环境（2 分钟）
**操作内容**：启动 Trae CN IDE 并打开任务二专用工作区。

**操作步骤**：

1. 双击桌面快捷方式 **Trae CN (任务二)**，系统将自动以 `TASK2` 文件夹作为工作区打开。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774871721172-ca7f8d79-8a28-41bc-8b37-924a8665ed89.png" width="2324" title="" crop="0,0,1,1" id="ub8223878" class="ne-image">**易发问题**：

+ **工作区未显示 **`TASK2`** 文件夹**：确认双击的是"任务二"快捷方式而非"任务一"

---

## 2.2 动手编写 Skill：让 Agent 学会远程构建（约 20 分钟）
**操作内容**：了解 Skill 的工程范式，从零编写一份 `remote-build` Skill，使 Agent 具备远程代码同步、编译和测试的能力。

> Skill 是 Agent 的"能力说明书"——通过编写一份 `SKILL.md` 文件，告诉 Agent 在什么场景下、使用哪些工具脚本、按什么流程完成任务。
>

---

### 步骤一：了解 Skill 目录结构
**操作内容**：浏览预置的远程操作脚本，以及`SKILL.md` 文件。

**操作步骤**：

1. 在资源管理器中，导航到 `.trae/skills/remote-build`目录。
2. 展开 `scripts`文件夹，浏览其中预置的脚本文件：

| 文件 | 功能说明 |
| --- | --- |
| `rsync_sync.sh` | 通过 rsync 将本地代码同步到远程服务器 |
| `remote_exec.sh` | 通过 SSH 在远程服务器上执行指定命令 |
| `ssh_connect.sh` | 建立 SSH 连接到远程服务器 |
| `config.ini` | 统一配置文件（远程主机地址、端口、用户名、密码、预定义任务等） |
| `compile.md` | 编译任务的命令参考 |
| `ut_test.md` | 单元测试任务的命令参考 |


3. 点击 `SKILL.md`文件（当前为空），后续需要在此文件中编写具体的** Skill **内容

**关键理解**：这些脚本已经封装好了底层的远程操作逻辑，我们需要做的是在 `SKILL.md` 中描述清楚"何时用、怎么用"，让 Agent 能够自主调用它们。

---

### 步骤二：编写 SKILL.md — Frontmatter 元信息
**操作内容**：在 `SKILL.md` 中填写 frontmatter 部分，定义 Skill 的名称和触发描述。

**操作步骤**：

在 `SKILL.md` 文件中填写 frontmatter 部分。frontmatter 是用 `---` 包裹的 YAML 块，用于定义 Skill 的名称和触发描述：

```markdown
---
name: "remote-build"
description: "将本地代码同步到远程服务器，编译并运行单元测试。当用户想要同步代码、编译或运行远程服务器上的单元测试时调用。"
---
```

**说明**：

+ `name`：Skill 的唯一标识名称。
+ `description`：描述 Skill 的功能与触发条件，Agent 会根据这段描述判断何时激活该 Skill。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1775885944909-a82b1700-fc2b-4498-8a48-84115e0630e1.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_0%2Cw_1920%2Ch_729" width="1920" title="" crop="0,0,1,0.6077" id="ub1869131" class="ne-image">

**易发问题**：

+ **frontmatter 格式错误导致 Skill 无法识别**：确保 frontmatter 的首尾各有三个短横线 `---`，且 `name` 和 `description` 的值用双引号包裹。多余的空格或缺少引号都会导致解析失败。
+ `description`** 描述过于笼统**：描述需要包含关键词（如"同步"、"编译"、"单元测试"），Agent 才能在用户提到这些词时精准匹配到该 Skill。

---

### 步骤三：编写 SKILL.md — 触发场景
**操作内容**：编写 Skill 的标题和适用场景列表，让 Agent 明确何时应该激活此 Skill。

**操作步骤**：

在 frontmatter 下方，继续编写 Skill 的标题和触发条件：

```markdown
# 远程构建技能

本技能帮助你将本地代码同步到远程服务器、编译项目并运行单元测试。

## 适用场景

在以下情况下调用本技能：
- 用户想要将本地代码同步到远程服务器
- 用户想要在远程服务器上编译代码
- 用户想要在远程服务器上运行单元测试
- 用户提到与远程服务器相关的"同步"、"编译"、"UT"、"单元测试"等操作
```

**说明**：`适用场景` 部分以列表形式明确列出触发场景，帮助 Agent 精准匹配用户意图。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1775886107598-e350aae3-58fc-4b54-a63a-c61425894cfa.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_0%2Cw_1920%2Ch_691" width="1920" title="" crop="0,0,1,0.5756" id="ub48d4182" class="ne-image">

**易发问题**：

+ **触发场景列表与 **`description`** 内容重复**：这是正常的。`description` 是给 Agent 的匹配引擎看的（较短），而"适用场景"是给 Agent 在执行时参考的详细说明（更具体），两者互补不冲突。
+ **列表中遗漏了某些场景**：至少要覆盖"同步"、"编译"、"测试"三个核心场景。如果后续发现 Agent 在某些指令下无法触发，可以回来补充场景描述。

---

### 步骤四：编写 SKILL.md — 可用脚本与配置
**操作内容**：以表格形式列出所有可用脚本及其功能，并补充配置说明。

**操作步骤**：

继续补充脚本清单和配置说明：

```markdown
## 可用脚本

所有脚本均位于本技能的 `scripts/` 子目录中：

| 脚本 | 说明 |
|------|------|
| [rsync_sync.sh](scripts/rsync_sync.sh) | 使用 rsync 将本地代码同步到远程服务器 |
| [remote_exec.sh](scripts/remote_exec.sh) | 通过 SSH 在远程服务器上执行命令 |
| [ssh_connect.sh](scripts/ssh_connect.sh) | 建立到远程服务器的 SSH 连接 |
| [config.ini](scripts/config.ini) | 所有脚本的配置文件 |

## 配置说明

`scripts/config.ini` 文件已预配置完毕，默认操作无需修改。
```

**说明**：以表格形式列出所有可用脚本及其功能，并说明配置文件已预置，无需修改。Agent 可据此选择合适的脚本。



<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1775886602740-bead1584-cc70-4c8e-8def-61c3e10000a5.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_0%2Cw_1920%2Ch_1030" width="1920" title="" crop="0,0,1,0.8585" id="u5b0280f2" class="ne-image">

**易发问题**：

+ **表格中的链接路径写错**：链接必须指向 `scripts/` 子目录（相对路径），而非绝对路径或上层目录。可以参考脚本文件的实际位置确认。

---

### 步骤五：编写 SKILL.md — 常用工作流程（约 4 分钟）
**操作内容**：编写 Skill 最核心的工作流程部分，定义代码同步、编译、测试三大操作的具体命令。

**操作步骤**：

```markdown
## 常用工作流

### 1. 同步代码到远程服务器

```bash
cd scripts
./rsync_sync.sh
```

### 2. 在远程服务器上编译

```bash
cd scripts
./remote_exec.sh -t compile
```

或参考 [compile.md](scripts/compile.md)。

### 3. 运行单元测试

```bash
cd scripts
./remote_exec.sh -t ut
```

或参考 [ut_test.md](scripts/ut_test.md)。

## 完整工作流示例

同步代码、编译并运行单元测试的完整流程：

```bash
cd scripts

# 第一步：同步代码
./rsync_sync.sh

# 第二步：编译
./remote_exec.sh -t compile

# 第三步：运行单元测试
./remote_exec.sh -t ut
```

```

**说明**：

+ `常用工作流` 是 Skill 的核心部分，以"场景 → 命令"的形式告诉 Agent 每个操作该执行什么。
+ `完整工作流示例` 提供完整的端到端流程参考。
+ Agent 在收到用户指令后，会根据这些工作流程自动选择并执行对应命令。

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774919730081-cf044d9f-99bf-462d-8caa-eba3acb74963.png" width="1612" title="" crop="0,0,1,1" id="ubdfd8e9f" class="ne-image">**易发问题**：

+ **Markdown 嵌套代码块格式混乱**：`SKILL.md` 中包含代码块嵌套（Markdown 内嵌 bash 代码块），注意区分外层 ` ```markdown ` 和内层 ` ```bash ` 的缩进与闭合，避免格式错乱。

---

### 步骤六：保存并验证 Skill（约 2 分钟）
**操作内容**：保存 `SKILL.md` 文件，确认目录结构和文件内容的完整性。

**操作步骤**：

1. 保存 `SKILL.md` 文件（`Ctrl + S`）。
2. 确认文件的最终目录结构为：

```plain
.trae/skills/remote-build/
├── SKILL.md                          ← 本节编写的 Skill 定义文件
└── scripts/
    ├── config.ini
    ├── rsync_sync.sh
    ├── remote_exec.sh
    ├── ssh_connect.sh
    ├── compile.md
    ├── ut_test.md
    ├── exclude.txt
    └── README.md
```

**验证要点**：

+ `SKILL.md` 的 frontmatter 中 `name` 和 `description` 字段已正确填写。
+ `适用场景` 中列出了同步、编译、测试三种触发场景。
+ `常用工作流` 中定义了每个场景对应的脚本命令。
+ 所有脚本路径引用正确（指向 `scripts/` 子目录）。

**易发问题**：

+ **忘记保存就进入下一步**：如果未保存 `SKILL.md`，Agent 读取到的仍然是空文件或旧内容，后续远程构建将无法触发 Skill。请养成编辑后立即 `Ctrl + S` 保存的习惯。

---

## 2.3 使用 Skill 完成远程构建与验证（代码同步 → 编译 → 单元测试）（15 分钟）
**操作内容**：通过一条指令让 Agent 自主规划并执行完整的远程构建流程（代码同步 → 编译 → 单元测试），验证 Skill 能否正确驱动 Agent 完成端到端构建。

**操作步骤**：

上一节编写的 `remote-build` Skill 已生效。在 Agent 对话框中输入以下指令：

```plain
请将 OmniOperator 的代码同步到远程服务器，然后进行编译并运行单元测试
```

+ 确认指令后，按 **Enter** 键提交。
+ Agent 将自动识别 `remote-build` Skill，自主规划执行步骤，依次完成同步、编译、测试。
+ 等待 Agent 执行完毕，观察输出日志。

> **说明**：OmniOperator 全量编译通常需要 **20 分钟以上**。为缩短课堂等待时间，远程服务器已启用 **ccache**（编译缓存），首次编译后后续编译可大幅加速（通常降至 1-2 分钟）。若首次编译仍耗时较长，请耐心等待，讲师可同步进行知识点讲解。
>

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774920980630-fc74ff12-5d0d-4979-a5bd-3ce82698f371.png" width="2560" title="" crop="0,0,1,1" id="ufcd4c4a9" class="ne-image">

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774923359007-815cd105-d936-4702-bd1f-15c9478774d4.png" width="2518" title="" crop="0,0,1,1" id="ua63e7021" class="ne-image">

<img src="https://cdn.nlark.com/yuque/0/2026/png/12811935/1774924014890-b77ab785-6221-4a3e-9b63-7f975940dc9a.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_429%2Cw_2517%2Ch_727" width="2517" title="" crop="0,0.3707,1,1" id="u15742c3b" class="ne-image">

**观察要点**：

+ Agent 是否自动识别到 `remote-build` Skill 并激活
+ Agent 是否按"同步 → 编译 → 测试"的顺序自主规划执行步骤
+ 每个步骤是否调用了正确的脚本（`rsync_sync.sh`、`remote_exec.sh -t compile`、`remote_exec.sh -t ut`）
+ 最终编译和测试结果是否全部通过

### 异常处理
在编译或测试阶段，若出现报错或失败用例：

1. 在 Agent 对话框中告知 Agent 进行自动修复。
2. 修复完成后，让 Agent 重新执行远程构建流程（同步 → 编译 → 测试）。

**易发问题**：

+ **Agent 没有识别到 Skill，直接尝试本地操作**：检查 `SKILL.md` 是否已保存、frontmatter 格式是否正确、文件是否位于 `.trae/skills/remote-build/` 目录下。也可在指令中更明确地提及"远程服务器"来帮助 Agent 匹配。
+ **代码同步阶段报连接错误**：可能是远程服务器网络波动，稍等片刻后重新发送同步指令。如果持续失败，请举手联系现场讲师。
+ **编译阶段耗时过长**：首次编译耗时长属于正常现象（ccache 尚未预热），讲师会在此期间继续讲解知识点。请耐心等待，不要中断 Agent 执行。
+ **编译报错但 Agent 无法自动修复**：将 Agent 输出的报错信息复制到对话框中，并提示"请根据以上编译错误修复代码并重新同步编译"，给予 Agent 更明确的修复指引。
+ **单元测试部分用例失败**：在对话框中告知 Agent 具体哪些用例失败，Agent 会针对性修复代码。修复后需重新执行编译和测试。

---

## 2.4 阶段小结与讨论（控制在 30 分钟内）
+ 总结任务二中端到端交付的完整流程。
+ 讨论 Agent 在编译、测试、提交环节的辅助效果。

---

# 🏅任务三：【自实现】鲲鹏大数据算子表达式 — 代码提交（约 30 分钟）
**目标**：综合运用任务一、任务二所学知识，自主创建/修改 Skill，让 Agent 自动完成创建分支、提交代码、创建 PR、生成 PR 总结等完整的代码提交流程。

> **说明**：本任务重点考察学员对 Agent + Skill 工作范式的理解与动手能力。学员需要参考任务二中编写 `remote-build` Skill 的经验，自主设计并实现一个面向代码提交场景的 Skill，让 Agent 能够按照规范完成从分支创建到 PR 提交的全流程。
>

---

## 3.1 了解需求：代码提交流程分析（约 5 分钟）
**操作内容**：分析代码提交的完整流程，梳理 Agent 需要具备的能力。

**操作步骤**：

1. 跟随讲师回顾代码提交的标准流程：

```plain
创建特性分支 → 暂存变更文件 → 提交到本地 → 推送到远程 → 创建 PR → 生成 PR 总结
```

2. 思考以下问题：
    - Agent 在每个环节需要执行什么操作？
    - 哪些环节需要遵循特定的规范（如分支命名、commit message 格式、PR 描述模板）？
    - 是否需要处理异常情况（如合并冲突）？
3. 切回 **Trae CN (任务一)** 工作区

**易发问题**：

+ **对 Git 工作流不熟悉**：可以在 Agent 对话框中询问 Agent 当前项目的 Git 规范。
+ **不清楚 PR 描述应该包含什么**：通常包括：变更目的、修改内容摘要、测试方式、影响范围。

---

## 3.2 自主设计 Skill：代码提交能力（约 10 分钟）
**操作内容**：参考任务二中 `remote-build` Skill 的编写经验，自主设计并编写一个代码提交 Skill。

**操作步骤**：

### 步骤一：创建 Skill 目录
1. 在资源管理器中，导航到 `.trae/skills/` 目录。
2. 在该目录下新建文件夹（名称由学员自行决定，建议语义清晰，如 `code-submit` 或 `git-workflow`）。
3. 在新建文件夹下创建 `SKILL.md` 文件。

### 步骤二：编写 SKILL.md
结合任务二中学习的 Skill 编写范式，独立完成 `SKILL.md` 的编写。编写时需考虑以下要点：

**frontmatter 部分**：

+ `name`：Skill 的唯一标识名称
+ `description`：描述 Skill 的功能与触发条件（需包含关键词，如"提交"、"分支"、"PR"、"Pull Request"等）

**正文部分建议包含**：

+ 适用场景：明确列出触发条件
+ 工作流程：按步骤定义 Agent 的操作序列
    - 创建特性分支（命名规范）
    - 暂存并提交代码（commit message 格式）
    - 推送到远程仓库
    - 创建 Pull Request
    - 生成 PR 总结
+ 异常处理：合并冲突等情况的处理策略
+ 规范说明：分支命名规范、commit message 格式、PR 描述模板

> **提示**：
>
> + 可以回顾任务二中 `remote-build` 的 `SKILL.md` 结构作为参考模板。
>

**易发问题**：

+ **不知道分支命名和 commit message 的格式**：可以查看项目中已有的 Git 历史记录（`git log`）作为参考，或者在 Agent 对话框中直接询问。
+ **Skill 编写完成后不确定是否正确**：可以先保存文件，在下一步中通过实际使用来验证。

---

## 3.3 使用 Skill 完成 Agent 驱动的代码提交（约 10 分钟）
**操作内容**：通过 Agent 对话，使用自主编写的 Skill 完成完整的代码提交流程。

**操作步骤**：

1. 确保 `SKILL.md` 已保存（`Ctrl + S`）。
2. 在 Agent 对话框中输入指令，触发代码提交流程：

```plain
请为 OmniOperator 项目创建一个新的特性分支，将当前修改提交并推送到远程仓库，然后创建 Pull Request 并生成 PR 总结
```

+ 确认指令后，按 **Enter** 键提交。
+ 观察 Agent 是否识别到新编写的 Skill，并按照 Skill 中定义的工作流程执行。
3. **观察与记录**：
    - Agent 是否按预期创建了特性分支？分支命名是否规范？
    - commit message 是否符合格式要求？
    - PR 是否成功创建？PR 总结内容是否完整？

**易发问题**：

+ **Agent 没有识别到新 Skill**：检查 `SKILL.md` 的 frontmatter 格式是否正确，确认文件位于 `.agents/skills/` 下的正确目录中。可在指令中更明确地提及 Skill 相关关键词。
+ **Agent 执行流程与 Skill 定义不一致**：Agent 可能根据上下文自行调整执行策略。可以在对话框中提示"请按照 Skill 中定义的工作流程执行"。
+ **PR 创建失败**：可能是远程仓库权限或分支冲突问题。

---

## 3.4 阶段总结与分享（约 30 分钟）
**操作内容**：展示与对比不同学员的 Skill 设计，总结 Agent + Skill 协作的最佳实践。

**操作步骤**：

1. **学员展示（自愿）**：邀请 2-3 位学员展示自己编写的 `SKILL.md`，讲解设计思路和遇到的问题。
2. **讲师点评**：对比不同 Skill 设计，总结以下最佳实践：
    - Skill 的触发描述（description）如何影响 Agent 的匹配精度
    - 工作流程的定义粒度：太粗导致 Agent 行为不确定，太细限制 Agent 灵活性
    - 异常处理策略的设计思路
3. **讨论拓展**：探讨还可以通过 Skill 让 Agent 掌握哪些能力（如自动化发布、代码审查、文档生成等）。

---

# 附录
## A. 常见问题
| 问题 | 解决方案 |
| --- | --- |
| Agent 对话窗口未显示 | 参考《Trae 基本使用指南》第 2 节 |
| Agent 未识别到 remote-build Skill | 检查 SKILL.md 的 frontmatter 格式是否正确，确认文件位于 `.trae/skills/remote-build/` 目录下 |
| 编译持续报错 | 检查远程服务器连接状态，确认 Skill 配置正确 |
| PR 提交被 pre-commit 拦截 | 让 Agent 自动修复；或检查代码格式是否符合规范 |
| Agent 未识别到自编写的代码提交 Skill | 检查 SKILL.md 的 frontmatter 格式、description 关键词、文件位置是否正确 |
| 模型切换后无响应 | 重启 Agent 对话窗口，重新选择 GLM-5.1 模型 |


## B. 操作速查
| 操作 | 对应步骤 |
| --- | --- |
| 切换模型为 GLM-5 | 任务一 1.3 步骤一 |
| 使用 `#` 选定文件夹 | 任务一 1.3 步骤二 |
| 审核设计方案 | 任务一 1.4 |
| 编写 remote-build Skill | 任务二 2.2 |
| 远程编译 | 任务二 2.3 |
| 远程 UT 测试 | 任务二 2.3 |
| 自主编写代码提交 Skill | 任务三 3.2 |
| Agent 驱动的代码提交与 PR | 任务三 3.3 |


## C. 任务二完整SKILL.md
```plain
---
name: "remote-build"
description: "将本地代码同步到远程服务器，编译并运行单元测试。当用户想要同步代码、编译或运行远程服务器上的单元测试时调用。"
---

# 远程构建技能

本技能帮助你将本地代码同步到远程服务器、编译项目并运行单元测试。

## 适用场景

在以下情况下调用本技能：
- 用户想要将本地代码同步到远程服务器
- 用户想要在远程服务器上编译代码
- 用户想要在远程服务器上运行单元测试
- 用户提到与远程服务器相关的"同步"、"编译"、"UT"、"单元测试"等操作

## 可用脚本

所有脚本均位于本技能的 `scripts/` 子目录中：

| 脚本 | 说明 |
|------|------|
| [rsync_sync.sh](scripts/rsync_sync.sh) | 使用 rsync 将本地代码同步到远程服务器 |
| [remote_exec.sh](scripts/remote_exec.sh) | 通过 SSH 在远程服务器上执行命令 |
| [ssh_connect.sh](scripts/ssh_connect.sh) | 建立到远程服务器的 SSH 连接 |
| [config.ini](scripts/config.ini) | 所有脚本的配置文件 |

## 配置说明

`scripts/config.ini` 文件已预配置完毕，默认操作无需修改。

如需自定义设置（例如更换远程主机），请参考以下配置文件结构：

```ini
[remote]
host=<远程主机地址>
user=<用户名>
password=<密码>
port=<SSH端口>

[sync]
source_path=.
target_path=/home/code/
```

## 常用工作流

### 1. 同步代码到远程服务器

```bash
cd scripts
./rsync_sync.sh                    # 使用默认配置
./rsync_sync.sh -n -v              # 试运行并显示详细信息
./rsync_sync.sh -d                 # 删除远程端多余文件
```

### 2. 在远程服务器上编译

```bash
cd scripts
./remote_exec.sh -t compile        # 运行预定义的编译任务
```

或参考 [compile.md](scripts/compile.md)。

### 3. 运行单元测试

```bash
cd scripts
./remote_exec.sh -t ut             # 运行预定义的单元测试任务
```

或参考 [ut_test.md](scripts/ut_test.md)。

### 4. 执行自定义命令

```bash
cd scripts
./remote_exec.sh 'ls -la /home/omni'
./remote_exec.sh -t sysinfo        # 运行预定义任务
./remote_exec.sh --list-tasks      # 列出所有可用任务
```

### 5. SSH 连接

```bash
cd scripts
./ssh_connect.sh                   # 使用默认配置连接
./ssh_connect.sh --setup-key       # 配置 SSH 密钥以实现免密登录
```

## 预定义任务

以下任务定义在 `config.ini` 中：

| 任务 | 说明 |
|------|------|
| `compile` | 编译 OmniOperator 和 Gluten |
| `ut` | 运行 OmniOperator 单元测试 |
| `sysinfo` | 显示系统信息 |
| `diskinfo` | 显示磁盘使用情况 |
| `meminfo` | 显示内存使用情况 |
| `procinfo` | 显示进程信息 |
| `netinfo` | 显示网络信息 |

## 完整工作流示例

同步代码、编译并运行单元测试的完整流程：

```bash
cd scripts

# 第一步：同步代码
./rsync_sync.sh

# 第二步：编译
./remote_exec.sh -t compile

# 第三步：运行单元测试
./remote_exec.sh -t ut
```

```


