# Initial setup and selected additions

Follow the installation and preservation rules in [SKILL.md](../SKILL.md)
before running scripts. For an existing or partial router, verify and repair
it first; do not use setup as a substitute for repairing validation errors.

Before running setup, check whether root `AGENTS.md` and `CLAUDE.md` already
exist and remember which are missing. Run the helper once with an existing
project root and the verified skill package:

```sh
python "<skill-dir>/scripts/setup.py" "<PROJECT_ROOT>"  # core only
# After selection, optionally pass --claude-md, --copilot-instructions,
# --include-skills, and/or --include-specialists
```

The helper installs the package, creates missing core routing files and
validates them. It never prompts or overwrites an existing index or map.
Optional flags create selected maps; if an existing index lacks a selected
map link, repair it when the intended selection is confirmed. When a user
selects an additional entry point in an existing project, run setup with the
corresponding flag; it preserves the existing index and instruction content.

## Entry points

- Pass `--claude-md` when Claude routing is selected. If `CLAUDE.md` is
  missing, the flag creates it with only `@AGENTS.md`. Without `CLAUDECODE`,
  an existing file with that import is left unchanged; an existing file
  without it gains the index import and pointer but not an `@AGENTS.md`
  import. When `CLAUDECODE` is set, add a normal Markdown pointer to the index
  to an existing `CLAUDE.md` if missing, preserving its contents and without
  adding an index import. For compatibility, `CLAUDECODE` without the flag
  creates a missing file with `@AGENTS.md`, the index import and a routing
  pointer.
- If an existing root `CLAUDE.md` is a symlink directly to the regular root
  `AGENTS.md`, leave the symlink untouched and update `AGENTS.md` only. Do not
  add `@AGENTS.md` to the shared file. Reject other symlink targets when
  `CLAUDE.md` is selected, including broken links. Verify the intended client
  follows symlinked instruction files before relying on them.
- If GitHub.com Copilot Chat support is selected, pass
  `--copilot-instructions` to create or update `.github/copilot-instructions.md`
  with a relative pointer to the same index. Preserve existing instructions.
  Chat must have the repository attached, and following the pointer depends on
  the surface and available context; do not assume links are expanded.

## Populate a newly created root entry

Setup creates routing entry points but does not discover project-specific
instructions. After successful setup, populate a newly created `AGENTS.md` with
concise, actionable guidance based on [AGENTS.md](https://agents.md/): a brief
project overview, setup/build and testing commands, code style and testing
conventions, and relevant security or contribution instructions, where
supported by project evidence.

Inspect the project's existing instructions, README, contribution guide,
manifests, scripts and CI configuration as relevant. Use only confirmed facts
and commands, including their working directory; do not copy the website's
example commands or invent policies. Distinguish commands verified from
source from commands actually run. Omit unsupported topics and report material
gaps; if no project-specific guidance can be established, report the entry as
incomplete rather than presenting the routing pointer as a complete guide.
Keep the existing routing pointer and avoid duplicating folder-level detail.

Do not rewrite or populate a pre-existing entry just because setup was run;
preserve its existing instructions and managed blocks. If a newly created
`CLAUDE.md` imports `AGENTS.md`, keep the contents specified by the entry-point
rules above and put shared guidance in `AGENTS.md`, not in a second copy. If
`CLAUDE.md` is an alias, edit only `AGENTS.md`. Do not create an additional
Claude entry unless selected or enabled by the existing setup rules. After
populating a new entry, rerun the validator and report failures or unresolved
evidence; if setup fails, do not populate it.

Setup validates proposed contents before writing and reports validation errors
and notes, distinguishing matching pre-existing errors from errors introduced
by the proposal. It checks observed paths again before publication, though
concurrent edits can still race with filesystem operations. A write-time or
final validation failure may leave partial changes; review the reported files
manually. Setup does not automatically roll back writes because an independent
editor may have changed them. New empty directories are removed where possible;
a crash can also leave partial changes.
