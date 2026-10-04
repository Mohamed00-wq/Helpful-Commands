# State Machines

> State Machines. Part of the [StepFunctions](../) cheatsheet.

## To create a Standard state machine

The following example creates a Standard workflow from an Amazon States
Language definition and an execution role.

```bash
aws stepfunctions create-state-machine \
    --name <state-machine-name> \
    --definition file://state-machine.json \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --type STANDARD
```

## To create an Express state machine

The following example creates an Express workflow, which is the type to use for
high event rates and short lived work.

```bash
aws stepfunctions create-state-machine \
    --name <state-machine-name> \
    --definition file://state-machine.json \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --type EXPRESS
```

## To send execution logs to CloudWatch Logs

The following example turns on logging at the `ALL` level, and every member name
in this shorthand is lower case, which trips people up the first time. Express
workflows have no history in the service, so this configuration is the only way
to see what they did.

```bash
aws stepfunctions create-state-machine \
    --name <state-machine-name> \
    --definition file://state-machine.json \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --logging-configuration 'level=ALL,includeExecutionData=true,destinations=[{cloudWatchLogsLogGroup={logGroupArn=arn:aws:logs:<region>:<account-id>:log-group:/aws/vendedlogs/states/<state-machine-name>}}]'
```

## To list state machines

The following example lists every state machine in the Region, and the list
holds the unqualified ARNs only, never a version or an alias.

```bash
aws stepfunctions list-state-machines
```

## To describe a state machine

The following example returns the definition, the role, the type, and the
logging configuration of a state machine.

```bash
aws stepfunctions describe-state-machine \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

## To update a state machine

The following example replaces the definition and publishes the change as a new
version in the same call.

```bash
aws stepfunctions update-state-machine \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --definition file://state-machine.json \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --publish \
    --version-description <version-description>
```

## To delete a state machine

The following example deletes a state machine, and Step Functions refuses the
call while any execution is still running. The API takes no dry run flag, so
check `list-executions` first.

```bash
aws stepfunctions delete-state-machine \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```
