# Pitfalls and Lessons Learned / 踩坑记录与经验

These observations came from one real deployment. Other institutions, Blackboard versions and mobile clients may behave differently.

这些问题来自一次真实部署。不同学校、Blackboard 版本和手机客户端的表现可能不同。

## 1. Power Automate was not the shortest path / Power Automate 并不是最短路径

Power Automate was considered first because it appears suitable for linking email, calendars and tasks.

最初考虑 Power Automate，是因为它看起来很适合连接邮件、日历和任务。

In practice, the deployment encountered several problems:

实际配置中遇到了以下问题：

- institutional Microsoft accounts could require administrator consent for third-party access;  
  学校 Microsoft 账号可能要求管理员批准第三方访问权限；
- multiple personal and institutional tenants produced confusing authentication states;  
  多个个人账号和学校租户容易造成混乱的登录与授权状态；
- the web interface could remain blank or fail to load correctly;  
  网页端可能出现白屏或长时间无法正确加载；
- mobile configuration and troubleshooting required disproportionate effort;  
  手机端配置和排错成本过高；
- a simple personal scheduling workflow gained more failure points than useful capability.  
  对于个人日程协调，新增的故障点超过了实际收益。

This does not mean that Power Automate is a bad product. It means that when institutional OAuth consent is unavailable and the goal is a personal workflow, it may not be the lowest-cost solution.

这并不意味着 Power Automate 本身不好，而是当学校不开放 OAuth 授权、目标又只是个人工作流时，它可能不是成本最低的方案。

The simpler route used here was:

本项目最终采用了更直接的路线：

```text
Institutional forwarding / 学校邮件转发
+ Blackboard iCal subscription / Blackboard iCal订阅
+ AI coordination / AI协调
+ Outlook normalized calendars / Outlook规范日历
+ TickTick/Dida email input / 滴答邮件入口
```

## 2. The same Gmail address may represent two identities / 同一个 Gmail 地址可能代表两种身份

A Gmail address can be both a Google account and the login name of a separately registered personal Microsoft account.

一个 Gmail 地址既可能是 Google 账号，也可能是单独注册的个人 Microsoft 账号登录名。

These identities do not share calendars or permissions. Outlook may show the Google mailbox while the desired calendars belong to the Microsoft identity.

这两个身份不会共享日历或权限。Outlook 可能显示的是 Google 邮箱，而真正需要的日历属于 Microsoft 身份。

The practical lesson is to verify which identity owns the target calendars and, when useful, add an Outlook.com alias to the Microsoft account.

实际经验是：必须确认目标日历到底属于哪个身份；必要时可以给个人 Microsoft 账号增加 Outlook.com 别名，减少混淆。

## 3. Outlook app visibility does not guarantee widget visibility / Outlook 应用可见不代表桌面小组件可见

A newly created calendar may appear inside Outlook but remain absent from an existing home-screen widget.

新建日历可能已经出现在 Outlook 应用中，但旧的桌面小组件仍然不显示。

Widgets may cache account and calendar selections. Re-adding the widget, reselecting calendars, refreshing the account or restarting the client may be required.

小组件可能缓存账号和日历选择。可能需要删除并重新添加小组件、重新勾选日历、刷新账号或重启客户端。

## 4. Blackboard Original may expose no Email column / Blackboard Original 可能没有 Email 通知列

Notification channels are controlled by the institution. Some deployments expose only `Dashboard` and `Mobile`, with no `Email` column.

通知渠道由学校管理员控制。有些 Blackboard 部署只显示 `Dashboard` 和 `Mobile`，没有 `Email` 列。

A student cannot enable a channel that the administrator has disabled. Therefore, Blackboard email notifications should not be assumed to exist.

学生无法自行开启管理员禁用的渠道。因此，不能把 Blackboard 邮件通知视为必然存在的主链路。

The external calendar subscription becomes especially important in this situation.

在这种情况下，Blackboard 外部日历订阅尤其重要。

## 5. Blackboard calendar feeds are read-only and may be delayed / Blackboard 日历订阅只读且可能延迟

An Outlook subscription to Blackboard is normally read-only.

Outlook 中订阅的 Blackboard 日历通常是只读的。

It may refresh slowly, omit announcements that were never added to the course calendar, and temporarily lose an item during synchronization.

它可能刷新较慢，也可能遗漏没有被教师正式加入课程日历的公告，还可能在同步过程中暂时缺失某个条目。

For this reason:

因此：

- do not try to edit the Blackboard calendar;  
  不要尝试编辑 Blackboard 日历；
- do not interpret one disappearance as a confirmed cancellation;  
  不要把一次消失直接视为确认取消；
- use it as evidence and a cross-checking layer, not the final editable calendar.  
  应把它作为证据和核对层，而不是最终可编辑日历。

## 6. Hiding Blackboard from the widget is different from deleting it / 在桌面隐藏 Blackboard 与删除订阅不同

Hiding the Blackboard calendar from a widget only changes presentation. The coordinator can still read it.

在小组件中隐藏 Blackboard 只影响显示，协调器仍然可以读取它。

Deleting or unsubscribing removes the source entirely and prevents future coordination.

删除或取消订阅会彻底移除信息源，使后续协调无法读取它。

The recommended arrangement is:

推荐的显示方式是：

```text
Visible on the home screen / 桌面显示:
- Schedule / 日程
- Deadlines / 截止日期

Hidden background source / 后台隐藏信息源:
- Blackboard
```

## 7. TickTick/Dida email-created tasks normally enter Inbox / 滴答邮件创建的任务通常进入收集箱

Routing an email-created task directly into an arbitrary list was not reliable in this deployment.

在本次部署中，通过邮件语法把任务直接稳定送入任意普通清单并不可靠。

Accepting the default Inbox reduced fragile syntax and made the workflow easier to audit.

接受默认进入收集箱，可以减少脆弱语法，也更便于统一检查。

## 8. Email is suitable for creating tasks, but weak for editing old tasks / 邮件适合创建任务，但不适合修改旧任务

A private task-email address is useful for creating a new task.

私人任务邮箱适合创建新任务。

However, email usually cannot reliably locate, edit or delete an already-created task.

但邮件通常无法可靠定位、修改或删除已经创建的旧任务。

The adopted compromise is:

本项目采用的折中方案是：

- update Outlook to the final authoritative state;  
  把 Outlook 更新为最终权威状态；
- send a new `Change` task when a deadline moves;  
  DDL 变化时新增一条“变更”任务；
- send a new `Cancel old task` reminder when a deadline is withdrawn;  
  DDL 取消时新增一条“取消旧任务”提醒；
- let the user remove or complete the stale task manually.  
  由用户手动处理旧任务。

## 9. Gmail forwarding may place valid school messages in spam / Gmail 转发可能把有效学校邮件放入垃圾邮件

Searching only the Inbox can miss valid forwarded messages.

只搜索收件箱可能漏掉有效的学校转发邮件。

The search scope should cover all mail locations, but spam messages must receive additional trust checks based on forwarding headers, sender, thread context and course information.

搜索范围应覆盖全部邮件位置，但垃圾邮件必须根据转发链、发件人、线程上下文和课程信息进行额外可信度核验。

`Search everywhere` must not mean `trust everything`.

“搜索全部位置”不能等于“信任所有内容”。

## 10. A timetable needs an explicit validity period / 课表必须有明确有效期

A timetable without a semester or date range should not be converted into a permanent recurring calendar series.

没有学期或日期范围的课表，不应被转换成永久循环日历。

Temporary room changes, holiday adjustments and make-up classes must apply only to the specified dates unless the notice explicitly states a long-term change.

临时换教室、节假日调课和补课只能作用于指定日期，除非通知明确说明是长期变化。

## 11. Source priority cannot be a simple fixed ranking / 来源优先级不能写成简单固定排名

Rules such as `email always wins` or `Blackboard always wins` are unsafe.

“邮件永远最高”或“Blackboard 永远最高”都不安全。

The decision should depend on which field is being changed, whether the notice is explicit, whether it is newer, and whether it clearly refers to the same item.

判断应取决于被修改的具体字段、通知是否明确、是否较新，以及是否清楚指向同一事项。

When those questions cannot be answered, the workflow should ask instead of guessing.

当这些问题无法回答时，流程应询问用户，而不是猜测。
