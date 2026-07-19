# Design Decisions / 设计决策

## 1. Why this is a coordination system rather than a forwarding system / 为什么这是协调系统，而不是转发系统

A forwarding system treats every incoming record as an independent instruction.

转发系统会把每条新记录都视为独立指令。

A coordination system treats several records as possibly describing one underlying real-world item.

协调系统则认为，多条记录可能描述同一个现实事项。

This distinction is essential because schools often publish the same class, assignment or deadline through a timetable, Blackboard and email, each with different fields.

这一差异非常重要，因为学校经常通过课表、Blackboard 和邮件重复发布同一课程、作业或 DDL，而且每个来源提供的字段不同。

## 2. Why Blackboard remains read-only / 为什么 Blackboard 保持只读

The subscribed Blackboard calendar is an externally controlled source. Editing or recreating items inside it would blur the boundary between source evidence and personal decisions.

订阅的 Blackboard 日历由外部系统控制。如果直接在其中编辑或重建事项，会模糊“原始证据”和“个人整理结果”的边界。

Therefore, Blackboard is treated as immutable evidence, while normalized events are written into separate personal calendars.

因此，本项目把 Blackboard 视为不可修改的证据，并把规范化结果写入独立的个人日历。

This separation makes auditing easier: the user can compare the original source with the editable result.

这种分离便于审计：用户可以对照原始来源与可编辑结果。

## 3. Why Schedule and Deadlines are separate / 为什么“日程”和“截止日期”分开

A compulsory class or exam occupies time and should block availability.

必须参加的课程或考试会占用时间，因此应阻塞可用时间。

A deadline is usually a point by which work must be completed; it should remain visible without making the whole day appear unavailable.

DDL 通常只是必须在某个时点前完成任务，不应让整天看起来都不可用。

The separation allows different defaults:

这种分离允许使用不同默认设置：

| Property / 属性 | Schedule / 日程 | Deadlines / 截止日期 |
|---|---|---|
| Calendar status / 日历状态 | Busy / 忙碌 | Free / 空闲 |
| Typical reminder / 典型提醒 | 15 minutes / 提前15分钟 | 1 day / 提前1天 |
| Main meaning / 主要含义 | Attend at this time / 此时必须出席 | Complete by this time / 在此之前完成 |

## 4. Why uncertainty stops automatic writing / 为什么不确定性会阻止自动写入

A false duplicate may be inconvenient, but an incorrect overwrite can hide the correct exam time or deadline.

错误重复虽然不方便，但错误覆盖可能隐藏正确考试时间或 DDL。

For this reason, the system uses asymmetric risk handling:

因此，本系统采用非对称风险处理：

- strong evidence allows automatic action / 强证据允许自动操作；
- moderate uncertainty blocks destructive changes / 中等不确定性阻止破坏性修改；
- explicit contradiction requires user confirmation / 明确矛盾要求用户确认。

The workflow favors preserving a safe current state over making a confident-looking guess.

流程优先保留安全的当前状态，而不是做出看似自信的猜测。

## 5. Why authority is evaluated per field / 为什么按字段判断权威性

A single source may be authoritative for one field but weak for another.

同一个来源可能对某个字段权威，但对另一个字段并不可靠。

For example:

例如：

- Blackboard may be authoritative for the configured due date.  
  Blackboard 可能对正式设置的截止时间最权威。
- The timetable may be authoritative for the normal weekly class time.  
  正式课表可能对常规每周上课时间最权威。
- A recent email may be authoritative for a temporary room change.  
  较新的邮件可能对临时换教室最权威。

A fixed global ranking would lose this nuance.

固定的全局来源排名会丢失这种细粒度判断。

## 6. Why the task manager is not the source of truth / 为什么任务管理器不是权威数据源

Task-email interfaces are convenient for creating reminders but weak for later mutation.

任务邮箱适合快速创建提醒，但不擅长后续修改。

The normalized Outlook entry is therefore treated as the final editable state, while the task manager is an execution aid.

因此，本项目把 Outlook 规范条目视为最终可编辑状态，把任务管理器视为执行辅助工具。

When an old task cannot be edited remotely, a new change or cancellation task makes the inconsistency visible rather than pretending the old task was removed.

当旧任务无法远程修改时，新增变更或取消提醒可以显式暴露差异，而不是假装旧任务已经被删除。

## 7. Why the workflow remains no-code / 为什么流程保持无代码

The design goal is reproducibility with consumer tools and explicit rules, not maximum engineering complexity.

设计目标是使用消费级工具和明确规则实现可复现流程，而不是追求最高工程复杂度。

A no-code approach lowers deployment barriers for students who cannot install servers, request institutional API access or maintain always-on infrastructure.

无代码方案降低了部署门槛，适合无法安装服务器、无法申请学校 API 权限或无法维护常驻基础设施的学生。

The repository still documents system-design concepts such as source normalization, identity resolution, conflict handling, state transitions and auditability.

即使没有代码，本仓库仍然包含信息源规范化、实体识别、冲突处理、状态转换和可审计性等系统设计思想。

## 8. Known trade-offs / 已知权衡

### Refresh delay / 刷新延迟

Calendar subscriptions may not update immediately.

日历订阅可能不会立即刷新。

Email may reveal an urgent change before the Blackboard feed does.

紧急变化可能先出现在邮件中，之后才同步到 Blackboard 日历。

### Limited task mutation / 任务修改能力有限

Email-based task creation does not provide robust update or deletion semantics.

基于邮件创建任务，通常不具备可靠的修改或删除语义。

### Dependence on source quality / 依赖源数据质量

If an instructor never configures a due date in Blackboard and sends no email, the workflow cannot infer the missing item safely.

如果教师既没有在 Blackboard 中设置 DDL，也没有发送邮件，流程无法安全推断缺失事项。

### Human confirmation cost / 人工确认成本

Conservative conflict handling requires occasional user input.

保守的冲突处理会偶尔要求用户确认。

This is intentional because the workflow prioritizes correctness over silent automation.

这是有意的设计，因为流程优先保证正确性，而不是追求完全静默的自动化。
