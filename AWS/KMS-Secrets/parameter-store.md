# Parameter Store (SSM)

> Parameter Store (SSM). Part of the [KMS-Secrets](../) cheatsheet.

## To get a parameter

The following example gets a parameter.

```bash
aws ssm get-parameters --names /my/app/db-password
```

## To get all params under a path

The following example gets all params under a path.

```bash
aws ssm get-parameters-by-path --path /my/app
```

## To create a secure parameter

The following example creates a secure parameter.

```bash
aws ssm put-parameter --name /my/app/db-password --value "P@ssw0rd!" --type SecureString
```

## To create a string parameter

The following example creates a string parameter.

```bash
aws ssm put-parameter --name /my/app/db-host --value "db.example.com" --type String
```

## To create a string list parameter

The following example creates a string list parameter.

```bash
aws ssm put-parameter --name /my/app/config --value '{"key":"value"}' --type StringList
```

## To delete parameters

The following example deletes parameters.

```bash
aws ssm delete-parameters --names /my/app/db-password
```

## To list all parameters

The following example lists all parameters.

```bash
aws ssm describe-parameters
```

## To get decrypted value

The following example gets decrypted value.

```bash
aws ssm get-parameter --name /my/app/db-password --with-decryption
```

```bash
# Create secure parameter
aws ssm put-parameter \
    --name /my/app/db-password \
    --value "P@ssw0rd!" \
    --type SecureString

# Get all params under path
aws ssm get-parameters-by-path \
    --path /my/app \
    --recursive \
    --with-decryption
```
