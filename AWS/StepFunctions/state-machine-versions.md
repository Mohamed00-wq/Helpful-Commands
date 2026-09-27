# State Machine Versions

> State Machine Versions. Part of the [StepFunctions](../StepFunctions.md) cheatsheet.

A version pins the definition at the moment you published it, which is what
makes a rollback a one line change later. Starting an execution against the
unqualified ARN runs whatever the latest revision happens to be.

## To publish a version

The following example publishes the current definition as an immutable numbered
version.

```bash
aws stepfunctions publish-state-machine-version \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --description <version-description>
```

## To list the versions of a state machine

The following example lists the published versions with their creation dates.

```bash
aws stepfunctions list-state-machine-versions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

## To delete a version

The following example deletes a version. The call fails while an alias still
references the version, so delete the alias or move it first. Deleting a version
does not stop the executions already running against it.

```bash
aws stepfunctions delete-state-machine-version \
    --state-machine-version-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<version-number>
```
