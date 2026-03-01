
---

## 1. 核心定义与设计思辨 (Core Definition & Speculation)

### 1.1 大模型的“巴别塔”困境
大语言模型 (LLM) 天生具有极强的发散性。对于同一个用户指令“帮我点赞”，不同的模型（甚至同一个模型在不同时间）可能会输出千奇百怪的 JSON：
*   GPT-4: `{"action": "tap", "target": "like_button"}`
*   Claude 3: `{"command": "click", "element": "heart_icon"}`
*   Llama 3: `{"operation": "press", "ui_component": "thumbs_up"}`

这种**语义漂移 (Semantic Drift)** 会导致严重的后果：
1.  **经验无法复用**：系统无法判断“click heart”和“tap like”是不是同一件事，导致离线沉淀的 Skill 无法被检索。
2.  **执行歧义**：动作面引擎无法解析非标准指令，导致执行失败或错误操作。

### 1.2 动作空间标准化 (Action Space Canonicalization)
AceAgent-Swarm 引入 **系统意图库 (System Intent Library)**，强制所有 Agent 在交互时必须使用**系统定义的唯一 Key**。
这相当于为 Agent 世界制定了一套“官方语言”：**DDD (Domain-Driven Design) 三段式意图**。

---

## 2. 意图库数据结构设计 (The Schema)

我们采用**分层、可扩展**的键值对结构，确保意图 Key 既具备人类可读性，又具备机器检索效率。

### 2.1 核心 Key 格式：三段式 DDD
`[Domain(App_ID)] : [Verb(动作)] : [Object(对象)]`

*   **Domain**: 应用包名或全局标识。
    *   *例*：`com.tencent.mm` (微信), `GLOBAL` (系统级操作)。
*   **Verb**: 标准化动作原语。
    *   *例*：`TAP` (点击), `SWIPE` (滑动), `INPUT` (输入), `LONG_PRESS` (长按)。
*   **Object**: 具体的业务功能对象。
    *   *例*：`SEND_BTN` (发送按钮), `SEARCH_BAR` (搜索框), `SCROLL_AREA` (滚动区)。

**完整示例**：
*   `com.tencent.mm:TAP:SEND_BTN` (微信发送)
*   `GLOBAL:SWIPE:UP` (全局上滑)
*   `com.taobao.taobao:INPUT:SEARCH_KEYWORD` (淘宝搜索输入)

### 2.2 意图元数据 (Metadata)
每个 Intent Key 在库中对应一个详细的 JSON 对象：

```json
{
  "key": "com.tencent.mm:TAP:PROFILE_AVATAR",
  "description": "点击聊天界面左上角的个人头像",
  "required_params": [], // 无需参数
  "optional_params": [],
  "anchors_hint": ["text:返回", "icon:more"], // 辅助锚点提示
  "risk_level": "LOW", // 风险等级
  "aliases": ["查看个人信息", "点头像", "view profile"] // 自然语言别名，用于检索
}
```

---

## 3. LLM 强约束机制 (Enforcement Mechanism)

有了标准库，如何强迫天马行空的 LLM 乖乖遵守？我们设计了**三道防线**。

### 3.1 第一道防线：Prompt 层的“枚举限制” (The Enum Constraint)
这是最直接、最高效的手段。利用 Function Calling 或 Structured Output 能力。

*   **机制**：
    在 Prompt 中，系统不让模型“生成”动作，而是让模型做**“选择题”**。
*   **Prompt 模板**：
    ```text
    当前页面识别到的可行操作（Action Space）如下：
    1. [com.tencent.mm:TAP:BACK_BTN] - 返回上一页
    2. [com.tencent.mm:TAP:VOICE_BTN] - 切换语音
    3. [com.tencent.mm:INPUT:CHAT_BOX] - 输入框
    
    用户指令：“回去。”
    请从上述列表中选择唯一的 Key。禁止捏造。
    ```
*   **效果**：模型输出被死死限制在当前页面的**有效操作集**内，杜绝了幻觉。

### 3.2 第二道防线：意图对齐网关 (Alignment Gateway)
针对长尾场景（模型输出了库里没有的动作，或 Prompt 过长无法完全枚举）。

*   **机制**：
    1.  模型输出原始自然语言意图：`"click the green button"`。
    2.  **向量检索引擎**介入：计算该描述与意图库中所有 `aliases` 的 Embedding 相似度。
    3.  **阈值判定**：
        *   相似度 > 0.95：自动纠正为标准 Key `com.app:TAP:SUBMIT_BTN`。
        *   相似度 < 0.8：判定为 **OOV (Out-Of-Vocabulary)**，标记为“未知新意图”。

### 3.3 第三道防线：动态经验注入 (RAG Injection)
利用 Agent 的历史记忆来约束当前决策。

*   **机制**：
    *   检索向量数据库：`Query(当前页面指纹 + 用户指令)`。
    *   找到历史成功的 Skill Blueprint：`BluePrint_ID: wx_send_msg_v2`。
    *   **注入 Prompt**：`"注意：你之前在类似场景下，成功执行过 [com.tencent.mm:TAP:SEND_BTN]。建议优先复用。"`

---

## 4. 意图库的进化与扩容 (Evolution)

系统意图库不是静态的，它具备**生命力**。

### 4.1 新意图发现 (Discovery)
当 LLM 被迫输出一个 **OOV (未知意图)**，且通过 VLM (视觉模型) 艰难执行成功后，系统会触发**“新词发现”**流程。

1.  **日志捕获**：记录下这次成功的操作 `(Screen_State, User_Instruction, Final_Coordinates)`。
2.  **离线复盘**：后台 AI 分析截图，提取点击位置的语义（比如是一个新上线的“直播”入口）。
3.  **自动命名**：生成新 Key `com.app:TAP:LIVE_ENTRY`。
4.  **入库**：写入意图库，并标记为 `Draft (草稿)` 状态。

### 4.2 社区共识 (Consensus)
*   当有 100 个不同的 Agent 都在该页面触发了 `com.app:TAP:LIVE_ENTRY`。
*   意图库将其状态升级为 `Stable (稳定)`。
*   **全网推送**：下一次所有 Agent 的本地库更新时，都会包含这个新意图。

---

## 5. 总结 (Summary)

AceAgent-Swarm 的意图库不仅是一个字典，它是连接人类自然语言与机器精准执行的**协议层 (Protocol Layer)**。

它实现了：
1.  **歧义消除**：让“点赞”在机器世界里有了唯一的身份证。
2.  **经验互通**：A 手机上的操作经验，因为 Key 相同，可以无缝传输给 B 手机。
3.  **模型解耦**：无论用 GPT-4 还是 Llama-3，只要输出同一个 Key，执行效果就完全一致。

这种高度标准化的设计，是实现**群体智能共享**的前提条件。

---

**下一篇文档预告**：
有了状态、动作、意图的标准化，我们如何通过离线计算，把海量的操作日志提炼成纯净的“幽灵骨架”？下一篇文档 **《文档五：离线进化——多帧交集与幽灵骨架提取算法》** 将揭秘 Agent 的“睡眠与做梦”机制。