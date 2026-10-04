# Invalidation

> Invalidation. Part of the [CloudFront](../) cheatsheet.

## To invalidate all files

The following example invalidates all files.

```bash
aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/*"
```

## To invalidate specific paths

The following example invalidates specific paths.

```bash
aws cloudfront create-invalidation --distribution-id E1234567890ABC --paths "/images/*" "/css/*"
```

## To list invalidations

The following example lists invalidations.

```bash
aws cloudfront list-invalidations --distribution-id E1234567890ABC
```

## To check invalidation status

The following example checks invalidation status.

```bash
aws cloudfront get-invalidation --distribution-id E1234567890ABC --id I1234567890ABC
```
