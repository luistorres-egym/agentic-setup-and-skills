---
name: project-context-router-egym
description: Builds and maintains project context routing through AGENTS.md and optional agent-specific pointers to .context-routing/index.md, which maps folders to verified CONTEXT.md files, with optional maps for agreed skills and specialists. Use when setting up or updating project context, routing tables, context maps, or folder-level context files. Also use when a meaningful change to behavior, interfaces, business rules, architecture, or responsibilities affects existing routes.
---

# Project Context Router EGYM

Give agents a small entry point to the project's code and business context,
with a folder context map and optional maps for agreed skills and specialists.
Read only the context and skills relevant to the current task.

Inspect existing instructions and routing first. Use **setup** when no routing
exists. If existing, partial, or legacy project-context-router routing is
present, start with **verification**, then enter **repair** for confirmed
problems before continuing maintenance. An index is a useful signal, not the
sole test; an unrelated `AGENTS.md` or `CLAUDE.md` alone is not evidence that
the router is installed. If the user requests only a read-only check, report
findings without repairing. Setup creates missing routing files and appends
root pointers; it preserves existing content and does not infer routes or
migrate older routing automatically.
Before initial setup, when adding a selected entry point or optional map, or
when an existing router has no recorded architecture-overview decision, read
[the setup guide](references/setup.md).
For existing routers, follow [the verify-and-repair procedure](references/validation-and-navigation.md#verify-then-repair).

## Before setup or verification — Install the skill in the project

Determine the loaded skill's actual base directory using the host's skill
metadata and verify the complete matching package, including `SKILL.md`,
`scripts/setup.py`, `scripts/validate.py`, and referenced guides and templates. Do not
substitute files from a different copy or version. If the source is missing or
incomplete, ask the user for the trusted complete package; do not fabricate
files or download an unspecified version.

For setup or repair, install this complete skill automatically in the project
before routing work. Do not ask the user to choose an installation scope.
Keep one shared copy in `.agents/skills/project-context-router-egym/`.
Create `.claude/skills/project-context-router-egym` as a relative symlink to
`../../.agents/skills/project-context-router-egym` so Claude Code can discover
the same copy. This layout does not depend on which client runs setup. For any
client, verify discovery rather than assuming it supports this layout.

The setup helper installs the package during setup. In repair mode, where setup
may be blocked by existing validation errors, install the package independently
before repairing. If the target already contains an identical complete package,
reuse it and the exact Claude alias. Never merge or overwrite a differing
destination or unrelated symlink. Existing copies in `.github/skills/` or a
non-matching Claude destination are conflicts: preserve them and ask how to
proceed rather than silently migrating or duplicating them. If symlinks are
unavailable, report the failure without claiming Claude discovery. Verify the installed files match the
trusted source. Reload or restart the client if needed and verify discovery when
the host supports it; otherwise report discovery as unverified. Run subsequent
checks from the project's installed package.

An explicitly read-only check must not install or change files; use the verified
loaded package for validation and report that project installation was skipped.
If installation is unavailable or the source cannot be established, stop before
setup or repair and report the blocker. Project skill files may be shared with
collaborators if committed.

## Step 1 — Establish the routing structure

Use this structure at the project root:

```text
AGENTS.md
CLAUDE.md                  # optional
.github/copilot-instructions.md  # optional
.context-routing/
  index.md
  skills.md        # optional
  specialists.md   # optional
```

- Root `AGENTS.md` points to `.context-routing/index.md`. Preserve unrelated
  instructions and managed blocks; keep routing tables in the routing folder.
- `CLAUDE.md` and `.github/copilot-instructions.md` are optional entry points;
  follow the setup guide for their selection, flags, and preservation rules.
- `index.md` contains the folder-to-context table, reading order, maintenance
  contract, and links to selected optional maps. It holds routing, not domain detail.
- `skills.md` optionally routes tasks to selected skills; `specialists.md`
  optionally routes tasks to selected project specialist agents.

Reuse verified entries when maintaining the system. Setup never
overwrites existing files; maintenance edits relevant files in place and
preserves unrelated content. Do not invent routes or automatically migrate
older routing. If a manual migration is requested, preserve unrelated guidance,
update moved links, and keep one maintained copy of each entry. Create root
entry files only when absent and append missing pointers to existing files.
State whether you are setting up or verifying and repairing the system.

Existing legacy `agents.md` folder maps remain supported. For a requested
migration, follow [legacy migration](references/validation-and-navigation.md#legacy-migration).

For initial setup or an existing router without a recorded
architecture-overview decision, follow the setup guide for root-entry
population and the decision process. During maintenance, preserve recorded
decisions and update an adopted overview in the same change when affected.

## Step 2 — Pick which folders get a CONTEXT.md

Inspect the project and propose specific folders and a detail tier for each:
**thin** for purpose and key files, **standard** for business context and
conventions as well, or **deep** for complex domains with architecture,
data flow, and impact. Favor distinct business domains, non-obvious logic,
and integration boundaries; a generic utility folder may need no file.

Skip dependencies, build output, vendored or generated code, and test fixtures.
Include test folders when their conventions matter.

Before creating or expanding folder contexts, use the host's user-question
tool (such as `AskUserQuestion` or `ask_user`) to ask these separately:

1. Show the proposed folder paths grouped by thin, standard, and deep, then
   ask: **Which folders should have a CONTEXT.md, and which detail tier should
   each use?** Offer accepting the recommendations or selecting folders and
   tiers. Allow the user to omit folders or assign different tiers.
2. Ask: **How many folder layers below the project root may contain a
   CONTEXT.md?** Offer **1 layer** (for example `src/`) and **2 layers**
   (for example `src/billing/`). The project root is layer 0. This limit
   applies to file locations, not how far to inspect source code.

Do not create a folder context until both decisions are known. If selected
paths exceed the chosen limit, identify them and ask the user to resolve the
conflict before creating them. Do not silently omit selections. If a
user-question tool is unavailable, obtain the decisions through the host's
supported user-input mechanism; do not assume approval.

Record the chosen limit in `index.md` as `Context scope: 1 layer` or
`Context scope: 2 layers`. In the folder table, include each selected tier in
the description, for example `Billing rules (tier: standard)`. The table
records selected folders alongside any preserved legacy routes; label legacy
routes without a recorded tier as unclassified, not as user-selected.
Create only the selected files within the limit.
Reuse recorded choices on maintenance without asking again unless the user
requests a change. If an existing project has no recorded choices, ask before
expanding its context scope; preserve existing files and routes, including
those outside the selected limit, unless the user explicitly authorizes changes.

## Step 3 — Write each CONTEXT.md

Use [the context template](references/context-md-template.md) for section
menus and examples:

- **Thin:** purpose and key files for simple folders.
- **Standard:** purpose, key files, business context, and conventions or gotchas.
- **Deep:** standard content plus architecture, data flow, dependencies, and
  links to related context files for core or complex domains.

Use the selected tier for each folder. Each file should explain what can safely
change here and what else it affects.
Keep shared context in one place and link to it using relative Markdown links.
Replace all template markers with project evidence. Verify declared key files,
workflow inputs and commands against the project; do not invent a check.
Declare paths in Key files and Inputs lists as `./path` relative to the context
file, or `repo:path` relative to the project root. Unmarked entries are not
validated as paths; preserve legacy contexts and update declarations when
maintaining them.
Keep thin contexts thin: workflow contracts are for repeatable procedures only.
Distinguish observed live behavior from planned, stubbed, or uncertain behavior.

## Step 4 — Write the routing maps

Use [the routing templates](references/routing-table-template.md) for the root
pointer, index, and map templates. Optional maps require explicit user selection.
Resolve Markdown links relative to the file containing them.

In `index.md`, use one row per folder that has a `CONTEXT.md`, including nested
folders. Include its path, what lives there, and a link to its context file.
Sort by folder path. Leave the table empty until actual routes are verified;
never copy illustrative rows into a project map.

When at least two routed contexts describe areas with a verified interaction
or dependency, add a **Related contexts** section at the end of `index.md`.
Link the existing contexts and briefly state the relationship and when a task
should consult the other context. Verify the relationship against project
sources; folder proximity alone is not evidence. Preserve the direction of
dependencies and do not imply that a relationship works both ways. Include
only useful, confirmed relationships, without copying domain details or
creating additional context files. If none are verified, omit the section and
report material uncertainty rather than inventing relationships.

Optional skill and specialist routes require explicit user selection; prior
selections and direct requests for named entries count. Discover and recommend
only relevant verified project candidates, including Copilot and Claude Code
skills and agents. Follow [optional route selection](references/optional-route-selection.md)
when considering these maps. Preserve existing maps and do not add disputed or
unselected routes.

Keep `index.md` short: select the folder context directly from its table, and
follow optional maps only when the task needs them. Move domain detail into the
owning context file rather than expanding the router into another specification.

## Step 5 — Keep the system current

Include the maintenance contracts from the map templates in their respective
files. In maintain mode, update affected context and routing in the same change:

- A meaningful change to behavior, interfaces, business rules, architecture,
  integrations, or responsibilities updates the relevant `CONTEXT.md` and any
  affected `index.md` entries (or the legacy folder map until migrated).
- When related areas or their context files change, verify and update the
  affected relationships in `index.md`; remove stale entries and keep the
  section at the end when present. Preserve unrelated index content.
- Respect the recorded folder selections, tiers, and layer limit. Ask before
  adding a new context outside those selections or changing the recorded scope.
- An explicitly selected skill or specialist change updates its optional map;
  do not add new routes without selection.
- When an adopted architecture overview is affected, update it in the same
  change; keep unchanged decisions and avoid unnecessary rewrites.
- A change to the routing entry points updates `index.md` and the root pointers.

Avoid rewriting context for edits that do not change its meaning. Report what
you updated rather than leaving maintenance for a separate follow-up.

## Step 6 — Set up, validate, and navigate

Follow [the setup guide](references/setup.md) for initial setup or when adding
a selected entry point or optional map. For existing routers, follow the
verify-and-repair procedure linked above; for read-only checks, run the
validator without changing files. Use Python 3.10 or newer and the verified
package from the installation check above. Replace `<skill-dir>` with its
actual directory and `<PROJECT_ROOT>` with the project's existing root:

```sh
python "<skill-dir>/scripts/validate.py" "<PROJECT_ROOT>"
# Add --claude-md and/or --copilot-instructions to require selected entry points.
# Skill development only:
python -B -m unittest discover -s "<skill-dir>/tests" -v
```

See [validation and navigation](references/validation-and-navigation.md) for
validator coverage, limitations, and required navigation checks.

## Step 7 — Verify context quality and report

Follow the proportional navigation and context-quality checks in [validation
and navigation](references/validation-and-navigation.md). Report changes,
validation results, unverified claims, and unresolved destinations; a passing
validator alone does not establish factual accuracy.
