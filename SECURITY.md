# Security / 安全说明

## 1. Sensitive information / 敏感信息

Never publish the following values in a public repository, issue, pull request, screenshot or discussion.

禁止在公开仓库、Issue、Pull Request、截图或讨论中发布以下内容。

- live Blackboard external calendar URLs / 真实 Blackboard 外部日历订阅链接；
- personal TickTick/Dida task-email addresses / 个人滴答任务邮箱；
- Gmail message links, message IDs or private thread content / Gmail 原文链接、邮件 ID 或私人线程内容；
- Outlook calendar IDs and event IDs / Outlook Calendar ID 和事件 ID；
- OAuth tokens, refresh tokens, cookies or connector credentials / OAuth Token、Refresh Token、Cookie 或连接器凭据；
- institutional addresses that are not intentionally public / 非有意公开的学校邮箱地址；
- student IDs, phone numbers, home addresses or private course data / 学号、手机号、家庭地址或私人课程数据；
- screenshots containing names, course rosters, submission links or private URLs / 含姓名、课程名单、提交入口或私人链接的截图。

## 2. Why a task-email address is a secret / 为什么任务邮箱属于秘密

A personal task-email address may function like a write capability. Anyone who obtains it may be able to create tasks in the owner’s account.

个人任务邮箱可能相当于一种“写入权限”。任何获得该地址的人都可能向账号中创建任务。

Treat it like a private API endpoint rather than an ordinary contact address.

应把它视为私人 API 入口，而不是普通联系邮箱。

## 3. Safe placeholders / 安全占位符

Use placeholders in public examples.

公开示例中应使用占位符。

```text
student@example.com
todo+YOUR_PRIVATE_TOKEN@mail.dida365.com
https://blackboard.example.edu/calendar/REDACTED
<CALENDAR_ID>
<EVENT_ID>
<GMAIL_MESSAGE_LINK>
<OAUTH_TOKEN>
```

## 4. Screenshots and exported files / 截图与导出文件

Before publishing a screenshot or exported calendar file, inspect the entire image or file rather than only the visible title.

发布截图或导出的日历文件前，应检查完整图像或文件，而不仅仅看可见标题。

Private data may appear in browser address bars, hover cards, event descriptions, QR codes, hidden query parameters, metadata or file properties.

私人信息可能出现在浏览器地址栏、悬浮卡片、事件正文、二维码、隐藏查询参数、元数据或文件属性中。

Do not commit `.ics`, `.eml` or `.msg` files unless they have been deliberately anonymized and manually reviewed.

除非经过主动脱敏和人工审查，否则不要提交 `.ics`、`.eml` 或 `.msg` 文件。

## 5. If a secret was published / 如果秘密已经公开

Deleting the current file is not enough because the value may remain in Git history, forks, caches or notifications.

只删除当前文件并不够，因为该值可能仍留在 Git 历史、Fork、缓存或通知中。

Recommended response:

建议处理步骤：

1. Remove the value from the current repository state.  
   从当前仓库版本中移除该值。
2. Rotate, regenerate or revoke the affected secret or feed URL.  
   轮换、重新生成或撤销受影响的秘密或订阅链接。
3. Review unexpected calendar events, tasks and account access.  
   检查异常日历事件、任务和账号访问记录。
4. Rewrite Git history when necessary.  
   必要时重写 Git 历史。
5. Assume copied values remain exposed even after repository cleanup.  
   即使清理仓库，也应假定已被复制的值仍然暴露。

## 6. Reporting a vulnerability / 报告安全问题

When reporting a security problem, describe the category and impact without posting a live credential or personal feed URL.

报告安全问题时，应说明问题类别和影响，但不要发布真实凭据或私人订阅链接。

Use redacted examples and rotate any secret that was accidentally included in a report.

请使用脱敏示例；如果报告中误包含秘密，应立即轮换该秘密。
