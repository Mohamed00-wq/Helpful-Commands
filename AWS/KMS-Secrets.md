# KMS and Secrets Manager (Encryption and Secrets)

> Commands for KMS keys, encryption contexts, Secrets Manager, and SSM Parameter Store.

## KMS Keys

### To list all KMS keys

The following example lists all KMS keys.

```bash
aws kms list-keys
```

### To get key details

The following example gets key details.

```bash
aws kms describe-key --key-id alias/my-key
```

### To create a KMS key

The following example creates a KMS key.

```bash
aws kms create-key --description "My encryption key" --tags TagKey=env,TagValue=prod
```

### To create a symmetric key

The following example creates a symmetric key.

```bash
aws kms create-key --key-spec SYMMETRIC_DEFAULT
```

### To create an asymmetric key

The following example creates an asymmetric key.

```bash
aws kms create-key --key-spec RSA_2048 --key-usage ENCRYPT_DECRYPT
```

### To enable a key

The following example enables a key.

```bash
aws kms enable-key --key-id xxx
```

### To disable a key

The following example disables a key.

```bash
aws kms disable-key --key-id xxx
```

### To schedule key deletion

The following example schedules key deletion.

```bash
aws kms schedule-key-deletion --key-id xxx --pending-window-in-days 7
```

### To cancel scheduled deletion

The following example cancels scheduled deletion.

```bash
aws kms cancel-key-deletion --key-id xxx
```

```bash
# Create KMS key
aws kms create-key --description "My encryption key"

# Create alias
aws kms create-alias --alias-name alias/my-key --target-key-id xxx

# Schedule deletion
aws kms schedule-key-deletion \
    --key-id xxx \
    --pending-window-in-days 7
```

## KMS Encryption/Decryption

### To encrypt data

The following example encrypts data.

```bash
aws kms encrypt --key-id alias/my-key --plaintext "Hello World"
```

### To decrypt data

The following example decrypts data.

```bash
aws kms decrypt --ciphertext-blob fileb://encrypted.bin
```

### To re-encrypt with different key

The following example re-encrypts with different key.

```bash
aws kms re-encrypt --ciphertext-blob fileb://encrypted.bin --key-id alias/new-key
```

### To generate a data key

The following example generates a data key.

```bash
aws kms generate-data-key --key-id alias/my-key --key-spec AES_256
```

### To generate random bytes

The following example generates random bytes.

```bash
aws kms generate-random --number-of-bytes 32
```

```bash
# Encrypt
aws kms encrypt \
    --key-id alias/my-key \
    --plaintext "Hello World" \
    --output text \
    --query CiphertextBlob

# Decrypt
aws kms decrypt \
    --ciphertext-blob fileb://encrypted.bin \
    --output text \
    --query Plaintext

# Generate data key (envelope encryption)
aws kms generate-data-key \
    --key-id alias/my-key \
    --key-spec AES_256
```

## KMS Grants

### To list grants on a key

The following example lists grants on a key.

```bash
aws kms list-grants --key-id xxx
```

### To create a grant

The following example creates a grant.

```bash
aws kms create-grant
    --key-id xxx
    --grantee-principal arn:aws:iam::ACCOUNT:role/my-role
    --operations Encrypt Decrypt
```

### To revoke a grant

The following example revokes a grant.

```bash
aws kms revoke-grant --key-id xxx --grant-id xxx
```

## KMS Key Policy

### To get key policy

The following example gets key policy.

```bash
aws kms get-key-policy --key-id xxx --policy-name default
```

### To set key policy

The following example sets key policy.

```bash
aws kms put-key-policy --key-id xxx --policy-name default --policy file://policy.json
```

## Secrets Manager

### To list all secrets

The following example lists all secrets.

```bash
aws secretsmanager list-secrets
```

### To get secret value

The following example gets secret value.

```bash
aws secretsmanager get-secret-value --secret-id my-secret
```

### To create a secret (JSON)

The following example creates a secret (JSON).

```bash
aws secretsmanager create-secret
    --name my-secret
    --secret-string '{"username":"admin","password":"P@ssw0rd!"}'
```

### To create a secret (text)

The following example creates a secret (text).

```bash
aws secretsmanager create-secret --name my-secret --secret-string "plain-text-value"
```

### To create a binary secret

The following example creates a binary secret.

```bash
aws secretsmanager create-secret --name my-secret --secret-binary fileb://binary.dat
```

### To update a secret

The following example updates a secret.

```bash
aws secretsmanager update-secret
    --secret-id my-secret
    --secret-string '{"username":"admin","password":"NewP@ss!"}'
```

### To delete a secret

The following example deletes a secret.

```bash
aws secretsmanager delete-secret --secret-id my-secret
```

### To force delete

The following example forces delete.

```bash
aws secretsmanager delete-secret --secret-id my-secret --force-delete-without-recovery
```

### To restore a deleted secret

The following example restores a deleted secret.

```bash
aws secretsmanager restore-secret --secret-id my-secret
```

```bash
# Create secret
aws secretsmanager create-secret \
    --name my-secret \
    --secret-string '{"username":"admin","password":"P@ssw0rd!"}'

# Get secret value
aws secretsmanager get-secret-value \
    --secret-id my-secret \
    --query SecretString \
    --output text
```

## Secrets Manager Rotation

### To get rotation config

The following example gets rotation config.

```bash
aws secretsmanager describe-secret --secret-id my-secret
```

### To rotate a secret manually

The following example immediately rotates a secret instead of waiting for its schedule.

```bash
aws secretsmanager rotate-secret --secret-id my-secret
```

### To update version stage

The following example updates version stage.

```bash
aws secretsmanager update-secret-version-stage
    --secret-id my-secret
    --version-stage AWSPENDING
    --version-id xxx
```

## Secrets Manager Resource Policy

### To get resource policy

The following example gets resource policy.

```bash
aws secretsmanager get-resource-policy --secret-id my-secret
```

### To set resource policy

The following example sets resource policy.

```bash
aws secretsmanager put-resource-policy --secret-id my-secret --resource-policy file://policy.json
```

### To remove resource policy

The following example removes resource policy.

```bash
aws secretsmanager delete-resource-policy --secret-id my-secret
```

## Parameter Store (SSM)

### To get a parameter

The following example gets a parameter.

```bash
aws ssm get-parameters --names /my/app/db-password
```

### To get all params under a path

The following example gets all params under a path.

```bash
aws ssm get-parameters-by-path --path /my/app
```

### To create a secure parameter

The following example creates a secure parameter.

```bash
aws ssm put-parameter --name /my/app/db-password --value "P@ssw0rd!" --type SecureString
```

### To create a string parameter

The following example creates a string parameter.

```bash
aws ssm put-parameter --name /my/app/db-host --value "db.example.com" --type String
```

### To create a string list parameter

The following example creates a string list parameter.

```bash
aws ssm put-parameter --name /my/app/config --value '{"key":"value"}' --type StringList
```

### To delete parameters

The following example deletes parameters.

```bash
aws ssm delete-parameters --names /my/app/db-password
```

### To list all parameters

The following example lists all parameters.

```bash
aws ssm describe-parameters
```

### To get decrypted value

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
