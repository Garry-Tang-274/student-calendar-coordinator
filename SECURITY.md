# Security / 安全说明

Never publish:

- Blackboard external calendar URLs;
- personal task email addresses;
- Gmail message links;
- Outlook calendar IDs;
- OAuth tokens or cookies;
- student identifiers or private course data.

公开仓库只能使用占位符，例如：

```
student@example.com
todo+YOUR_TOKEN@mail.dida365.com
https://blackboard.example.edu/calendar/REDACTED
```

A task-email address can act like a write permission. Treat it as a secret.
