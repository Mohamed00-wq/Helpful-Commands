# Service file template

Every service in `AWS/` is a folder. `AWS/<ACRONYM>.md` is the index, and
`AWS/<ACRONYM>/<section>.md` holds the commands, one resource group per file. It
is based on the
[AWS CLI code example style guide](https://aws.github.io/aws-cli/docs_styleguide.html).

## Layout

```text
AWS/DynamoDB.md              index: title, summary, links, ## Workflows
AWS/DynamoDB/tables.md       one section of the old DynamoDB.md
AWS/DynamoDB/items.md
AWS/DynamoDB/query-and-scan.md
```

A topic filename is its section name lowercased, with punctuation and
parentheses replaced by hyphens: `## VPC Endpoints (PrivateLink)` becomes
`vpc-endpoints.md`. Keep a topic file to a single `##` section's worth of
examples; if it grows past roughly 200 lines, split it again.

## Skeleton: index file

````markdown
# <ACRONYM> (<Full Service Name>)

> One or two sentences: what the service does and what this file covers.

<Optional prose: the one distinction a reader needs before the commands.>

## Sections

- [<Resource group>](<ACRONYM>/<resource-group>.md)
- [<Resource group>](<ACRONYM>/<resource-group>.md)

## Workflows

<Numbered multi-step procedure.>
````

## Skeleton: topic file

````markdown
# <Resource group>

> <Resource group>. Part of the [<ACRONYM>](../<ACRONYM>.md) cheatsheet.

## To <imperative verb phrase>

<One sentence of prose, present tense, third person.>

```bash
aws <group> <subcommand> \
    --flag <value> \
    --flag <value>
```
````

Note the heading levels. A topic file has exactly one `#`, the resource group,
and its examples are `## To ...`. Splitting a service file means promoting every
heading one level, because the old `##` section title becomes the new `#`.

## Rules

1. **Title** is `# <ACRONYM> (<Full Service Name>)` on the index. The acronym
   matches the index filename, so `S3.md` starts with `# S3 (Simple Storage Service)`.
   A topic file's `#` is its resource group instead.
2. **One blockquote line** under the title. No badges. On a topic file the
   blockquote points back at the index.
3. **`##` sections** group by resource, not by verb (`## Instances`, not `## List`).
4. **`### To <verb phrase>`** headings are imperative and start with "To".
   In a topic file they are `## To ...`, because the resource group took the `#`.
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
10. **Multi-step procedures** stay in the index as a `## Workflows` section with
    numbered steps and a note on what to capture from each step's output. They
    chain commands from several topic files, so they do not belong in one.
11. **Destructive commands** get a `--dryrun` example alongside the real one.
12. **List commands** that can truncate get a pagination example.
13. **Cross-file references use links.** "See the section below" breaks once the
    content moves into a sibling file. Name the file and link to it relatively
    instead, pointing from `finding-changes.md` at `log-files.md`.
14. **No license footer.** The licence lives in `/LICENSE` once.

## Placeholder conventions

| Kind | Form | Example |
| --- | --- | --- |
| Resource id | `<id>` or service prefix | `--instance-ids <id>`, `i-0123456789abcdef0` |
| Bucket | `<bucket>` | `s3://<bucket>` |
| File | `<file>` | `aws s3 cp <file> s3://<bucket>/` |
| Account id | `<account-id>` | `arn:aws:iam::<account-id>:role/<role>` |
| Region | `<region>` | `--region <region>` |

## Adding a new service

1. Create the folder `AWS/<ACRONYM>/` and the index `AWS/<ACRONYM>.md`, both
   using the acronym in caps, matching the service's own naming.
2. Follow the index and topic skeletons above exactly.
3. Add the index to the table in `README.md`.
4. Run `npx markdownlint-cli2 "**/*.md"` and fix every warning.

## Splitting a service that grew too large

1. Pick the `##` sections to move. A topic file should stay near or under 200
   lines; the index keeps only the title, summary, links, and `## Workflows`.
2. For each moved section, create `AWS/<ACRONYM>/<slug>.md` and **promote every
   heading one level**: the section title becomes the file's `#`, and each
   `### To ...` becomes `## To ...`.
3. Copy the section body across unchanged. Do not retype the commands, and do
   not summarise them; a summary block duplicates content that is already there.
4. Add the slug to the index's `## Sections` list, in the original order.
5. Turn any "the section below" or "the values above" wording into a link.
6. Re-run the linter and the duplicate-command check from `CONTRIBUTING.md`.
