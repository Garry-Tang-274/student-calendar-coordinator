# Pitfalls and Lessons Learned / 踩坑记录

## Power Automate

Power Automate was considered but introduced extra complexity:

- institutional Microsoft accounts may require administrator consent;
- multiple tenants/accounts create confusing authentication states;
- mobile and web troubleshooting cost was high;
- simple personal calendar coordination did not justify the additional failure points.

这不是说 Power Automate 不好，而是在学校租户限制较多时，它可能不是最低成本方案。

## Blackboard

Blackboard is treated as a read-only information source.

Do not assume:

- missing item = cancelled;
- every course event is published;
- every notification channel is available.

## Outlook widgets

An event visible inside Outlook may not immediately appear in a home-screen widget. Widgets may cache calendar selections.

## Account identity

A Gmail address can also be used to register a personal Microsoft account. Google identity and Microsoft identity are separate.

## TickTick/Dida

Email-created tasks usually enter Inbox. Email is reliable for creation, but not reliable for modifying or deleting existing tasks.

## Design principle

Normalize information first. Do not display every source calendar directly, otherwise Blackboard, timetable and final calendar create duplicate visual information.
