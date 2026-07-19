# Contributing / 贡献指南

Issues and pull requests are welcome.

欢迎提交 Issue 和 Pull Request。

## 1. Language requirement / 语言要求

Every documentation file in this repository must remain fully bilingual.

本仓库中的每个文档文件都必须保持完整中英双语。

Bilingual does not mean adding a one-sentence translation or an English summary. Titles, explanations, rules, examples, warnings and expected results should have matching content in both languages.

“双语”不等于只增加一句翻译或英文摘要。标题、解释、规则、示例、警告和预期结果都应在两种语言中保持对应内容。

Accepted structures include:

可接受的结构包括：

- an English paragraph followed by its Chinese counterpart / 英文段落后紧跟对应中文段落；
- a bilingual table where each cell contains both languages / 每个单元格同时包含中英文的双语表格；
- a complete English section followed by a structurally equivalent complete Chinese section / 完整英文部分后跟结构对等的完整中文部分。

## 2. Privacy requirement / 隐私要求

Do not submit live credentials or personal source data.

禁止提交真实凭据或个人源数据。

Before opening a contribution, remove or replace:

提交贡献前，请移除或替换：

- private Blackboard feed URLs / 私人 Blackboard 订阅链接；
- personal task-email addresses / 个人任务邮箱；
- Gmail message links or message IDs / Gmail 原文链接或邮件 ID；
- Outlook calendar or event IDs / Outlook Calendar ID 或事件 ID；
- OAuth data, cookies or access tokens / OAuth 数据、Cookie 或访问令牌；
- names, student IDs, phone numbers and private course details / 姓名、学号、手机号和私人课程信息。

Use the placeholders documented in [SECURITY.md](SECURITY.md).

请使用 [SECURITY.md](SECURITY.md) 中规定的占位符。

## 3. Report deployment context / 说明部署环境

When reporting behavior, include relevant context without exposing secrets.

报告系统行为时，请提供相关环境信息，但不得泄露秘密。

Useful context includes:

有用的环境信息包括：

- Blackboard Original or Ultra / Blackboard Original 或 Ultra；
- web, Android or iOS client / 网页端、Android 或 iOS 客户端；
- whether the account is personal Microsoft, institutional Microsoft or Google / 账号属于个人 Microsoft、学校 Microsoft 还是 Google；
- whether the behavior was observed once or reproduced repeatedly / 该行为是偶发一次还是可以重复复现；
- whether the source was direct, forwarded or in spam / 信息源是直接邮件、转发邮件还是垃圾邮件。

## 4. Distinguish observation from universal fact / 区分单次观察与通用事实

Do not present one institution’s configuration as universal product behavior.

不要把某一学校的配置当成所有用户都适用的产品行为。

Use wording such as:

建议使用以下表述：

- `Observed in one Blackboard Original deployment` / `在某次 Blackboard Original 部署中观察到`；
- `May depend on institutional administrator settings` / `可能取决于学校管理员设置`；
- `Not reproduced across all clients` / `尚未在所有客户端复现`。

## 5. Adding a new rule / 新增规则

A proposed coordination rule should include:

新增协调规则时，应包括：

1. the problem it solves / 它解决的问题；
2. the evidence used for matching / 用于匹配的证据；
3. the safe behavior under uncertainty / 不确定时的安全行为；
4. at least one anonymized example / 至少一个匿名示例；
5. the expected effect on Schedule, Deadlines and task-manager output / 对“日程”“截止日期”和任务管理器输出的预期影响。

## 6. Validation / 验证

When changing workflow semantics, update [examples/scenarios.md](examples/scenarios.md) with a matching bilingual test scenario.

修改工作流语义时，请在 [examples/scenarios.md](examples/scenarios.md) 中增加对应的中英双语测试场景。

A change should not weaken the following safety properties:

修改不得削弱以下安全属性：

- no blind duplicate creation / 不机械创建重复项；
- no destructive overwrite under unresolved conflict / 冲突未解决时不进行破坏性覆盖；
- no secret publication / 不公开秘密；
- no invented date or time / 不猜测日期或时间；
- no false success report after tool failure / 工具失败后不谎称成功。
