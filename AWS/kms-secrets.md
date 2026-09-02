# 🔑 KMS & Secrets Manager

> Advanced KMS and Secrets Manager CLI commands for keys, secrets, rotation, and encryption — compiled while studying for AWS SAA-C03.

![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![CLI](https://img.shields.io/badge/AWS-CLI-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## KMS Keys

| Command | Description |
|---|---|
| `aws kms list-keys` | List all KMS keys |
| `aws kms describe-key --key-id alias/my-key` | Get key details |
| `aws kms create-key --description "My encryption key" --tags TagKey=env,TagValue=prod` | Create a KMS key |
| `aws kms create-key --key-spec SYMMETRIC_DEFAULT` | Create a symmetric key |
| `aws kms create-key --key-spec RSA_2048 --key-usage ENCRYPT_DECRYPT` | Create an asymmetric key |
| `aws kms enable-key --key-id xxx` | Enable a key |
| `aws kms disable-key --key-id xxx` | Disable a key |
| `aws kms schedule-key-deletion --key-id xxx --pending-window-in-days 7` | Schedule key deletion |
| `aws kms cancel-key-deletion --key-id xxx` | Cancel scheduled deletion |

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

---

## KMS Encryption/Decryption

| Command | Description |
|---|---|
| `aws kms encrypt --key-id alias/my-key --plaintext "Hello World"` | Encrypt data |
| `aws kms decrypt --ciphertext-blob fileb://encrypted.bin` | Decrypt data |
| `aws kms re-encrypt --ciphertext-blob fileb://encrypted.bin --key-id alias/new-key` | Re-encrypt with different key |
| `aws kms generate-data-key --key-id alias/my-key --key-spec AES_256` | Generate a data key |
| `aws kms generate-random --number-of-bytes 32` | Generate random bytes |

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

---

## KMS Grants

| Command | Description |
|---|---|
| `aws kms list-grants --key-id xxx` | List grants on a key |
| `aws kms create-grant --key-id xxx --grantee-principal arn:aws:iam::ACCOUNT:role/my-role --operations Encrypt Decrypt` | Create a grant |
| `aws kms revoke-grant --key-id xxx --grant-id xxx` | Revoke a grant |

```bash
aws kms create-grant \
  --key-id xxx \
  --grantee-principal arn:aws:iam::ACCOUNT:role/my-role \
  --operations Encrypt Decrypt
```

---

## KMS Key Policy

| Command | Description |
|---|---|
| `aws kms get-key-policy --key-id xxx --policy-name default` | Get key policy |
| `aws kms put-key-policy --key-id xxx --policy-name default --policy file://policy.json` | Set key policy |

```bash
aws kms get-key-policy --key-id xxx --policy-name default
aws kms put-key-policy \
  --key-id xxx \
  --policy-name default \
  --policy file://policy.json
```

---

## Secrets Manager

| Command | Description |
|---|---|
| `aws secretsmanager list-secrets` | List all secrets |
| `aws secretsmanager get-secret-value --secret-id my-secret` | Get secret value |
| `aws secretsmanager create-secret --name my-secret --secret-string '{"username":"admin","password":"P@ssw0rd!"}'` | Create a secret (JSON) |
| `aws secretsmanager create-secret --name my-secret --secret-string "plain-text-value"` | Create a secret (text) |
| `aws secretsmanager create-secret --name my-secret --secret-binary fileb://binary.dat` | Create a binary secret |
| `aws secretsmanager update-secret --secret-id my-secret --secret-string '{"username":"admin","password":"NewP@ss!"}'` | Update a secret |
| `aws secretsmanager delete-secret --secret-id my-secret` | Delete a secret |
| `aws secretsmanager delete-secret --secret-id my-secret --force-delete-without-recovery` | Force delete |
| `aws secretsmanager restore-secret --secret-id my-secret` | Restore a deleted secret |

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

---

## Secrets Manager Rotation

| Command | Description |
|---|---|
| `aws secretsmanager describe-secret --secret-id my-secret` | Get rotation config |
| `aws secretsmanager rotate-secret --secret-id my-secret` | Manually rotate |
| `aws secretsmanager update-secret-version-stage --secret-id my-secret --version-stage AWSPENDING --version-id xxx` | Update version stage |

```bash
aws secretsmanager rotate-secret --secret-id my-secret
```

---

## Secrets Manager Resource Policy

| Command | Description |
|---|---|
| `aws secretsmanager get-resource-policy --secret-id my-secret` | Get resource policy |
| `aws secretsmanager put-resource-policy --secret-id my-secret --resource-policy file://policy.json` | Set resource policy |
| `aws secretsmanager delete-resource-policy --secret-id my-secret` | Remove resource policy |

```bash
aws secretsmanager put-resource-policy \
  --secret-id my-secret \
  --resource-policy file://policy.json
```

---

## Parameter Store (SSM)

| Command | Description |
|---|---|
| `aws ssm get-parameters --names /my/app/db-password` | Get a parameter |
| `aws ssm get-parameters-by-path --path /my/app` | Get all params under a path |
| `aws ssm put-parameter --name /my/app/db-password --value "P@ssw0rd!" --type SecureString` | Create a secure parameter |
| `aws ssm put-parameter --name /my/app/db-host --value "db.example.com" --type String` | Create a string parameter |
| `aws ssm put-parameter --name /my/app/config --value '{"key":"value"}' --type StringList` | Create a string list parameter |
| `aws ssm delete-parameters --names /my/app/db-password` | Delete parameters |
| `aws ssm describe-parameters` | List all parameters |
| `aws ssm get-parameter --name /my/app/db-password --with-decryption` | Get decrypted value |

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

---

## 📄 License

Feel free to use, modify, and share this cheatsheet.
