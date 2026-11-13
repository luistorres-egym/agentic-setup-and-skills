# Validation, fresh navigation, and context quality

## Structural and basic content checks

Use Python 3.10 or newer; the package uses only the standard library. First
follow the installation instructions in SKILL.md: setup and repair install the
complete skill in `.agents/skills/project-context-router-egym/` and create a
relative per-skill alias under `.claude/skills/`. A loaded instruction alone does
not guarantee that its scripts are available. Use the verified project package
for `<skill-dir>`; a read-only check uses the loaded package without installing:

```text
python "<skill-dir>/scripts/validate.py" "<PROJECT_ROOT>"
```

When developing or updating this skill, also run its fixture suite:

```text
python -B -m unittest discover -s "<skill-dir>/tests" -v
```

For setup, selected entry points, Python prerequisites, and helper failure
handling, follow [the setup guide](setup.md). The validator accepts a direct
index link or `@AGENTS.md` backed by a valid root pointer. Its matching flags
require selected entry points; otherwise unselected files without a pointer
are notes. For an existing or partial router, run the validator first and
follow the workflow below. An explicitly read-only check reports findings
without making repairs.

## Verify then repair

Before changing an existing router, inspect its root entries, index or legacy
folder map, linked contexts, and recorded selections. Run the validator first,
requiring selected entry points when appropriate. Review errors and notes,
check relevant routes and claims against project sources, and record initial
findings so they remain distinct from repairs.

Repair only confirmed problems within the requested scope. Preserve unrelated
instructions, managed blocks, selections, and legacy routes. Do not invent
folder selections, create disputed contexts, remove potentially valuable
content, or migrate without user approval. Ask when a repair is ambiguous;
otherwise continue safe repairs and report unresolved issues. Verify unmarked
paths before treating them as paths. Do not rerun setup to repair an invalid
router: its pre-publication validation can reject existing errors.

Rerun the validator and applicable source checks and navigation walks. Report
repairs, remaining errors, unverified claims, and unavailable checks. A clean
verification is a no-op; if checks cannot run, report the blocker.

Legacy indexes linked to an `agents.md` folder map remain supported with an
extra-hop note. Nothing is migrated or deleted automatically.

## Legacy migration

Only when migration is explicitly requested, preserve unrelated index content,
transfer the verified folder table and maintenance contract to the index, update
known links, and keep one authoritative folder table. Verify every former route
and inspect references before removing the old map; keep it until external
consumers and unresolved references are accounted for.

The validator checks entry points, table shape, route identities, containment,
supported relative links and optional maps. A linked context cannot be empty
or heading-only. Recognizable unfinished markers such as `<folder path>`,
`<what...>`, `<fill this in>`, `{TODO}`, `[TODO]` and `[TODO: ...]` are errors
outside fenced examples. Generic type parameters, comparisons and ordinary
prose mentioning TODO are not template markers. Standalone markers in inline
code are checked; markers embedded in inline command or route examples are not.

Declared backticked paths in bullet lists under `Key files`, `Inputs`, or
Reference/Required/Working/Per-run/Future-output inputs headings are checked;
standalone bold headings also work. Prefix a path with `./` to resolve it from
the context file's directory, or `repo:` to resolve it from the project root.
For example, `./Makefile` and `repo:src/file with spaces.py` are explicit paths;
`Invoice.total` is not. Declarations must remain inside the project after
resolving symlinks. Key files and reference/required inputs must exist.
Working/per-run/future-output inputs may be absent if their parent exists.
Marked globs, absolute paths and backslashes are invalid; commands and fenced
examples are not executed. Use separate bullets for required/reference and
future working inputs; mixed declarations are conservatively checked as required.

Existing unmarked entries are preserved and reported as unverified notes, not
validated paths. This includes extensionless names such as `Makefile`; it may
also include identifiers. Prefix actual paths when maintaining a context.
URLs, external paths, unmarked globs and command-like examples are skipped.

Inline Markdown links support angle-bracket destinations with spaces. The
parser ignores fenced examples and remote links. Reference-style/HTML/wiki
links, nested destination parentheses and fragment targets are outside its
scope. External capability paths are checked only for local availability;
their definitions are not executed. Cycles are visited once, not rejected.

Size notes are advisory: entry files over about 60 lines or contexts over about
8,000 estimated tokens deserve inspection. Estimates use characters divided
by four, not a model tokenizer. The validator does not measure the complete task
load, prove source claims, run documented commands, detect semantic duplication
or discover all unmapped source folders.

## Fresh navigation walkthrough

For a genuinely small maintenance edit with no consequential behavior or route
change, check affected links and claims against their sources, run the validator,
and report if an independent walk was not performed. Initial setup, routing
redesigns, and consequential changes require the independent walks below.

After structural validation, give a fresh isolated agent the project entry
point, a representative task and acceptance criteria. Supply no chosen route,
project history, author's narrative or earlier verdict. Use the established
delegation policy; context evaluation does not authorize application edits.
For consequential maintenance, walk affected routes and necessary boundaries.
For initial setup or a routing redesign, cover these applicable cases:

1. **Local task:** Start at root `AGENTS.md` or `CLAUDE.md` as appropriate, select a folder directly from
   `index.md`, open its context, identify source evidence and find a concrete
   verification step. Target entry plus at most two additional reads to reach
   the first relevant context; source inspection follows afterward.
2. **Cross-folder task:** Select both contexts, using the index's related-context
   section when applicable. Verify its relationship against project sources,
   identify the real integration boundary, explain what changes affect and do
   not affect, and find checks for both sides. A valid link alone does not
   establish that the relationship is accurate.
3. **Workflow-dependent task:** Follow a relevant selected skill or specialist
   map, verify its entry point and explain the check and stop conditions.
   Do not create a route merely to test it; mark this case not applicable when
   no such workflow is needed.

Record actual paths opened in order, the reason for each read, discovered
verification steps and pass/fail evidence in the task report or a requested
artifact. Keep review evidence out of maintained context files. Fail a walk
that scans all contexts to choose a route, opens unrelated contexts, cannot
find required inputs/checks or needs undocumented prior knowledge. Necessary
dependencies are permitted; document justified extra hops for large or legacy
layouts. If isolation is unavailable, report the independent walk as pending.

## Context-quality review

Have the fresh evaluator check the contexts used in the walks:

- **Scope:** Explain the folder's job and useful non-obvious details. Thin
  contexts need no artificial pipeline schema; contracts are for repeatable work.
- **One maintained home:** Shared facts have an owner and links. Routers carry
  short descriptions and destinations, rather than copied domain explanations.
- **Accuracy:** Spot-check important rules, interfaces and commands against
  owning source/tests. Distinguish live behavior from plans/stubs. Cite evidence
  and record material contradictions; a date alone does not establish freshness.
- **Impact:** Verify affected callers/integrations and important boundaries
  against source before claiming something is unaffected.
- **Size:** Estimate the project material actually loaded by each walk, excluding
  system/tool overhead. Investigate loads around 8,000 estimated tokens as a
  starting guideline, not a universal failure threshold. Improve routing or
  shorten owned context before adding navigation documents.

Fix failures and rerun affected checks and walks. Do not call context verified
while an applicable walk or source check is missing. Report unresolved limits,
skipped checks and deferred migration explicitly. A link-validator pass alone
does not establish factual quality.
