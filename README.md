# Student Calendar Coordinator / 学生日程协调器

A no-code, human-in-the-loop workflow for coordinating institutional email, Blackboard calendar feeds, formal timetables, Outlook Calendar, and task managers without blindly duplicating every item.

一个无需编程、保留人工确认环节的学生信息协调工作流，用于整合学校邮件、Blackboard 日历订阅、正式课表、Outlook Calendar 与任务管理器，同时避免机械复制和重复事项。

## Core idea / 核心思想

Treat every source as evidence about an underlying real-world item, not as an instruction to create another calendar entry.

把每个来源视为“同一现实事项的证据”，而不是“再创建一条日程的命令”。

The workflow first determines whether incoming information is:

流程会先判断新信息属于：

- the same item as an existing event / 已有事项的同一事件；
- complementary information / 可以互补的信息；
- a confirmed update or cancellation / 明确的修改或取消；
- a possible duplicate / 疑似重复；
- a genuine conflict requiring confirmation / 需要人工确认的真实冲突；
- optional or informational content that should not be auto-added / 不应自动加入的可选或纯信息内容。

## Workflow / 工作流

```text
School email / 学校邮件 ───────────────┐
Blackboard calendar / Blackboard日历 ──┼─> Matching, merging and conflict handling
Formal timetable / 正式课表 ───────────┤    匹配、互补与冲突处理
Existing normalized events / 已有规范项 ┘                 │
                                                          ├─> Outlook Schedule / 日程
                                                          ├─> Outlook Deadlines / 截止日期
                                                          ├─> TickTick/Dida Inbox / 滴答收集箱
                                                          └─> User confirmation / 人工确认
```

See the bilingual Mermaid diagrams for the complete data flow and decision logic.  
完整数据流与决策逻辑见中英双语 Mermaid 流程图。

- [Workflow diagrams / 工作流图](docs/flowchart.md)

## Main capabilities / 主要能力

- Multi-source deduplication based on course codes, names, links, time, context and update relationships.  
  基于课程代码、名称、链接、时间、上下文和变更关系进行多来源去重。
- Field-level completion, such as using email to add a room missing from Blackboard.  
  按字段互补，例如使用邮件补充 Blackboard 中缺失的教室。
- Update and cancellation chains without leaving stale duplicates.  
  正确处理延期、改期和取消，不遗留旧版本重复项。
- Separation of compulsory events from actionable deadlines.  
  区分必须出席的时间段事件与需要完成的硬性 DDL。
- Human confirmation for ambiguous, optional or conflicting information.  
  对含糊、可选或互相冲突的信息保留人工确认。
- A hidden Blackboard calendar can remain an information source while only normalized calendars are shown on the home screen.  
  Blackboard 可在桌面隐藏但继续作为后台信息源，桌面只显示整理后的规范日历。

## Recommended Outlook layout / 推荐的 Outlook 结构

- `Schedule / 日程`: normalized compulsory time-block events.  
  `日程`：整理后的硬性时间段事件。
- `Deadlines / 截止日期`: normalized hard deadlines.  
  `截止日期`：整理后的硬性截止事项。
- `Blackboard`: read-only background source; it may be hidden from widgets.  
  `Blackboard`：只读后台信息源，可以从桌面小组件中隐藏。
- Default `Calendar`: kept separate and not used as the school automation target.  
  默认 `Calendar`：与学校自动化分离，不作为自动写入目标。

## Repository contents / 仓库内容

- [Architecture and coordination rules / 架构与协调规则](docs/architecture.md)
- [Workflow diagrams / 工作流图](docs/flowchart.md)
- [Design decisions / 设计决策](docs/design.md)
- [Setup guide / 配置教程](docs/setup.md)
- [Pitfalls and lessons learned / 踩坑记录](docs/pitfalls.md)
- [Reusable coordinator prompt / 可复用协调提示词](prompts/calendar-coordinator-template.md)
- [Anonymized validation scenarios / 匿名验收场景](examples/scenarios.md)
- [Security guidance / 安全说明](SECURITY.md)
- [Contribution guide / 贡献指南](CONTRIBUTING.md)
- [Project history / 项目变更记录](CHANGELOG.md)

## Important limitation / 重要限制

This repository documents a workflow design and prompt specification, not a self-hosted software package. Actual capabilities depend on the automation platform, connected-service permissions, institutional policies and calendar-feed delay.

本仓库记录的是工作流设计与提示词规范，不是可独立部署的软件包。实际能力取决于自动化平台、连接器权限、学校策略以及日历订阅延迟。

## Privacy / 隐私

Never publish live Blackboard feed URLs, personal task-email addresses, Gmail message links, Outlook calendar IDs, OAuth data, student identifiers or private course information.

禁止公开真实 Blackboard 订阅链接、个人任务邮箱、Gmail 原文链接、Outlook Calendar ID、OAuth 数据、学号或私人课程信息。

See [SECURITY.md](SECURITY.md) for bilingual guidance.  
完整中英双语说明见 [SECURITY.md](SECURITY.md)。

## License / 许可证

The project uses the MIT License. The English text in `LICENSE` is the legally controlling version; a Chinese reference translation is included for accessibility.

本项目采用 MIT 许可证。`LICENSE` 中的英文文本为具有法律效力的正式版本，同时附有中文参考译文以便理解。
