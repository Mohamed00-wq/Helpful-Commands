# Executions

> Executions. Part of the [StepFunctions](../) cheatsheet.

The `--input` value is JSON text, and the workflow sees it as its top level
input, so the keys of that object are what the first state's `Parameters` block
resolves against.

## To start an execution with inline JSON

The following example starts an execution and passes the input inline.

```bash
aws stepfunctions start-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input '{"orderId":"1234","region":"eu-west-1"}'
```

## To start an execution with input from a file

The following example reads the input from a file, which keeps large payloads
out of your shell history.

```bash
aws stepfunctions start-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input file://input.json
```

`input.json`:

```json
{
  "orderId": "1234",
  "region": "eu-west-1"
}
```

## To start an execution with a name you choose

The following example names the execution, and on a Standard workflow the same
name with the same input is idempotent, so a retried request returns the
original execution instead of starting a second one.

```bash
aws stepfunctions start-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --name <execution-name> \
    --input '{"orderId":"1234"}'
```

## To run an Express workflow and read its output

The following example runs an Express workflow to completion and returns the
output in the same call, so there is no execution to poll afterwards.

```bash
aws stepfunctions start-sync-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input '{"orderId":"1234"}' \
    --query 'output' --output text | jq -r '.result'
```

## To read the status of an Express execution

The following example prints the status the synchronous call returned, which is
`SUCCEEDED`, `FAILED`, or `TIMED_OUT` once the five minute limit is reached.

```bash
aws stepfunctions start-sync-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input '{"orderId":"1234"}' \
    --query 'status' --output text
```

## To list the executions of a state machine

The following example lists the executions of a state machine, newest first.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

## To list failed executions

The following example lists only the executions that failed, which is the
shortcut to the failures worth investigating.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --status-filter FAILED
```

## To page through executions

The following example caps each page at 50 executions, and the CLI follows the
token until every execution is returned.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --max-items 50
```

## To check the status of an execution

The following example returns the status of one execution, which is the call to
repeat until it stops saying `RUNNING`.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query 'status' --output text
```

## To read the output of an execution

The following example prints the output as text rather than as a JSON string
nested inside JSON, and the value is still JSON so `jq` reads the fields
directly.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query 'output' --output text | jq -r '.result'
```

## To read the error of a failed execution

The following example returns the error and cause a failed execution recorded,
which is more useful than the event history for a first look.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query '[error,cause]' --output text
```

## To read the event history of an execution

The following example lists each state the execution entered, in order, and
omitting `--include-execution-data` keeps the response small on a long run.

```bash
aws stepfunctions get-execution-history \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query 'events[].[id,type,previousEventId]' --output table
```

## To stop a running execution

The following example stops a running Standard execution, and the call fails on
an Express execution because those always run to completion or to the five
minute limit.

```bash
aws stepfunctions stop-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --error PaymentDeclined \
    --cause "customer declined the charge"
```
