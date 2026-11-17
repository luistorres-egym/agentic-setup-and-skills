# Initial setup and selected entry points

Read this guide before initial setup, or when adding a selected entry point or
optional map to an existing project. Follow the installation check and shared
preservation, selection, context-writing, validation, and reporting rules in
`SKILL.md`. Do not run setup for a read-only request.

## Select entry points and maps

The core setup creates the root `AGENTS.md` pointer and
`.context-routing/index.md` if missing. Select optional entry points and maps
only as requested or approved by the user; preserve existing instructions.

- Pass `--claude-md` to create or update `CLAUDE.md` when Claude routing is
  selected. If the file is missing, the flag creates it with only `@AGENTS.md`.
  Without `CLAUDECODE`, an existing file with that import is left unchanged; an
  existing file without it gains the index import and pointer but not an
  `@AGENTS.md` import. When `CLAUDECODE` is set, add a normal Markdown pointer
  to the index to an existing `CLAUDE.md` if missing, preserving its contents
  and without adding an index import. For compatibility, `CLAUDECODE` without
  the flag creates a missing file with `@AGENTS.md`, the index import and a
  routing pointer.
- If an existing root `CLAUDE.md` is a symlink directly to the regular root
  `AGENTS.md`, leave the symlink untouched and update `AGENTS.md` only. Do not
  add `@AGENTS.md` to the shared file. Reject other symlink targets when
  `CLAUDE.md` is selected, including broken links.
  Verify the intended client follows symlinked instruction files before relying
  on them.
- If GitHub.com Copilot Chat support is selected, pass
  `--copilot-instructions` to create or update `.github/copilot-instructions.md`
  with a relative pointer to the same index. Preserve existing instructions.
  Chat must have the repository attached, and following the pointer depends on
  the surface and available context; do not assume links are expanded.
- After selection, pass `--include-skills` and/or `--include-specialists` to
  create the selected optional maps.

## Architecture overview

When there is no recorded architecture-overview decision, ask whether the user
wants an `ARCHITECTURE.md`. A direct request counts as selection. Continue
independent routing work while waiting; create or update the overview only
after selection. An existing file alone does not establish a decision: inspect
and preserve it, and ask whether to adopt it. Record the decision outside the
folder table in `index.md` as `Architecture overview: selected` or
`Architecture overview: declined`. A previously recorded index link to the
overview also counts as selection; preserve it unless its removal is authorized.
Preserve prior decisions and do not ask again unless the user changes the choice.

For a selected overview, add an ordinary Markdown link
`[Architecture overview](ARCHITECTURE.md)` to the relevant root `AGENTS.md`
or `CLAUDE.md` entry point, with an instruction to read it when useful to the
task. If both are used independently, add it to each. If `CLAUDE.md` imports
or is a direct alias of `AGENTS.md`, the link in `AGENTS.md` is sufficient;
do not duplicate it or add an `@ARCHITECTURE.md` import. Preserve existing
instructions, avoid duplicate links, and create the overview before adding
the link. Do not add a link if the overview was declined.

Use the [architecture overview](architecture-overview.md) as the template for
a selected overview. Replace its examples and placeholders with verified
project information; distinguish plans and uncertainty from observed facts.
Preserve unrelated existing content and do not include credentials.

## Populate new root entries

Before setup, check whether root `AGENTS.md` and `CLAUDE.md` already exist and
remember which are missing. Setup creates the routing entry points but does
not discover project-specific instructions. After successful setup, populate
a newly created `AGENTS.md` with concise, actionable guidance based on
[AGENTS.md](https://agents.md/): a brief project overview, setup/build and
testing commands, code style and testing conventions, and relevant security
or contribution instructions, where supported by project evidence.

Inspect the project's existing instructions, README, contribution guide,
manifests, scripts and CI configuration as relevant. Use only confirmed facts
and commands, including their working directory; do not copy the website's
example commands or invent policies. Distinguish commands verified from
source from commands actually run. Omit unsupported topics and report material
gaps; if no project-specific guidance can be established, report the entry as
incomplete rather than presenting the routing pointer as a complete guide.
Keep the routing pointer and avoid duplicating folder-level detail.

Do not rewrite or populate a pre-existing entry just because setup was run;
preserve its existing instructions and managed blocks. If a newly created
`CLAUDE.md` imports `AGENTS.md`, put shared guidance in `AGENTS.md`, not in a
second copy. If `CLAUDE.md` is an alias, edit only `AGENTS.md`. Do not create
an additional Claude entry unless selected or enabled by the setup rules.
After populating a new entry, rerun the validator and report failures or
unresolved evidence; if setup fails, do not populate it.

## Run setup

Use Python 3.10 or newer with its standard library and the verified matching
skill package. Before running the helper, check that the host has a Python 3.10+
interpreter using the command intended to run it (for example,
`python3 --version`, `python --version`, or `py -3 --version`). If none is
available, use `AskUserQuestion` to request authorization to install Python
3.10 or newer. Do not install it without approval; if approval is declined,
stop and report the prerequisite. After approval, use a host-supported installer
and verify the interpreter version before continuing. The Python helper itself
cannot prompt for this because it must already be running under Python.
Replace `<skill-dir>` with its actual directory and
`<PROJECT_ROOT>` with the project's existing root directory:

```sh
python "<skill-dir>/scripts/setup.py" "<PROJECT_ROOT>"  # core only
# After selection, optionally pass --claude-md, --copilot-instructions,
# --include-skills, and/or --include-specialists.
```

For initial setup, run the helper once. It installs the package, creates
missing core routing files and validates them; optional flags create selected
entry points and maps. It never prompts or overwrites an existing index or map.
If an existing index lacks a link to a selected map, add the relative link
during maintenance after confirming the selection, then rerun setup. The helper
rejects proposed maps until the existing index links them. If the selection is
not confirmed, report the gap and ask before editing. When adding a selected
entry point or map to an existing project, run setup with its corresponding
flag; it preserves the index and other instruction content.

Setup validates proposed contents before writing and reports validation errors
and notes, distinguishing matching pre-existing errors from errors introduced
by the proposal. It checks observed paths again before publication, though
concurrent edits can still race with filesystem operations. A write-time or
final validation failure may leave partial changes; review the reported files
manually. Setup does not automatically roll back writes because an independent
editor may have changed them. New empty directories are removed where possible;
a crash can also leave partial changes. Continue with the shared validation
and quality checks in `SKILL.md`.
