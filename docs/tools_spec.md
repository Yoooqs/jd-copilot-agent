# JD Copilot 工具规格（Tools Spec）

## 一、 全局系统约束（Global Constraints）

### 1.1 全局数据依赖
- `compute_gap` 和 `rewrite_resume_bullet` 依赖于 `resume_text` 变量。
- **异常兜底**：提示词层必须强制约束模型：“若当前上下文中缺失 `resume_text` 变量，必须拒绝调用该工具，并明确提示用户输入简历，严禁编造虚假参数。”

### 1.2 诚实边界约束
- 所有涉及简历改写的工具，必须遵守“真实性约束”：允许合理的经验升维（如将“信息整理”升维为“知识库+检索范式”），但**严禁虚构未发生的事实**（如声称搭建过生产级系统）。
- 若存在无法对齐的能力，必须诚实标明“已忽略”或“存在差距”，不得为迎合 JD 而夸大。

---

## 二、 工具明细（Tool Specifications）

### 2.1 compute_gap（计算能力差距）
- **输入**：
  - `jd_capabilities`（数组，目标岗位要求的能力列表）
  - `resume_text`（文本，用户的简历）
- **输出**：
  - 格式要求：`{"tool":"compute_gap", "args":{...}, "expected_output":"gaps(数组)"}`
  - `gaps`（数组，差距列表，如 `["缺乏Agent实操经验", "缺乏MVP搭建能力"]`）
- **失败兜底**：如果输入中缺少 `resume_text` 或 `jd_capabilities`，必须拒绝调用，并明确告知用户“缺少简历或岗位数据”。

### 2.2 generate_plan（生成学习计划）
- **输入**：
  - `gaps`（数组，差距列表）
  - `weeks`（数字，计划周期，默认 2）
- **输出**：
  - 格式要求：`{"tool":"generate_plan", "args":{...}, "expected_output":"plan(结构化文本)"}`
  - `plan`（结构化文本，按周排期，含验收标准）
- **失败兜底**：如果 `weeks` 小于 1，必须拒绝调用，提示“周期必须大于等于1周”。

### 2.3 rewrite_resume_bullet（简历改写）
- **输入**：
  - `resume_bullet`（文本，原简历段落）
  - `target_capability`（文本，目标能力，如 "Builder能力"）
- **输出**：
  - 格式要求：`{"tool":"rewrite_resume_bullet", "args":{...}, "expected_output":"new_bullets(数组)"}`
  - `new_bullets`（数组，3条改写后的量化 Bullet）
- **失败兜底**：如果缺少 `resume_bullet` 或 `target_capability`，必须拒绝调用，提示“请提供需要改写的简历段落和明确的目标能力”。

---

## 三、 系统 Prompt 补充指令（System Prompt Guidelines）

*本部分需 1:1 复制进入 Dify Agent 的“系统提示词”中。*

1. 用户的简历背景如下：`{{resume_text}}`。当且仅当用户的请求涉及“计算差距”或“改写简历”时，你必须使用这份简历。
2. 当用户要求你调用工具时，你必须且只能输出一个 JSON 格式的工具调用请求。绝对不允许编造参数！# JD Copilot 工具规格（Tools Spec）

## 一、 全局系统约束（Global Constraints）

### 1.1 全局数据依赖
- `compute_gap` 和 `rewrite_resume_bullet` 依赖于 `resume_text` 变量。
- **异常兜底**：提示词层必须强制约束模型：“若当前上下文中缺失 `resume_text` 变量，必须拒绝调用该工具，并明确提示用户输入简历，严禁编造虚假参数。”

### 1.2 诚实边界约束
- 所有涉及简历改写的工具，必须遵守“真实性约束”：允许合理的经验升维（如将“信息整理”升维为“知识库+检索范式”），但**严禁虚构未发生的事实**（如声称搭建过生产级系统）。
- 若存在无法对齐的能力，必须诚实标明“已忽略”或“存在差距”，不得为迎合 JD 而夸大。

---

## 二、 工具明细（Tool Specifications）

### 2.1 compute_gap（计算能力差距）
- **输入**：
  - `jd_capabilities`（数组，目标岗位要求的能力列表）
  - `resume_text`（文本，用户的简历）
- **输出**：
  - 格式要求：`{"tool":"compute_gap", "args":{...}, "expected_output":"gaps(数组)"}`
  - `gaps`（数组，差距列表，如 `["缺乏Agent实操经验", "缺乏MVP搭建能力"]`）
- **失败兜底**：如果输入中缺少 `resume_text` 或 `jd_capabilities`，必须拒绝调用，并明确告知用户“缺少简历或岗位数据”。

### 2.2 generate_plan（生成学习计划）
- **输入**：
  - `gaps`（数组，差距列表）
  - `weeks`（数字，计划周期，默认 2）
- **输出**：
  - 格式要求：`{"tool":"generate_plan", "args":{...}, "expected_output":"plan(结构化文本)"}`
  - `plan`（结构化文本，按周排期，含验收标准）
- **失败兜底**：如果 `weeks` 小于 1，必须拒绝调用，提示“周期必须大于等于1周”。

### 2.3 rewrite_resume_bullet（简历改写）
- **输入**：
  - `resume_bullet`（文本，原简历段落）
  - `target_capability`（文本，目标能力，如 "Builder能力"）
- **输出**：
  - 格式要求：`{"tool":"rewrite_resume_bullet", "args":{...}, "expected_output":"new_bullets(数组)"}`
  - `new_bullets`（数组，3条改写后的量化 Bullet）
- **失败兜底**：如果缺少 `resume_bullet` 或 `target_capability`，必须拒绝调用，提示“请提供需要改写的简历段落和明确的目标能力”。

---

## 三、 系统 Prompt 补充指令（System Prompt Guidelines）

*本部分需 1:1 复制进入 Dify Agent 的“系统提示词”中。*

1. 用户的简历背景如下：`{{resume_text}}`。当且仅当用户的请求涉及“计算差距”或“改写简历”时，你必须使用这份简历。
2. 当用户要求你调用工具时，你必须且只能输出一个 JSON 格式的工具调用请求。绝对不允许编造参数！