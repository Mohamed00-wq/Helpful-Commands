# Aliases

> Aliases. Part of the [StepFunctions](../) cheatsheet.

An alias is a stable name that points at one version, and moving the alias is
how a deployment happens without changing the ARN anything calls. An alias can
also split traffic across two versions by weight, which is how a canary runs.

## To create an alias pointing at a version

The following example creates an alias that routes all executions to one
version.

```bash
aws stepfunctions create-state-machine-alias \
    --name <alias-name> \
    --description <alias-description> \
    --routing-configuration '[{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<version-number>","weight":100}]'
```

## To point an alias at a new version

The following example moves the alias to another version, and executions that
were already running stay on the version they started on.

```bash
aws stepfunctions update-state-machine-alias \
    --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name> \
    --routing-configuration '[{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<version-number>","weight":100}]'
```

## To split an alias across two versions

The following example sends 90 percent of executions to the new version and the
rest to the old one. A split routing configuration always takes exactly two
versions.

```bash
aws stepfunctions update-state-machine-alias \
    --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name> \
    --routing-configuration '[{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<new-version-number>","weight":90},{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<old-version-number>","weight":10}]'
```

## To list the aliases of a state machine

The following example lists the aliases and the version each one currently
points at.

```bash
aws stepfunctions list-state-machine-aliases \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

## To delete an alias

The following example deletes an alias and leaves the versions it referenced in
place.

```bash
aws stepfunctions delete-state-machine-alias \
    --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name>
```
