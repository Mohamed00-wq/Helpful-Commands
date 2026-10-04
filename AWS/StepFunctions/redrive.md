# Redrive

> Redrive. Part of the [StepFunctions](../) cheatsheet.

Redrive restarts a Standard workflow execution that failed, aborted, or timed
out, continuing from the state that failed with the original input and the
original execution ARN, and it does not rerun the states that already
succeeded. An execution is redrivable for 14 days after the state machine
finished it, its event history has to stay under 24,999 events, and Express
workflows cannot be redriven at all.

## To find the executions that can be redriven

The following example lists the failed executions that have not been redriven
yet, which is the queue you work through.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --status-filter FAILED \
    --redrive-filter NOT_REDRIVEN
```

## To redrive a failed execution

The following example redrives one failed execution, and the call returns the
redrive date rather than a new ARN because the redriven run reuses the original
execution.

```bash
aws stepfunctions redrive-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name>
```

## To check whether a redrive is possible

The following example returns the redrive state alongside the execution status,
and `redriveStatus` reads `REDRIVABLE`, `NOT_REDRIVABLE`, or
`REDRIVABLE_BY_MAP_RUN` where a failed Map Run is the way back in.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query '[status,redriveCount,redriveStatus]' --output text
```

## To list the executions waiting to be redriven

The following example lists the executions sitting in the `PENDING_REDRIVE`
state, which is where a failed execution waits for a redrive.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --status-filter PENDING_REDRIVE
```
