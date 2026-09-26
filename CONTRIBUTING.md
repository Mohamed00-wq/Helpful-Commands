# Contributing

Thanks for improving the cheatsheet. This is a documentation-only repository, so
the bar is that every command is correct and every example follows one format.

## Before you start

Read [TEMPLATE.md](TEMPLATE.md). It defines the file structure and the rules
below in detail. The short version:

- One command per example.
- Every example is an `### To <verb phrase>` heading, one sentence of prose, and
  one fenced code block.
- Long commands wrap with `\` and a 4-space indent.

## Adding a command to an existing file

1. Find the `##` section for the resource the command belongs to, for example
   `## Instances` in `AWS/EC2.md`. If no section fits, add one.
2. Add the example in the same shape as its neighbours.
3. Use the placeholder style from `TEMPLATE.md`.
4. Run the linter and fix every warning.

## Adding a new service

1. Create `AWS/<ACRONYM>.md` using the acronym in caps.
2. Follow `TEMPLATE.md` exactly.
3. Add a row to the table in `README.md`, grouped under the right category.
4. Run the linter.

## Rules that are not negotiable

- **No deprecated APIs.** Launch configurations, ClassicLink, and
  `put-bucket-acl` style calls do not belong here. If you find one, replace it
  and note why in the pull request.
- **No invented resource ids.** Write `<bucket>`, not `my-bucket-12345-prod`.
  Placeholders that look real get copy-pasted into the wrong account.
- **No duplicate content.** If a command already appears in the file, do not
  add it again in a summary block at the end of a section.
- **Real runnable commands.** If you cannot run it, do not claim that you did.
- **No command tables.** A command inside a markdown table cannot be copied
  cleanly. Tabular reference data, such as the EBS volume type comparison, is
  fine because it is data and not commands.

## Checking your work

```bash
# Lint every markdown file
npx markdownlint-cli2 "**/*.md"

# Confirm no example block repeats a command verbatim
# (the same subcommand with different flags is fine and expected)
awk '/^```bash/{f=!f; next} f' AWS/<FILE>.md \
  | tr '\\' ' ' | tr -s ' ' \
  | sed 's/^ //; s/ $//' \
  | grep -E '^aws ' | sort | uniq -d
```

The last command should print nothing. A non-empty result means the same
command appears in two examples, which means one of them should be removed.

## Commit messages

Describe the change, for example `Replace launch configurations with launch
templates in ASG.md`. Do not number commits ("3rd commit"); the history is
already ordered.
