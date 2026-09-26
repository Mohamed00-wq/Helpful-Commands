# Service file template

Every service file in `AWS/` follows this structure. It is based on the
[AWS CLI code example style guide](https://aws.github.io/aws-cli/docs_styleguide.html).

## Skeleton

````markdown
# <ACRONYM> (<Full Service Name>)

> One or two sentences: what the service does and what this file covers.

## <Resource group>

### To <imperative verb phrase>

<One sentence of prose, present tense, third person.>

```bash
aws <group> <subcommand> \
    --flag <value> \
    --flag <value>
```
````

## Rules

1. **Title** is `# <ACRONYM> (<Full Service Name>)`. No emoji, no "Cheat Sheet" suffix.
   The acronym matches the filename, so `S3.md` starts with `# S3 (Simple Storage Service)`.
2. **One blockquote line** under the title. No badges.
3. **`##` sections** group by resource, not by verb (`## Instances`, not `## List`).
4. **`### To <verb phrase>`** headings are imperative and start with "To".
5. **One prose sentence per example.** AWS style: "The following example lists all
   instances in your account." Not a bullet, not a fragment.
6. **One command per code block.** Long commands use `\` continuations indented
   **4 spaces** (not 2).
7. **No `--output` flag** unless the example is specifically demonstrating a
   non-JSON output format. Set `json` once in `~/.aws/config` instead.
8. **Placeholders** use angle brackets: `s3://<bucket>`, `--instance-ids <id>`.
   Real resource IDs from AWS docs are not invented.
9. **No tables of commands.** A command in a markdown table cannot be copied cleanly.
   Genuinely tabular reference data (for example the EBS volume type comparison)
   stays a table, because it is data and not commands.
10. **Multi-step procedures** are a `## Workflow` section with numbered steps and
    a note on what to capture from each step's output.
11. **Destructive commands** get a `--dryrun` example alongside the real one.
12. **List commands** that can truncate get a pagination example.
13. **No license footer.** The licence lives in `/LICENSE` once.

## Placeholder conventions

| Kind | Form | Example |
| --- | --- | --- |
| Resource id | `<id>` or service prefix | `--instance-ids <id>`, `i-0123456789abcdef0` |
| Bucket | `<bucket>` | `s3://<bucket>` |
| File | `<file>` | `aws s3 cp <file> s3://<bucket>/` |
| Account id | `<account-id>` | `arn:aws:iam::<account-id>:role/<role>` |
| Region | `<region>` | `--region <region>` |

## Adding a new service

1. Copy `TEMPLATE.md`'s skeleton into `AWS/<ACRONYM>.md`.
2. Use the acronym in caps for the filename, matching the service's own naming.
3. Add the file to the table in `README.md`.
4. Run `npx markdownlint-cli2 "**/*.md"` and fix every warning.
