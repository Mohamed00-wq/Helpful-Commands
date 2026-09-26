# Step Functions (AWS Step Functions)

> Commands for state machines, executions, versions, aliases, and redrive.

The service command is `aws stepfunctions`; the `aws sfn` name in older
documentation is not accepted by AWS CLI v2. The workflow type decides what you
can do afterwards: a Standard workflow runs exactly-once for up to a year, keeps
its execution history in the service, and can be stopped and redriven, while an
Express workflow runs at-least-once for up to five minutes, streams its history
to CloudWatch Logs instead, and can be neither stopped nor redriven.

## State Machines

### To create a Standard state machine

The following example creates a Standard workflow from an Amazon States
Language definition and an execution role.

```bash
aws stepfunctions create-state-machine \
    --name <state-machine-name> \
    --definition file://state-machine.json \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --type STANDARD
```

### To create an Express state machine

The following example creates an Express workflow, which is the type to use for
high event rates and short lived work.

```bash
aws stepfunctions create-state-machine \
    --name <state-machine-name> \
    --definition file://state-machine.json \
    --role-arn arn:aws:iam::<account-id>:role/<role> \
    --type EXPRESS
```

### To send execution logs to CloudWatch Logs

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

### To list state machines

The following example lists every state machine in the Region, and the list
holds the unqualified ARNs only, never a version or an alias.

```bash
aws stepfunctions list-state-machines
```

### To describe a state machine

The following example returns the definition, the role, the type, and the
logging configuration of a state machine.

```bash
aws stepfunctions describe-state-machine \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

### To update a state machine

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

### To delete a state machine

The following example deletes a state machine, and Step Functions refuses the
call while any execution is still running. The API takes no dry run flag, so
check `list-executions` first.

```bash
aws stepfunctions delete-state-machine \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

## Executions

The `--input` value is JSON text, and the workflow sees it as its top level
input, so the keys of that object are what the first state's `Parameters` block
resolves against.

### To start an execution with inline JSON

The following example starts an execution and passes the input inline.

```bash
aws stepfunctions start-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input '{"orderId":"1234","region":"eu-west-1"}'
```

### To start an execution with input from a file

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

### To start an execution with a name you choose

The following example names the execution, and on a Standard workflow the same
name with the same input is idempotent, so a retried request returns the
original execution instead of starting a second one.

```bash
aws stepfunctions start-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --name <execution-name> \
    --input '{"orderId":"1234"}'
```

### To run an Express workflow and read its output

The following example runs an Express workflow to completion and returns the
output in the same call, so there is no execution to poll afterwards.

```bash
aws stepfunctions start-sync-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input '{"orderId":"1234"}' \
    --query 'output' --output text | jq -r '.result'
```

### To read the status of an Express execution

The following example prints the status the synchronous call returned, which is
`SUCCEEDED`, `FAILED`, or `TIMED_OUT` once the five minute limit is reached.

```bash
aws stepfunctions start-sync-execution \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --input '{"orderId":"1234"}' \
    --query 'status' --output text
```

### To list the executions of a state machine

The following example lists the executions of a state machine, newest first.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

### To list failed executions

The following example lists only the executions that failed, which is the
shortcut to the failures worth investigating.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --status-filter FAILED
```

### To page through executions

The following example caps each page at 50 executions, and the CLI follows the
token until every execution is returned.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --max-items 50
```

### To check the status of an execution

The following example returns the status of one execution, which is the call to
repeat until it stops saying `RUNNING`.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query 'status' --output text
```

### To read the output of an execution

The following example prints the output as text rather than as a JSON string
nested inside JSON, and the value is still JSON so `jq` reads the fields
directly.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query 'output' --output text | jq -r '.result'
```

### To read the error of a failed execution

The following example returns the error and cause a failed execution recorded,
which is more useful than the event history for a first look.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query '[error,cause]' --output text
```

### To read the event history of an execution

The following example lists each state the execution entered, in order, and
omitting `--include-execution-data` keeps the response small on a long run.

```bash
aws stepfunctions get-execution-history \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query 'events[].[id,type,previousEventId]' --output table
```

### To stop a running execution

The following example stops a running Standard execution, and the call fails on
an Express execution because those always run to completion or to the five
minute limit.

```bash
aws stepfunctions stop-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --error PaymentDeclined \
    --cause "customer declined the charge"
```

## State Machine Versions

A version pins the definition at the moment you published it, which is what
makes a rollback a one line change later. Starting an execution against the
unqualified ARN runs whatever the latest revision happens to be.

### To publish a version

The following example publishes the current definition as an immutable numbered
version.

```bash
aws stepfunctions publish-state-machine-version \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --description <version-description>
```

### To list the versions of a state machine

The following example lists the published versions with their creation dates.

```bash
aws stepfunctions list-state-machine-versions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

### To delete a version

The following example deletes a version. The call fails while an alias still
references the version, so delete the alias or move it first. Deleting a version
does not stop the executions already running against it.

```bash
aws stepfunctions delete-state-machine-version \
    --state-machine-version-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<version-number>
```

## Aliases

An alias is a stable name that points at one version, and moving the alias is
how a deployment happens without changing the ARN anything calls. An alias can
also split traffic across two versions by weight, which is how a canary runs.

### To create an alias pointing at a version

The following example creates an alias that routes all executions to one
version.

```bash
aws stepfunctions create-state-machine-alias \
    --name <alias-name> \
    --description <alias-description> \
    --routing-configuration '[{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<version-number>","weight":100}]'
```

### To point an alias at a new version

The following example moves the alias to another version, and executions that
were already running stay on the version they started on.

```bash
aws stepfunctions update-state-machine-alias \
    --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name> \
    --routing-configuration '[{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<version-number>","weight":100}]'
```

### To split an alias across two versions

The following example sends 90 percent of executions to the new version and the
rest to the old one. A split routing configuration always takes exactly two
versions.

```bash
aws stepfunctions update-state-machine-alias \
    --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name> \
    --routing-configuration '[{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<new-version-number>","weight":90},{"stateMachineVersionArn":"arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<old-version-number>","weight":10}]'
```

### To list the aliases of a state machine

The following example lists the aliases and the version each one currently
points at.

```bash
aws stepfunctions list-state-machine-aliases \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>
```

### To delete an alias

The following example deletes an alias and leaves the versions it referenced in
place.

```bash
aws stepfunctions delete-state-machine-alias \
    --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name>
```

## Redrive

Redrive restarts a Standard workflow execution that failed, aborted, or timed
out, continuing from the state that failed with the original input and the
original execution ARN, and it does not rerun the states that already
succeeded. An execution is redrivable for 14 days after the state machine
finished it, its event history has to stay under 24,999 events, and Express
workflows cannot be redriven at all.

### To find the executions that can be redriven

The following example lists the failed executions that have not been redriven
yet, which is the queue you work through.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --status-filter FAILED \
    --redrive-filter NOT_REDRIVEN
```

### To redrive a failed execution

The following example redrives one failed execution, and the call returns the
redrive date rather than a new ARN because the redriven run reuses the original
execution.

```bash
aws stepfunctions redrive-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name>
```

### To check whether a redrive is possible

The following example returns the redrive state alongside the execution status,
and `redriveStatus` reads `REDRIVABLE`, `NOT_REDRIVABLE`, or
`REDRIVABLE_BY_MAP_RUN` where a failed Map Run is the way back in.

```bash
aws stepfunctions describe-execution \
    --execution-arn arn:aws:states:<region>:<account-id>:execution:<state-machine-name>:<execution-name> \
    --query '[status,redriveCount,redriveStatus]' --output text
```

### To list the executions waiting to be redriven

The following example lists the executions sitting in the `PENDING_REDRIVE`
state, which is where a failed execution waits for a redrive.

```bash
aws stepfunctions list-executions \
    --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
    --status-filter PENDING_REDRIVE
```

## Workflows

### To run a Standard workflow and read its output

1. Start the execution and capture the execution ARN, because every later step
   needs it and the ARN contains the generated execution name.

   ```bash
   EXECUTION_ARN=$(aws stepfunctions start-execution \
       --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
       --name <execution-name> \
       --input '{"orderId":"1234"}' \
       --query 'executionArn' --output text)
   echo "$EXECUTION_ARN"
   ```

2. Wait for the execution to reach a terminal state. `SUCCEEDED`, `FAILED`,
   `TIMED_OUT`, and `ABORTED` are all terminal, and anything else means keep
   waiting.

   ```bash
   while :; do
       STATUS=$(aws stepfunctions describe-execution \
           --execution-arn "$EXECUTION_ARN" \
           --query 'status' --output text)
       echo "$STATUS"
       case "$STATUS" in
           SUCCEEDED|FAILED|TIMED_OUT|ABORTED) break ;;
       esac
       sleep 5
   done
   ```

3. Read the output, or the error and cause when the status is not `SUCCEEDED`.

   ```bash
   aws stepfunctions describe-execution \
       --execution-arn "$EXECUTION_ARN" \
       --query 'output' --output text | jq -r '.result'
   aws stepfunctions describe-execution \
       --execution-arn "$EXECUTION_ARN" \
       --query '[error,cause]' --output text
   ```

### To publish a version and shift an alias

1. Update the definition and publish it in one call, then capture the version
   number that comes back.

   ```bash
   VERSION=$(aws stepfunctions update-state-machine \
       --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
       --definition file://state-machine.json \
       --role-arn arn:aws:iam::<account-id>:role/<role> \
       --publish \
       --version-description <version-description> \
       --query 'stateMachineVersionArn' --output text)
   echo "$VERSION"
   ```

2. Point the alias at the new version. Because the routing configuration is the
   only thing that changes, rolling back is the same call with the old version
   number.

   ```bash
   aws stepfunctions update-state-machine-alias \
       --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name> \
       --routing-configuration "[{\"stateMachineVersionArn\":\"$VERSION\",\"weight\":100}]"
   ```

3. Confirm the alias resolved to the version you just published.

   ```bash
   aws stepfunctions describe-state-machine-alias \
       --state-machine-alias-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name>:<alias-name>
   ```

### To redrive every failed execution of a state machine

1. Collect the execution ARNs of the failures that have not been redriven yet,
   which is what the loop below consumes.

   ```bash
   aws stepfunctions list-executions \
       --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
       --status-filter FAILED \
       --redrive-filter NOT_REDRIVEN \
       --query 'executions[].executionArn' --output text
   ```

2. Redrive each execution in turn. Redrive reuses the original ARN and input, so
   a second pass over the same list is harmless.

   ```bash
   for execution in $(aws stepfunctions list-executions \
       --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
       --status-filter FAILED \
       --redrive-filter NOT_REDRIVEN \
       --query 'executions[].executionArn' --output text); do
       aws stepfunctions redrive-execution --execution-arn "$execution"
   done
   ```

3. Check the outcome on the executions you just touched.

   ```bash
   aws stepfunctions list-executions \
       --state-machine-arn arn:aws:states:<region>:<account-id>:stateMachine:<state-machine-name> \
       --query 'executions[].[name,status,redriveCount]' --output table
   ```
