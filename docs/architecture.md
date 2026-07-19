# Architecture / 系统架构

## 1. Purpose / 目标

This workflow builds one normalized personal view from several school information sources. It does not treat every source item as a separate event.

本流程把多个学校信息源协调成一套规范化的个人日程视图，而不是把每个来源条目都当成独立事件。

Its main objective is to keep the final calendar complete, non-duplicated, editable and responsive to postponements, room changes and cancellations.

核心目标是让最终日历信息完整、没有重复、可以编辑，并能正确响应延期、换教室和取消。

## 2. Source layer / 信息源层

| Source / 来源 | Typical strengths / 典型优势 | Typical limitations / 典型限制 |
|---|---|---|
| School email / 学校邮件 | Explicit corrections, cancellations, rooms, detailed requirements and temporary changes / 明确更正、取消、教室、详细要求和临时变化 | Inconsistent wording; may be forwarded, delayed or placed in spam / 格式不统一，可能经转发、延迟或进入垃圾邮件 |
| Blackboard calendar feed / Blackboard 日历订阅 | Formally configured deadlines, tests, assignment names and some submission items / 正式设置的 DDL、测试、作业名称和部分提交项 | Read-only, may refresh slowly, and does not contain every announcement / 只读、可能刷新较慢，并非所有通知都会进入日历 |
| Formal timetable / 正式课表 | Reliable regular weekly baseline for classes / 可靠的常规每周课程基线 | Cannot represent every temporary exception / 无法表达所有临时例外 |
| Existing normalized calendars / 已有规范日历 | Preserve previous decisions and merged fields / 保存之前的判断和互补字段 | Can become stale if a later update is missed / 如果漏掉后续更新，可能变成旧信息 |
| Task-manager sent-mail history / 任务管理器已发邮件记录 | Helps prevent duplicate task creation / 帮助避免重复创建任务 | Usually cannot directly edit or delete an old task / 通常无法直接修改或删除旧任务 |

## 3. Coordination layer / 协调层

Before any write operation, the coordinator must compare all available sources and answer five questions.

在执行任何写入操作前，协调器必须比较所有可用来源，并回答五个问题。

1. Is this a new underlying item?  
   这是一个新的现实事项吗？
2. Is it the same item as an existing event or deadline?  
   它是否与已有日程或 DDL 属于同一事项？
3. Does it add missing information without conflict?  
   它是否提供了可以无冲突补充的信息？
4. Does it explicitly modify or cancel an older version?  
   它是否明确修改或取消了旧版本？
5. Is the evidence insufficient or contradictory?  
   证据是否不足或互相矛盾？

## 4. Output layer / 输出层

- `Schedule / 日程`: compulsory events occupying a time interval.  
  `日程`：占用一个时间段、必须出席的事项。
- `Deadlines / 截止日期`: hard deadlines requiring completion or submission.  
  `截止日期`：要求完成或提交的硬性 DDL。
- `TickTick/Dida Inbox / 滴答收集箱`: actionable deadline tasks.  
  `滴答收集箱`：可执行、可勾选的 DDL 待办。
- `Confirmation queue / 待确认列表`: ambiguous, optional or conflicting items.  
  `待确认列表`：含糊、可选或互相冲突的事项。
- `Daily digest / 日汇总`: FYI information and coordination audit results.  
  `日汇总`：纯信息内容和协调审计结果。

The Blackboard calendar remains a read-only source. It may be hidden from a home-screen widget while still being read by the coordinator.

Blackboard 日历始终作为只读信息源。即使桌面小组件不显示它，协调器仍然可以读取其内容。

## 5. Identity matching / 同一事项识别

Exact title matching is not sufficient. The coordinator should combine the following signals.

不能只比较标题是否完全一致。协调器应综合以下线索。

- course code, course name and common abbreviations / 课程代码、课程名称和常见缩写；
- core terms of an assignment, test, lab, class or event / 作业、测试、实验、课程或活动的核心词；
- email thread, Blackboard item, submission link and source link / 邮件线程、Blackboard 项目、提交链接和来源链接；
- date, start/end time, deadline and temporal proximity / 日期、开始结束时间、截止时间和时间接近程度；
- instructor, organizer, location and item type / 教师、组织者、地点和事项类型；
- explicit relationship words such as corrected, postponed, supersedes, rescheduled, withdrawn or cancelled / “更正、延期、替代前通知、改期、撤回、取消”等明确关系词。

### 5.1 Definite match / 明确同一事项

Strong evidence exists, such as the same course code and assignment name, the same submission link, or an explicit statement that a message replaces an earlier notice.

存在强证据，例如课程代码与作业名称相同、提交链接相同，或新通知明确声明替代旧通知。

Automatic merge, update or deletion is allowed.

可以自动合并、更新或删除。

### 5.2 Probable match / 高度疑似同一事项

The course and date are close, but an important difference in time, location or title cannot be explained.

课程和日期接近，但时间、地点或名称存在无法解释的重要差异。

Do not create a second item and do not overwrite the current item. Ask the user.

不得创建第二份，也不得覆盖当前条目，必须询问用户。

### 5.3 Definite non-match / 明确不同事项

The course, item type or time logic clearly indicates separate underlying activities.

课程、事项类型或时间逻辑明确表明它们是不同的现实事项。

Only then should both items coexist.

只有在这种情况下，两个条目才应同时保留。

## 6. Field-level completion / 字段级互补

For one underlying item, keep one normalized record and merge non-conflicting fields.

对于同一现实事项，只保留一条规范记录，并合并不冲突的字段。

- The timetable may provide the regular time and base room.  
  课表可以提供常规时间和基础教室。
- Blackboard may provide the official item name, deadline and submission link.  
  Blackboard 可以提供正式事项名称、截止时间和提交入口。
- Email may provide a temporary room, detailed instructions, file format, online link, postponement or cancellation.  
  邮件可以提供临时教室、详细要求、文件格式、线上链接、延期或取消。

A blank field in a newer source must not erase a confirmed field already stored.

较新来源中的空白字段不能删除旧记录中已经确认的信息。

The normalized body should record which sources contributed which facts and when the item was last checked.

规范条目的正文应记录哪些来源贡献了哪些信息，以及最后核验时间。

## 7. Authority and conflict resolution / 权威判断与冲突解决

Authority is field-specific and context-sensitive. It is not a permanent global ranking.

权威性取决于具体字段和上下文，而不是永久固定的全局排名。

- A recent explicit official correction controls the field it changes.  
  较新的明确官方更正，对其修改的字段具有优先权。
- The timetable is the baseline for regular weekly classes.  
  正式课表是常规每周课程的基线。
- A temporary change for one week must not permanently rewrite later weeks.  
  针对某一周的临时变化不能永久改写后续周次。
- Blackboard is strong evidence for formally configured deadlines and submission links.  
  Blackboard 对正式设置的 DDL 和提交入口具有较强可信度。
- Email may supplement Blackboard or override stale data when it explicitly announces a change.  
  邮件可以补充 Blackboard，或在明确宣布变更时覆盖旧信息。
- An unresolved conflict between trustworthy sources requires confirmation.  
  两个可信来源发生无法解决的冲突时，必须人工确认。
- One temporary Blackboard disappearance is not sufficient evidence of cancellation.  
  Blackboard 条目一次暂时消失，不足以证明事项已经取消。

## 8. Change chains / 变更链

| Observed chain / 观察到的变更链 | Expected action / 预期操作 |
|---|---|
| Create → postpone, first seen in the same run / 首次运行同时看到新增→延期 | Create only the final version / 只创建最终版本 |
| Create → cancel, first seen in the same run / 首次运行同时看到新增→取消 | Create nothing / 不创建任何条目 |
| Previously created → postpone / 已经创建后发生延期 | Update the normalized Outlook item and send a task-change reminder / 更新 Outlook 规范条目，并发送任务变更提醒 |
| Previously created → cancel / 已经创建后发生取消 | Delete the normalized Outlook item and send a cancellation reminder / 删除 Outlook 规范条目，并发送取消提醒 |
| Possible duplicate with unexplained differences / 疑似重复但差异无法解释 | Preserve the safe state and ask / 保留当前安全状态并询问 |

## 9. Standard titles and fields / 统一标题与字段

### Schedule title / 日程标题

```text
COURSE_CODE｜Item name
课程代码｜事项名称
```

### Deadline title / DDL 标题

```text
Deadline｜COURSE_CODE_OR_CATEGORY｜Item name
截止｜课程代码或类别｜事项名称
```

### Suggested body fields / 建议正文字段

- Status / 状态
- Course or category / 课程或类别
- Time / 时间
- Location or platform / 地点或平台
- Requirements / 要求
- Related event or deadline / 关联活动或 DDL
- Sources / 来源
- Original or submission links / 原文或提交链接
- Last verification time / 最后核验时间

Unknown fields should be omitted or marked as pending confirmation. They must not be guessed for cosmetic completeness.

未知字段应省略或标记为待确认，不能为了格式完整而猜测。

## 10. Safety principle / 安全原则

When uncertain, preserve the current safe state and ask. A duplicate is inconvenient, but silently overwriting the correct time or deleting a valid deadline is more harmful.

不确定时，应保留当前安全状态并询问。重复虽然不方便，但静默覆盖正确时间或删除有效 DDL 的危害更大。
