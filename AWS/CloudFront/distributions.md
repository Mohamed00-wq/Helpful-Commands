# Distributions

> Distributions. Part of the [CloudFront](../CloudFront.md) cheatsheet.

## To list all distributions

## To get distribution details

The following example gets distribution details.

```bash
aws cloudfront get-distribution --id E1234567890ABC
```

## To create a distribution

The following example creates a distribution.

```bash
aws cloudfront create-distribution --distribution-config file://dist-config.json
```

## To update a distribution

The following example updates a distribution.

```bash
aws cloudfront update-distribution
    --id E1234567890ABC
    --distribution-config file://dist-config.json
    --if-match E1234567890ABC
```

## To delete a distribution

The following example deletes a distribution.

```bash
aws cloudfront delete-distribution --id E1234567890ABC --if-match E1234567890ABC
```

## To disable a distribution

The following example disables a distribution.

```bash
aws cloudfront disable-distribution --id E1234567890ABC --if-match E1234567890ABC
```

## To enable a distribution

The following example enables a distribution.

```bash
aws cloudfront enable-distribution --id E1234567890ABC --if-match E1234567890ABC
```
