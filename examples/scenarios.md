# Validation Scenarios / 验收测试场景

All examples are anonymized. Names, codes, links and dates are fictional.

所有示例均已匿名化，课程代码、名称、链接和日期都是虚构的。

## Scenario 1: Blackboard-only deadline / 场景一：只有 Blackboard 的 DDL

### Input / 输入

```text
Blackboard:
BIO1001 Assignment 2
Due: 2026-09-18 23:59
Submission: https://blackboard.example.edu/REDACTED
```

No matching email, timetable item or normalized deadline exists.

不存在匹配的邮件、课表事项或规范 DDL。

### Expected result / 预期结果

- Create one normalized deadline: `Deadline｜BIO1001｜Assignment 2`.  
  创建一条规范 DDL：`截止｜BIO1001｜Assignment 2`。
- Store the due time and submission link.  
  保存截止时间和提交入口。
- Send one task-manager item.  
  向任务管理器发送一条任务。
- Do not require a second source merely because email is absent.  
  不能因为没有邮件就要求必须存在第二来源。

## Scenario 2: Timetable, Blackboard and email describe one class / 场景二：课表、Blackboard 和邮件描述同一节课

### Input / 输入

```text
Timetable:
BIO1001 Laboratory, Tuesday 14:00–16:00, Lab D303

Blackboard:
BIO1001 Practical Session, Tuesday 14:00–16:00

Email:
This Tuesday's BIO1001 practical has moved to Lab D305.
Bring a laboratory notebook.
```

### Expected result / 预期结果

- Recognize one underlying class rather than three events.  
  识别为同一节课，而不是创建三条事件。
- Use the timetable for the regular time.  
  使用课表提供的常规时间。
- Use the email for the occurrence-specific room and requirement.  
  使用邮件提供的本次教室和携带要求。
- Create or update one normalized event: `BIO1001｜Laboratory`.  
  创建或更新一条规范事件：`BIO1001｜Laboratory`。
- Record all contributing sources.  
  记录所有贡献信息的来源。

## Scenario 3: Explicit deadline postponement / 场景三：明确延期

### Input / 输入

```text
Blackboard:
Report due Friday 17:00

Later official email:
The report deadline has been extended to Monday 12:00.
This message supersedes the previous deadline.
```

### Expected result / 预期结果

- Match the email to the same report.  
  把邮件匹配到同一份报告。
- Replace Friday 17:00 with Monday 12:00 in the normalized Outlook deadline.  
  在 Outlook 规范 DDL 中把周五17:00更新为周一12:00。
- Do not create a second deadline.  
  不创建第二条 DDL。
- If both versions are first discovered in one run, send only the final task.  
  如果首次运行同时发现两个版本，只发送最终任务。
- If the old task was sent in an earlier run, send a separate change reminder.  
  如果旧任务已在更早运行中发送，则新增一条变更提醒。

## Scenario 4: Cancellation before first processing / 场景四：首次处理前已经取消

### Input / 输入

The first coordination run sees both an event announcement and a later official cancellation.

第一次协调运行同时看到活动发布通知和后续官方取消通知。

### Expected result / 预期结果

- Create no Outlook event.  
  不创建 Outlook 事件。
- Send no task-manager item.  
  不发送任务管理器任务。
- Report that the item was withdrawn before first processing.  
  报告该事项在首次处理前已经撤回。

## Scenario 5: Trustworthy sources conflict without a clear update relation / 场景五：可信来源冲突，但没有明确更新关系

### Input / 输入

```text
Blackboard:
Exam room A101

Official email sent later the same day:
Exam room A103
```

The email does not say that it corrects or replaces the Blackboard entry.

邮件没有说明它是在更正或替代 Blackboard 条目。

### Expected result / 预期结果

- Do not silently overwrite A101.  
  不静默覆盖 A101。
- Do not create a duplicate exam event.  
  不创建重复考试事件。
- Preserve the current safe state and ask the user.  
  保留当前安全状态并询问用户。
- Show both source values and timestamps.  
  展示双方来源值和发布时间。

## Scenario 6: Optional seminar with a registration deadline / 场景六：带报名截止的自愿讲座

### Input / 输入

```text
Attendance is optional.
Seminar: Introduction to Spatial Omics
Event: 2026-10-12 15:00
Registration deadline: 2026-10-10
```

### Expected result / 预期结果

- Do not automatically create the event or deadline.  
  不自动创建活动或 DDL。
- Explain the topic, value, time, location and registration deadline.  
  说明主题、价值、时间、地点和报名截止。
- Ask whether the user wants it added.  
  询问用户是否需要加入。

## Scenario 7: One email contains an event and a separate deadline / 场景七：一封邮件同时包含活动和独立 DDL

### Input / 输入

```text
Mandatory safety briefing: 2026-08-03 10:00–11:00
Signed declaration form due: 2026-08-01
```

### Expected result / 预期结果

- Create one Schedule item for the briefing.  
  为说明会创建一条“日程”。
- Create one Deadline item for the declaration form.  
  为承诺书创建一条“截止日期”。
- Cross-reference the two records.  
  在两条记录中互相注明关联。
- Send only the deadline to the task manager.  
  只把 DDL 发送到任务管理器。

## Scenario 8: Credible forwarded message is in spam / 场景八：可信学校转发进入垃圾邮件

### Input / 输入

A forwarded institutional message appears in Gmail Spam. The forwarding headers, original sender, course code and thread context are consistent with known school messages.

一封学校转发邮件出现在 Gmail 垃圾邮件中，但转发头、原始发件人、课程代码和线程上下文与已知学校邮件一致。

### Expected result / 预期结果

- Do not ignore it merely because it is in Spam.  
  不能仅因为它在垃圾邮件中就忽略。
- Verify the forwarding chain and context before acting.  
  操作前核验转发链和上下文。
- Process it if verification succeeds.  
  核验通过后正常处理。
- Report risk and avoid automatic writing if verification fails.  
  核验失败时报告风险，不自动写入。

## Scenario 9: Blackboard item disappears for one synchronization pass / 场景九：Blackboard 条目在一次同步中消失

### Input / 输入

A previously observed deadline is absent in one hourly Blackboard read. No cancellation email exists.

此前看到的 DDL 在一次小时级 Blackboard 读取中缺失，但没有取消邮件。

### Expected result / 预期结果

- Do not delete the normalized deadline.  
  不删除规范 DDL。
- Recheck in later runs.  
  在后续运行中重新检查。
- Delete only after explicit cancellation or repeated disappearance with additional official evidence.  
  只有明确取消，或连续消失并有其他官方证据时才删除。

## Scenario 10: Date is known but exact time is absent / 场景十：只有日期，没有精确时刻

### Input / 输入

```text
The application must be submitted on 2026-11-05.
```

No exact time is stated.

没有提供精确时间。

### Expected result / 预期结果

- Create a date-level deadline marker.  
  创建日期级 DDL 标记。
- State that the exact time was not supplied.  
  注明来源没有提供精确时刻。
- Do not guess 23:59.  
  不得猜测23:59。
- Use a date-only task subject.  
  使用仅日期的任务标题。

## Scenario 11: Temporary class change must not alter future weeks / 场景十一：临时调课不能影响后续周次

### Input / 输入

```text
Regular timetable:
CHEM1001, Monday 09:00, Room B201

Official email:
The class on 2026-09-14 will move to Tuesday 10:00, Room B305.
```

### Expected result / 预期结果

- Modify only the occurrence on 2026-09-14.  
  只修改2026-09-14对应的那一次课程。
- Keep later weeks on the regular Monday baseline.  
  后续周次继续使用周一的常规课表基线。

## Scenario 12: New source is missing a field already confirmed / 场景十二：新来源缺少已经确认的字段

### Input / 输入

An existing normalized event has a confirmed room. A later Blackboard refresh contains the same event but leaves the location blank.

已有规范事件中存在已确认教室，而后续 Blackboard 刷新中的同一事件地点为空。

### Expected result / 预期结果

- Keep the confirmed room.  
  保留已经确认的教室。
- Do not treat the blank value as an instruction to erase it.  
  不把空白值视为删除教室的指令。
- Update only fields that contain meaningful new information.  
  只更新包含有效新信息的字段。
