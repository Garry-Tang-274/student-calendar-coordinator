# 学生日程协调器
# Student Calendar Coordinator

![Status](https://img.shields.io/badge/status-workflow%20specification-2f81f7)
![Approach](https://img.shields.io/badge/approach-human--in--the--loop-8957e5)
![Language](https://img.shields.io/badge/docs-bilingual-d8aa56)
![License](https://img.shields.io/badge/license-MIT-3fb950)

这是一个无需编程、保留人工确认环节的学生信息协调工作流，用于整合学校邮件、Blackboard 日历订阅、正式课表、Outlook Calendar 与任务管理器，同时避免机械复制、重复事项和未经验证的自动写入。

This is a no-code, human-in-the-loop workflow for coordinating institutional email, Blackboard calendar feeds, formal timetables, Outlook Calendar, and task managers while avoiding blind duplication, stale records, and unverified automated writes.

> **核心思想：** 每个来源都是关于同一现实事项的证据，而不是“再创建一条日程”的命令。
>
> **Core idea:** Every source is evidence about an underlying real-world item, not an instruction to create another calendar entry.

[架构规则](docs/architecture.md) · [工作流图](docs/flowchart.md) · [配置教程](docs/setup.md) · [生产验收](docs/operational-validation.md) · [协调提示词](prompts/calendar-coordinator-template.md) · [个人主页](https://garry-tang-274.github.io)

[Architecture](docs/architecture.md) · [Workflow diagrams](docs/flowchart.md) · [Setup guide](docs/setup.md) · [Operational validation](docs/operational-validation.md) · [Coordinator prompt](prompts/calendar-coordinator-template.md) · [Portfolio](https://garry-tang-274.github.io)

## 它解决什么问题
## What It Solves

学校邮件、课程日历、正式课表和任务管理器经常描述的是同一个现实事项，但字段完整度、更新时间和可靠性不同。机械地把每条信息复制到日历，会产生重复、冲突和过期记录。

Institutional email, course calendars, formal timetables, and task managers often describe the same real-world item with different completeness, update times, and reliability. Blindly copying every message into a calendar creates duplicates, conflicts, and stale records.

协调器会先判断新信息属于同一事项、字段互补、明确更新、取消、疑似重复、真实冲突，还是不应自动加入的可选或纯信息内容。

The coordinator first determines whether incoming information is the same item, complementary evidence, a confirmed update, a cancellation, a possible duplicate, a genuine conflict, or optional and informational content that should not be auto-added.

## 工作流
## Workflow

```text
学校邮件 / School email ───────────────┐
Blackboard 日历 / Blackboard calendar ──┼─> 匹配、互补与冲突处理
正式课表 / Formal timetable ───────────┤    Matching, merging, conflict handling
已有规范项 / Existing normalized items ┘                 │
                                                          ├─> 日程 / Schedule
                                                          ├─> 截止日期 / Deadlines
                                                          ├─> 任务收集箱 / Task Inbox
                                                          └─> 人工确认 / User confirmation
```

完整数据流与判断逻辑见 [`docs/flowchart.md`](docs/flowchart.md)。

See [`docs/flowchart.md`](docs/flowchart.md) for the complete data flow and decision logic.

## 主要能力
## Main Capabilities

- 基于课程代码、名称、链接、时间、上下文和更新关系进行多来源去重。
- Multi-source deduplication based on course codes, names, links, time, context, and update relationships.
- 按字段互补，例如使用邮件补充 Blackboard 中缺失的教室。
- Field-level completion, such as using email to add a room missing from Blackboard.
- 正确处理延期、改期与取消，不遗留旧版本重复项。
- Update and cancellation chains without leaving stale duplicates.
- 区分必须出席的时间段事件与需要完成的硬性截止日期。
- Separation of compulsory time-block events from actionable hard deadlines.
- 对含糊、可选或相互冲突的信息保留人工确认。
- Human confirmation for ambiguous, optional, or conflicting information.

## 推荐的 Outlook 结构
## Recommended Outlook Layout

- `Schedule / 日程`：整理后的硬性时间段事件。
- `Schedule`: normalized compulsory time-block events.
- `Deadlines / 截止日期`：整理后的硬性截止事项。
- `Deadlines`: normalized hard deadlines.
- `Blackboard`：只读后台信息源，可从桌面小组件中隐藏。
- `Blackboard`: a read-only background source that may be hidden from widgets.
- 默认 `Calendar`：与学校自动化分离，不作为自动写入目标。
- Default `Calendar`: kept separate and not used as the school-automation target.

## 当前运行规则
## Current Operating Profile

当前实际使用时，只允许把新事项写入 `日程` 或 `截止日期`：需要占用一个明确时间段、要求到场或参加的事项进入 `日程`；必须在某个日期或时刻前完成的事项进入 `截止日期`。默认 Calendar 和 Blackboard 都不得作为写入目标。

In the current operating profile, new items may be written only to `Schedule` or `Deadlines`: items that occupy a definite time block or require attendance go to `Schedule`, while items that must be completed by a date or time go to `Deadlines`. The default Calendar and Blackboard are never write targets.

当用户直接提供一个事件、时间、地点等完整信息时，不需要为了“确认日历类别”再次询问；由协调器依据事项性质选择 `日程` 或 `截止日期`。只有事项本身的时间、对象或含义仍然有实质歧义时才需要确认。

When the user directly supplies an event with sufficient time and location details, the coordinator should choose `Schedule` or `Deadlines` from the item type rather than asking the user to select a calendar. Confirmation is required only when the event itself remains materially ambiguous.

## 最近补充：生产运行验收
## Recent Addition: Operational Validation

[`docs/operational-validation.md`](docs/operational-validation.md) 记录真实运行中最重要的验收规则：账号显示名不等于日历归属、技术标识变化不等于语义变化、尝试投递不等于确认送达，以及正式启用前必须使用匿名测试矩阵。

[`docs/operational-validation.md`](docs/operational-validation.md) records the most important production rules: account labels do not prove calendar ownership, technical identifier changes are not semantic changes, attempted delivery is not confirmed delivery, and anonymous validation scenarios are required before routine use.

日历写入必须从准确目标日历重新读取；邮件发送应有发件箱、服务商响应或收件端证据；任务创建应在目标任务收集箱确认。

Calendar writes must be read back from the exact destination calendar; sent mail should have Sent-folder, provider, or recipient-side evidence; task creation should be confirmed in the destination task inbox.

## 仓库内容
## Repository Contents

- [`docs/architecture.md`](docs/architecture.md)：架构与协调规则。
- [`docs/architecture.md`](docs/architecture.md): architecture and coordination rules.
- [`docs/design.md`](docs/design.md)：关键设计决策。
- [`docs/design.md`](docs/design.md): key design decisions.
- [`docs/setup.md`](docs/setup.md)：配置教程。
- [`docs/setup.md`](docs/setup.md): setup guide.
- [`docs/pitfalls.md`](docs/pitfalls.md)：踩坑记录与经验。
- [`docs/pitfalls.md`](docs/pitfalls.md): pitfalls and lessons learned.
- [`examples/scenarios.md`](examples/scenarios.md)：匿名验收场景。
- [`examples/scenarios.md`](examples/scenarios.md): anonymized validation scenarios.
- [`SECURITY.md`](SECURITY.md)：安全与隐私边界。
- [`SECURITY.md`](SECURITY.md): security and privacy boundaries.

## 重要限制
## Important Limitation

本仓库记录的是工作流设计与提示词规范，不是可独立部署的软件包。实际能力取决于自动化平台、连接器权限、学校策略与日历订阅延迟。

This repository documents a workflow design and prompt specification, not a self-hosted software package. Actual capabilities depend on the automation platform, connected-service permissions, institutional policies, and calendar-feed delay.

## 隐私
## Privacy

禁止公开真实 Blackboard 订阅链接、个人任务邮箱、Gmail 原文链接、Outlook Calendar ID、OAuth 数据、学号或私人课程信息。

Never publish live Blackboard feed URLs, personal task-email addresses, Gmail message links, Outlook Calendar IDs, OAuth data, student identifiers, or private course information.

## 许可
## License

本项目采用 MIT 许可证。`LICENSE` 中的英文文本为具有法律效力的正式版本，同时附有中文参考译文。

The project uses the MIT License. The English text in `LICENSE` is legally controlling, with a Chinese reference translation included for accessibility.
