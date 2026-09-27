# Function URLs

> Function URLs. Part of the [Lambda](../Lambda.md) cheatsheet.

## To create a public function URL

The following example creates a public function URL.

```bash
aws lambda create-function-url-config --function-name my-function --auth-type NONE
```

## To create an IAM-protected URL

The following example creates an IAM-protected URL.

```bash
aws lambda create-function-url-config --function-name my-function --auth-type AWS_IAM
```

## To list function URLs

The following example lists function URLs.

```bash
aws lambda list-function-url-configs --function-name my-function
```

## To get function URL details

The following example gets function URL details.

```bash
aws lambda get-function-url-config --function-name my-function
```

## To delete function URL

The following example deletes function URL.

```bash
aws lambda delete-function-url-config --function-name my-function
```
