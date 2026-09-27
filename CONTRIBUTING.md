# Contributing

Thanks for improving the cheatsheet. This is a documentation-only repository, so
the bar is that every command is correct and every example follows one format.

## Before you start

Read [TEMPLATE.md](TEMPLATE.md). It defines the file structure and the rules
below in detail. The short version:

- One command per example.
- Every example is a `To <verb phrase>` heading, one sentence of prose, and one
  fenced code block.
- Long commands wrap with `\` and a 4-space indent.
- One resource group per file. `AWS/<SERVICE>.md` is an index; the commands live
  in `AWS/<SERVICE>/<resource-group>.md`.

## Adding a command to an existing file

1. Find the topic file for the resource the command belongs to, for example
   `AWS/EC2/instances.md` for `## Instances`. If the resource group has no file
   yet, create one from the topic skeleton in `TEMPLATE.md` and add it to the
   `## Sections` list in `AWS/EC2.md`.
2. Add the example in the same shape as its neighbours, at `##` heading level.
3. Use the placeholder style from `TEMPLATE.md`.
4. Run the linter and fix every warning.

## Adding a new service

1. Create the folder `AWS/<ACRONYM>/`, the index `AWS/<ACRONYM>.md`, and one
   topic file per resource group.
2. Follow `TEMPLATE.md` exactly.
3. Add the index to the table in `README.md`.
4. Run the linter.

## Rules that are not negotiable

- **No deprecated APIs.** Launch configurations, ClassicLink, and
  `put-bucket-acl` style calls do not belong here. If you find one, replace it
  and note why in the pull request.
- **No invented resource ids.** Write `<bucket>`, not `my-bucket-12345-prod`.
  Placeholders that look real get copy-pasted into the wrong account.
- **No duplicate content.** If a command already appears in the service, do not
  add it again in a summary block at the end of a file, or in a second topic
  file. The duplicate check below spans the whole service.
- **Real runnable commands.** If you cannot run it, do not claim that you did.
- **No command tables.** A command inside a markdown table cannot be copied
  cleanly. Tabular reference data, such as the EBS volume type comparison, is
  fine because it is data and not commands.

## Checking your work

```bash
# Lint every markdown file
npx markdownlint-cli2 "**/*.md"

# Confirm no example repeats a command verbatim across a service's topic files.
# The same subcommand with different flags is fine and expected.
# Workflows are excluded: a workflow repeats commands from the topic files it
# chains, on purpose, so it can be run top to bottom.
awk '
  /^```bash/ { f = !f; next }
  f {
    line = $0
    if (line ~ /\\[[:space:]]*$/) {
      sub(/\\[[:space:]]*$/, "", line)
      buf = buf line " "
      next
    }
    print buf line
    buf = ""
  }
  END { if (buf != "") print buf }
' AWS/<SERVICE>/*.md \
  | tr -s ' ' | sed 's/^ //; s/ $//' \
  | grep -E '^aws ' | sort | uniq -d
```

That should print nothing. A non-empty result means the same command appears in
two examples, which means one of them should be removed. The `awk` joins
`\` continuation lines into one logical command before comparing, otherwise
every wrapped command looks like a duplicate of itself.

The last command should print nothing. A non-empty result means the same
command appears in two examples, which means one of them should be removed.

## Commit messages

Describe the change, for example `Replace launch configurations with launch
templates in ASG.md`. Do not number commits ("3rd commit"); the history is
already ordered.
