# Task Definitions

> Task Definitions. Part of the [ECS](../ECS.md) cheatsheet.

## To list all task definitions

The following example lists all task definitions.

```bash
aws ecs list-task-definitions
```

## To get task definition details

The following example gets task definition details.

```bash
aws ecs describe-task-definition --task-definition my-task:1
```

## To register a task definition

The following example registers a task definition.

```bash
aws ecs register-task-definition --cli-input-json file://task-def.json
```

## To deregister a revision

The following example deregisters a revision.

```bash
aws ecs deregister-task-definition --task-definition my-task:1
```

## To list task definition families

The following example lists task definition families.

```bash
aws ecs list-task-definition-families
```
