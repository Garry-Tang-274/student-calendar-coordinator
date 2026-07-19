# Setup Guide / 配置教程

This guide describes a generic deployment. Menu names and available permissions may differ by institution, Blackboard version, Outlook client and task manager.

本教程描述的是通用部署方式。不同学校、Blackboard 版本、Outlook 客户端和任务管理器的菜单名称与权限可能不同。

## 1. Prerequisites / 前置条件

You need:

你需要：

- a personal Microsoft account with Outlook Calendar access / 一个可使用 Outlook Calendar 的个人 Microsoft 账号；
- access to the institution’s Blackboard calendar export / 能访问学校 Blackboard 日历导出功能；
- an institution-approved email-forwarding route or direct mailbox access / 学校允许的邮件转发方式或直接邮箱访问权限；
- a task manager that can create tasks through a private email address, such as TickTick/Dida / 支持通过私人邮箱创建任务的任务管理器，例如滴答清单；
- an automation or AI platform that can read the selected sources and write to the normalized calendars / 能读取所选信息源并写入规范日历的自动化或 AI 平台。

## 2. Create the Outlook calendar structure / 创建 Outlook 日历结构

In the personal Microsoft account, create two writable calendars:

在个人 Microsoft 账号中创建两个可写日历：

```text
Schedule / 日程
Deadlines / 截止日期
```

Keep the default `Calendar`, but do not use it as the school automation target.

保留默认 `Calendar`，但不要把它作为学校事项的自动写入目标。

Recommended roles:

推荐用途：

| Calendar / 日历 | Purpose / 用途 | Default display / 默认显示状态 |
|---|---|---|
| Schedule / 日程 | Compulsory time-block events / 必须出席的时间段事件 | Busy / 忙碌 |
| Deadlines / 截止日期 | Hard completion or submission deadlines / 硬性完成或提交 DDL | Free / 空闲 |
| Blackboard | Read-only source and cross-checking layer / 只读信息源与核对层 | Optional on widget / 小组件中可隐藏 |
| Default Calendar | Personal non-school use / 个人非学校事项 | User choice / 用户自行决定 |

## 3. Subscribe to the Blackboard calendar / 订阅 Blackboard 日历

In Blackboard Original, open the calendar and locate:

在 Blackboard Original 中打开日历，并寻找：

```text
Get External Calendar Link
```

Copy the private subscription URL. In Outlook on the web, choose:

复制私人订阅链接。在 Outlook 网页版中选择：

```text
Add calendar → Subscribe from web
添加日历 → 从 Web 订阅
```

Name the calendar `Blackboard`.

把该日历命名为 `Blackboard`。

Important differences:

重要区别：

- `Subscribe from web` keeps a live subscription that may refresh later.  
  “从 Web 订阅”会保留可后续刷新的订阅关系。
- Uploading an `.ics` file creates only a one-time snapshot.  
  上传 `.ics` 文件只会导入一次性快照。
- The private URL must not be published.  
  私人订阅链接不得公开。
- The subscription is normally read-only.  
  该订阅通常是只读的。

## 4. Configure Blackboard notifications / 配置 Blackboard 通知

Notification channels vary by institution.

Blackboard 通知渠道因学校而异。

If the bulk-notification page shows only `Dashboard` and `Mobile`, with no `Email` column, the email channel is not available to the student.

如果批量通知页面只有 `Dashboard` 和 `Mobile`、没有 `Email` 列，说明学生无法使用该邮件渠道。

Keep important dashboard notifications enabled, including assignment availability, assignment due dates, tests, course messages and other hard-deadline categories.

建议保留重要 Dashboard 通知，例如作业发布、作业 DDL、测试、课程消息和其他硬性截止类别。

A due-date reminder seven days in advance is useful when the goal is to place tasks into a planning system early rather than merely issue a last-minute warning.

如果目标是让任务尽早进入规划系统，而不仅仅在最后时刻提醒，那么提前七天的 DDL 提醒是合理的。

## 5. Configure institutional email access / 配置学校邮件访问

Preferred options, in order:

推荐方式，按优先顺序排列：

1. direct read access through an approved connector / 使用学校允许的连接器直接读取；
2. institution-approved forwarding to a personal Gmail account / 学校允许的邮件转发到个人 Gmail；
3. manual forwarding for selected messages / 对特定邮件手动转发。

When using forwarding, preserve original sender, subject, timestamp and thread context whenever possible.

使用转发时，应尽量保留原始发件人、主题、时间戳和线程上下文。

The automation search should cover all mail locations, including spam, but spam messages require stronger verification before any calendar write.

自动化搜索应覆盖所有邮件位置，包括垃圾邮件；但垃圾邮件在写入日历前必须经过更严格的可信度核验。

## 6. Configure the task manager / 配置任务管理器

Obtain the account’s private task-email address from the task manager’s official settings.

从任务管理器官方设置中获取账号的私人任务邮箱。

Use a placeholder in public documentation:

公开文档中使用占位符：

```text
todo+YOUR_PRIVATE_TOKEN@mail.dida365.com
```

Treat this address as a secret because it may allow anyone who knows it to create tasks.

该地址可能允许任何知情者创建任务，因此必须把它当作秘密。

Accepting the default Inbox is usually more reliable than depending on undocumented email-subject syntax to select a custom list.

接受默认进入收集箱，通常比依赖未稳定公开的邮件标题语法指定普通清单更可靠。

## 7. Configure the automation schedule / 配置自动化执行频率

A practical schedule is:

实用的执行安排是：

- hourly source coordination / 每小时协调一次信息源；
- silence when nothing changes outside the digest time / 非汇总时段无变化保持静默；
- one complete daily digest / 每天一次完整汇总；
- Friday coordination of the following week’s timetable / 每周五协调下一周课表。

The platform should report tool failures honestly rather than silently skipping a source.

平台在工具失败时必须如实报告，不能静默跳过信息源。

## 8. Supply a valid timetable / 提供有效课表

A timetable should include:

课表应包括：

- validity start and end dates / 有效期开始和结束日期；
- weekday / 星期；
- course code and course name / 课程代码和课程名称；
- start and end time / 开始和结束时间；
- location / 地点；
- class type when relevant / 适用时注明课程类型。

Do not create an indefinite recurring series from a timetable with no validity period.

不得把没有有效期的课表创建成无限循环日程。

## 9. Configure home-screen widgets / 配置桌面小组件

Recommended visible calendars:

建议桌面显示：

```text
Schedule / 日程
Deadlines / 截止日期
```

Recommended hidden background source:

建议后台隐藏：

```text
Blackboard
```

Hiding Blackboard changes only presentation. Deleting or unsubscribing removes the information source.

隐藏 Blackboard 只影响显示；删除或取消订阅会移除信息源。

If a new calendar appears inside Outlook but not in the widget, remove and re-add the widget, then reselect the calendars.

如果新日历在 Outlook 应用内可见、但小组件中不可见，可删除并重新添加小组件，再重新勾选日历。

## 10. Acceptance testing / 验收测试

Before relying on the workflow, test at least the following cases:

正式使用前，至少测试以下情况：

1. Blackboard-only new deadline / 只有 Blackboard 的新 DDL；
2. the same item in Blackboard and email / Blackboard 与邮件出现同一事项；
3. complementary location or submission information / 地点或提交入口互补；
4. explicit postponement / 明确延期；
5. explicit cancellation / 明确取消；
6. optional seminar / 自愿讲座；
7. ambiguous time or time zone / 时间或时区不明确；
8. trustworthy forwarded message in spam / 垃圾邮件中的可信学校转发；
9. Blackboard item temporarily disappearing / Blackboard 条目暂时消失；
10. one message containing both an event and a deadline / 一封邮件同时包含活动与 DDL。

Expected results for these cases are documented in [examples/scenarios.md](../examples/scenarios.md).

这些情况的预期结果见 [examples/scenarios.md](../examples/scenarios.md)。
