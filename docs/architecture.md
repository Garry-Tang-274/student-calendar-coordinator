# Architecture / 系统架构

## 1. Purpose / 目标

This workflow does not copy every source item into a personal calendar. It builds a normalized view of the same underlying school activity from multiple sources.

本流程不会把每个来源中的条目机械复制到个人日历，而是把多个来源对同一现实事项的描述协调成一条规范记录。

## 2. Sources / 信息源

| Source | Typical strengths | Typical limitations |
|---|---|---|
| School email / 学校邮件 | corrections, cancellations, rooms, detailed requirements / 更正、取消、地点、详细要求 | wording is inconsistent; may arrive in spam / 格式不统一，可能进垃圾邮件 |
| Blackboard calendar feed | configured deadlines, tests, submission items / 正式 DDL、测试、提交项 | read-only, delayed, incomplete / 只读、可能延迟、并非所有信息都进入日历 |
| Formal timetable / 正式课表 | regular weekly baseline / 常规周课表基线 | cannot represent every temporary change / 无法覆盖所有临时变化 |
| Existing normalized calendars | previous decisions and merged fields / 已有判断与互补信息 | may become stale if updates are missed / 漏处理更新时可能过期 |

## 3. Normalized outputs / 规范化输出

- `Schedule / 日程`: compulsory time-block events.
- `Deadlines / 截止日期`: hard deadlines.
- TickTick/Dida Inbox / 滴答收集箱: actionable deadline tasks.
- Confirmation queue / 待确认列表: optional, ambiguous or conflicting items.
- Daily summary / 日汇总: FYI items and coordination results.

The Blackboard calendar remains a read-only source. It may be hidden from a home-screen widget without being removed from the workflow.

Blackboard 日历只作为只读信息源。桌面小组件可以隐藏它，但不能删除订阅，否则流程会失去该信息源。

## 4. Item identity / 同一事项识别

Exact title matching is insufficient. The coordinator should combine:

- course code, course name and aliases;
- assignment, test, lab or event core terms;
- email thread, submission link and source link;
- date/time proximity;
- instructor, organizer, location and item type;
- explicit relations such as corrected, postponed, supersedes, rescheduled, withdrawn or cancelled.

不能只按标题完全一致去重。应综合课程代码、名称缩写、事项核心词、邮件线程、提交链接、日期时间、教师、地点、事项类型，以及“更正、延期、替代前通知、取消”等关系词。

### Definite match / 明确同一事项

Strong evidence exists. Automatic merge, update or deletion is allowed.

存在强证据时，可自动合并、更新或删除。

### Probable match / 高度疑似

The course and date are close, but a key difference cannot be explained. Do not create a duplicate and do not overwrite. Ask the user.

课程和日期接近，但关键差异无法解释时，不新增重复项，也不覆盖原项，必须询问。

### Definite non-match / 明确不同

Only clearly distinct items should coexist.

只有明确不同的事项才分别保留。

## 5. Field-level merging / 字段级互补

For one underlying item, preserve one normalized entry and merge non-conflicting fields:

- timetable: regular time and base room;
- Blackboard: official item name, due date and submission link;
- email: temporary room, detailed instructions, file format, online link, postponement or cancellation.

A blank field in a new source must not erase a confirmed field already stored.

同一事项只保留一条规范记录。新来源为空白时，不能抹掉旧记录中已经确认的信息。

## 6. Authority and conflicts / 权威与冲突

Authority is field-specific, not a fixed global ranking.

优先级应按字段和上下文判断，而不是简单规定“邮件永远最高”或“Blackboard 永远最高”。

- A recent explicit official correction controls the field it changes.
- The timetable is the normal weekly baseline.
- A one-week change must not permanently rewrite later weeks.
- Blackboard is strong evidence for configured due dates and submission links.
- An unresolved conflict between trustworthy sources requires confirmation.
- A Blackboard item disappearing once is not enough evidence of cancellation.

## 7. Change chains / 变更链

| Observed chain | Expected action |
|---|---|
| create → postpone, first seen together | create only the final version / 只创建最终版 |
| create → cancel, first seen together | create nothing / 不创建 |
| previously created → postpone | update Outlook; send a change reminder to task manager / 更新 Outlook，并发送变更提醒 |
| previously created → cancel | delete normalized event; send cancellation reminder / 删除规范条目，并发送取消提醒 |

## 8. Standard titles / 统一标题

```text
Course event: COURSE_CODE｜Item name
Deadline: 截止｜COURSE_CODE_OR_CATEGORY｜Item name
```

Examples:

```text
BIO1001｜Laboratory class
截止｜BIO1001｜Submit pre-lab form
截止｜Administration｜Submit scholarship documents
```

## 9. Safety principle / 安全原则

When uncertain, preserve the current safe state and ask. A duplicate is annoying; silently overwriting the correct time or deleting a valid deadline is worse.

不确定时，应保留当前安全状态并询问。重复虽然烦人，但错误覆盖正确时间或删除有效 DDL 的风险更高。
