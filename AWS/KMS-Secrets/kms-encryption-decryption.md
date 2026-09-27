# KMS Encryption/Decryption

> KMS Encryption/Decryption. Part of the [KMS-Secrets](../KMS-Secrets.md) cheatsheet.

## To encrypt data

The following example encrypts data.

```bash
aws kms encrypt --key-id alias/my-key --plaintext "Hello World"
```

## To decrypt data

The following example decrypts data.

```bash
aws kms decrypt --ciphertext-blob fileb://encrypted.bin
```

## To re-encrypt with different key

The following example re-encrypts with different key.

```bash
aws kms re-encrypt --ciphertext-blob fileb://encrypted.bin --key-id alias/new-key
```

## To generate a data key

The following example generates a data key.

```bash
aws kms generate-data-key --key-id alias/my-key --key-spec AES_256
```

## To generate random bytes

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
