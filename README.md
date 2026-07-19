# Student Calendar Coordinator / 学生日程协调器

中文 | English

一个无需编程的学校信息协调工作流，将学校邮件、Blackboard 日历、正式课表、Outlook Calendar 和待办系统整合。

A no-code workflow coordinating institutional email, Blackboard calendar feeds, timetables, Outlook Calendar and task lists.

## Core idea / 核心思想

不要把每条通知机械复制成新的日历事件，而是识别同一个现实事项在不同来源中的信息，进行去重、互补、更新、取消和人工确认。

Treat sources as evidence of real-world items, not as instructions to blindly create duplicates.

## Workflow

```
School Email ─┐
Blackboard ───┼──> Matching + Conflict Handling ──> Outlook Schedule
Timetable ────┤                              ├──> Outlook Deadlines
Existing Events┘                             └──> TickTick/Dida Inbox
```

## Features / 功能

- multi-source deduplication / 多来源去重
- complementary field merging / 信息互补
- postponement and cancellation handling / 延期取消处理
- hard event vs deadline separation / 日程与DDL分离
- human confirmation for uncertainty / 不确定事项人工确认

## Lessons learned / 踩坑记录

包含真实部署中发现的问题：

- Power Automate 受到学校管理员权限、多账号和稳定性限制；
- Gmail 地址作为 Microsoft 账号和 Google 账号时身份不同；
- Outlook 应用显示但桌面小组件可能不刷新；
- Blackboard 通知权限由学校管理员控制；
- Blackboard 订阅应作为信息源，而不是最终编辑日历；
- TickTick 邮件入口适合新增任务，但不适合可靠修改旧任务。

## Security / 安全

仓库不包含任何真实邮箱、Blackboard 私人订阅链接、日历ID、Token 或个人信息。

See `SECURITY.md`.

## Structure

```
docs/        architecture, setup, pitfalls
prompts/     reusable coordinator prompts
examples/    anonymized test cases
```
