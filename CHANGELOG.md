# Changelog / 变更记录

All notable changes to this repository are documented here.

本文件记录仓库中的重要变化。

## 1.0.0 — 2026-07-20

### Added / 新增

- Published the initial public, anonymized and bilingual workflow design.  
  发布首个公开、脱敏、中英双语的工作流设计版本。
- Added multi-source coordination across institutional email, Blackboard calendar feeds, formal timetables, Outlook Calendar and TickTick/Dida.  
  增加学校邮件、Blackboard 日历订阅、正式课表、Outlook Calendar 与滴答清单之间的多来源协调。
- Added item-identity matching beyond exact-title comparison.  
  增加超越标题完全匹配的同一事项识别规则。
- Added field-level completion for time, location, requirements, links and deadlines.  
  增加时间、地点、要求、链接和 DDL 的字段级互补规则。
- Added explicit handling for postponement, rescheduling, room changes, cancellation and temporary feed disappearance.  
  增加延期、改期、换教室、取消和订阅条目暂时消失的处理规则。
- Added human confirmation for probable duplicates, optional activities and unresolved trustworthy conflicts.  
  增加对疑似重复、可选活动和无法解决的可信来源冲突的人工确认机制。
- Added separate normalized calendars for compulsory events and hard deadlines.  
  增加硬性时间段事件与硬性 DDL 分离的规范日历设计。
- Added a reusable bilingual coordinator prompt template.  
  增加可复用的中英双语协调提示词模板。
- Added bilingual architecture, setup, design, pitfalls, security and validation documentation.  
  增加中英双语的架构、配置、设计、踩坑、安全和验收文档。

### Deployment evolution / 部署演化

The original concept considered a Power Automate-centered implementation.

最初方案考虑以 Power Automate 为核心。

The practical deployment then evolved through the following stages:

实际部署随后经历了以下演化：

```text
Power Automate-centered design / Power Automate中心方案
        ↓
Institutional permission and account limitations / 学校权限与多账号限制
        ↓
Institutional email forwarding / 学校邮件转发
+ Blackboard external calendar subscription / Blackboard外部日历订阅
        ↓
AI-based coordination and human confirmation / AI协调与人工确认
        ↓
Normalized Outlook calendars / Outlook规范日历
+ TickTick/Dida task inbox / 滴答任务收集箱
```

### Security / 安全

- Removed all real email addresses, private task-email addresses, Blackboard feed URLs, calendar IDs, message links and test identifiers.  
  移除了所有真实邮箱、私人任务邮箱、Blackboard 订阅链接、Calendar ID、邮件原文链接和测试标识。
- Added placeholders and secret-rotation guidance.  
  增加占位符和秘密轮换指导。

## Future directions / 后续方向

Potential future work includes:

潜在后续工作包括：

- visual flow diagrams / 可视化流程图；
- institution-specific adapters while preserving generic core rules / 在保留通用核心规则的前提下增加学校特定适配；
- structured test matrices for more Blackboard versions / 为更多 Blackboard 版本建立结构化测试矩阵；
- optional code implementations that follow the same documented semantics / 基于同一文档语义的可选代码实现。
