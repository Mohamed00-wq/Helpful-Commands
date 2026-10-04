# Session Manager

> Session Manager. Part of the [SystemsManager](../) cheatsheet.

## To start a session

The following example starts a session.

```bash
aws ssm start-session --target i-xxx
```

## To terminate a session

The following example terminates a session.

```bash
aws ssm terminate-session --session-id xxx
```

## To list active sessions

The following example lists active sessions.

```bash
aws ssm describe-sessions --state Active
```
