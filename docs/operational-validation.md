# Operational Validation / 生产运行验收

This document records validation rules learned from operating the workflow with real mail, calendars and task delivery. A workflow is not considered successful merely because an assistant reports that an action was attempted.

本文记录这套流程在真实邮件、日历和任务投递中形成的验收规则。不能仅因为助手声称“已经执行”，就把工作流视为成功。

## 1. Account labels are not calendar ownership / 账号显示名不等于日历归属

Several email addresses or aliases may point to the same Microsoft account, while Outlook may also display Google mailboxes and institutional identities in the same client.

多个邮箱地址或别名可能指向同一个 Microsoft 账号，Outlook 客户端还可能同时显示 Google 邮箱与学校身份。

Validation must identify the account that actually owns the target calendars, not infer ownership from the visible mailbox address or the address used to sign in.

验收时必须确认哪个账号真正拥有目标日历，不能根据客户端显示的邮箱地址或登录地址推断归属。

Before enabling writes, create a harmless test event, read it back from the intended calendar, and verify that it appears under the expected account on both web and mobile clients.

启用正式写入前，应创建一个无害测试日程，从目标日历重新读取，并确认它在网页端和移动端都出现在预期账号下。

## 2. Route events and deadlines strictly / 严格区分日程与截止日期

Compulsory time-block activities belong in `Schedule / 日程`; hard submission deadlines belong in `Deadlines / 截止日期`.

必须占用时间段的硬性活动写入 `Schedule / 日程`；需要提交或完成的硬性截止事项写入 `Deadlines / 截止日期`。

A deadline may also create a task reminder, but the calendar record remains authoritative for its date and time. A task inbox is not a replacement for the normalized deadline calendar.

截止日期可以同时创建任务提醒，但其日期与时间仍以规范化日历记录为准。任务收集箱不能替代规范化的截止日期日历。

Optional events, FYI notices and ambiguous invitations should not be written automatically unless the user has supplied an explicit policy.

可选活动、纯信息通知和含糊邀请不应自动写入，除非用户已经提供明确规则。

## 3. Technical identifier changes are not semantic changes / 技术标识变化不等于内容变化

Calendar feeds and learning platforms may regenerate internal IDs, links, sequence numbers or synchronization metadata without changing the event meaning.

日历订阅和教学平台可能重新生成内部 ID、链接、序列号或同步元数据，但事项含义并未变化。

Do not notify the user when only a technical identifier changes. Compare title, course, date, time, location, action requirement, status and explicit update language before classifying an item as changed.

只有技术标识变化时，不要提醒用户。应比较标题、课程、日期、时间、地点、行动要求、状态和明确的变更措辞，再判断事项是否发生变化。

A notification should be produced only when the semantic state changes, such as a new deadline, changed time, changed room, cancellation, restored event or altered required action.

只有语义状态变化时才应提醒，例如新增截止日期、时间变化、教室变化、取消、恢复或行动要求改变。

## 4. Attempted delivery is not confirmed delivery / 尝试投递不等于确认送达

For outbound email, task creation and calendar writes, retain evidence from the destination or provider whenever possible.

对外发邮件、任务创建和日历写入，应尽可能保留目标端或服务提供方的证据。

A successful mail action should be supported by a Sent-folder record, provider response or recipient-side receipt. If no outbound record exists, report the action as failed or unverified rather than completed.

邮件操作成功应有发件箱记录、服务提供方响应或收件端回执作为依据。如果没有外发记录，应报告为失败或待验证，而不是已完成。

A successful calendar write should be read back from the exact destination calendar. A successful task delivery should be confirmed in the task inbox or by the absence of a bounce combined with recipient-side appearance.

日历写入成功后，应从准确的目标日历重新读取。任务投递成功应在任务收集箱确认，或同时满足没有退信且收件端已经出现。

## 5. Use a validation matrix before trusting automation / 在信任自动化前使用验收矩阵

Test at least the following classes with anonymized synthetic messages: compulsory event, hard deadline, optional event, update, cancellation, duplicate, conflicting sources, FYI message and irrelevant mail.

至少使用匿名合成消息测试以下类别：硬性活动、硬性截止日期、可选活动、修改、取消、重复、来源冲突、纯信息通知和无关邮件。

For each test, record expected classification, expected destination, expected write action, whether confirmation is required and the observed result.

每个测试都应记录预期分类、预期目标、预期写入动作、是否需要确认和实际结果。

A workflow is ready for routine use only when classification and destination routing are correct, duplicates are not created, cancellations remove or update stale state, and every claimed write can be verified.

只有分类与目标路由正确、不生成重复项、取消能够移除或更新旧状态，并且每个声称完成的写入都可验证时，工作流才适合日常运行。

## 6. Preserve a human override path / 保留人工接管路径

When evidence is conflicting, stale or incomplete, pause writes and ask for confirmation instead of selecting a source silently.

证据冲突、过期或不完整时，应暂停写入并请求确认，而不是静默选择某个来源。

The user must be able to correct the authoritative event, suppress a noisy source, change routing rules and mark a specific exception without rebuilding the entire workflow.

用户必须能够修改权威事项、屏蔽高噪声来源、调整路由规则和标记单项例外，而不必重建整个工作流。
