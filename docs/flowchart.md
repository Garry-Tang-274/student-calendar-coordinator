# Workflow Diagrams / 工作流图

The diagrams below use bilingual labels so that the logic can be understood without switching documents.

以下流程图使用中英双语标签，无需切换文档即可理解逻辑。

## 1. Overall data flow / 总体数据流

```mermaid
flowchart LR
    A[School email<br/>学校邮件]
    B[Blackboard calendar<br/>Blackboard日历]
    C[Formal timetable<br/>正式课表]
    D[Existing normalized items<br/>已有规范条目]
    E[Task sent-mail history<br/>任务已发邮件记录]

    F[Identity matching<br/>同一事项识别]
    G[Field completion<br/>字段互补]
    H[Conflict and change handling<br/>冲突与变更处理]

    I[Schedule<br/>日程]
    J[Deadlines<br/>截止日期]
    K[TickTick/Dida Inbox<br/>滴答收集箱]
    L[User confirmation<br/>人工确认]
    M[Daily digest<br/>日汇总]

    A --> F
    B --> F
    C --> F
    D --> F
    E --> F
    F --> G
    G --> H
    H --> I
    H --> J
    H --> K
    H --> L
    H --> M
```

## 2. Decision before writing / 写入前决策

```mermaid
flowchart TD
    A[New source record<br/>新来源记录]
    B{Does a related item exist?<br/>是否存在相关事项?}
    C[Create a candidate<br/>形成新事项候选]
    D{Match confidence<br/>匹配置信度}
    E[Definite same item<br/>明确同一事项]
    F[Probable same item<br/>高度疑似同一事项]
    G[Definite different item<br/>明确不同事项]
    H[Merge or update fields<br/>合并或更新字段]
    I[Preserve current state and ask<br/>保留当前状态并询问]
    J[Create separate normalized item<br/>创建独立规范条目]

    A --> B
    B -- No / 否 --> C
    B -- Yes / 是 --> D
    D --> E
    D --> F
    D --> G
    E --> H
    F --> I
    G --> J
    C --> J
```

## 3. Authority by field / 按字段判断权威性

```mermaid
flowchart TD
    A[Conflicting values<br/>字段值冲突]
    B{Explicit correction?<br/>是否明确更正?}
    C[Use corrected value<br/>采用更正值]
    D{Clearly newer and same item?<br/>是否较新且明确同一事项?}
    E[Use newer targeted value<br/>采用较新的针对性值]
    F{Non-conflicting fields?<br/>字段是否其实不冲突?}
    G[Merge complementary fields<br/>合并互补字段]
    H[Conflict — confirmation required<br/>冲突——需要确认]

    A --> B
    B -- Yes / 是 --> C
    B -- No / 否 --> D
    D -- Yes / 是 --> E
    D -- No / 否 --> F
    F -- Yes / 是 --> G
    F -- No / 否 --> H
```

## 4. Change-chain handling / 变更链处理

```mermaid
stateDiagram-v2
    [*] --> NewItem: Announcement / 发布
    NewItem --> Active: Confirmed / 确认有效
    NewItem --> CancelledBeforeCreation: Cancellation already present / 首次处理前已取消
    Active --> Updated: Postponed or changed / 延期或修改
    Updated --> Active: Final state stored / 保存最终状态
    Active --> Cancelled: Explicit cancellation / 明确取消
    CancelledBeforeCreation --> [*]: Create nothing / 不创建
    Cancelled --> [*]: Delete normalized item / 删除规范条目
```

## 5. Home-screen presentation / 桌面显示层

```mermaid
flowchart LR
    A[Blackboard read-only source<br/>Blackboard只读信息源]
    B[Coordinator<br/>协调器]
    C[Schedule widget<br/>日程小组件]
    D[Deadline widget<br/>DDL小组件]

    A --> B
    B --> C
    B --> D

    E[Blackboard hidden from widget<br/>Blackboard在小组件中隐藏]
    E -. presentation only / 只影响显示 .-> A
```

Hiding Blackboard from the widget does not disable it as a source. Unsubscribing from it does.

在小组件中隐藏 Blackboard 不会停用该信息源；取消订阅才会。
