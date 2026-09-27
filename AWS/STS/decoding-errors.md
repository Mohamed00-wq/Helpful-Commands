# Decoding Errors

> Decoding Errors. Part of the [STS](../STS.md) cheatsheet.

## To decode an access denied message

The following example decodes the opaque string that comes back with an
`AccessDenied` error, which names the missing permission and the identity that
was denied.

```bash
aws sts decode-authorization-message --encoded-message <encoded-message>
```

## To list expired presigned request tokens

The following example lists presigned requests that have already expired but
are still being accepted because the originating credentials are still valid.

```bash
aws sts list-expired-grants
```
